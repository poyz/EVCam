<div align="center">
  <img src="assets/logo.png" alt="EVCam Logo" width="200"/>
  
  # EVCam - EV Dashcam
  
  <p>
    <strong>An in-cabin surround-view dashcam app custom-built for Geely Galaxy series vehicles, with a "thousand-mile eye" remote monitoring feature for anytime, anywhere viewing</strong>
  </p>
  
  <p>
    <img src="https://img.shields.io/badge/Android-9.0+-green?style=flat-square&logo=android" alt="Android"/>
    <img src="https://img.shields.io/badge/API-28+-brightgreen?style=flat-square" alt="API"/>
    <img src="https://img.shields.io/badge/License-GPLv3-blue?style=flat-square" alt="License"/>
    <img src="https://img.shields.io/badge/Language-Java-red?style=flat-square&logo=openjdk&logoColor=white" alt="Java"/>
  </p>
</div>

---

## 📱 Project Overview

This app supports Geely Galaxy series vehicles (Galaxy E5, Galaxy L6/L7, etc.). In theory it also works on other Longying-1 (SE1000) vehicles without advanced driver-assistance, and supports phone-side preview. It can record video and take photos from up to **4 cameras** simultaneously, and supports remote recording, photo capture, and live preview commands via a **DingTalk bot** for remote monitoring.

### ✨ Core Features

- 🎨 **FlymeAuto-style UI** - Mimics the official FlymeAuto interface design with an immersive status bar; beautiful and suited to head-unit usage habits
- 🎥 **Video recording & photo capture** - Multi-camera synchronized recording and real-time photo capture; choose which cameras participate in recording
- 👁️ **"Thousand-mile eye" remote monitoring** - View camera feeds remotely via a DingTalk bot
-  **No speed limit** - Recording can be started at any time, bypassing the official 30 km/h speed restriction
- 🔄 **Auto-start & background keep-alive** - Boot auto-start + foreground service + WorkManager + accessibility service multi-layer keep-alive
- 💾 **Multiple storage locations** - Supports internal storage and USB drive, with automatic cleanup of old files exceeding limits
- 🎬 **Segmented recording** - Auto-segmentation at 1/3/5 minutes for easy management and playback
- ⏱️ **Timestamp watermark** - Optional timestamp overlay on videos and photos
- 🖼️ **Floating window shortcut** - Configurable-size and -transparency floating button that shows recording status in real time
- 🌙 **Screen-off recording (locked-car recording)** - Continue recording after the screen turns off for locked-car surveillance
- 🔧 **Multi-model adaptation** - Supports Galaxy E5, E5-multi-button, Galaxy L6/L7, L7-multi-button, phone, and custom models

---

## 🛠️ Tech Stack
- **Language**: Java
- **Minimum version**: Android 9.0 (API 28)
- **Target version**: Android 14+ (API 36)
- **Camera API**: Camera2 API
- **Video encoding**: MediaRecorder (hardware) / OpenGL + MediaCodec (software)
- **Build tool**: Gradle 8.x (Kotlin DSL)
- **UI components**: Material Design Components
- **Image loading**: Glide 4.16.0
- **Networking**: OkHttp 4.12.0
- **DingTalk integration**: DingTalk Stream SDK 1.3.12
- **Background tasks**: WorkManager 2.9.0

### 🚗 Supported Models

| Model | Cameras | Recording Mode | Notes |
|------|---------|---------------|-------|
| Galaxy E5 | 4 | MediaRecorder | Default model |
| Galaxy E5-multi-button | 4 | MediaRecorder | Simplified UI |
| Galaxy L6/L7 | 4 | OpenGL+MediaCodec | Auto-adapts encoding mode |
| Galaxy L7-multi-button | 4 | OpenGL+MediaCodec | Simplified UI |
| Phone | 2 | MediaRecorder | Front + rear cameras |
| Custom model | 1/2/4 | Selectable | Fully custom configuration |

---

## 📦 Quick Start

### Requirements

- **JDK**: 17 or higher (JDK 25 recommended)
- **Android Studio**: Hedgehog (2023.1.1) or higher
- **Gradle**: 8.0+
- **Test device**: A real Android 9.0+ device (multiple cameras recommended)

### Clone the project

```bash
git clone https://github.com/your-username/EVCam.git
cd EVCam
```

### Configure JDK (Windows)

The project provides a convenient batch script for configuring JDK 25:

```batch
# Build using the provided script (auto-sets JAVA_HOME)
build-with-jdk25.bat
```

Or configure the environment variables manually:

```batch
set JAVA_HOME=C:\Program Files\Java\jdk-25.0.2
set PATH=%JAVA_HOME%\bin;%PATH%
```

### DingTalk bot configuration

