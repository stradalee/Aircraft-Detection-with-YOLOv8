# ✈️ Aircraft Detection with YOLOv8

An object detection project built during my senior year of high school.
Detects aircraft in satellite and aerial imagery using a fine-tuned YOLOv8 model.

---

## 📌 Overview

| Item | Details |
|------|---------|
| Goal | Detect aircraft locations in satellite imagery |
| Model | YOLOv8s (Ultralytics) |
| Dataset | [Airbus Aircraft Sample Dataset (Kaggle)](https://www.kaggle.com/datasets/airbusgeo/airbus-aircrafts-sample-dataset) |
| Environment | Kaggle Notebook (GPU) |
| Classes | 1 (`Aircraft`) |

---

## 🗂️ Project Structure

```
aircraft-detection-with-yolov8.ipynb   # Full pipeline notebook
data.yaml                              # YOLOv8 training config (auto-generated)
runs/
└── detect/
    ├── train/                         # Training outputs (weights, metrics, visualizations)
    └── val/                           # Validation results
```

---

## ⚙️ Pipeline

### 1. Preprocessing
- High-resolution images are tiled into **512×512 patches**
- **64-pixel overlap** between tiles to avoid missing aircraft near boundaries
- Bounding boxes with more than 30% truncation are excluded (`TRUNCATED_PERCENT = 0.3`)
- Polygon annotations from `annotations.csv` are converted to YOLO format (`x_center, y_center, w, h`)

### 2. Data Split
- Train / validation split using 5-Fold Cross Validation (Fold 1)

### 3. Training
```bash
yolo task=detect mode=train model=yolov8s.pt data=data.yaml epochs=10 imgsz=512
```

### 4. Validation
```bash
yolo task=detect mode=val model=runs/detect/train/weights/best.pt data=data.yaml
```

---

## 📦 Dependencies

```
ultralytics
albumentations==1.0.3
torch
pandas
numpy
Pillow
plotly
tqdm
```

---

## 🚀 Getting Started

1. Add the [Airbus Aircraft Sample Dataset](https://www.kaggle.com/datasets/airbusgeo/airbus-aircrafts-sample-dataset) to your Kaggle environment
2. Run the notebook cells in order
3. GPU acceleration is recommended (Kaggle free GPU is sufficient)

---

## 📝 Reflection

This was my first end-to-end experience training an object detection model. Working with satellite imagery presented a unique challenge — aircraft appear small and densely packed — which made me realize how critical the tiling strategy is to overall detection performance.
