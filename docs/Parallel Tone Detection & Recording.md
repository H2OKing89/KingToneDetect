# **Parallel Tone Detection & Recording**

The focus of this section is to handle simultaneous tone detection for multiple fire departments and ensure that recordings and notifications are processed in parallel without overlap. This is critical in scenarios where multiple departments may be paged simultaneously, such as joint responses or incidents requiring multiple units. The system is designed to ensure that all departments receive their recordings and notifications independently, even when they are being processed simultaneously.

#### **Tone Detection**
- **Tone Detection Process**: 
  - The program listens for specific tones associated with each department, as defined in the `tone.cfg` file. When a tone is detected that matches the configurations, the system immediately logs the tone and begins the notification process.
  - **Parallel Detection**: Multiple tones can be detected in parallel, allowing the system to handle simultaneous tone triggers from different departments. This is controlled by the `parallel_tone_detection` setting in the configuration.
  - **Tone Parameters**:
    - `atonelength`: Specifies the length of the A tone.
    - `btonelength`: Specifies the length of the B tone.
    - `tone_tolerance`: Allows a small deviation in the tone frequencies to account for hardware or environmental inconsistencies.
    - `release_time`: Ensures the system waits before releasing the audio device after detecting silence.

#### **Recording**
- **Parallel Recording**:
  - Once a tone is detected, the system initiates recording for the corresponding department. If multiple tones are detected at the same time, the system supports parallel recordings, meaning each department’s recording is captured independently.
  - **Recording Isolation**: Parallel recordings are isolated from each other, ensuring that the audio streams don’t interfere or overlap.
  - **Recording Parameters**:
    - `record_delay`: Delay before recording starts after tone detection to prevent capturing unwanted noise at the start.
    - `mini_record_time` and `max_record_time`: Set the minimum and maximum duration for each recording.
    - **Adaptive Silence Detection**: The system automatically adjusts the silence detection thresholds to handle varying noise levels, ensuring that recordings stop only when the dispatcher has finished speaking.
    - `convert_to_mp3`: After recording in WAV format, files are converted to MP3 for easy sharing and reduced file size.

- **Disk Space Management**:
  - The system monitors available disk space to ensure there is enough room for new recordings. If the available disk space falls below a critical threshold, the program will stop recording and send an error notification to administrators.
  - **Parameters**:
    - `disk_space_limit_mb`: The minimum disk space required to continue recording.
    - `disk_space_error_threshold_mb`: If available space drops below this value, the system will stop recording.
    - `on_disk_space_error`: Defines the action to take when disk space is critically low (e.g., stop recording).

#### **Notification Handling**
- **Pre-Notification**:
  - Before recording starts, pre-notifications are sent to the relevant department via email or Pushover to alert them of the upcoming recording. These notifications serve as an early warning for dispatch events.

- **Post-Notification**:
  - Once the recording is complete, notifications are sent to the department with a link to the recorded file. The notifications can be sent via email or Pushover, depending on the department's preferences.
  - **Parallel Notification Sending**: Notifications are sent in parallel, ensuring that multiple departments are notified simultaneously without waiting for each other. The system ensures that each department receives its own notification without overlap.
  - **Notification Retry Logic**: If a notification fails to send, the system will retry the process up to the configured number of retries, ensuring reliable delivery.

#### **Configuration Parameters**
```ini
[Tone_Audio_Parameters]
atonelength = 0.6  ; Length of the A tone (in seconds)
btonelength = 0.6  ; Length of the B tone (in seconds)
tone_tolerance = 0.02  ; Allowable deviation in Hz for tone detection
release_time = 2  ; Time (seconds) to release audio device after silence
parallel_tone_detection = true  ; Enable parallel tone detection for multiple departments
max_parallel_recordings = 5  ; Maximum number of parallel recordings allowed
recording_priority = equal  ; Set recording priority (equal, department-based)

[Recording]
record_delay = 3.5  ; Delay before recording starts after tone detection (seconds)
mini_record_time = 30  ; Minimum recording time (seconds)
max_record_time = 40  ; Maximum recording time (seconds)
adaptive_silence_threshold = true  ; Automatically adjust silence threshold
initial_silence_threshold = -40  ; Starting threshold for silence detection (dB)
min_silence_threshold = -60  ; Minimum silence threshold allowed (dB)
silence_detection_grace_period = 5.0  ; Time before silence detection starts (seconds)
silence_duration_threshold = 10.0  ; Maximum silence before stopping recording (seconds)
convert_to_mp3 = true  ; Convert WAV to MP3 after recording
mp3_bitrate = 64000  ; Bitrate for MP3 conversion (bps)
wav_file_path = /audio/wav/  ; Directory for WAV files
mp3_file_path = /audio/mp3/  ; Directory for MP3 files
audio_codec_path = /codec/  ; Directory for audio codec used during conversion
disk_space_limit_mb = 500  ; Stop recording if disk space falls below this threshold (in MB)
max_parallel_recordings = 5  ; Maximum number of parallel recordings allowed
recording_isolation = true  ; Enable isolation for parallel recordings
recording_priority = equal  ; Priority handling for simultaneous recordings
disk_space_error_threshold_mb = 100  ; Trigger an alert if available space drops below this
on_disk_space_error = stop_recording  ; Action to take when disk space is critically low

[Notification]
send_notifications_in_parallel = true  ; Allow notifications to be sent in parallel
notification_retry_on_fail = true  ; Retry failed notifications per department
max_notification_retries = 3  ; Retry sending notifications up to 3 times
```

#### **Logging Reminder**
- Ensure that the logging system captures key events for tone detection, recording start/stop times, disk space warnings, and notification failures. This will help with troubleshooting and overall system reliability.
- Logging should include:
  - **Tone Detection Logs**: Log each detected tone and the associated department.
  - **Recording Logs**: Log the start and end times of each recording.
  - **Notification Logs**: Log when notifications are sent and whether they succeed or fail.
  - **Error Handling Logs**: Log disk space errors, notification failures, and retry attempts.

#### **Next Steps**:
- Implement the configuration settings for parallel tone detection and recording.
- Set up the notification retry logic for each department to ensure notifications are delivered reliably.
- Ensure the logging configuration covers all critical events, including tone detection, recording, and notifications.

