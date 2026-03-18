데이터 분석 학습 및 실무 경험을 정리한 포트폴리오 저장소입니다.
NLP, 머신러닝, 통계 분석, SQL을 아우르는 다양한 도메인의 프로젝트를 포함합니다.

---

## Projects

| 프로젝트 | 설명 | 주요 기술 |
|----------|------|-----------|
| [Reddit](#reddit) | Reddit 게시글 NLP 분석 및 콘텐츠 추천 파이프라인 | Python, NLP, LDA |
| [Churn Prediction](#churn-prediction) | 이커머스 고객 이탈 예측 모델 | Python, scikit-learn |
| [NYC Taxi Trip](#nyc-taxi-trip) | NYC 택시 운행 시간 예측 및 EDA | R |
| [Urban Life Analysis](#urban-life-analysis) | 국내 7개 도시 생활 패턴 SQL 분석 | MySQL |

---

## Reddit

> Reddit 게시글 참여도 분석 및 세그먼트 기반 토픽 추천 시스템 (2025.01 – 2025.03)

- LDA 토픽 모델링 + Welch t-test + FDR 보정으로 **통계적으로 유의한 positive topic** 도출 (평균 효과 log_score = 0.35)
- KMeans 클러스터링으로 게시글을 **4개 세그먼트** 분류 (세그먼트별 평균 score 1.76 ~ 9.63)
- A/B 테스트 검증을 통과한 토픽만 필터링하여 세그먼트별 콘텐츠 추천

**Stack** `Python 3.10` `scikit-learn` `LDA` `scipy`

---

## Churn Prediction

> 이커머스 고객 이탈 여부를 예측하는 이진 분류 모델을 개발합니다. SQL 기반 리스크 플래그 생성과 고객 세그먼트 분류를 포함합니다.

- SQL 집계로 리스크 플래그 파생 변수 생성 (is_inactive, has_complaint, is_new 등)
- Logistic Regression vs Random Forest 비교 → **Random Forest 최종 선정 (AUC 0.958)**
- 고객을 Low / Medium / High Risk 3단계로 세분화

**Stack** `Python 3.10` `scikit-learn` `SQL`

---

## NYC Taxi Trip

> NYC 택시 운행 데이터를 탐색적으로 분석하고, 운행 시간을 예측하는 회귀 모델을 개발합니다.

- 피처 엔지니어링: Haversine 거리, 시간대, 주말 여부
- Linear Regression / Decision Tree / Random Forest 교차검증 비교
- 시간대·요일별 이용 패턴 및 맨해튼 승차 밀도 분석

**Stack** `R` `ggplot2` `randomForest` `caret`

---

## Urban Life Analysis

> MySQL 기반 관계형 DB를 설계하고, 국내 7개 도시의 이동·소비·날씨·이벤트·여가 데이터를 통합 분석합니다.

- 6개 테이블·외래 키 제약 조건을 포함한 DB 스키마 설계
- 1인당 이동량·소비액, 날씨와 이동량 관계, 이벤트-소비 패턴 분석
- 윈도우 함수(RANK, DENSE_RANK, SUM OVER)로 도시별 순위 및 누적 지표 산출

**Stack** `MySQL 8.0` `SQL`

---

## Tech Stack

| 분야 | 도구 |
|------|------|
| Language | Python 3.10, R, SQL |
| ML / Modeling | scikit-learn, randomForest, caret, rpart |
| Data Processing | pandas, numpy, data.table, lubridate |
| Visualization | matplotlib, seaborn, ggplot2 |
| Database | MySQL, SQLite |
| ETL | SQLAlchemy, pymysql |
