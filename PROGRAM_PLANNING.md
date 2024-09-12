# Program Planning for KingToneDetect

This document outlines the key scenarios that have been addressed or are currently being worked on during the development of KingToneDetect. These scenarios help ensure that the program is robust, resilient, and user-friendly.

## Completed Tasks

### 1. **Runaway Recording**
To prevent runaway recordings, the following safeguards have been implemented:
- **Max Recording Time**: The `max_record_time` is set to 40 seconds, ensuring that recordings cannot exceed this time even if no silence is detected.
- **Adaptive Silence Detection**: An adaptive silence threshold is used to account for varying background noise levels, starting with an initial threshold of -40 dB and adjusting down to a minimum of -60 dB.
- **Silence Detection Grace Period**: A 5-second grace period (`silence_detection_grace_period`) is applied after tone detection to avoid prematurely stopping the recording during short pauses in dispatcher communication.
- **Silence Duration Threshold**: If silence persists for more than 10 seconds (`silence_duration_threshold`), the recording will stop automatically, preventing unnecessary continuation.
- **Audio Format and Storage**: Recorded audio is stored in both WAV and MP3 formats, with MP3 conversion handled automatically to save storage space and facilitate easy sharing.

---

### 2. **Incorrect Tone Detection**
To improve tone detection accuracy and avoid false positives or missed tones, the following measures have been implemented:
1. **Configurable via `config.ini`:**
   - **Tone Offset**: The `tone_offset` setting allows for calibration of the tone frequencies to account for slight variations in the detected tone frequencies.
   - **Tone Tolerance**: The `tone_tolerance` allows for small deviations in tone frequency, with a default of 0.02 Hz.
   - **Tone Duration**: To prevent false detections, the program ignores tones shorter than 0.6 seconds.
   - **Audio Threshold**: The `audio_threshold` is set to 54 dB to filter out background noise and focus on relevant tones.
   
2. **Hardcoded in Program Logic:**
   - **Logging Detected Tones**: Detected tones and mismatches are logged for debugging.
   - **Test/Debug Mode**: Additional logging is available in test mode for accuracy checking.

---

### 3. **Email/Notification Failures**
To prevent failures when sending emails or notifications, the following safeguards have been implemented:
1. **Configurable via `config.ini`:**
   - **Retry Mechanism**: The program will retry sending notifications with 2 retries and a 30-second delay.
   - **Timeouts**: Timeouts for both email and Pushover are set to 30 seconds to prevent hanging during notification attempts.
   - **Fallback Notification Method**: Pushover is used as a fallback if email fails after retries.
   - **Rate Limiting**: A rate limit is applied to prevent notification spam, with a maximum of 3 notifications per hour.
   
2. **Hardcoded in Program Logic:**
   - **Retry Logic**: Notifications are resent if they fail, and fallback methods are triggered if all retries fail.
   - **Logging and Error Handling**: Any failure is logged, and administrators are alerted.

---

### 4. **FTP Upload Failures**
To handle potential failures when uploading audio files to the FTP server due to network or server issues, the following measures have been implemented:
1. **Configurable via `config.ini`:**
   - **Retry Mechanism**: The system will attempt up to 3 retries with a 30-second delay between attempts.
   - **Timeout Settings**: FTP timeouts are set to 30 seconds to prevent hanging uploads.
   - **Passive Mode**: FTP is configured to use passive mode for better compatibility.
   - **Local Backup for Failed Uploads**: Failed uploads are stored locally for future re-upload attempts.

2. **Hardcoded in Program Logic:**
   - **Retry Logic**: If uploads fail, the system will retry up to 3 times and save the file locally if all retries fail.
   - **Local Backup Mechanism**: Failed uploads are stored in a backup folder for reprocessing.
   - **Error Logging**: Any FTP failures are logged for future troubleshooting.

---

### 5. **Disk Space Exhaustion**
Continuous recordings without proper cleanup could fill up disk space. To prevent disk space exhaustion, the following measures will be implemented:

1. **File Cleanup Process**:
   - **Retention Policy**: Audio files will be deleted or archived after they exceed the specified retention period (`file_retention_days`).
   - **Maximum Storage Limit**: Once the total disk space used for recordings exceeds the specified limit (`max_storage_limit`), older files will be cleaned up to free space.
   - **Cleanup Interval**: The cleanup process will run at regular intervals (`cleanup_interval_hours`) to ensure that storage is managed continuously.
   - **Archiving**: Optionally, files can be moved to an archive directory instead of being deleted (`archive_old_files` and `archive_directory`).

2. **Disk Monitoring**:
   - **Low Disk Space Warning**: Disk space usage will be monitored at set intervals (`disk_monitoring_interval_hours`). If the available space drops below a set threshold (`low_disk_threshold`), a warning will be logged, and notifications will be sent.
   - **Backup for Failed Uploads**: If cleanup fails, audio files will be backed up to a local directory for manual cleanup or reprocessing later.

