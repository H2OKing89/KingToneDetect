# **Error Handling & Retry Logic** section of the document:

---

# **4. Error Handling & Retry Logic**

This section defines the centralized error handling, retry logic, and fallback mechanisms for the entire system, covering tone detection, recording, and notifications. Critical failures are logged, and retry attempts are made for recoverable issues.

## **4.1 Centralized Error Handling**

### **Logging Configuration**
The system includes robust logging for troubleshooting and error reporting. Logs can be stored locally, rotated, and archived to prevent excessive disk usage.

### **Configurable Parameters (in `config.ini`)**
```ini
; ==================
; LOGGING CONFIGURATION
; ==================
[Logging]
log_file_directory = /logs/  ; Directory for log files
log_file_max_size = 1000000  ; Max size for log files (in bytes)
log_console = true  ; Enable logging to console
log_console_level = INFO  ; Console logging level (DEBUG, INFO, WARNING, ERROR, CRITICAL)
log_console_format = %(asctime)s - %(name)s - %(levelname)s - %(message)s
log_level = INFO  ; Logging level for file output (DEBUG, INFO, WARNING, ERROR, CRITICAL)
log_file_format = %(asctime)s - %(name)s - %(levelname)s - %(message)s
log_file_rotation = true  ; Enable log rotation to prevent large log files
log_rotation_interval = daily  ; Rotate logs daily
log_backup_count = 5  ; Keep 5 rotated log files

; Log Archiving Configuration
[Log_Archive]
log_archive_enable = true
log_archive_cleanup = true
log_archive_directory = /logs/archive/  ; Directory for archived log files
log_archive_days = 7  ; Retain archived logs for 7 days
log_archive_file_count = 10  ; Maximum number of archived log files
max_total_log_size = 5000000  ; Optional: Max total size for archived logs (in bytes)
```

### **Logging Details**
- **Log Rotation**: Log files are rotated daily to avoid excessively large files. A maximum of 5 rotated logs is retained.
- **Log Archiving**: Old logs are archived for up to 7 days, with a maximum of 10 archived logs kept.
- **Fallback**: If archiving logs reaches the total size limit (`max_total_log_size`), older logs are deleted to free up space.

---

## **4.2 Retry and Fallback Logic**

### **Overview**
Retry logic is essential to ensure critical processes, such as tone detection, recording, and notifications, are retried in case of temporary failures. If retries fail, a fallback mechanism ensures the administrator is alerted.

### **Configurable Parameters (in `config.ini`)**
```ini
; ==================
; Retry and Fallback Configuration
; ==================
[Retry_Settings]
retry_attempts = 3   ; Number of retry attempts for failed processes
retry_delay = 30     ; Delay (in seconds) between retry attempts
retry_backoff = 2    ; Retry backoff multiplier for exponential backoff
fallback_method = email  ; Fallback notification method for Admin if retries fail
```

### **Retry Logic Explanation**
- **Retry Attempts**: The system will attempt up to 3 retries if a process (like a notification) fails.
- **Retry Delay**: There is a 30-second delay between each retry attempt.
- **Exponential Backoff**: For each retry attempt, the retry delay increases exponentially, making subsequent attempts longer by a multiplier (`retry_backoff` = 2).
  
### **Fallback Notification for Admin**
- **Admin Notifications**: If all retry attempts fail, the fallback notification method (default: email) will be triggered to alert the system administrator.
- **No Fallback for Tone Notifications**: Tone-related notifications (e.g., to fire departments) will not have a fallback mechanism to avoid over-complication.

---

## **4.3 Error Handling and Notification**

### **Admin Notifications for Critical Errors**
Errors such as system failures, recording issues, or failed notifications will trigger admin notifications. If all retries fail for admin notifications, the system falls back to email alerts.

### **Notification Logging**
All error notifications, including retries and final outcomes, are logged with detailed messages to assist with troubleshooting.



