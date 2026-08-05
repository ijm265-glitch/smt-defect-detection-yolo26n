# SMT Defect Detection System (YOLO26n)

> **Single End-to-End YOLO26n Model for SMT Multi-Process Defect Detection**  
> Baseline 대비 모델 파이프라인 단일화 및 탐지 성능(mAP@50 **96.9%**) 대폭 개선

---

## Project Overview

본 프로젝트는 **SMT(표면실장기술) 제조 공정**의 품질 개선을 위한 실시간 객체 탐지(Object Detection) 모델 구축 프로젝트입니다. 

기존 데이터 제공측(Baseline)은 사전 공정과 납땜 공정을 별도의 모델로 분리하여 학습을 진행했으나, 본 프로젝트에서는 **경량화 모델(YOLO26n) 단 하나로 전체 공정을 통합 학습**하였으며, **mAP@50 기준 96.9%**를 달성하였습니다.
---

## Performance Comparison

### 1. Baseline vs. Unified Model Summary

| Model | Task Scope | Model Count | mAP@0.5 |
| :--- | :--- | :---: | :---: |
| **Baseline (YOLOv11)** | 사전공정 결함 검출 | 1개 | 84.10% |
| **Baseline (YOLOv11)** | 납땜공정 결함 검출 | 1개 | 78.40% |
| **Ours (YOLO26n)** | **전 공정 통합 검출** | **1개 (Unified)** | **96.90%** |

---

## Key Improvements & Engineering Highlights

### 1. **검출 정확도(mAP) 향상**
* 최신 YOLO26n 모델을 활용하여 기존 분리 모델 대비 성능을 대폭 개선했습니다. (납땜공정 78.40% / 사전공정 84.10% $\rightarrow$ 통합 모델 96.90%)

### 2. **경량화 및 Edge Device 적용 최적화**
* Lightweight Nano 사이즈 계열인 **YOLO26n** 및 **512x512 해상도**를 채택하여, ESP32-CAM과 같은 경량 임베디드 Edge 디바이스나 실시간 RTSP 스트리밍 환경에서도 적은 VRAM 소모로 높은 FPS 추론이 가능합니다.

---

## Detailed Metrics (Validation Results)

* **mAP@50 (B):** `0.96905` (96.91%)
* **mAP@50-95 (B):** `0.88002` (88.00%)
* **Precision (B):** `0.92406` (92.41%)
* **Recall (B):** `0.92132` (92.13%)

---

## Environment & Usage

### Dependencies
* Python 3.10+
* PyTorch
* Ultralytics YOLO

### Quick Start
```python
from ultralytics import YOLO

# 1. Load trained model
model = YOLO('SMT_best.pt')

# 2. Run Inference on SMT frame
results = model.predict(source='smt_frame.jpg', imgsz=512, verbose=False)

# 3. Visualize results
rendered_frame = results[0].plot()
```

---

## Data Source & Acknowledgement

- **Dataset Name:** 18. SMT 공정 개선 멀티모달 데이터
- **Source:** [AI-Hub (aihub.or.kr)](https://aihub.or.kr)

* 본 프로젝트는 과학기술정보통신부의 재원으로 한국지능정보사회진흥원의 지원을 받아 구축된 "18. SMT 공정 개선 멀티모달 데이터"를 활용하여 수행되었습니다.
* This project was developed using datasets from 'The Open AI Dataset Project (AI-Hub, S. Korea)'. All data information can be accessed through AI-Hub.
* **Model Deployment Note:** AI허브 이용약관에 따라, 해당 데이터셋으로 학습되어 생성된 AI 모델 및 관련 서비스는 자유로운 배포 및 활용이 가능합니다.
* In accordance with AI-Hub terms of use, AI models and services trained on this dataset can be freely deployed and utilized.

## License

This project is open-source and provided under the **AGPL-3.0 License** in accordance with the [Ultralytics Licensing Terms](https://github.com/ultralytics/ultralytics/blob/main/LICENSE).

- **Open Source Use:** Free to use, modify, and distribute under AGPL-3.0 (Requires open-sourcing derived works).
- **Commercial Use:** For commercial deployment without open-sourcing code, an Enterprise License must be obtained from Ultralytics.
- **Dataset License:** The dataset used in this project is subject to [AI-Hub Terms of Use](https://aihub.or.kr).
