# ✈️ Aircraft Detection with YOLOv8
 
고등학교 3학년 시절 진행한 항공기 객체 탐지(Object Detection) 프로젝트입니다.  
위성/항공 이미지에서 YOLOv8 모델을 활용해 항공기를 자동으로 탐지합니다.
 
---
 
## 📌 프로젝트 개요
 
| 항목 | 내용 |
|------|------|
| 목표 | 위성 이미지에서 항공기 위치 탐지 |
| 모델 | YOLOv8s (Ultralytics) |
| 데이터셋 | [Airbus Aircraft Sample Dataset (Kaggle)](https://www.kaggle.com/datasets/airbusgeo/airbus-aircrafts-sample-dataset) |
| 실행 환경 | Kaggle Notebook (GPU) |
| 클래스 수 | 1 (`Aircraft`) |
 
---
 
## 🗂️ 프로젝트 구조
 
```
aircraft-detection-with-yolov8.ipynb   # 전체 파이프라인 노트북
data.yaml                              # YOLOv8 학습 설정 파일 (자동 생성)
runs/
└── detect/
    ├── train/                         # 학습 결과 (weights, 메트릭, 시각화)
    └── val/                           # 검증 결과
```
 
---
 
## ⚙️ 주요 파이프라인
 
### 1. 데이터 전처리
- Airbus 항공기 데이터셋의 고해상도 이미지를 **512×512 타일**로 분할
- 타일 간 **64픽셀 오버랩**으로 경계 영역의 항공기 누락 방지
- 잘린 비율이 30% 미만인 바운딩 박스는 제외 (`TRUNCATED_PERCENT = 0.3`)
- `annotations.csv`의 폴리곤 좌표를 YOLO 포맷(`x_center, y_center, w, h`)으로 변환
### 2. 데이터 분할
- 5-Fold Cross Validation 방식으로 train / val 분리 (Fold 1 사용)
### 3. 모델 학습
```bash
yolo task=detect mode=train model=yolov8s.pt data=data.yaml epochs=10 imgsz=512
```
 
### 4. 모델 검증
```bash
yolo task=detect mode=val model=runs/detect/train/weights/best.pt data=data.yaml
```
 
---
 
## 📦 주요 라이브러리
 
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
 
## 🚀 실행 방법
 
1. Kaggle에서 [Airbus Aircraft Sample Dataset](https://www.kaggle.com/datasets/airbusgeo/airbus-aircrafts-sample-dataset) 추가
2. 노트북을 Kaggle 환경에서 순서대로 실행
3. GPU 가속 사용 권장 (Kaggle 무료 GPU 사용 가능)
---
 
## 📝 회고
 
고등학교 3학년 때 처음으로 객체 탐지 모델을 직접 학습시켜 본 프로젝트입니다.  
위성 이미지 특성상 항공기가 작고 밀집되어 있어, 타일링 전처리 방식이 탐지 성능에 중요하다는 점을 직접 경험했습니다.
 
