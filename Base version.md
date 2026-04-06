Here is the complete, detailed technology breakdown for your **Deep Learning-Based Video Anomaly Detection System**:

---

# 🧠 Technology Stack — Full Breakdown

---

## 🐍 Core Language

### Python 3.7+
The entire project runs on Python. It acts as the glue layer connecting deep learning, computer vision, GUI, and cloud services. Chosen for its dominant ML/AI ecosystem.

---

## 🤖 Deep Learning & AI

### TensorFlow / Keras (`tensorflow >= 2.10.0`)
**The primary deep learning engine.**
- Builds, trains, and saves neural networks in `.h5` (HDF5) format
- Runs frame-by-frame inference during video analysis
- Uses **Intel oneDNN** acceleration automatically on compatible CPUs
- **Layers used:** `Dense`, `LSTM`, `GlobalAveragePooling2D`, `BatchNormalization`, `Dropout`, `Embedding`
- **Optimizer:** Adam with categorical cross-entropy loss

---

### EfficientNetB0 (`keras.applications.efficientnet`)
**Pre-trained CNN used purely as a feature extractor.**
- Pre-trained on **ImageNet** (1.2M images, 1000 classes)
- Classification top removed → outputs a rich feature vector per frame
- Input: 224×224 RGB frame → Output: compact feature vector
- `GlobalAveragePooling2D` flattens spatial output to 1D
- That vector is then classified as Normal or Anomaly by the custom head
- **Why EfficientNetB0?** Best accuracy-to-parameters ratio — fast enough for real-time video processing

---

### LSTM — Long Short-Term Memory
**Temporal sequence modeling across video frames.**
- Processes sequences of 5–10 consecutive feature vectors
- Two stacked LSTM layers + BatchNormalization + Dropout
- Captures gradual anomalies (fighting beginning, person falling)
- Saved as `lstm_temporal_model.h5`

---

### Autoencoder (Unsupervised)
**Reconstruction-based anomaly detection.**
- Trained only on normal activity — learns what "normal" looks like
- At inference: high reconstruction error = looks abnormal
- Architecture: Encoder (128→64 bottleneck) → Decoder (64→128→output)
- Saved as `autoencoder_model.h5`

---

### YOLOv8 (`ultralytics >= 8.0.0`)
**Real-time object and person detection.**
- Uses pre-trained `yolov8n.pt` (nano — fastest variant)
- Detects persons, vehicles, objects with bounding boxes
- Supports YOLOv8, YOLOv9, YOLO11 in a single API

---

### Hugging Face Transformers (`transformers >= 4.20.0`)
**Vision-language models for frame captioning.**
- Supports **BLIP, BLIP-2, Qwen2-VL, LLaVA**
- Generates natural language descriptions of what's happening in a frame
- Used in extended pipelines for scene-level understanding

---

### PyTorch (`torch >= 1.12.0`, `torchvision >= 0.13.0`)
**Backend runtime for YOLO and Transformers.**
- Handles tensor operations for BLIP/YOLO inference
- `torchvision` provides image transform utilities

---

### MediaPipe (`mediapipe >= 0.10.0`)
**Human pose estimation — 33 body keypoints in real-time.**
- Detects hand, elbow, knee, spine positions per frame
- Identifies unusual postures: falls, aggressive gestures, abnormal stances
- Google's lightweight alternative to OpenPose

---

## 👁️ Computer Vision

### OpenCV (`opencv-python >= 4.5.0`)
**Core video and image processing — used across every stage.**

| Function | Detail |
|---|---|
| Video decoding | `cv2.VideoCapture` reads `.mp4` files |
| Color conversion | BGR (OpenCV default) → RGB (model expected) |
| Frame resizing | Every frame resized to 224×224 before model |
| FPS / metadata | Reads video FPS and total frames for 1-frame/sec sampling |
| Frame extraction | `extract.py` saves frames to disk for training |

---

## 🔢 Numerical Computing

### NumPy (`numpy >= 1.21.0`)
**All array and numerical operations.**
- Saves feature datasets as `.npy` binary files (fast load/save)
- Powers the frame-level **voting logic** — aggregates per-frame predictions into a final video verdict
- Array reshaping, statistics, indexing throughout all scripts

---

### Pandas (`pandas >= 1.3.0`)
**CSV label management.**
- Reads `train2.csv` / `test2.csv` mapping video filenames → labels
- DataFrame interface for filtering, querying, and organizing label metadata

