# KingToneDetect

**KingToneDetect** is an open-source tone detection and alerting system designed to process incoming audio signals, specifically for emergency services like fire departments. The system detects specific tones, records audio, and sends alerts via email, Pushover, or FTP. It also features built-in retry mechanisms, adaptive silence detection, and local backups for reliable performance in critical environments.

## Features

- **Tone Detection**: Detect specific tones with adjustable tolerance and frequency settings.
- **Audio Recording**: Records audio in WAV format, with automatic conversion to MP3.
- **Adaptive Silence Detection**: Automatically stops recording based on configurable silence thresholds.
- **Alerts and Notifications**: Sends notifications via email, Pushover, or FTP after tone detection, with retry mechanisms for reliability.
- **Local Backup for Failed Uploads**: In case of FTP upload failures, audio files are saved locally for future re-upload.

## Installation

### Prerequisites

- Python 3.x
- Required packages (install via pip):
  ```bash
  pip install -r requirements.txt
  ```

### Configuration

1. **Edit the configuration files**:
   - `config.ini`: Main configuration file for audio, notification, and FTP settings.
   - `.env`: Store sensitive information like passwords, API keys, etc.

2. **config.ini Sample**:
   ```ini
   ; Audio Configuration
   [audio_configuration]
   input_device_index = 0
   audio_threshold = 54
   tone_offset = 0.0
   tone_tolerance = 0.02
   tone_duration = 0.6
   audio_channel = mono

   ; Recording Configuration
   [Recording]
   max_record_time = 40
   convert_to_mp3 = true
   wav_file_path = /audio/wav/
   mp3_file_path = /audio/mp3/

   ; FTP Configuration
   [Audio_Upload_FTP]
   audio_upload_ftp_enable = true
   audio_upload_ftp_server = ftp.kingpaging.com
   audio_upload_ftp_port = 21
   audio_upload_ftp_username = ${AUDIO_FTP_USER}
   audio_upload_ftp_password = ${AUDIO_FTP_PASSWORD}
   ```

## Usage

1. **Start the program**:
   ```bash
   python kingtonedetect.py
   ```

2. **Monitor Logs**: The program logs all actions, including tone detections, recording start/stop, and any notification or upload failures.

## Contributing

We welcome contributions! If you'd like to improve this project or report any issues, please fork the repository and submit a pull request with your changes. Be sure to follow the contribution guidelines below.

1. Fork the repository
2. Create a new branch for your feature or bug fix
3. Make your changes and commit them
4. Submit a pull request for review

## License

This project is licensed under the **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC BY-NC-SA 4.0)**. See the [LICENSE](LICENSE) file for details.

You are free to:
- Share — copy and redistribute the material in any medium or format
- Adapt — remix, transform, and build upon the material

Under the following terms:
- Attribution — You must give appropriate credit, provide a link to the license, and indicate if changes were made.
- NonCommercial — You may not use the material for commercial purposes.
- ShareAlike — If you remix, transform, or build upon the material, you must distribute your contributions under the same license as the original.

## Contact

For any questions or support, feel free to reach out on Discord: **h2oking**, or open an issue in the repository.