3. **Configurable via `config.ini`:**
   - **Retention Policy**: Configure how long files should be kept before cleanup.
     - `file_retention_days = 7` (default retention period)
   - **Max Storage Limit**: Set a maximum storage limit for audio files to prevent overuse of disk space.
     - `max_storage_limit = 1000` (in MB)
   - **Low Disk Space Threshold**: Set a threshold to trigger warnings when disk space is low.
     - `low_disk_threshold = 100` (in MB)

4. **Hardcoded in Program Logic**:
   - **File Cleanup Logic**: The program will periodically check for old files and delete or archive them according to the retention policy.
   - **Disk Monitoring**: The program will check for available disk space and send alerts if space is critically low.

---


### 6. **Configuration Errors**
Incorrect or missing settings in `config.ini` or `.env` files can lead to program failures. To prevent such issues, the following validation and defaulting measures have been planned and will be implemented:

1. **Validation Process**:
   - **Mandatory Fields Check**: The program will check that all necessary configuration fields are present in both `config.ini` and `.env`. If any mandatory fields (such as `email_server` or `ftp_password`) are missing, the program will log an error and terminate with an informative message.
   - **Data Type Validation**: The program will validate the types of configuration parameters to ensure they are correctly formatted (e.g., ports are integers, paths are strings).
   - **Range Validation**: Parameters like thresholds and limits will be validated to ensure they fall within acceptable ranges (e.g., `audio_threshold` must be between 0-100 dB).
   - **Graceful Handling of `.env` Misconfigurations**: Missing or incorrect values in the `.env` file will be handled by either logging an error or using safe defaults. The program will notify users of any critical issues in the `.env` file that may prevent successful execution.
   
2. **Default Values**:
   - The program will provide **default values** for non-critical settings to ensure smooth operation even if certain configuration entries are missing.
   - If optional values such as `email_priority` or `max_record_time` are not specified, they will default to safe values to prevent failures.
   - Missing or empty environment variables in `.env` will also trigger default values to avoid interrupting the execution.

3. **Configurable via `config.ini` and `.env`:**
   - Critical settings from both `config.ini` and `.env` will be checked to ensure they are set correctly, including:
     - **Email Settings**: `email_server`, `email_port`, and `email_from` will be validated to ensure correct email setup.
     - **FTP Settings**: The program will verify that the FTP server, username, and password are provided.
     - **Pushover Settings**: Pushover API keys will be checked for correctness.
   - **Optional Settings**: Parameters like `email_priority` and `max_record_time` will use default values if they are not present in the configuration:
     - `email_priority` will default to 3 if not specified.
     - `max_record_time` will default to 40 seconds.
     - `file_retention_days` will default to 7 days.

4. **Hardcoded in Program Logic**:
   - **Validation Logic**: A validation function will check that critical fields are present in both `config.ini` and `.env`, and their types and values are correct. If any mandatory fields are missing or incorrectly set, the program will log an error and exit gracefully.
   - **Graceful Exit**: If critical configuration data (such as credentials) is missing, the program will terminate with a detailed error message to prevent failures during runtime.
   - **Fallback to Defaults**: If non-critical fields (like thresholds or timeout settings) are missing, the program will substitute default values and continue running.

---

### Example Configuration Validation:

1. **Configurable Fields**:
   - The program will ensure that critical fields (e.g., `email_server`, `audio_upload_ftp_server`, `pushover_user_key`) are set, logging an error if any are missing or misconfigured.
   - Non-critical fields will fall back to defaults when omitted, such as `email_priority = 3` and `max_record_time = 40`.

2. **Hardcoded Validations**:
   - **Mandatory Fields**: The program will check and validate the required configuration fields from both `config.ini` and `.env`, ensuring they are correctly formatted and present.
   - **Default Values**: If optional fields or environment variables are missing, they will be automatically populated with safe defaults.



---

### 7. **High CPU/Memory Usage**

Audio processing and tone detection could consume a lot of resources. To manage and reduce CPU and memory usage, the following strategies will be implemented:

1. **Performance Monitoring**:
   - The program will regularly monitor CPU and memory usage to detect spikes or unusual patterns.
   - Set up **alert thresholds** for both CPU and memory usage. When these thresholds are exceeded, the program will log a warning or send notifications, depending on the configuration.

2. **Optimization of Algorithms**:
   - **Efficient Audio Processing**: The program will optimize audio processing algorithms by using efficient libraries (such as **NumPy**, **SciPy**, or **PyAudio**) and minimizing redundant operations.
   - **Memory Optimization**: The program will use memory-efficient data structures and lazy loading for large files, minimizing memory overhead.
   - **Multithreading/Asynchronous Processing**: When appropriate, audio processing will be spread across multiple CPU cores to balance the load.