To use the remote control feature, you need to configure a DingTalk bot:

1. Create a DingTalk enterprise internal app (Stream mode)
2. Obtain the `Client ID` (formerly AppKey/SuiteKey) and `Client Secret` (formerly AppSecret/SuiteSecret)
3. Create `app/src/main/java/com/kooo/evcam/dingtalk/DingTalkConfig.java`:

```java
package com.kooo.evcam.dingtalk;

public class DingTalkConfig {
    // DingTalk app credentials (new parameter names)
    public static final String CLIENT_ID = "your Client ID";
    public static final String CLIENT_SECRET = "your Client Secret";
    
    // Upload mode config
    public static final boolean ENABLE_UPLOAD = true; // whether to enable upload
}
```

**Note**: 
- DingTalk has renamed the old AppKey/AppSecret to Client ID/Client Secret
- If you don't need the DingTalk feature, you can comment out the relevant code in `MainActivity.java`

---

## 🔨 Build & Install

### Build a Debug version

```bash
# Windows
gradlew.bat assembleDebug

# Linux/macOS
./gradlew assembleDebug
```

Output location: `app\build\outputs\apk\debug\app-debug.apk`

### Build a Release version

The project is configured with the AOSP public test signature and can be built directly:

```bash
# Windows
gradlew.bat assembleRelease

# Linux/macOS
./gradlew assembleRelease
```

Output location: `app\build\outputs\apk\release\app-release.apk`

### Install to device

```bash
# Install Debug version
gradlew.bat installDebug

# Or install manually with adb
adb install app\build\outputs\apk\debug\app-debug.apk
```

---

## 📖 Usage Guide

### First launch

1. **Select model** - On first launch a guide screen appears; select your model (Galaxy E5/L6/L7/Phone/Custom)

2. **Grant permissions** - Be sure to use "App Manager" or other permission management software to grant EVCam all required permissions

3. **Camera preview** - After permissions are granted, the app auto-initializes the cameras and shows the preview

4. **Check logs** - Tap the "Show Logs" button at the bottom to view camera initialization status

### Recording video

1. Tap the **"Start Recording"** button (or tap the floating window)
2. All selected cameras start recording simultaneously
3. Recording auto-segments by the configured duration (default 1 minute)
4. You can take photos during recording (tap the "Photo" button)
5. Tap **"Stop Recording"** to end recording

**Video storage location**: `/sdcard/DCIM/EVCam_Video/` (or USB drive)  
**File naming format**: `yyyyMMdd_HHmmss_{position}.mp4` (e.g. `20260125_153045_front.mp4`)

### Taking photos

- In preview or recording state, tap the **"Photo"** button
- Photos are captured from all active cameras simultaneously
- Optionally add a timestamp watermark

**Photo storage location**: `/sdcard/DCIM/EVCam_Photo/` (or USB drive)  
**File naming format**: `yyyyMMdd_HHmmss_{position}.jpg`

### Viewing recordings

The app has built-in playback and gallery features:

1. Tap the menu icon in the top-left (☰)
2. Select **"Video Playback"** or **"Photo Playback"**
3. Tap a thumbnail for fullscreen viewing/playback
4. Multi-select delete supported

### Floating window

Once the floating window is enabled, you can conveniently control recording even when the app is in the background:

- **Red dot** - not recording
- **Green blinking** - recording
- **Tap** - open the app main screen
- **Drag** - move the floating window

In settings you can adjust the floating window size (10 levels) and transparency.

### Screen-off recording (locked-car recording)

With screen-off recording enabled:
- Recording starts automatically when the screen turns off
- Recording stops automatically when the screen turns on
- Suitable for security monitoring after locking the car

### DingTalk remote control

After configuring the DingTalk bot, you can send commands via DingTalk:

- `photo` - take a remote photo and upload
- `record <duration>` - start recording for the specified duration (seconds)
- `status` - query app running status
- `preview` - get a screenshot of the current camera preview

### App settings

Tap menu → "App Settings" to configure:

| Setting | Description |
|--------|------------|
| Model selection | Select model or custom camera config |
| Recording mode | Auto/MediaRecorder/OpenGL+MediaCodec |
| Segment duration | 1 min/3 min/5 min |
| Storage location | Internal storage/USB drive |
| Storage limit | Max storage for video and photos (GB) |
| Recording cameras | Choose which cameras participate in recording |
| Floating window | On/off, size, transparency |
| Timestamp overlay | Whether to add timestamps to videos/photos |
| Boot auto-start | Auto-launch app on boot |
| Auto-record on start | Auto-start recording after app launch |
| Screen-off recording | Auto-record when screen turns off |
| Keep-alive service | Prevent the app from being killed by the system |
| Prevent sleep | Keep the device awake |

### Resolution/bitrate settings

