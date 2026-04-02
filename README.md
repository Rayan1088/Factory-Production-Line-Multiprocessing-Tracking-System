# 🏭 Factory-Production-Line-Multiprocessing-Tracking-System

- App Link - https://factory-appuction-line-multiprocessing-tracking-system-wvyohbv.streamlit.app/ 

A real-time factory production line simulation and tracking system built with Python, featuring parallel processing capabilities and an interactive Streamlit dashboard.

[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://factory-appuction-line-multiprocessing-tracking-system-wvyohbv.streamlit.app/)
[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 🌐 Live Demo

**[Launch the App →](https://factory-appuction-line-multiprocessing-tracking-system-wvyohbv.streamlit.app/)**

---

## 📋 Overview

This project simulates a factory production line environment where multiple manufacturing stages operate concurrently using Python's `multiprocessing` module. The system tracks items as they move through various production stages, providing real-time visibility into throughput, bottlenecks, and overall efficiency.

### Key Features

- **Parallel Processing** — Multiple production stages run simultaneously using Python multiprocessing
- **Real-Time Tracking** — Monitor items as they progress through each manufacturing stage
- **Interactive Dashboard** — Streamlit-powered web interface for visualization and control
- **Production Metrics** — Track throughput, cycle times, queue lengths, and efficiency rates
- **Configurable Stages** — Customize production line configurations and processing parameters

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      Streamlit Dashboard                        │
│              (Real-time Visualization & Control)                │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Main Process Controller                      │
│                  (Orchestration & Monitoring)                   │
└─────────────────────────────────────────────────────────────────┘
                              │
          ┌───────────────────┼───────────────────┐
          ▼                   ▼                   ▼
    ┌──────────┐        ┌──────────┐        ┌──────────┐
    │ Stage 1  │   →    │ Stage 2  │   →    │ Stage 3  │
    │ Process  │        │ Process  │        │ Process  │
    └──────────┘        └──────────┘        └──────────┘
          │                   │                   │
          └───────────────────┴───────────────────┘
                              │
                              ▼
                    ┌──────────────────┐
                    │   Shared Queue   │
                    │  (IPC Mechanism) │
                    └──────────────────┘
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8 or higher
- pip (Python package manager)

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

4. **Run the application**
   ```bash
   streamlit run app.py
   ```

5. **Open your browser** and navigate to `http://localhost:8501`

---

## 📦 Dependencies

| Package | Purpose |
|---------|---------|
| `streamlit` | Web dashboard and UI components |
| `multiprocessing` | Parallel process execution (built-in) |
| `pandas` | Data manipulation and analysis |
| `plotly` | Interactive visualizations |
| `numpy` | Numerical computations |

---

## 💡 How It Works

### Production Line Simulation

1. **Item Generation** — Raw items enter the production line at configurable intervals
2. **Stage Processing** — Each stage processes items with defined cycle times and variability
3. **Queue Management** — Inter-process queues handle item flow between stages
4. **Tracking & Logging** — Every item's journey is tracked with timestamps and metrics

### Multiprocessing Implementation

The system leverages Python's `multiprocessing` module to achieve true parallelism:

```python
from multiprocessing import Process, Queue

# Each production stage runs as an independent process
stage_process = Process(target=stage_worker, args=(input_queue, output_queue))
stage_process.start()
```

This approach ensures:
- **True parallel execution** across CPU cores
- **Isolated memory spaces** for each stage
- **Thread-safe communication** via queues
- **Realistic simulation** of factory operations

---

## 📊 Features in Detail

### Dashboard Components

| Component | Description |
|-----------|-------------|
| **Production Overview** | Real-time status of all production stages |
| **Throughput Charts** | Items processed per unit time |
| **Queue Visualization** | Current queue depths at each stage |
| **Cycle Time Analysis** | Processing time statistics |
| **Bottleneck Detection** | Identifies slow stages |

### Configuration Options

- Number of production stages
- Processing time per stage
- Time variability (standard deviation)
- Queue capacity limits
- Simulation speed

---

## 📁 Project Structure

```
Factory-Production-Line-Multiprocessing-Tracking-System/
│
├── app.py                 # Main Streamlit application
├── requirements.txt       # Python dependencies
├── README.md              # Project documentation
│
├── src/                   # Source code modules
│   ├── production_line.py # Production line logic
│   ├── stage.py           # Individual stage implementation
│   └── tracker.py         # Item tracking system
│
└── utils/                 # Utility functions
    ├── config.py          # Configuration management
    └── visualization.py   # Chart generation helpers
```

---

## 🎯 Use Cases

- **Educational Tool** — Learn about multiprocessing and concurrent programming
- **Factory Simulation** — Model real-world manufacturing scenarios
- **Bottleneck Analysis** — Identify production inefficiencies
- **Capacity Planning** — Test different production configurations
- **Process Optimization** — Experiment with cycle time adjustments

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

- Built with [Streamlit](https://streamlit.io/)
- Visualization powered by [Plotly](https://plotly.com/)
- Inspired by real-world manufacturing systems

---

<p align="center">
  <b>⭐ Star this repo if you find it useful!</b>
</p>