---

### Scikit-learn (`scikit-learn >= 1.0.0`)
**Classical ML utilities.**
- `LabelEncoder` — converts `"normal"/"anomaly"` strings ↔ integers; saved to `label_encoder.npy` and reloaded at runtime
- `classification_report` — Precision, Recall, F1-score evaluation after training

---

## 🖥️ GUI Framework

### PySide6 (`PySide6 >= 6.0.0`)
**The complete desktop application — official Qt 6 Python bindings.**

| Component | Role |
|---|---|
| `QMainWindow` | Main window container |
| `QThread + Signal/Slot` | Background ML inference — keeps UI responsive |
| `QMediaPlayer + QVideoWidget` | Real-time in-app video playback |
| `QFileDialog` | Native OS file picker |
| `QProgressBar` | Live 0–100% processing progress |
| `QTextEdit + QScrollArea` | Scrollable analysis report area |
| `QGroupBox + QGridLayout` | Organized control sections |
| `QGraphicsDropShadowEffect` | Drop shadows on video frame |
| `QPainter + QPixmap` | Background image with dark semi-transparent overlay |
| `QMessageBox` | 🟢 Green popup (Normal) / 🔴 Red popup (Anomaly) |

> **Critical design:** `QThread` offloads TensorFlow inference to a background thread — the UI never freezes during video analysis.

### Qt Multimedia
- `QMediaPlayer + QVideoWidget` = built-in video playback
- Backed by **FFmpeg 7.1.1** (confirmed in app startup logs)

---

## ☁️ Cloud AI Integration

### Google Gemini AI (`google-generativeai`)
**Optional multimodal AI for deep scene understanding.**

**How it works (in `gemini_video_service.py`):**
1. Upload video to Google servers via `genai.upload_file()`
2. Poll until Gemini finishes processing (`state == ACTIVE`)
3. Send a structured prompt: describe actions, behaviors, safety concerns, temporal progression
4. Second Gemini call evaluates the description → structured JSON verdict:
   `{"is_abnormal": bool, "abnormal_activity": str, "confidence": str, "severity": str}`
5. Results merged into the application's analysis report panel

- **Model:** `gemini-2.0-flash-exp` (configurable in `.env`)
- **Async:** `asyncio` / `await` for non-blocking API calls
- **Auto-cleanup:** Uploaded files deleted from Google servers after analysis
- **Activation:** Only runs if `GOOGLE_AI_API_KEY` set in `.env` and user enables via GUI toggle

---

### Google Cloud Video Intelligence (`google-cloud-videointelligence >= 2.0.0`)
**Enterprise video annotation API.**
- Label detection, object tracking, shot detection, explicit content detection
- Available for enterprise/extended pipelines beyond the core app

---

### AWS Boto3 (`boto3 >= 1.26.0`)
**Amazon Web Services SDK.**
- Integration with **Amazon Rekognition Video** for activity detection
- **S3** access for video file storage/retrieval
- Makes the project cloud-agnostic (GCP + AWS both supported)

---

### Requests (`requests >= 2.28.0`)
**HTTP client for REST API calls.**
- Simple GET/POST with JSON support for any external REST endpoints

---

## 🖼️ Image Processing

### Pillow / PIL (`Pillow >= 8.3.0`)
**Image format handling.**
- Required by Hugging Face Transformers to load/preprocess images for BLIP/LLaVA
- Used alongside QPainter for GUI background image processing

---

### Matplotlib (`matplotlib >= 3.5.0`)
**Visualization and plotting.**
- Training/validation accuracy and loss curves
- Motion intensity histograms
- Analysis visualizations in `monitoring_dashboard.py`

---

## 📊 Utilities

### tqdm (`tqdm >= 4.62.0`)
**Progress bars for long-running loops.**
- Wraps frame processing loops in `main.py` during dataset preparation
- Shows ETA and throughput while processing thousands of frames

---

### python-dotenv (`python-dotenv >= 0.19.0`)
**Secure environment variable management.**
- Loads `.env` at startup → injects `GOOGLE_AI_API_KEY`, `GEMINI_MODEL` etc. into the process
- Keeps all API keys and secrets out of source code

---

### asyncio (Python Standard Library)
**Asynchronous I/O for cloud API calls.**
- Powers the entire Gemini service pipeline
- `asyncio.run()` bridges async Gemini calls into the synchronous `QThread` context

