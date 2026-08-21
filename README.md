# Exam Gaze Monitor POC

A browser-based proof-of-concept that uses webcam and AI to monitor students during online exams. This tool detects:

- Whether a student is looking at the screen
- If eyes are closed
- If the exam tab/window is hidden or minimized
- Possible virtual camera usage
- Head pose deviations indicating possible phone use

> **Warning:** This is a proof-of-concept only. It is NOT a guaranteed anti-cheat solution. A determined student can bypass client-side monitoring using virtual cameras, pre-recorded video, second devices, or other methods.

---

## Live Demo

**[Click here to try the live demo](https://ce47.github.io/exam_test/)**

---

## Features

- ✅ Detects webcam presence using `navigator.mediaDevices.enumerateDevices()`
- ✅ Requests camera access (browser permission required — cannot be bypassed)
- ✅ Real-time face detection and head pose estimation using `face-api.js`
- ✅ Flags when student looks away for more than 5 seconds
- ✅ Detects closed eyes using Eye Aspect Ratio (EAR)
- ✅ Detects tab/window minimization using `visibilitychange` event
- ✅ Heuristic virtual camera detection based on camera label keywords
- ✅ Works on desktop, mobile, and tablets (with front-facing camera)
- ✅ Free and open-source
- ✅ Lightweight — runs entirely in the browser

---

## How It Works

### 1. Camera Detection
The page checks for available video input devices. If no webcam is found, proctoring cannot start.

### 2. Face Detection
Uses `face-api.js` with TinyFaceDetector and 68-point facial landmarks to detect the student's face.

### 3. Head Pose Estimation
Calculates:
- **Yaw** (left/right head turn)
- **Pitch** (up/down head tilt)
- **Eye Aspect Ratio (EAR)** for eye closure detection

### 4. Gaze Monitoring Logic

| Metric | Threshold | Meaning |
|--------|-----------|---------|
| Yaw | > 0.18 | Head turned left/right |
| Pitch deviation | > 0.20 | Head tilted up/down |
| EAR | < 0.22 | Eyes closed |
| Away duration | > 5 seconds | Possible phone use |

### 5. Virtual Camera Detection
Checks the camera label against a list of keywords commonly associated with virtual cameras (OBS, ManyCam, Snap Camera, etc.).

### 6. Tab Visibility Monitoring
Uses the `visibilitychange` event to detect when the exam tab is hidden or minimized.

---

## Usage

1. Open the [live demo](https://ce47.github.io/exam_test/)
2. Click **Start Proctoring**
3. Allow camera access when prompted
4. Monitor the status display:
   - 🟢 Green: Looking at screen
   - 🔴 Red: Looking away / No face detected / Eyes closed
   - 🟡 Yellow: Not started / Loading / Virtual camera detected
5. Check the log panel for timestamped events

---

## Running Locally

To run locally, serve the `index.html` file using any local server:

**Python 3:**
```bash
python -m http.server 8000
