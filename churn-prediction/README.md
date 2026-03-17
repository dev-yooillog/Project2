# Churn Prediction

> 이커머스 고객 이탈 예측 모델 개발 및 리스크 세그먼트 분류

---

## 프로젝트 개요

이커머스 고객 데이터를 기반으로 이탈(Churn) 여부를 예측하는 분류 모델을 개발합니다. SQL을 활용한 리스크 플래그 파생 변수 생성, Logistic Regression 및 Random Forest 모델 비교, 고위험 고객 세그먼트 식별까지 전체 분석 파이프라인을 포함합니다.

---

## 분석 파이프라인

```
01_data_load_test.ipynb     # 데이터 로드 및 탐색적 분석 (EDA)
        ↓
02_sql_aggregation.ipynb    # SQL 기반 리스크 플래그 파생 변수 생성
        ↓
03_ml_modeling.ipynb        # 머신러닝 모델 학습 및 평가
        ↓
outputs/                    # 시각화 결과 및 고위험 고객 목록
```

---

## 파일 구조

```
├── data/
│ 
│ 
├── notebooks/
│   ├── 01_data_load_test.ipynb         # EDA 및 변수 분포 확인
│   ├── 02_sql_aggregation.ipynb        # 리스크 플래그 생성 (SQL)
│   └── 03_ml_modeling.ipynb           # 모델 학습·평가·세그먼트 분석
├── outputs/
│   ├── variable_distribution.png       # 주요 변수 분포
│   ├── churn_relationships.png         # 변수별 이탈 여부 관계 (Box plot)
│   ├── sql_risk_flags.png              # 리스크 플래그별 이탈률
│   ├── roc_curve.png                   # ROC Curve 비교
│   ├── confusion_matrix.png            # 혼동 행렬 (Random Forest)
│   ├── feature_importance.png          # 특성 중요도 Top 10
│   ├── risk_segmentation.png           # 리스크 세그먼트 분포 및 실제 이탈률
│   ├── risk_profile.png                # 고위험/저위험 고객 특성 비교
│   ├── high_risk_profile.png           # 고위험 고객 상세 프로파일
│   └── high_risk_customers_sample.csv  # 고위험 고객 목록
└── requirements.txt
```

---

## 주요 변수

| 변수 | 설명 |
|------|------|
| `Tenure` | 서비스 이용 기간 |
| `CashbackAmount` | 캐시백 금액 |
| `DaySinceLastOrder` | 마지막 주문 후 경과 일수 |
| `NumberOfDeviceRegistered` | 등록 기기 수 |
| `OrderCount` | 주문 횟수 |
| `SatisfactionScore` | 만족도 점수 |
| `Complain` | 불만 제기 여부 |
| `CityTier` | 거주 도시 등급 |
| `WarehouseToHome` | 창고-집 간 거리 |
| `HourSpendOnApp` | 앱 사용 시간 |

**SQL 기반 파생 리스크 플래그**

| 플래그 | 설명 | 이탈률 |
|--------|------|--------|
| `is_inactive` | 비활성 고객 | ~50% |
| `has_complaint` | 불만 제기 고객 | ~32% |
| `is_new` | 신규 고객 | ~17% |
| `low_orders` | 주문 수 적은 고객 | ~17% |
| `dissatisfied` | 불만족 고객 | ~12% |
| `no_cashback` | 캐시백 없는 고객 | ~0% |

---

## 모델 성능

| 모델 | AUC |
|------|-----|
| Logistic Regression | 0.845 |
| **Random Forest** | **0.958** |

**최종 선택 모델: Random Forest**

**혼동 행렬 (Random Forest)**

|  | 예측: Retained | 예측: Churned |
|--|----------------|---------------|
| 실제: Retained | 917 (TN) | 19 (FP) |
| 실제: Churned | 71 (FN) | 119 (TP) |

---

## 특성 중요도 Top 10 (Random Forest)

| 순위 | 변수 | 중요도 |
|------|------|--------|
| 1 | Tenure | ~37% |
| 2 | CashbackAmount | ~16% |
| 3 | DaySinceLastOrder | ~10% |
| 4 | NumberOfDeviceRegistered | ~7% |
| 5 | OrderCount | ~5% |
| 6 | has_complaint | ~5% |
| 7 | SatisfactionScore | ~5% |
| 8 | Complain | ~5% |
| 9 | CityTier | ~4% |
| 10 | dissatisfied | ~2% |

서비스 이용 기간(Tenure)이 이탈 예측에 가장 큰 영향을 미치며, 캐시백 금액과 마지막 주문 경과 일수가 그 뒤를 잇습니다.

---

## 리스크 세그먼트 분류

| 세그먼트 | 고객 수 | 실제 이탈률 |
|----------|---------|------------|
| Low Risk | ~900명 | ~5% |
| Medium Risk | ~150명 | ~52% |
| High Risk | ~80명 | ~97% |

고위험 고객 목록은 `outputs/high_risk_customers_sample.csv`에 저장됩니다.

---

## 주요 인사이트

- 이용 기간이 짧은 신규 고객의 이탈률이 높으며, Tenure가 이탈 예측의 핵심 변수
- 비활성 고객(is_inactive)의 이탈률이 50%로 가장 높은 리스크 플래그
- 캐시백 혜택이 고객 유지에 중요한 역할을 함 (고위험 고객의 CashbackAmount가 저위험 대비 낮음)
- 불만 제기 이력이 있는 고객의 이탈률이 평균 대비 약 2배 높음

---

## 기술 스택

- **언어**: Python 3.10
- **데이터 처리**: pandas, numpy
- **시각화**: matplotlib, seaborn
- **머신러닝**: scikit-learn (LogisticRegression, RandomForestClassifier)
- **데이터베이스**: SQL (SQLite 또는 MySQL)
