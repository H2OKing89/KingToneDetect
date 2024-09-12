# **Tone Detection and Audio Handling**

## **1. Overview**
Tone detection is the core functionality of KingToneDetect. It listens to an audio input and detects specific tones associated with various fire departments. Once a tone is detected, the system initiates the recording process and sends pre-notifications to the relevant department(s). Tone detection is also the starting point for logging, error handling, and subsequent notifications.

## **2. Workflow**

### **2.1 Tone Detection**
- The system continuously listens for specific tones in the audio input. Each department has a unique tone frequency assigned, which is configured in the `tone.cfg` file.
- When a tone is detected that matches the frequency range of a configured department, the system logs the detection, sends pre-notifications (if enabled), and starts recording.
  
  **Process:**
  1. The program captures the audio stream from the configured input device.
  2. It analyzes the stream for matching tones based on the frequencies listed in `tone.cfg`.
  3. Once a match is found, it triggers pre-notifications and prepares the system to record.

  **Multiple Tone Detection**:  
  - If multiple tones are detected (e.g., multiple departments are paged at the same time), the system processes each department independently. Recording may run in parallel for each department, while notifications are handled sequentially to avoid conflicts.

### **2.2 Pre-Notification**
- Once a tone is detected, the program sends an immediate pre-notification to the department to alert them of the page. This happens before recording is completed.
- Notifications are sent via email or Pushover, as configured in the `tone.cfg` file for each department.
- The program can also trigger external scripts at this stage (defined in the `pre_external_script` setting).

### **2.3 Audio Recording**
- After detecting the tone, the system begins recording the audio.
  1. **Start Recording**: Recording begins immediately after the tone is detected.
  2. **Silence Detection**: The recording stops after a period of silence (configurable via `silence_threshold` in `config.ini`).
  3. **Max Record Time**: A maximum recording time (`max_record_time`) is set to prevent indefinite recordings.
  4. **Storage**: Audio is saved in WAV format, with an option to convert to MP3.
  5. **Post-Processing**: After recording, the system can run a post-processing script if defined in the `post_recording_external_script` setting.

## **3. Configuration Settings**

### **config.ini**
- **tone_offset**: Adjusts the frequency offset for tone detection, allowing for minor deviations in the detected tone.
- **tone_duration**: Specifies the minimum duration a tone must last to be considered valid.
- **audio_threshold**: Sets the minimum decibel level for detecting tones, filtering out background noise.
- **max_record_time**: Defines the maximum length of the recording to prevent indefinite recordings.
- **silence_threshold**: Stops the recording after detecting a defined period of silence.

### **tone.cfg**
- **department_name**: Name of the department associated with the tone.
- **btone** / **atone**: The frequencies associated with each department's tone.
- **notify_email** / **notify_pushover**: Enable or disable notifications for each department.
- **pre_external_script**: Path to a script that runs immediately upon detecting a tone.
- **post_recording_external_script**: Path to a script that runs after the recording is complete.

### **Example of `tone.cfg` Entry:**
```ini
[Tone1]
department_name = "Raymond"
btone = 1122.5
atone = 1185.2
notify_email = true
notify_pushover = true
pre_external_script = /path/to/pre_script.sh
post_recording_external_script = /path/to/post_script.sh
```

## **4. Error Handling**

### **4.1 Potential Errors**
- **Tone Detection Failure**: If no tone is detected within a set time frame, the system logs a warning and notifies the admin.
- **Multiple Tone Detection Conflict**: If two tones are detected simultaneously, the system processes them separately, with each recording handled in parallel but notifications staggered.
- **Audio Device Failure**: If the audio input device is not working, the system will log an error and notify the admin via Pushover or email.
  
### **4.2 Error Logging and Notifications**
- **Tone Mismatches**: If a tone is detected but does not match any configured department, it is logged as a mismatch and an alert is sent to the admin.
- **Retry Logic**: If a tone is missed due to audio issues, the system will attempt to reprocess the last few seconds of audio before marking it as a failure.
- **Admin Notifications**: All critical errors (e.g., device failures, tone mismatches) will trigger an admin notification via Pushover or email.

## **5. Future Improvements**
- **Advanced Noise Filtering**: Implement advanced noise filtering to reduce false positives from background noise.
- **Zone and Location Integration**: Future versions will incorporate zone and location data from audio transcripts to improve the relevance of notifications.
- **Improved Multi-Tone Handling**: Optimizing how multiple tones are detected and processed to further reduce potential conflicts.

## **6. Version Control**
- **v1.0.0** (September 2024): Initial release of the Tone Detection and Audio Handling logic. Includes detection, recording, pre- and post-notification processes, and basic error handling.

---
