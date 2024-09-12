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

## Ongoing Tasks

### 5. **Disk Space Exhaustion**
Continuous recordings without proper cleanup could fill up disk space. Implement **file cleanup** for old recordings and monitor disk usage.

### 6. **Configuration Errors**
Incorrect or missing settings in `config.ini` or `.env` can lead to failures. Add **validation** and default values in your code.

### 7. **High CPU/Memory Usage**
Audio processing and tone detection could consume a lot of resources. Implement **performance monitoring** and optimize processing algorithms.

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
