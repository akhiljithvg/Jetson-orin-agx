# 🤖 Jetson Orin AGX Sign & Signal Detection

An optimized edge computer vision pipeline engineered to detect and classify road signs and traffic signals in real-time on the **NVIDIA Jetson Orin AGX** platform.

![Project Status](https://img.shields.io/badge/Status-Completed-success)
![Platform](https://img.shields.io/badge/Platform-NVIDIA%20Jetson%20Orin%20AGX-green)
![Framework](https://img.shields.io/badge/Framework-YOLOv5n-orange)

---

## 📸 Overview

This project demonstrates a high-speed, low-latency computer vision pipeline designed for autonomous navigation systems. By deploying a custom-trained **YOLOv5n** model on embedded edge hardware, the system is capable of detecting and classifying key road signs and traffic signals in real-time, enabling automated decision-making for mobile robots.

* **Challenge:** Deploy a highly accurate and low-latency computer vision model for traffic sign and signal detection on an edge robotics platform with minimal computing overhead.
* **Action:** Engineered a custom YOLOv5n vision pipeline optimized for Jetson Orin AGX and Raspberry Pi, training the edge model to detect stop, parking, directional signs, and traffic lights.
* **Result:** Successfully deployed the edge model, achieving high classification accuracy and real-time inference speed (30+ FPS) suitable for autonomous navigation tasks.

---

## 🛠️ Software & Hardware Stack

### Hardware
* **Edge Compute:** NVIDIA Jetson Orin AGX Developer Kit (or Raspberry Pi fallback)
* **Sensor:** USB Webcam or Raspberry Pi Camera Module V2/V3
* **Target Vehicle:** Differential Drive Robot / Autonomous Mobile Robot (AMR)

### Software
* **Operating System:** Ubuntu 22.04 LTS (JetPack 5.x / 6.x)
* **Inference Framework:** PyTorch, TensorRT (for maximum performance)
* **Computer Vision:** OpenCV (Python)
* **Model Architecture:** YOLOv5n (Nano)

---

## 📁 Repository Structure

```
├── config/
│   └── dataset.yaml          # Dataset configuration (classes: stop, parking, lights, etc.)
├── models/
│   ├── yolov5n.pt            # Pre-trained base weights
│   └── best.pt               # Custom trained edge weights
├── scripts/
│   ├── detect.py             # Inference pipeline script
│   └── export.py             # Script to export weights to TensorRT/ONNX
├── requirements.txt          # Python dependencies
└── README.md                 # Project documentation
```

---

## 🚀 Getting Started

### 1. Prerequisites & JetPack Setup
Ensure your Jetson Orin AGX is flashed with **JetPack 5.1+** (incorporating CUDA 11.x/12.x and TensorRT).

Install system dependencies:
```bash
sudo apt-get update && sudo apt-get install -y \
    python3-pip \
    python3-opencv \
    libopenblas-base \
    libopenmpi-dev
```

### 2. Clone & Install Dependencies
Clone this repository and set up the virtual environment:
```bash
git clone https://github.com/akhiljithvg/Jetson-Orin-AGX-Sign-Signal-Detection.git
cd Jetson-Orin-AGX-Sign-Signal-Detection
pip3 install -r requirements.txt
```

*Note: For maximum performance, compile PyTorch and torchvision with CUDA support enabled directly on the Jetson, or use NVIDIA's official PyTorch L4T containers.*

### 3. Running Real-Time Inference
Run the inference script on your live camera feed:
```bash
python3 scripts/detect.py --weights models/best.pt --source 0 --img-size 640 --conf-thres 0.5
```
* `--weights`: Path to your custom trained weights.
* `--source`: `0` for local USB camera, or a path to an MP4 video file.
* `--img-size`: Image dimensions for inference (640x640 default).

### 4. TensorRT Optimization (Optional but Recommended)
To achieve maximum frame rate (30+ FPS), export the PyTorch model (`.pt`) to TensorRT (`.engine`):
```bash
python3 scripts/export.py --weights models/best.pt --include engine --device 0 --half
```
Run detection using the optimized TensorRT engine:
```bash
python3 scripts/detect.py --weights models/best.engine --source 0 --img-size 640
```

---

## 📊 Model Details

The model is trained to detect the following classes:
1. `stop` — Red octagonal stop signs.
2. `parking` — Blue/white parking area indicators.
3. `traffic_light_red` — Red traffic signal.
4. `traffic_light_green` — Green traffic signal.
5. `traffic_light_yellow` — Yellow traffic signal.
6. `direction_left` / `direction_right` — Arrow direction signs.

---

## 📄 License
This project is licensed under the MIT License - see the LICENSE file for details.
