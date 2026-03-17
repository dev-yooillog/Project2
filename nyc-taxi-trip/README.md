# NYC Taxi Trip Duration Analysis

> NYC 택시 이용 데이터를 기반으로 운행 시간 예측 모델 개발 및 이용 패턴 분석

---

## 프로젝트 개요

뉴욕시 택시 운행 데이터를 탐색적으로 분석하고, 운행 시간(trip_duration)을 예측하는 회귀 모델을 개발합니다. 시간대·요일·거리 등 피처 엔지니어링을 수행하고 Linear Regression, Decision Tree, Random Forest 세 모델을 교차검증으로 비교합니다.

---

## 데이터

| 항목 | 내용 |
|------|------|
| 파일 | `data/train.csv` |
| 출처 | Kaggle - NYC Taxi Trip Duration |
| 필터링 조건 | 승객 수 1~6명, 운행 시간 36,000초 이하 |

**주요 컬럼**

| 컬럼 | 설명 |
|------|------|
| `pickup_datetime` | 승차 일시 |
| `pickup_latitude` / `pickup_longitude` | 승차 위치 |
| `dropoff_latitude` / `dropoff_longitude` | 하차 위치 |
| `passenger_count` | 승객 수 |
| `trip_duration` | 운행 시간 (초, 타겟 변수) |

---

## 파일 구조

```
.
├── visualization_analysis.Rmd              # EDA 및 시각화 분석
├── visualization_analysis.html            # EDA 리포트 (렌더링 결과)
├── nyc_taxi_trip_duration_analysis.Rmd    # 심화 분석 및 모델링
├── nyc_taxi_trip_duration_analysis.html   # 심화 분석 리포트 (렌더링 결과)
└── data/
    └── train.csv
```

---

## 분석 구성

### visualization_analysis.Rmd — EDA 및 시각화

**피처 엔지니어링**

| 파생 변수 | 설명 |
|-----------|------|
| `hour` | 승차 시각 (0~23) |
| `is_weekend` | 주말 여부 |
| `day_of_week` | 요일 |
| `distance_km` | Haversine 공식으로 계산한 직선 거리 (km) |
| `speed_kmh` | 평균 속도 (km/h) |

**시각화 분석 항목**

- 시간대별 평균 운행 시간 (오후 2~3시 최장, 새벽 2~5시 최단)
- 주중/주말 비교 (평일이 전반적으로 더 길고, 출퇴근 시간대에 차이 뚜렷)
- 요일별 평균 운행 시간
- 거리-운행 시간 산점도 및 상관관계
- 속도 분포
- 승객 수별 평균 운행 시간
- 승차 위치 밀도 히트맵 (맨해튼 중심부 집중)

---

### nyc_taxi_trip_duration_analysis.Rmd — 심화 분석 및 모델링

**추가 피처**

| 파생 변수 | 설명 |
|-----------|------|
| `distance_bin` | 거리 구간 (0-2km / 2-5km / 5-10km / 10-20km / 20km+) |
| `duration_bin` | 운행 시간 구간 (0-5분 / 5-10분 / 10-15분 / 15-20분 / 20-30분 / 30분+) |
| `duration_zscore` / `distance_zscore` | Z-score 기반 이상치 탐지 |

**분석 항목**

- 이상치 탐지 (|Z| > 3 기준)
- 상관관계 히트맵
- 거리 구간별 평균 운행 시간 및 속도
- 운행 시간 구간별 평균 거리 및 주말 비율

**모델링**

| 모델 | 교차검증 |
|------|---------|
| Linear Regression | 5-fold CV |
| Decision Tree (rpart) | 5-fold CV, CP 튜닝 |
| Random Forest | 3-fold CV (속도 최적화) |

- 타겟 변수: `log1p(trip_duration)`
- 사용 피처: `distance_km`, `hour`, `is_weekend`, `passenger_count`

**모델 평가**

- CV RMSE 기준 최적 모델 선정
- 모델별 특성 중요도 비교 (정규화)
- 잔차 분석 (Residuals vs Fitted, 잔차 분포, Q-Q Plot)
- Predicted vs Actual 시각화

---

## 주요 인사이트

- `distance_km`이 모든 모델에서 운행 시간 예측에 가장 중요한 변수
- 오후 2~3시에 평균 운행 시간이 가장 길고, 새벽 시간대에 가장 짧음
- 평일이 주말보다 운행 시간이 길며, 출퇴근 시간대에 차이가 두드러짐
- 승차 위치는 맨해튼 중심부에 집중

---

## 실행 방법

```r
# 필요 패키지 설치
install.packages(c(
  "data.table", "lubridate", "ggplot2",
  "rpart", "randomForest", "caret",
  "scales", "reshape2", "gridExtra"
))

# R Markdown 렌더링
rmarkdown::render("visualization_analysis.Rmd")
rmarkdown::render("nyc_taxi_trip_duration_analysis.Rmd")
```

---

## 기술 스택

- **언어**: R
- **데이터 처리**: data.table, lubridate
- **시각화**: ggplot2, gridExtra, scales
- **머신러닝**: rpart, randomForest, caret
