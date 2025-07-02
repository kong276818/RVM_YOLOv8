# RVM_YOLOv8
RVM_YOLOv8


# 🗑️ RVM_YOLOv8

## 🧠 프로젝트 개요

**RVM_YOLOv8**은 초분광(Hyperspectral) 또는 일반 RGB 기반 폐기물 이미지 데이터를 대상으로 **YOLOv8**을 활용한 분리수거 분류 및 객체 탐지 모델입니다.

현장 카메라로 촬영된 이미지 내 폐기물(플라스틱, 캔, 종이, 종이상자 등)을 **실시간으로 탐지 및 분류**합니다.

---

## 📁 디렉터리 구조

```bash
RVM_YOLOv8/
├── dataset-resized/          # 클래스별 원본 이미지 (클래스명 디렉터리 포함)
├── dataset-yolo/
│   ├── images/train/         # YOLOv8 학습용 이미지
│   └── labels/train/         # YOLOv8 학습용 라벨 (YOLO format)
├── trashnet_yolo/
│   ├── yolov8_trash/         # 학습 결과 (weights, runs 등)
│   └── rvm_detect_result/    # RVM 실험 이미지 추론 결과
└── rvmt/                     # 실험 대상 RVM 이미지 폴더