Tap menu → "Resolution Settings" for fine adjustment:

- **Resolution** - choose a resolution supported by the camera
- **Bitrate** - Low/Standard/High (affects video quality and file size)
- **Frame rate** - Standard/Low (lower frame rate reduces file size)

### Color/noise-reduction adjustment

Tap the adjustment button on the main screen for real-time adjustment:

- Exposure compensation
- White balance mode
- Tone mapping
- Edge enhancement
- Noise reduction mode
- Effect mode

---

## 🏗️ Architecture

### Core components

```
EVCam/
├── MainActivity.java              # Main screen, UI controller
├── AppConfig.java                 # App configuration management
├── camera/                        # Camera management module
│   ├── MultiCameraManager.java   # Multi-camera orchestrator
│   ├── SingleCamera.java          # Single camera wrapper (Camera2 API)
│   ├── VideoRecorder.java         # Video recorder (MediaRecorder)
│   ├── CodecVideoRecorder.java    # Video recorder (OpenGL+MediaCodec)
│   ├── EglSurfaceEncoder.java     # EGL Surface encoder
│   ├── ImageAdjustManager.java    # Image adjustment manager
│   ├── CameraCallback.java        # Camera event callback interface
│   └── RecordCallback.java        # Recording event callback interface
├── dingtalk/                      # DingTalk integration module
│   ├── DingTalkStreamManager.java # Stream client management
│   ├── DingTalkCommandReceiver.java # Command parsing & execution
│   ├── PhotoUploadService.java    # Photo upload service
│   └── VideoUploadService.java    # Video upload service
├── FloatingWindowService.java     # Floating window service
├── StorageHelper.java             # Storage path management (incl. USB detection)
├── StorageCleanupManager.java     # Storage auto-cleanup
├── KeepAliveManager.java          # Keep-alive manager
├── CameraForegroundService.java   # Foreground service
├── SettingsFragment.java          # App settings screen
├── ResolutionSettingsFragment.java # Resolution settings screen
├── CustomCameraConfigFragment.java # Custom camera configuration
├── PlaybackFragment.java          # Video playback screen
└── PhotoPlaybackFragment.java     # Photo browsing screen
```

### Camera initialization flow

```
1. Permission check → request camera, audio, storage permissions
2. TextureView ready → wait for TextureView to finish initializing
3. Camera detection → query CameraManager for available cameras
4. Adaptive config → allocate cameras per model config:
   - Galaxy E5: 4 cameras, fixed ID mapping
   - Galaxy L6/L7: 4 cameras, Codec mode
   - Phone: 2 cameras (front + rear)
   - Custom: user-configured cameras
5. Sequential open → open cameras in system-limit order
6. Preview start → establish CaptureSession to begin preview
```

### Recording flow

```
User taps "Start Recording"
    ↓
MultiCameraManager creates VideoRecorder/CodecVideoRecorder for each selected camera
    ↓
VideoRecorder.prepare() configures the recorder and returns a Surface
    ↓
SingleCamera adds the recording Surface to the CaptureSession
    ↓
All recorders start in sync → timed segmentation → auto-create new segments
    ↓
User taps "Stop Recording"
    ↓
All recorders stop → clear Surfaces → rebuild preview Session
```

### Threading model

- **Main thread**: UI updates, button responses, TextureView callbacks
- **Camera HandlerThread**: each SingleCamera has its own background thread for Camera2 API calls
- **Codec encoding thread**: CodecVideoRecorder's dedicated encoding thread
- **Logcat reader thread**: dedicated thread reading system logs
- **DingTalk Stream thread**: WebSocket connection and message handling
- **WorkManager background tasks**: scheduled keep-alive tasks
- **Storage cleanup thread**: scheduled checks and cleanup of over-limit files

---

## 🔍 Development & Debugging

### Viewing logs

```bash
# View camera-related logs (detailed)
adb logcat -v time -s CameraService:V Camera3-Device:V Camera3-Stream:V Camera3-Output:V camera3:V MainActivity:D MultiCameraManager:D SingleCamera:D VideoRecorder:D

# View app logs
adb logcat -v time | findstr "com.kooo.evcam"

# Clear the log buffer
adb logcat -c
```

### Device management

```bash
# List connected devices
adb devices

# Uninstall the app
adb uninstall com.kooo.evcam

# Manually grant permissions
adb shell pm grant com.kooo.evcam android.permission.CAMERA
adb shell pm grant com.kooo.evcam android.permission.RECORD_AUDIO
adb shell pm grant com.kooo.evcam android.permission.WRITE_EXTERNAL_STORAGE
```

### Viewing recorded files

