# 🛡️ Project Report: Deep Learning-Based Video Anomaly Detection System

## 1. Executive Summary
This project represents a state-of-the-art hybrid security solution that combines traditional deep learning (CNNs and LSTMs) with modern Generative AI (Vision-Language Models). The system is designed to autonomously monitor surveillance footage, mathematically identify statistical anomalies, and provide human-readable forensic reports with precise timestamps.

---

## 2. Technical Stack & Model Architecture
The system utilizes a 3-layered detection pipeline to ensure both speed and accuracy:

### A. Spatial Feature Extraction
- **Model:** `EfficientNet-B0`
- **Function:** Processes raw pixels into high-density 1D feature vectors.
- **Input Size:** 224x224 RGB.

### B. Local Detection & Heuristics
- **YOLOv8 Nano:** Real-time object detection for tracking people and calculating scene complexity.
- **LSTM (Long Short-Term Memory):** Learns temporal sequences to differentiate between normal walking and chaotic motion.
- **Autoencoder:** An unsupervised model that flags anomalies based on "Reconstruction Error" (identifying visual data it has never seen before).

### C. Generative AI Verification
- **Model:** `Gemini 2.5 Flash`
- **Function:** Acts as the "Final Juror." It analyzes detected incidents to provide a 1-paragraph chronological summary with exact timestamps, reducing false positives caused by simple motion.

---

## 3. Dataset & Training Metrics
The intelligence of the system is derived from a massive dataset processed locally:

| Metric | Total Count |
| :--- | :--- |
| **Total Source Videos** | 236 Clips |
| **Total Sliced Frames** | 32,364 Images |
| **Training Set (85%)** | 29,608 Frames |
| **Testing/Validation Set (15%)** | 2,756 Frames |

- **Maintenance:** Data is maintained via `train2.csv` and `test2.csv` manifests, ensuring a strict separation between learning and testing.
- **Labeling:** Focuses on classes such as *Abuse, Violence, Theft, Vandalism, and Suspicious Behavior.*

---

## 4. System Workflow (The 12-Step Pipeline)
1. **Video Input:** Ingestion of `.mp4` surveillance files.
2. **Frame Extraction:** OpenAI-based slicing at 1 frame per second.
3. **Preprocessing:** BGR to RGB conversion, normalization, and resizing.
4. **Feature Extraction:** Feature vector generation via EfficientNetB0.
5. **Human Logic:** Contextual scene evaluation.
6. **Object Detection:** YOLOv8 bounding boxe mapping.
7. **Temporal Analysis:** LSTM motion sequence modeling.
8. **Unsupervised Learning:** Autoencoder baseline comparison.
9. **Classification:** Normal/Anomaly prediction with confidence scores.
10. **Aggregation:** Frame-level data converted into video-level decisions.
11. **Report Generation:** Gemini AI timeline synthesis.
12. **User Interface:** PySide6 Dashboard with interactive playback and logs.

---

## 5. Key Features
- **Dual-Layer Reporting:** Provides "Local System Diagnostics" (raw math) and a "Gemini AI Narrative" (human context).
- **Graceful Error Handling:** If the AI API is offline, the local detection system remains 100% functional, ensuring security is never compromised.
- **High Efficiency:** Optimized for local GPU/CPU execution using lightweight models like YOLOv8n and EfficientNetB0.

---

## 6. Conclusion
The "Anamolyy" detection system successfully bridges the gap between raw mathematical variance and human operational understanding. By training on over 32,000 frames, the system provides a robust local baseline, while the Gemini integration ensures that security personnel receive concise, actionable intelligence rather than simple alerts.

---
**Developed by:** Prabhu Bhalke / Deep-Learning-Based-Video-Anomaly-Detection-System
**Year:** 2026
