# RVM_YOLOv8
RVM_YOLOv8


🧠 프로젝트 개요
RVM_YOLOv8은 초분광 또는 일반 RGB 기반 폐기물 이미지 데이터를 대상으로 YOLOv8을 활용한 분리수거 분류 및 객체 탐지 모델입니다.
현장 카메라로 촬영된 이미지 내 폐기물(플라스틱, 캔, 종이, 종이상자 등)을 실시간으로 탐지하여 분류합니다.

📁 디렉터리 구조
bash
복사
편집
RVM_YOLOv8/
├── dataset-resized/          # 클래스별 원본 이미지 (클래스명 디렉터리 포함)
├── dataset-yolo/
│   ├── images/train/         # YOLOv8 학습용 이미지
│   └── labels/train/         # YOLOv8 학습용 라벨 (YOLO format)
├── trashnet_yolo/
│   ├── yolov8_trash/         # 학습 결과 (weights, runs 등)
│   └── rvm_detect_result/    # RVM 실험 이미지 추론 결과
└── rvmt/                     # 실험 대상 RVM 이미지 폴더
⚙️ 전처리 및 라벨링 자동화 코드
python
복사
편집
import os
import glob
from PIL import Image

class_map = {
    "cardboard": 0, "glass": 1, "metal": 2,
    "paper": 3, "plastic": 4, "trash": 5
}

image_root = "dataset-resized"
output_img_dir = "dataset-yolo/images/train"
output_lbl_dir = "dataset-yolo/labels/train"

os.makedirs(output_img_dir, exist_ok=True)
os.makedirs(output_lbl_dir, exist_ok=True)

# 작은 중심 + 작은 박스 크기로 라벨 고정 (중앙부 작은 박스)
cx, cy, bw, bh = 0.5, 0.5, 0.4, 0.4

for class_name, class_id in class_map.items():
    for ext in ["*.jpg", "*.jpeg", "*.png"]:
        paths = glob.glob(os.path.join(image_root, class_name, ext))
        for path in paths:
            img_name = os.path.basename(path)
            os.system(f'cp "{path}" "{os.path.join(output_img_dir, img_name)}"')
            with open(os.path.join(output_lbl_dir, img_name.rsplit('.', 1)[0] + ".txt"), "w") as f:
                f.write(f"{class_id} {cx} {cy} {bw} {bh}\n")
🏋️‍♂️ 모델 학습
bash
복사
편집
yolo detect train \
  model=yolov8n.pt \
  data=your_data.yaml \
  epochs=100 \
  imgsz=640 \
  project=trashnet_yolo \
  name=yolov8_trash
🔍 RVM 테스트 이미지 추론
bash
복사
편집
yolo detect predict \
  model=trashnet_yolo/yolov8_trash/weights/best.pt \
  source=/home/woorim/trashnet/rvmt \
  imgsz=640 \
  conf=0.25 \
  save=True \
  project=trashnet_yolo \
  name=rvm_detect_result
📈 성능 (예시)
mAP@0.5: 0.995 → 정확한 탐지 위치 기준으로 매우 우수

mAP@0.5:0.95: 0.565 → 클래스 간 경계 혼동 있음 (fine-tuning 필요)

✅ 향후 개선 방향
이미지 중심 이외의 객체 위치 고려하여 bounding box 다변화

background (컨베이어, 조명 등) noise 제거 또는 보정

plastic/metal 혼동 방지 위한 데이터 수집 및 fine-tuning

hyperspectral data 접목한 정밀 분류

