# Version Control - Core Workflow Document

## Changelog
- **v1.0.0** (September 14, 2024): Initial release of the core workflow document. Includes heartbeat monitoring, tone detection, pre/post notifications, audio recording logic, and FTP configuration.

## Document History

| Version  | Date               | Author           | Description                                                       |
|----------|--------------------|------------------|-------------------------------------------------------------------|
| v1.0.0   | September 14, 2024  | Quentin King      | Initial release. Includes core workflow details and config structure. |
---

# **KingToneDetect Workflow Reference**

## **Overview**
KingToneDetect is a program designed to detect tones from dispatch audio, process recordings, and send notifications based on configurable department settings. This document provides an overview of the core workflows, configuration files, and key system components.

---

## **Core Workflow**

1. **Tone Detection**:
   - The system listens for specific frequencies (tones) that are associated with different fire departments.
   - Tones are defined in the `tone.cfg` file, which maps frequencies to department names.

2. **Pre-Notification**:
   - When tones are detected, the system immediately sends out a **pre-notification** to alert the appropriate department.
   - This notification is sent based on the department’s preferences configured in `tone.cfg` (email, Pushover, or both).

3. **Recording**:
   - The program starts recording the dispatcher’s audio after tone detection.
   - Recording will continue until either a silence threshold is reached or the maximum recording time (defined in the config) is met.
   - The recording is saved as a WAV file, and optionally converted to MP3 for easier distribution.

4. **Post-Notification**:
   - Once recording is complete, the system sends a **post-notification** to the department, which includes a link to the recorded audio file.
   - Notifications are customized per department based on `tone.cfg`.

5. **Error Handling**:
   - If any errors occur during tone detection, recording, or notification processes, the system will log the issue and send an **admin notification** via Pushover to ensure issues are addressed promptly.

---

## **Configuration Files**

### **tone.cfg**
This file is used to define department-specific settings, including tones, notification preferences, and external scripts.

#### **Key Fields in `tone.cfg`:**
- **department_name**: Name of the fire department.
- **btone / atone**: The frequencies for tone detection.
- **notify_email**: Toggle to enable or disable email notifications for this department.
- **notify_pushover**: Toggle to enable or disable Pushover notifications.
- **pre_external_script**: Optional external script to run upon tone detection.
- **post_recording_external_script**: Optional external script to run after recording.
- **post_email_list**: List of email addresses to notify.
- **pushover_token / pushover_api**: Credentials for sending Pushover notifications.

For more detailed examples of this configuration, refer to **Document Two** (Program Planning/Scenarios Document).

---

### **config.ini**
This is the primary configuration file for the core program. It contains system-wide settings such as email credentials, FTP configurations, heartbeat monitoring, and performance monitoring parameters.

#### **Key Sections in `config.ini`:**
- **Email Settings**: Defines the email server, port, and authentication details.
- **FTP Settings**: Configures FTP upload options for audio files.
- **Recording Settings**: Maximum recording time, silence thresholds, and audio format options.
- **Heartbeat Monitoring**: Settings to configure the optional heartbeat functionality.
- **Error Handling**: Sets the retry limits and timeouts for notification and upload failures.
- **Performance Monitoring**: CPU and memory usage thresholds.

For detailed configuration options, refer to the specific sections in **Document Two**.

---

### **.env File**
The `.env` file is used to securely store sensitive information such as API keys, email passwords, and FTP credentials. These values are referenced in the program without hardcoding them into the scripts.

---

## **Notification Workflow**

1. **Department-Specific Notifications**:
   - Each department’s notification preferences are stored in `tone.cfg`. Departments can choose to receive email notifications, Pushover notifications, or both.
   - If any critical fields (like Pushover API keys or email lists) are missing, the system will skip the notification for that department and log the issue without causing an error.

2. **Pre- and Post-Notifications**:
   - A **pre-notification** is sent when tones are detected, alerting the department that a page has been received.
   - After the recording is complete, a **post-notification** is sent with a link to the recorded audio file.

3. **Error Handling for Notifications**:
   - If a notification fails (due to incorrect credentials or network issues), the system will log the error and send an **admin notification** via Pushover, alerting the administrator to the problem.

---

## **Error Handling**

### **Admin Notifications**
- Admin notifications are sent for critical failures such as:
  - Failed notifications (email or Pushover).
  - FTP upload failures.
  - Configuration errors in `config.ini` or `.env`.
- Admin notifications are sent via Pushover, as configured in the `config.ini`.

### **Retry Logic**
- The system includes retry mechanisms for both notifications and FTP uploads. If an operation fails (e.g., sending an email), it will retry up to a predefined limit (`retry_count`) before giving up and sending an admin alert.

---

## **Performance Monitoring**

To ensure the program runs efficiently, performance monitoring is enabled. This feature checks CPU and memory usage at regular intervals. If resource usage exceeds the configured thresholds, the system will log a warning and alert the admin if necessary.

---

Got it! Let’s update the **Heartbeat Monitoring** section to reflect that it will be saved locally on the system without the FTP component.

Here’s the revised version:

---

# **KingToneDetect Workflow Reference**

...


## **Optional: Heartbeat Monitoring**

KingToneDetect includes an optional **heartbeat monitoring** feature to ensure the system is running correctly. This heartbeat is saved as a local file on the system and can be used by external monitoring tools to verify that the program is alive and functioning.

### **How Heartbeat Monitoring Works**:
- The heartbeat feature updates a small file on the local system at regular intervals.
- The file can be monitored by external tools to ensure that the program is operational.
- If the file is not updated within the expected timeframe, it can indicate that the system is down or experiencing issues.

### **Heartbeat Configuration**:
- **Heartbeat Frequency**: The interval at which the heartbeat file is updated, configurable in `config.ini`.
- **Local File Path**: The path where the heartbeat file will be saved.
- **Optional Monitoring**: Users can integrate third-party monitoring tools to track the heartbeat and ensure system health.

Example configuration in `config.ini`:
```ini
[Heartbeat]
heartbeat_enable = true  ; Enable or disable heartbeat monitoring
heartbeat_file_path = /logs/heartbeat/  ; Path to store heartbeat logs
heartbeat_update_interval = 60.0  ; Interval in seconds for updating heartbeat
```

### **Benefits of Local Heartbeat Monitoring**:
- Ensures that the system is running and healthy.
- Provides real-time information about the program’s status.
- Can be monitored by external tools or scripts that check for regular updates to the heartbeat file.

---

## **Future Extensions**

This document outlines the core workflow and functionality of KingToneDetect. Future improvements or changes can be documented here as the system evolves.

