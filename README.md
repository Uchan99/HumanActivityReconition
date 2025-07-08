# 🧠 Human Activity Recognition with Specialized Models

## 📌 프로젝트 개요

본 프로젝트는 센서 기반의 인간 행동 인식(HAR) 정확도를 향상시키기 위해, 전체 활동을 **'정적(Static)'과 '동적(Dynamic)'으로 전략적으로 분리**하고, 각 활동 특성에 맞게 **전용 모델을 별도로 설계**하는 이중 분류 방식(Two-Stage Classification)을 제안합니다.

전통적으로 하나의 통합 모델로 모든 활동을 예측하던 접근에서 벗어나, **각 활동의 특성에 최적화된 딥러닝 구조를 적용**함으로써 더욱 정밀한 예측 성능을 달성하였습니다.

---

## ✨ 핵심 특징

- **정적 vs 동적 활동 분리**: 데이터셋의 활동 레이블을 분석하여 두 개의 카테고리로 분리하고, 각각 전용 모델을 훈련함.
- **전용 딥러닝 모델 설계**:
  - **정적 활동**: Transformer와 ResNet을 병렬로 구성한 **Attention Fusion** 구조.
  - **동적 활동**: PCA로 차원 축소 후, 3-Layer LSTM과 Attention을 조합한 **Time-Series 기반 모델**.
- **고성능 성과**: UCI HAR 데이터셋 기준 F1 score 최대 **0.981**을 기록.

---

## 🧪 활용 데이터셋

| Dataset   | 설명 |
|-----------|------|
| [UCI HAR](https://archive.ics.uci.edu/ml/datasets/human+activity+recognition+using+smartphones) | 스마트폰 IMU 센서 기반 6가지 일상 활동 수록 |
| [WISDM](https://www.cis.fordham.edu/wisdm/dataset.php) | 가속도 기반 행동 인식, 다양한 일상 행동 포함 |
| [PAMAP2](https://archive.ics.uci.edu/ml/datasets/PAMAP2+Physical+Activity+Monitoring) | 고해상도 센서, 12개 활동 포함 |
| [mHealth](https://mhealth.cs.ucy.ac.cy/) | 다양한 센서와 사람의 신체 부위에 부착된 장비 기반 |

> 모든 데이터셋에 대해 동일한 분리 전략과 모델을 적용하여 일반화 가능성을 검증함.

---

## ⚙️ 방법론: Two-Stage Classification

### ✅ Stage 1: 데이터 분리 (Preprocessing)

- 학습 전에 활동 라벨을 기준으로 데이터를 **정적 / 동적**으로 분리합니다.
- **정적 활동 (Static)**: `LAYING`, `SITTING`, `STANDING`
- **동적 활동 (Dynamic)**: `WALKING`, `WALKING_UPSTAIRS`, `WALKING_DOWNSTAIRS`

> 해당 단계는 모델 학습이 아닌 **데이터 전처리 전략**에 해당하며, 사전 분류 모델은 사용되지 않았습니다.

---

### ✅ Stage 2: 활동 유형별 전용 분류 모델

#### 🔹 Static Activity Classifier

- **모델 구조**: Attention Fusion
  - **Transformer Encoder** → 전역적인 시계열 특성 추출
  - **ResNet1D** → 지역적 특징 및 미세한 차이 학습
  - **Feature Fusion** → 두 브랜치의 출력을 병합하여 높은 분류 정확도 확보
- **성능 (UCI HAR 기준)**: **F1 Score = 0.962**
- 📄 관련 파일: `250112_UCI(Static)_AttentionFusion(Trans, RESNet)_962.ipynb`

#### 🔸 Dynamic Activity Classifier

- **전처리**: PCA로 주요 특성만 보존하여 차원 축소
- **모델 구조**:
  - 3-Layer **LSTM**으로 시간적 흐름 학습
  - **Attention Mechanism**으로 중요한 시점에 가중치 부여
- **성능 (UCI HAR 기준)**: **F1 Score = 0.981**
- 📄 관련 파일: `250123_UCI_dynamic_PCA_98.ipynb`

---

## 📊 성능 요약

| 분류 유형 | 사용 모델 | 주요 기술 요소 | F1 Score |
|-----------|-----------|----------------|----------|
| 정적 활동 | Attention Fusion (Transformer + ResNet) | Local-Global Feature Fusion | 0.962 |
| 동적 활동 | PCA + LSTM with Attention | 차원 축소 + Time-Aware Attention | 0.981 |

---

## 📁 레포지토리 구조

```bash
HumanActivityReconition/
├── static_model/
│   └── 250112_UCI(Static)_AttentionFusion(Trans, RESNet)_962.ipynb
├── dynamic_model/
│   └── 250123_UCI_dynamic_PCA_98.ipynb
├── data/               # 원본 및 분리된 데이터셋
├── utils/              # 공통 전처리 및 함수
└── README.md           # 본 문서
```
