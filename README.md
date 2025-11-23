# AI-Powered Drone for Fire Scenario Reconnaissance

> **An autonomous UAV system that converts raw aerial video into real-time actionable intelligence for first responders using YOLOv8 and Pose Estimation.**

## 📖 Overview

Wildfires and structural fires pose immense risks to first responders. Traditional drone reconnaissance often provides only raw video feeds, increasing the cognitive load on pilots who must manually scan for victims.

This project bridges the gap between data and decision-making by developing a **Ground Control Station (GCS)** application that autonomously detects fire, smoke, and victims. Crucially, it performs **real-time triage** by analyzing victim posture (e.g., "Waving" or "Prone") to assign risk levels, enabling prioritized rescue operations.

---

## ✨ Key Features

* **Multi-Class Detection:** Simultaneously detects **Fire**, **Smoke**, and **Humans** using a custom-trained YOLOv8 model.
* **Smart Triage System:** Integrates **YOLOv8-Pose** to analyze skeletal keypoints. The system distinguishes between "Standing" (Low Risk) and "Prone/Waving" (High Risk) victims automatically.
* **Actionable Visualization:** Overlays color-coded bounding boxes and clear text labels (e.g., "Waving - RESCUE NOW") directly onto the live video feed.
* **Off-Board Processing:** Runs heavy AI inference on the GCS laptop via a WiFi RTSP link, eliminating the need for expensive onboard companion computers.
* **Optimized Performance:** Achieves detection with a confidence threshold of 0.35 and maintains low latency ($\le 5s$) via intelligent frame skipping.

---

<img width="512" height="237" alt="unnamed" src="https://github.com/user-attachments/assets/de169436-bd63-453d-a581-799a31e3b567" />
<img width="512" height="238" alt="unnamed-3" src="https://github.com/user-attachments/assets/292725a1-0b30-4b7a-b4be-a62ad5c92cdc" />
<img width="512" height="237" alt="unnamed-2" src="https://github.com/user-attachments/assets/2232c78e-d513-4e04-b2d8-96a203b65cc8" />

## 🛠️ System Architecture

The system utilizes a unidirectional data flow where the drone acts as the data capture edge, and the laptop acts as the processing node:

1.  **UAV Hardware:** Captures video via an SJCAM SJ4000 and transmits it via WiFi RTSP.
2.  **GCS Application:** A Python script utilizing **OpenCV** ingests the stream.
3.  **AI Pipeline:**
    * **Stage 1:** YOLOv8 detects objects (Fire, Smoke, Human).
    * **Stage 2:** If a human is detected, the region is cropped and passed to YOLOv8-Pose.
    * **Stage 3:** A custom algorithm classifies posture based on keypoint geometry (Nose vs. Wrists/Ankles).
4.  **Output:** Annotated video is displayed to the operator in real-time.

---

## 🚁 Hardware Specifications

| Component | Model/Details | Function |
| :--- | :--- | :--- |
| **Frame** | Mark4 7-inch Carbon Fiber | Physical Platform |
| **Flight Controller** | Pixhawk 2.4.8 | Autonomous Navigation & Stability |
| **Camera** | SJCAM SJ4000 WiFi | 1080p RTSP Video Source |
| **Telemetry** | 3DR 433MHz | MAVLink Data Link (GPS/Battery) |
| **RC Link** | ELRS 2.4GHz | Pilot Control |
| **Video TX** | SpeedyBee TX800 | Analog FPV Backup for Pilot |
| **Motors** | T-Motor Velox V2207 | Propulsion |

*[Full hardware list and costs available in the project report]*.

---

## 💻 Software Setup

### Prerequisites
* Python 3.8+
* A laptop with WiFi capability

### Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/yourusername/fire-recon-drone.git](https://github.com/yourusername/fire-recon-drone.git)
    cd fire-recon-drone
    ```

2.  **Install dependencies:**
    ```bash
    pip install ultralytics opencv-python torch numpy
    ```
    **

3.  **Model Weights:**
    * Place your custom trained model `best.pt` (Fire/Smoke/Human) in the root directory.
    * The `yolov8n