3. **Configurable via `config.ini`:**
   The following options will allow users to configure performance-related parameters:
   ```ini
   ; Performance Monitoring Configuration
   [Performance_Monitoring]
   enable_monitoring = true  ; Enable/disable performance monitoring
   cpu_usage_threshold = 80  ; Trigger a warning if CPU usage exceeds this percentage
   memory_usage_threshold = 70  ; Trigger a warning if memory usage exceeds this percentage
   monitor_interval_seconds = 60  ; Interval for checking CPU/memory usage (in seconds)
   ```

4. **Hardcoded in Program Logic**:
   - The program will include logic to monitor CPU and memory usage at regular intervals using system libraries (such as `psutil` in Python). If the usage exceeds predefined thresholds, the program will log a warning and can notify administrators through the configured notification system.
   - **Graceful Handling of High Load**: If high CPU or memory usage is detected, non-critical processes may be paused or deferred, or the program may trigger a high-load alert (e.g., via Pushover or email).

5. **Example Code for Performance Monitoring** (Python):
   ```python
   import psutil
   import time

   def monitor_performance(cpu_threshold, memory_threshold, interval):
       while True:
           # Get the current CPU and memory usage
           cpu_usage = psutil.cpu_percent(interval=1)
           memory_info = psutil.virtual_memory()
           memory_usage = memory_info.percent

           # Log a warning if CPU usage exceeds the threshold
           if cpu_usage > cpu_threshold:
               log_warning(f"High CPU usage detected: {cpu_usage}%")

           # Log a warning if memory usage exceeds the threshold
           if memory_usage > memory_threshold:
               log_warning(f"High memory usage detected: {memory_usage}%")

           # Wait for the next interval
           time.sleep(interval)

   def log_warning(message):
       print(f"WARNING: {message}")  # Replace with actual logging
   ```

### Next Steps:
1. **Monitor Performance**: Implement the CPU and memory monitoring function, and log any instances where the program exceeds the configured thresholds.
2. **Optimize Algorithms**: Review and optimize the audio processing algorithms to reduce the overall resource consumption. Consider implementing multithreading or asynchronous processing where necessary.
3. **Test under Load**: Simulate high-load scenarios to ensure the program performs efficiently without crashing or consuming excessive resources.
```

## Ongoing Tasks






### 8. **Power Outages**
If power is lost while recording, files could be corrupted or lost. Consider **auto-saving** in chunks or using a UPS for critical systems.

### 9. **Simultaneous Tone Detection**
Multiple tones detected at once could cause overlapping recordings. Use a **queue** to process recordings in sequence.

### 10. **Audio Device Failure**
The program may stop receiving audio if the input device fails. Implement **device monitoring** and fallback to alternative devices.

### 11. **Wrong Credentials**
Incorrect or missing credentials (FTP, email, Pushover) could prevent uploads or notifications. Validate credentials at startup and notify the user.

### 12. **Audio Format Incompatibility**
Some systems or integrations might not support certain audio formats (WAV, MP3). Provide **multiple output formats** or **conversion options**.

### 13. **Misconfigured `.env` File** 
Incorrect or missing values in the `.env` file could prevent the program from working properly. Handle empty or missing environment variables gracefully.
> **Note:**  
> Number 13 (Misconfigured `.env` File) has been merged with Number 6 (Configuration Errors).  
> Any `.env` misconfiguration issues will now be handled under the broader configuration validation strategy of Number 6.


### 14. **Overlapping FTP Uploads**
If multiple recordings are uploaded simultaneously, it could cause conflicts on the FTP server. Implement **file locking** or stagger uploads.

### 15. **Silent Dispatcher or Noise**
The program may fail to stop recording if the dispatcher pauses too long or background noise is detected. Adjust the **silence detection sensitivity** or implement noise reduction.

### 16. **Notification Spam**
The program might send too many notifications (e.g., multiple notifications for the same event). Implement **rate limiting** for notifications to avoid spamming.

### 17. **Heartbeat Logging Failure**
If the heartbeat logging or update to FTP fails, it may not be noticed immediately. Consider **backup logging** or redundancy for critical systems.

### 18. **Unexpected Audio File Corruption**
Files could get corrupted during recording or conversion. Implement **file integrity checks** after recording and conversion processes.

### 19. **Webhook Integration Failures**
If the webhook integration fails, you may miss critical alerts. Implement **retry logic** and alternative notification mechanisms (e.g., fallback to email).

### 20. **Timezone or Timestamp Mismatches**
If timestamps are logged incorrectly due to timezone issues, it could cause confusion or errors. Ensure proper **timezone handling** in the program.

---

These scenarios help ensure the program is robust, resilient, and user-friendly. Ongoing tasks focus on optimizing performance and addressing potential edge cases.
