# **3.4 Wrong Credentials**

#### **Problem**:  
Incorrect or missing credentials (e.g., FTP, email, Pushover) could prevent critical processes such as notifications, file uploads, or system alerts from functioning properly. Without proper error handling, these failures may go unnoticed, leaving the system in an unpredictable state.

#### **Solution**:  
Implement a **credential validation process** at system startup that checks the required credentials from the `.env` file and `config.ini`. If any credentials are missing or invalid, the program should:
1. **Log an error** detailing the specific issue (missing or incorrect credential).
2. **Send an admin notification** (via email or Pushover) alerting the system administrator of the issue.
3. **Gracefully stop affected processes** while keeping the rest of the system running.

#### **Implementation Steps**:

1. **Credential Validation at Startup**:
   - On startup, the system will validate that all credentials (FTP, email, Pushover) are present and correctly formatted.
   - The validation process checks the `.env` file for sensitive information (e.g., API keys, passwords) and the `config.ini` for structural settings (e.g., server URLs, ports).
   - If any required credentials are missing or improperly configured, an error will be logged and an admin notification will be triggered.
   
2. **Credential Check Logic**:
   - Credentials for each subsystem (FTP, email, Pushover) will be validated separately. For example:
     - **FTP**: The server URL, username, password, and port must be verified.
     - **Email**: The SMTP server, port, username, and password must be validated, along with any security protocols (e.g., STARTTLS).
     - **Pushover**: The user key and API key must be present and valid.

3. **Error Handling**:
   - If a credential is invalid or missing:
     - The system will log the issue with the type of missing or invalid credential.
     - The system will send an admin notification with the error details.
     - The process relying on the invalid credential (e.g., email notifications, FTP uploads) will be paused until the issue is resolved.

4. **Retry and Fallback**:
   - For critical credentials (like FTP for file uploads or Pushover for notifications), the system will attempt to retry using a **retry mechanism** (configurable via the `config.ini`).
   - If credentials remain invalid after retry attempts, the system will fallback to a secondary notification method (e.g., fallback from Pushover to email).

#### **Configuration Parameters**:

To support the validation of credentials and fallback mechanisms, the following parameters can be included in the `config.ini` file:

```ini
; ==================
; CREDENTIAL VALIDATION SETTINGS
; ==================

[Credential_Validation]
validate_on_startup = true  ; Enable/disable credential validation at startup
credential_retry_attempts = 3  ; Number of retry attempts if credential validation fails
credential_retry_delay = 30  ; Time (in seconds) between retry attempts
invalid_credential_action = pause_process  ; Action to take on invalid credentials (pause_process, stop_system)
fallback_method = email  ; Fallback method for notifications if Pushover fails
```

#### **Example Workflow**:

1. **Startup**:
   - The program reads the `.env` and `config.ini` files.
   - It checks the credentials for FTP, email, and Pushover services.
   - If credentials are correct, the program proceeds with normal operation.

2. **Invalid Credentials**:
   - If FTP credentials are incorrect (e.g., wrong password or missing server URL), the system logs the error and sends an admin notification.
   - The program will pause FTP uploads and attempt to retry based on the `retry_attempts` parameter in `config.ini`.

3. **Notification to Admin**:
   - If credentials remain invalid after retries, the system falls back to email (if the issue is with Pushover) or logs a critical error if no fallback method is available.

4. **Process Pausing**:
   - Affected services (e.g., FTP upload or notifications) will pause until the credentials are corrected. Other parts of the program will continue to function as normal.

