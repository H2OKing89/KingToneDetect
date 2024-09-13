# Notifications and External Scripts**

## **1. Notification Process**

This section outlines the notification process, covering both pre- and post-notifications, and the management of external scripts for KingToneDetect.

### **1.1 Pre-Notification**

- **Purpose**: The pre-notification is sent immediately after a tone is detected to alert the department that a page has been received. No audio recording or link is provided at this stage.
  
- **Notification Methods**: 
  - **Email**: The pre-notification email is sent to the email addresses specified for the department in `tone.cfg`.
  - **Pushover**: A push notification is sent to the Pushover account associated with the department's Pushover API key.

- **Configurable Parameters in `tone.cfg`**:
  ```ini
  [Tone1]
  department_name = "Raymond"
  btone = 1122.5
  atone = 1185.2
  notify_email = true
  notify_pushover = true
  pre_external_script = /path_to_script/pre_script.sh  ; Script executed before recording starts
  post_recording_external_script = /path_to_script/post_script.sh  ; Script executed after recording ends
  post_email_list = raymond@firedept.com  ; List of emails for post-notification
  pushover_token = your_pushover_token
  pushover_api = your_pushover_api_key
  ```

- **External Script Handling**:
  - **Pre-Script Execution**: Before recording begins, the pre-detection external script will execute if a path is specified. If no script is provided, logging will note, "No parameters found to call external script."
  - **Logging**: For debugging purposes, the system will log if external script paths are missing or if the execution is successful.

---

### **1.2 Post-Notification**

- **Purpose**: The post-notification is sent after the recording has been saved and processed. This notification includes a link to the audio recording or an attachment (MP3) for the department.

- **Notification Methods**:
  - **Email**: The post-notification email will contain a link to the recording or the MP3 file attached if configured.
  - **Pushover**: A push notification will be sent with a link to the audio recording.

- **Configurable Parameters in `tone.cfg`**:
  ```ini
  [Tone1]
  department_name = "Raymond"
  notify_email = true
  notify_pushover = true
  post_email_list = raymond@firedept.com
  pushover_token = your_pushover_token
  pushover_api = your_pushover_api_key
  ```

- **External Script Handling**:
  - **Post-Script Execution**: After the recording has been saved, the post-recording external script will execute if specified in the `tone.cfg`.
  - **Error Handling for External Scripts**: If any of the external scripts fail, error logging will capture details, and the system will send a notification to the admin via Pushover.

---

## **2. Notification Configuration**

### **2.1 Global Email Settings**

The following settings apply to all email notifications. Emails will only be sent after the recording has been saved (no pre-notifications via email).

- **Global Configuration in `config.ini`**:
  ```ini
  [Tone_Email]
  email_enable = true
  email_subject = [d] Page Received at [t]
  email_body = [d] Page Received at [t] <file_mp3>
  email_user = ${EMAIL_USER}  ; Loaded from .env
  email_pwd = ${EMAIL_PASSWORD}  ; Loaded from .env
  email_server = smtp.example.com  ; Placeholder for SMTP server
  email_port = 587
  email_priority = 3
  email_from = ${EMAIL_FROM}  ; Loaded from .env
  email_security = STARTTLS
  email_authentication = 1
  email_send_sequential = none
  bcc = 0
  ```

### **2.2 Global Pushover Settings**

The following settings apply to all Pushover notifications, both pre- and post-recording. 

- **Global Configuration in `config.ini`**:
  ```ini
  [Tone_Pushover]
  pushover_enable = true
  pushover_title = [d] Page Received at [t]
  pushover_message = [d] Page Received at [t]
  pushover_priority = 2
  pushover_retry = 30  ; retry interval in seconds
  pushover_expire = 3600  ; expiry time in seconds
  pushover_timeout = 30  ; timeout for sending notifications
  pushover_sound = alien
  ```

---

## **3. Error Handling and Logging**

### **3.1 Notification Failures**

- **Retry Mechanism**: If notifications fail (due to network issues or incorrect credentials), the system will retry sending the notification up to 3 times, with a 30-second delay between retries.
  
- **Fallback to Admin Notification**: If a notification fails, the system will send an alert to the admin (via Pushover) with error details.
  
- **Logging**:
  - **Pre-Notification Errors**: If any pre-notifications fail (email or Pushover), the system will log the failure and send an admin alert.
  - **Post-Notification Errors**: Any failures in post-notifications will be logged and result in a fallback admin notification.

### **3.2 External Script Failures**

- **Pre-Script Failures**: If a pre-recording script fails, it will be logged, and the recording will still proceed. Admin notifications will be sent to inform of the error.
  
- **Post-Script Failures**: If a post-recording script fails, the error will be logged, and admin notifications will be triggered. The recorded audio will still be saved.

---

## **4. Future Extensions**

### **4.1 MP3 File Attachment in Emails**

In future versions, the program will have the option to attach the MP3 file to the email notification. This will allow departments to receive the audio file directly in the post-notification email without requiring them to download it from a link.

- **Configurable in `tone.cfg`**:
  ```ini
  attach_mp3 = true  ; If true, attach the MP3 to the post-notification email
  ```

---

### **5. Summary**

This document outlines the notification process and external script handling in KingToneDetect. It includes configurations for emails, Pushover notifications, pre- and post-recording scripts, error handling, and logging to ensure robust and reliable operation. Future enhancements such as MP3 attachments are planned to further improve the system.

--- 