---

### FFmpeg (External System Tool)
**Multimedia codec engine.**
- Confirmed in app logs: `"qt.multimedia.ffmpeg: Using Qt multimedia with FFmpeg version 7.1.1"`
- Required by Qt Multimedia for video decoding and playback
- Also used by OpenCV for reading unusual video formats
- Must be installed separately and added to system `PATH`
- Supported: MP4, AVI, MOV, MKV, WebM

---

## 🗂️ Key Data & Model Files

| File | Format | Purpose |
|---|---|---|
| `train2.csv` / `test2.csv` | CSV | Video filename → label mapping |
| `X_train.npy` / `X_test.npy` | NumPy Binary | Pre-extracted CNN feature vectors |
| `y_train.npy` / `y_test.npy` | NumPy Binary | Integer-encoded label arrays |
| `label_encoder.npy` | NumPy Binary | Saved LabelEncoder class mappings |
| `anomaly_detection_model.h5` | HDF5 | Main classification neural network |
| `high_accuracy_model.h5` | HDF5 | High-accuracy variant (used by the app) |
| `autoencoder_model.h5` | HDF5 | Unsupervised autoencoder |
| `lstm_temporal_model.h5` | HDF5 | LSTM temporal model |
| `yolov8n.pt` | PyTorch | YOLOv8 nano object detector weights |
| `analysis_config.json` | JSON | Analysis thresholds and parameters |
| `.env` | Key=Value | API keys and model config (not in git) |

---

## 🏗️ Architecture Flow (Text)

```
VIDEO FILE (.mp4)
      │
      ▼
OpenCV — decode video, sample 1 frame/sec, BGR→RGB, resize 224×224
      │
      ▼
EfficientNetB0 (TensorFlow) — extract feature vector per frame
      │
      ▼
Custom Dense Classifier (Keras) — predict Normal or Anomaly per frame
      │
      ▼
Voting Logic (NumPy) — aggregate frame predictions → video verdict
      │
      ├── NORMAL  →  🟢 Green popup (PySide6 QMessageBox)
      │
      └── ANOMALY → Trigger detailed analysis
                         │
           ┌─────────────┴──────────────────┐
           │                                │
    Local VideoAnalyzer            [Optional] Gemini AI
    • Motion & temporal analysis   • Upload video to Google
    • Behavioral scoring           • Multimodal scene description
    • Risk assessment report       • JSON anomaly verdict
           │                                │
           └─────────────┬──────────────────┘
                         ▼
              PySide6 GUI — display scrollable report + video playback
```

---

## 📦 Quick Reference Table

| Technology | Layer | Core Role |
|---|---|---|
| Python 3.7+ | Language | Project backbone |
| TensorFlow / Keras | Deep Learning | Train & run neural networks |
| EfficientNetB0 | Deep Learning | CNN feature extraction |
| LSTM | Deep Learning | Temporal sequence modeling |
| Autoencoder | Deep Learning | Unsupervised anomaly scoring |
| YOLOv8 | Deep Learning | Object & person detection |
| Transformers (BLIP/LLaVA) | Deep Learning | Frame captioning |
| PyTorch | Deep Learning | YOLO & Transformers backend |
| MediaPipe | Computer Vision | Pose & gesture estimation |
| OpenCV | Computer Vision | Video decode & frame processing |
| NumPy | Numerics | Arrays, voting logic, math |
| Pandas | Data | CSV label management |
| Scikit-learn | ML Utilities | Label encoding, evaluation metrics |
| PySide6 / Qt6 | GUI | Full desktop application |
| Qt Multimedia | GUI | In-app video playback |
| Gemini AI | Cloud AI | Multimodal scene analysis |
| Google Cloud Video Intelligence | Cloud AI | Enterprise annotation |
| AWS Boto3 | Cloud | Rekognition / S3 integration |
| Pillow | Image | Format conversion |
| Matplotlib | Visualization | Training curves, plots |
| tqdm | Utilities | Progress bars |
| python-dotenv | Utilities | Secure API key management |
| asyncio | Runtime | Non-blocking async API calls |
| FFmpeg | System | Video codec engine |

---

That's the complete technology breakdown — **24 technologies** across 8 layers, none of your project files were modified. Let me know if you'd like a deeper dive into any specific component.
