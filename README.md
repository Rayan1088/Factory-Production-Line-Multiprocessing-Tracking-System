# 🏭 Factory Production Line Multiprocessing Tracking System

A real-time object detection and tracking system for factory production lines using **fine-tuned YOLOv11** with Python multiprocessing capabilities and an interactive Streamlit dashboard.

[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://factory-appuction-line-multiprocessing-tracking-system-wvyohbv.streamlit.app/)
[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![YOLOv11](https://img.shields.io/badge/YOLOv11-Ultralytics-purple.svg)](https://docs.ultralytics.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 🌐 Live Demo

**[Launch the App →](https://factory-appuction-line-multiprocessing-tracking-system-wvyohbv.streamlit.app/)**

---

## 📋 Overview

This system performs real-time object detection and tracking on factory production line videos using a **fine-tuned YOLOv11m model**. It features dual tracker architecture for monitoring different production stages simultaneously, with automatic counting via line-crossing detection and persistent storage to both local SQLite and Turso cloud databases.

### Key Features

- **YOLOv11 Object Detection** — Fine-tuned YOLOv11m model for custom object classes (boxes, cement bags)
- **Real-Time Object Tracking** — Trajectory-based tracking with configurable counting lines
- **Parallel Processing** — Python multiprocessing for simultaneous dual-video processing
- **Line-Crossing Detection** — Automated in/out counting using geometric line intersection
- **Interactive Dashboard** — Streamlit web interface for live video processing and database viewing
- **Dual Database Support** — Local SQLite with automatic fallback, plus Turso cloud database
- **Configurable Parameters** — YAML-based configuration for all tracking and display settings

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      Streamlit Dashboard                        │
│           (Live Video Display & Database Records)               │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│               Multiprocessing Controller (main.py)              │
│              (Process Spawning, Monitoring, Cleanup)            │
└─────────────────────────────────────────────────────────────────┘
                              │
          ┌───────────────────┴───────────────────┐
          ▼                                       ▼
    ┌──────────────┐                        ┌──────────────┐
    │  Tracker 1   │                        │  Tracker 2   │
    │  (Boxes)     │                        │ (Cement Bags)│
    │              │                        │              │
    │ • 3 Counting │                        │ • 1 Counting │
    │   Lines      │                        │   Line       │
    │ • In/Out     │                        │ • Out Count  │
    │   Counting   │                        │   Only       │
    └──────────────┘                        └──────────────┘
          │                                       │
          └───────────────┬───────────────────────┘
                          ▼
              ┌──────────────────────┐
              │   YOLOv11m Model     │
              │   (Fine-tuned)       │
              │   weights/best.pt    │
              └──────────────────────┘
                          │
                          ▼
              ┌──────────────────────┐
              │   Database Manager   │
              │  ┌────────────────┐  │
              │  │ Local SQLite   │  │
              │  └────────────────┘  │
              │  ┌────────────────┐  │
              │  │ Turso Cloud DB │  │
              │  └────────────────┘  │
              └──────────────────────┘
```

---

## 🧠 Model Training

The object detection model was trained on Kaggle using the Ultralytics YOLOv11 framework.

### Training Configuration

| Parameter | Value |
|-----------|-------|
| **Base Model** | YOLOv11m (yolo11m.pt) |
| **Image Size** | 640×640 |
| **Epochs** | 300 (early stopping with patience=100) |
| **Batch Size** | Auto (-1) |
| **Optimizer** | Auto |
| **Learning Rate** | 0.001 → 0.0001 (cosine annealing) |
| **Momentum** | 0.98 |
| **Weight Decay** | 0.005 |
| **Warmup Epochs** | 3 |

### Data Augmentation

| Augmentation | Value |
|--------------|-------|
| Mosaic | 1.0 |
| Flip Up-Down | 0.9 |
| Flip Left-Right | 0.9 |
| HSV Hue | 0.015 |
| HSV Saturation | 0.7 |
| HSV Value | 0.4 |
| Rotation | ±10° |
| Scale | 0.5 |
| Translation | 0.1 |
| Erasing | 0.4 |
| Auto Augment | RandAugment |

### Dataset

| Class | Instances |
|-------|-----------|
| Box | ~200 |
| Cement Bag | ~200 |

### Model Performance

| Metric | Value |
|--------|-------|
| **mAP@0.5** | 0.944 |
| **mAP@0.5:0.95** | 0.76 |
| **Precision** | 0.95 |
| **Recall** | 0.87 |
| **F1 Score** | 0.90 @ conf 0.136 |

### Training Curves

The model was trained for 286 epochs before early stopping triggered. Training showed consistent convergence with:

- **Box Loss**: 1.96 → 0.45
- **Classification Loss**: 4.21 → 0.27
- **DFL Loss**: 2.05 → 0.86

### Confusion Matrix

```
              Predicted
              Box    Cement Bag   Background
Actual
Box           203        0           28
Cement Bag     0         -            0
Background    21         0            -

Normalized: Box recall = 88%, Background FP rate = 12%
```

### Training Notebook

The complete training pipeline is available in the Jupyter notebook:
```
notebooks/fine-tune-yolov11m-for-real-time-object-detection.ipynb
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8 or higher
- pip (Python package manager)
- GPU recommended for real-time inference (CPU supported)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Rayan1088/Factory-Production-Line-Multiprocessing-Tracking-System.git
   cd Factory-Production-Line-Multiprocessing-Tracking-System
   ```

2. **Create a virtual environment** (recommended)
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Add your model and videos**
   - Place your fine-tuned YOLOv11 model at `weights/best.pt`
   - Place video files in `video data/` directory
   - Update paths in `config.yaml` if needed

5. **Run the application**

   **Option A: Streamlit Web App**
   ```bash
   streamlit run app.py
   ```

   **Option B: Local Multiprocessing Mode**
   ```bash
   python main.py
   ```

6. **Open your browser** and navigate to `http://localhost:8501`

---

## 📁 Project Structure

```
Factory-Production-Line-Multiprocessing-Tracking-System/
│
├── app.py                          # Streamlit web application
├── main.py                         # Multiprocessing entry point
├── config.yaml                     # Configuration parameters
├── requirements.txt                # Python dependencies
├── tracking_data.db                # Local SQLite database
├── LICENSE                         # MIT License
├── README.md                       # Project documentation
│
├── weights/
│   └── best.pt                     # Fine-tuned YOLOv11m model
│
├── video data/
│   ├── box.mp4                     # Box tracking video
│   └── pac.mp4                     # Cement bag tracking video
│
├── src/
│   ├── __init__.py
│   ├── tracker_1.py                # Box tracker (3 counting lines, in/out)
│   ├── tracker_2.py                # Cement bag tracker (1 counting line)
│   ├── multiprocessing.py          # Process management & monitoring
│   ├── databaseManager.py          # SQLite & Turso database handler
│   ├── utils.py                    # YOLO loading, frame processing, drawing
│   ├── logger.py                   # Logging configuration
│   └── exception.py                # Custom exception handling
│
├── logs/
│   └── log_YYYY-MM-DD.log          # Daily log files
│
└── notebooks/
    └── fine-tune-yolov11m-for-real-time-object-detection.ipynb
```

---

## ⚙️ Configuration

All parameters are controlled via `config.yaml`:

```yaml
# Model and Video Paths
MODEL_PATH: 'weights/best.pt'
VIDEO_SOURCE_1: 'video data/box.mp4'
VIDEO_SOURCE_2: 'video data/pac.mp4'

# Frame Processing
SKIP_FRAMES_FOR_TRACKER_1: 2        # Process every 3rd frame
SKIP_FRAMES_FOR_TRACKER_2: 10       # Process every 11th frame
FRAME_WIDTH: 800
FRAME_HEIGHT: 500

# Tracking Parameters
MAX_HISTORY_FOR_SAVE_IN_DICTIONARY: 100
MAX_FRAMES_MISSING: 30              # Remove track after N missing frames
MINIMUM_MOVEMENT_THRESHOLD: 5       # Pixels

# Database Configuration
DATABASE_SAVE_TIME_INTERVAL: 60     # Auto-save interval in seconds
LOCAL_DB_PATH: 'tracking_data.db'
TURSO_DB_URL: 'libsql://your-database.turso.io'
TURSO_DB_TOKEN: 'your-token'

# Counting Lines (x1, y1, x2, y2)
HORIZONTAL_LINE_1_FOR_TRACKER_1: [82, 332, 265, 332]
HORIZONTAL_LINE_2_FOR_TRACKER_1: [298, 332, 501, 332]
HORIZONTAL_LINE_3_FOR_TRACKER_1: [536, 332, 740, 332]
COUNTING_LINE_1_FOR_TRACKER_2: [386, 299, 515, 299]
```

---

## 📦 Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `ultralytics` | 8.3.179 | YOLOv11 object detection framework |
| `opencv-python-headless` | - | Video processing (headless for cloud) |
| `torch` | - | Deep learning backend |
| `torchvision` | - | Vision utilities |
| `streamlit` | ≥1.28.0 | Web dashboard framework |
| `pandas` | - | Data manipulation |
| `numpy` | <2 | Numerical computations |
| `libsql-client` | - | Turso database connectivity |
| `lap` | 0.5.12 | Linear assignment for tracking |
| `pyyaml` | - | Configuration parsing |

---

## 🎯 How It Works

### Object Detection Pipeline

1. **Video Input** — Frames are read from video files with configurable skip intervals
2. **YOLO Inference** — Fine-tuned YOLOv11m detects objects with bounding boxes
3. **Tracking** — Objects are tracked across frames using trajectory history
4. **Line Crossing** — Geometric intersection detection counts objects crossing defined lines
5. **Database Storage** — Counts are periodically saved to SQLite/Turso databases

### Counting Logic

The system uses **line intersection detection** to count objects:

```python
# Movement vector crosses counting line?
intersects = check_line_intersect(prev_x, prev_y, curr_x, curr_y, line_coords)

if intersects:
    if dy > 0:  # Moving down
        out_count += 1
    else:       # Moving up
        in_count += 1
```

### Safe Model Loading

The system implements defense-in-depth for loading fine-tuned models:

```python
# 4 fallback methods for secure model loading
# Method 1: Safe globals + monkey patching
# Method 2: Context manager for safe globals
# Method 3: Comprehensive patching approach
# Method 4: Direct loading with warnings suppressed
```

---

## 📊 Dashboard Features

| Page | Description |
|------|-------------|
| **Video Processing** | Side-by-side live video feeds with real-time tracking visualization |
| **Database Records** | View and download tracking counts as CSV with timestamps |

### Dashboard Controls

- **Start Tracking** — Begin dual-video processing
- **Stop Processing** — Halt and cleanup all trackers
- **Refresh Data** — Reload database records
- **Download CSV** — Export counts for analysis

---

## 🔧 Running Modes

### 1. Streamlit Cloud App
```bash
streamlit run app.py
```
- Runs in browser with cloud database (Turso)
- Headless OpenCV for server deployment
- Progress bar and status indicators

### 2. Local Multiprocessing
```bash
python main.py
```
- Full OpenCV GUI windows
- Mouse callback for coordinate detection
- Keyboard controls (Q to quit, S to save)
- True parallel processing with spawn method

---

## 📝 Database Schema

### `tracking_box_counts`
| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key (auto-increment) |
| total_box_in | INTEGER | Boxes moving into zone |
| total_box_out | INTEGER | Boxes moving out of zone |
| date_time | DATETIME | Timestamp of record |

### `tracking_cement_bag_counts`
| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key (auto-increment) |
| total_cement_bag_out | INTEGER | Bags exiting production line |
| date_time | DATETIME | Timestamp of record |

---

## 📈 Sample Results

### Training Progress
![Training Results](results.png)

### Validation Predictions
![Validation Predictions](val_batch0_pred.jpg)

### Precision-Recall Curve
![PR Curve](PR_curve.png)

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/AmazingFeature`)
3. **Commit** your changes (`git commit -m 'Add AmazingFeature'`)
4. **Push** to the branch (`git push origin feature/AmazingFeature`)
5. **Open** a Pull Request

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Rayan** — [@Rayan1088](https://github.com/Rayan1088)

---

## 🙏 Acknowledgments

- [Ultralytics YOLOv11](https://docs.ultralytics.com/) — State-of-the-art object detection
- [Streamlit](https://streamlit.io/) — Rapid web app development
- [Turso](https://turso.tech/) — Edge database for global deployment
- [OpenCV](https://opencv.org/) — Computer vision library
- [Kaggle](https://kaggle.com/) — GPU resources for model training

---

<p align="center">
  <b>⭐ Star this repo if you find it useful!</b>
</p>
