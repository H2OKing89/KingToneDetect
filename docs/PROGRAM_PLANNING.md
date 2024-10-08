# **Program Planning & Scenarios for KingToneDetect**

## **1. Workflow Overview**

This section outlines the complete workflow of KingToneDetect, from tone detection to post-recording notifications.

### **1.1 Tone Detection**
- **Input**: The program listens to an audio stream for specific tones assigned to fire departments.
- **Process**: When a tone is detected that matches a configured department (from `tone.cfg`), the system logs the tone and proceeds to the next step.
- **Output**: Detection triggers the following actions: 
  - Sends **pre-notifications** to the respective department.
  - Prepares the system to start recording.
  - **Parallel Detection**: Allows simultaneous tone detection for multiple departments.

### **1.2 Pre-Notification**
- **Purpose**: To immediately alert the department that a tone has been detected.
- **Notification Methods**: Emails, Pushover notifications, or both, depending on the department’s preferences set in `tone.cfg`.
- **External Scripts**: Pre-detection external scripts can be executed if specified in the configuration.

### **1.3 Audio Recording**
- **Recording Start**: Recording begins once the tone is detected.
- **Parallel Recording**: The system allows parallel recordings for multiple departments, isolating them to prevent interference.
- **Silence Detection**: Recording ends based on a silence threshold (configurable) or after a maximum recording time.
- **Storage**: The recording is saved in WAV format by default, with optional MP3 conversion.
- **Post-Processing**: After recording is complete, external post-processing scripts can be executed if specified.

### **1.4 Post-Notification**
- **Purpose**: To notify the department that the audio recording is ready and provide a link to the recorded file.
- **Notification Methods**: Sends out the link via email, Pushover, or both, based on the preferences defined for each department in `tone.cfg`.
- **Parallel Notification Sending**: Notifications are sent in parallel, ensuring no delay between departments.
- **External Scripts**: After recording, external scripts may be triggered if defined.

### **1.5 Heartbeat Monitoring**
- **Purpose**: To ensure the system is running smoothly, a heartbeat file is updated locally on the system at regular intervals.
- **Monitoring**: Third-party tools can monitor the heartbeat file for system health.
- **Configurable**: Users can configure the heartbeat interval and file path in `config.ini`.

---

## **2. Error Handling and Scenarios**

This section covers various potential issues that the program may face and how they are handled or mitigated.

### **2.1 Runaway Recording**
- **Problem**: The program may continue recording indefinitely if silence isn’t detected.
- **Solution**: A **maximum recording time** is configured to prevent runaway recordings. Silence detection thresholds are used to stop recording after a period of inactivity.

### **2.2 Incorrect Tone Detection**
- **Problem**: False positives or missed tones due to incorrect frequency detection.
- **Solution**: Tone tolerance, duration, and thresholds are finely tuned. Logging is added for tone mismatches, and debug mode can be used to troubleshoot issues.

### **2.3 Email/Notification Failures**
- **Problem**: Notifications may fail to send due to incorrect configuration or network issues.
- **Solution**: A **retry mechanism** is implemented with fallback to alternative methods (e.g., Pushover). The program logs errors and sends an admin notification when failures occur.

### **2.4 FTP Upload Failures**
- **Problem**: Audio files may fail to upload to the server due to connectivity issues.
- **Solution**: Retries are attempted before saving failed uploads locally. Local backups are created for files that failed to upload after retries.

### **2.5 Disk Space Exhaustion**
- **Problem**: Continuous recordings without cleanup could fill up disk space.
- **Solution**: Implement **file retention** policies to delete or archive older recordings, and regularly monitor disk space.

---

## **3. To-Do List and Future Extensions**

This section outlines the future tasks that will be worked on and additional features that may be integrated into the system.

### **3.1 Zone and Location Information**
- **Problem**: Lack of zone and location data in notifications can lead to confusion.
- **Solution**: Integrate **zone** and **location** data to provide more detailed notifications and logs for departments.

### **3.2 Power Outages**
- **Problem**: If power is lost while recording, files could be corrupted or lost.
- **Solution**: Consider **auto-saving** recordings in chunks and implement **backup power** (e.g., a UPS) to keep the system running during outages.

### **3.3 Audio Device Failure**
- **Problem**: The program may stop receiving audio if the input device fails.
- **Solution**: Implement **device monitoring** and have fallback mechanisms for alternative input devices.

### **3.5 Audio Format Incompatibility**
- **Problem**: Some systems or integrations may not support certain audio formats (e.g., WAV, MP3).
- **Solution**: Provide **multiple output formats** or **conversion options** to ensure compatibility across systems.

### **3.6 Overlapping FTP Uploads**
- **Problem**: Multiple recordings being uploaded simultaneously could cause conflicts on the FTP server.
- **Solution**: Implement **file locking** or stagger uploads to prevent conflicts.

### **3.8 Notification Spam**
- **Problem**: The program might send too many notifications (e.g., multiple notifications for the same event).
- **Solution**: Implement **rate limiting** for notifications to avoid spamming department personnel.

### **3.9 Heartbeat Logging Failure**
- **Problem**: If heartbeat logging or updates fail, it may go unnoticed for too long.
- **Solution**: Implement **backup logging** or redundancy for critical heartbeat monitoring systems.

### **3.10 Unexpected Audio File Corruption**
- **Problem**: Files could get corrupted during recording or conversion.
- **Solution**: Implement **file integrity checks** after recording and conversion processes to ensure data accuracy.

