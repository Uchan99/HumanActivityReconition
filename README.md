# 🧠 Human Activity Recognition with Specialized Models

## 📌 프로젝트 개요

본 프로젝트는 센서 기반의 인간 행동 인식(HAR) 정확도를 향상시키기 위해, 전체 활동을 **<mark>정적(Static)</mark>**과 **<mark>동적(Dynamic)</mark>**으로 전략적으로 분리하고, 각 활동 특성에 맞는 **전용 모델을 설계**하는 이중 분류 방식(Two-Stage Classification)을 제안합니다.

전통적인 통합 모델 접근에서 벗어나, **활동 유형별 최적화된 구조**를 적용함으로써 **<mark>모델 해석력</mark>**과 **<mark>정밀도</mark>** 모두를 극대화하였습니다.

---

## ✨ 핵심 특징

- **전략적 데이터 분리**: 활동 레이블을 기준으로 Static / Dynamic을 사전 정의하여 분리 학습
- **전용 아키텍처 설계**:
  - 정적: ResNet + Transformer 기반 Attention Fusion 구조
  - 동적: PCA 전처리 + LSTM + Attention 구조
- **<mark>다양한 HAR 데이터셋</mark>에 대한 일반화 실험**: UCI HAR, WISDM, PAMAP2, mHealth 사용

---

## 🧪 활용 데이터셋

| Dataset   | 설명 |
|-----------|------|
| [UCI HAR](https://archive.ics.uci.edu/ml/datasets/human+activity+recognition+using+smartphones) | 스마트폰 IMU 센서 기반 6가지 일상 활동 수록 |
| [WISDM](https://www.cis.fordham.edu/wisdm/dataset.php) | 가속도 기반 행동 인식, 다양한 일상 행동 포함 |
| [PAMAP2](https://archive.ics.uci.edu/ml/datasets/PAMAP2+Physical+Activity+Monitoring) | 고해상도 센서, 12개 활동 포함 |
| mHealth | [mhealth_full.csv](./mhealth_full.csv) 직접 제공 (공식 링크 없음) |

---

## ⚙️ 방법론: Two-Stage Classification

### ✅ Stage 1: 데이터 분리 (Preprocessing)

- 학습 전에 활동 레이블을 기준으로 **정적 / 동적 활동으로 분리**
- **정적 활동 (Static)**: `LAYING`, `SITTING`, `STANDING`
- **동적 활동 (Dynamic)**: `WALKING`, `WALKING_UPSTAIRS`, `WALKING_DOWNSTAIRS`

---

### ✅ Stage 2: 활동 유형별 전용 분류 모델

#### 🔹 Static Activity Classifier

- **모델 구조**: Attention Fusion = ResNet + Transformer 병렬 구조
- **<mark>F1 Score = 96.28</mark> (UCI-HAR 기준)**
- 📄 관련 파일: `250112_UCI(Static)_AttentionFusion(Trans, RESNet)_962.ipynb`

📊 아키텍처 다이어그램:

<div align="center">
  <img src="images/static_model_overall.png" width="700"/>
  <br><em>Attention Fusion: 전체 구조</em>
</div>

<div align="center">
  <img src="images/static_model_transformer.png" width="600"/>
  <br><em>Transformer Encoder (Global Feature Extractor)</em>
</div>

<div align="center">
  <img src="images/static_model_resnet.png" width="600"/>
  <br><em>ResNet Encoder (Local Feature Extractor)</em>
</div>

---

#### 🔸 Dynamic Activity Classifier

- **모델 구조**: PCA 차원 축소 + 3-Layer LSTM + Attention3D
- **<mark>F1 Score = 98.12</mark> (UCI-HAR 기준)**
- 📄 관련 파일: `250123_UCI_dynamic_PCA_98.ipynb`

<div align="center">
  <img src="images/dynamic_model.png" width="700"/>
  <br><em>Attention LSTM 기반 동적 활동 인식 구조</em>
</div>

---

## 📊 모델 성능 요약

| Dataset   | <mark>Static F1</mark> | <mark>Dynamic F1</mark> | <mark>Total F1</mark> | <mark>Precision</mark> | <mark>Recall</mark> | <mark>Accuracy</mark> |
|-----------|------------------------|-------------------------|-----------------------|------------------------|---------------------|------------------------|
| UCI-HAR   | 96.27                  | 98.12                   | 97.20                | 97.30                  | 97.17               | 97.20                  |
| PAMAP2    | 99.68                  | 99.11                   | 99.40                | 99.19                  | 99.11               | 99.40                  |
| WISDM     | 98.71                  | 99.20                   | 98.95                | 99.13                  | 99.16               | 98.96                  |

---

## 🔬 Ablation Study (ResNet Block 수에 따른 Static 모델 성능)

| Dataset   | 1-Block | 2-Block | 3-Block | <mark>4-Block</mark> | 5-Block |
|-----------|---------|---------|---------|------------------------|---------|
| UCI-HAR   | 93.21   | 93.63   | 95.77   | <mark>96.28</mark>     | 94.49   |
| PAMAP2    | 99.79   | 99.72   | 99.55   | <mark>99.68</mark>     | <mark>99.68</mark> |
| WISDM     | 97.79   | 97.97   | 97.24   | <mark>98.71</mark>     | 97.42   |
| mHealth   | 100.00  | 100.00  | 100.00  | <mark>100.00</mark>    | 100.00  |

---

## 🔍 혼동 행렬 예시 (UCI-HAR, Static)

| 실제\예측 | Sitting | Standing | Laying |
|-----------|---------|----------|--------|
| Sitting   | <mark>452</mark> | 39       | 0      |
| Standing  | 19      | <mark>513</mark>  | 0      |

> Recall 기준: Sitting = <mark>92.06%</mark>, Standing = <mark>96.43%</mark>

---

## 📁 레포지토리 구조

```bash
HumanActivityReconition/
├── 250112_UCI(Static)_AttentionFusion(Trans, RESNet)_962.ipynb
├── 250123_UCI_dynamic_PCA_98.ipynb
├── HAR_CM_250222.xlsx
├── mhealth_full.csv
├── images/
│   ├── static_model_overall.png
│   ├── static_model_transformer.png
│   ├── static_model_resnet.png
│   └── dynamic_model.png
└── README.md