```bash
# List videos
adb shell ls -la /sdcard/DCIM/EVCam_Video/

# Pull videos to local
adb pull /sdcard/DCIM/EVCam_Video/ ./recordings/

# List photos
adb shell ls -la /sdcard/DCIM/EVCam_Photo/

# Pull photos to local
adb pull /sdcard/DCIM/EVCam_Photo/ ./photos/
```

### Running tests

```bash
# Unit tests
gradlew.bat test

# Device tests (requires a connected device)
gradlew.bat connectedAndroidTest
```

---

## ❓ FAQ

### 1. Camera won't open

**Possible causes**:
- Trying to open the camera before TextureView is ready
- Permissions not granted (check logcat for "Missing permission" errors)
- Device has no available cameras
- Exceeded the system limit on simultaneously-open cameras
- Incorrect camera ID config (custom model)

**Solutions**:
- Ensure TextureView has triggered the `onSurfaceTextureAvailable` callback
- Grant permissions manually in settings, or reinstall the app
- Use `adb shell dumpsys media.camera` to view device camera info
- Lower the `maxOpenCameras` config (default 4)
- Check the custom model's camera ID settings

### 2. Recording fails

**Possible causes**:
- DCIM/EVCam_Video directory not writable
- Camera not open or preview not started
- MediaRecorder/MediaCodec config doesn't match camera capabilities
- Insufficient storage
- USB drive write speed too slow

**Solutions**:
- Check that storage permission is granted
- Ensure camera preview is working before starting recording
- Check logcat for MediaRecorder/MediaCodec error messages
- Clean device storage or adjust the storage limit
- For L6/L7 models, try switching recording mode

### 3. Preview not showing

**Possible causes**:
- TextureView size is zero
- SurfaceTexture unavailable
- Camera preview resolution not supported
- Camera2 API error

**Solutions**:
- Check the TextureView width/height settings in the layout file
- Confirm the `onSurfaceTextureAvailable` callback fired
- Check the resolution negotiation process in the logs
- Use `adb logcat -s CameraService:V` to view low-level errors

### 4. App killed by the system

**Solutions**:
- Enable the foreground service (the app shows a notification)
- Turn off battery optimization in system settings
- Allow the app to auto-start
- Enable the accessibility service (Settings → Accessibility → EVCam keep-alive service)
- Enable the "Prevent Sleep" option

### 5. DingTalk bot not responding

**Possible causes**:
- Incorrect Client ID/Client Secret config
- Network connection issues
- Stream connection not established

**Solutions**:
- Check the `DingTalkConfig.java` config
- Ensure the device is online
- Check the WebSocket connection status in the logs
- Restart the app to re-establish the connection

### 6. USB drive storage issues

**Possible causes**:
- USB drive not inserted correctly or not recognized
- USB drive filesystem not supported
- USB drive write speed too slow causing recording stutter

**Solutions**:
- Check that the USB drive is inserted correctly
- Use a FAT32 or exFAT formatted USB drive
- The app auto-uses a relay-write mechanism to mitigate slow USB drives
- If the USB drive is unavailable, the app auto-falls back to internal storage

---

## 📋 To-do List

- [x] ~~Add video quality selection (HD/SD/Smooth)~~ ✅ Implemented as bitrate selection
- [x] ~~Implement timestamp watermark~~ ✅ Implemented
- [ ] Exterior speaker loudspeaker feature
- [ ] More remote vehicle-control features (AC, windows, doors, etc.)
- [ ] Auto-start recording based on specified vehicle state
- [ ] Manual upload feature (selective upload of recordings)
- [x] ~~More personalization settings (recording duration, storage path, etc.)~~ ✅ Implemented

---

## 🤝 Contributing

Contributions, bug reports, and feature suggestions are welcome!

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the **GPL-3.0** open-source license.

You are free to use, modify, and distribute this project, subject to the GPL-3.0 terms.

### Obligations
- Retain the copyright and license notice, and provide the GPL-3.0 text (LICENSE) with distribution.
- Any modified or derivative work must be released under GPL-3.0 and provide the corresponding source code when distributed.
- You may not add additional restrictions or technical measures that prevent others from exercising GPL rights.

### Commercial use
GPL-3.0 **permits** commercial use and distribution/sale, but as long as you distribute externally, you must comply with the open-source obligations above (provide source code, retain notices, same license, etc.).

See the [LICENSE](LICENSE) file for full terms.

---

## 💖 Support the Author

This project is 100% Vibe Coding and has cost hundreds of dollars in AI agent subscriptions. If this project helps you, tips are welcome!

<div align="center">
  <img src="assets/donate.jpg" alt="Donation QR code" width="300"/>
  <p><em>Scan to buy the author a coffee ☕</em></p>
</div>

---

## 📧 Contact

- **WeChat**: greenteacher46 (please note your reason for contacting)

---