### **3.11 Webhook Integration Failures**
- **Problem**: Webhook integration may fail, leading to missed critical alerts.
- **Solution**: Implement **retry logic** for webhooks and use alternative notification mechanisms if webhooks fail (e.g., email or Pushover).

### **3.12 Timezone or Timestamp Mismatches**
- **Problem**: Incorrect timestamps due to timezone issues could lead to confusion.
- **Solution**: Ensure proper **timezone handling** in the program to correctly log and display timestamps.

---

## **4. Completed Tasks**

This section lists all the tasks that have been completed and their respective solutions.

### **3.4 Wrong Credentials**
- **Problem**: Incorrect or missing credentials (e.g., FTP, email, Pushover) could prevent notifications or uploads.
- **Solution**: Validate credentials at startup and notify the user of any missing or invalid credentials.

### **3.7 Silent Dispatcher or Noise**
- **Problem**: The program may fail to stop recording if the dispatcher pauses too long, or background noise is detected as a tone.
- **Solution**: Adjust the **silence detection sensitivity** and implement **noise reduction** for better performance.

### **3.10 Unexpected Audio File Corruption**
- **Problem**: Files could get corrupted during recording or conversion.
- **Solution**: Implement **file integrity checks** after recording and conversion processes to ensure data accuracy.

### **4.1 Runaway Recording**
- **Problem**: The program may continue recording indefinitely if silence isn’t detected.
- **Solution**: A **maximum recording time** of 40 seconds has been configured. Adaptive silence detection thresholds are used to stop recording after 10 seconds of continuous silence.

### **4.2 Incorrect Tone Detection**
- **Problem**: False positives or missed tones due to incorrect frequency detection.
- **Solution**: The `tone_offset` setting was added to account for variations in tone frequencies. Tolerance and duration parameters ensure tones are accurately detected.

### **4.3 Email/Notification Failures**
- **Problem**: Email or Pushover notifications may fail due to misconfigurations or network issues.
- **Solution**: A retry mechanism was added, with fallback to an alternative notification method if failures occur. Rate limiting and timeouts prevent notification spam.

### **4.4 FTP Upload Failures**
- **Problem**: Audio files may fail to upload due to server or network issues.
- **Solution**: A retry mechanism is in place for FTP uploads. Local backups are created if the system cannot upload after multiple attempts.

### **4.5 Disk Space Exhaustion**
- **Problem**: Continuous recordings without cleanup could fill up disk space.
- **Solution**: A file retention policy has been added. Older recordings are automatically deleted or archived after a configurable number of days. Disk usage is monitored, and alerts are sent when available space drops below a set threshold.

---

## **5. Version Control**

This section outlines the changes made to the document over time and provides a historical record of all updates.

Of course! Here’s an updated changelog based on the work and progress we've done, including all the sections, updates, and enhancements:

---

# **Changelog - KingToneDetect**

### **v1.2.0** (September 12, 2024)
- **Parallel Tone Detection & Recording**: Added configuration for parallel recording and notification delivery without overlap. Max recordings set to 5, and recording isolation added to prevent conflicts.
- **File Integrity Checks**: Introduced file integrity checks after recording and conversion to handle potential corruption. Implemented retry logic and corruption handling with administrator notification.
- **Corruption Handling**: Added the ability to retry corrupted file conversions, back up corrupted files, and notify administrators when recovery fails.
- **Disk Space Monitoring**: Disk space thresholds added to halt recordings when space is low, ensuring no interruptions from storage issues.
- **Updated Recording Parameters**: Introduced a 3.5-second recording delay and adaptive silence detection thresholds. Added logic for MP3 conversion, parallel recordings, and priority handling.
- **Logging Integration Reminder**: Added a reminder to integrate detailed logging mechanisms across all modules during final testing.
- **Potential Enhancements**: Documented future potential improvements for graceful stopping during low disk space, stronger checksum validation, and automated recovery.

### **v1.1.0** (September 11, 2024)
- **Simultaneous Tone Detection**: Added support for parallel tone detection and notifications for multiple departments. Configured max_parallel_recordings to prevent notification overlap.
- **Disk Space Management**: Implemented disk space monitoring and cleanup processes for audio recordings. Added archive functionality for retaining old files and setting disk space limits.
- **Error Handling & Retry Logic**: Centralized error handling and retry logic for notifications, FTP uploads, and other processes. Added fallback notifications for administrators when critical errors occur.

### **v1.0.0** (September 10, 2024)
- **Initial Release**: Core planning document created. Included workflow overviews, notification settings, and initial configuration parameters. Added scenarios for tone detection, recording, and email/Pushover notifications.

---

This updated **changelog** summarizes the latest enhancements and the work we’ve done, especially around parallel detection, file integrity checks, and error handling. Let me know if any additional changes are required before you wrap up!

---

### Document History

| Version  | Date               | Author           | Description                                                                                 |
|----------|--------------------|------------------|---------------------------------------------------------------------------------------------|
| v1.2.0   | September 12, 2024  | Quentin King      | Added parallel tone detection, recording, and notification processing with updated parameters. |
| v1.1.0   | September 11, 2024  | Quentin King      | Added simultaneous tone detection solution and disk space management strategy.                |
| v1.0.0   | September 11, 2024  | Quentin King      | Initial release with basic scenarios and planning details.                                   |

---

