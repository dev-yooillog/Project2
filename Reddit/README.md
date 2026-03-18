# Reddit Post Analysis

> Reddit 게시글 참여도 분석 및 세그먼트 기반 토픽 추천 시스템

---

## 프로젝트 개요

Reddit 게시글 데이터를 분석하여 높은 참여도를 보이는 토픽을 식별하고, 게시글 세그먼트별 최적 토픽을 추천하는 시스템을 구축합니다. 통계적 검정, 클러스터링, 추천 기법을 결합하여 Reddit 콘텐츠 전략을 개선할 수 있도록 설계했습니다.

- **기간**: 2025년 1월 – 2025년 3월

---

## 배경 및 문제 정의

Reddit에는 다양한 토픽과 게시글이 존재하지만, 어떤 토픽이 높은 사용자 참여를 유도하는지 명확하지 않았습니다. 단순 점수나 조회수 기반 분석만으로는 토픽 성과 평가와 세그먼트별 추천이 어려워 데이터 기반으로 해결하고자 시작했습니다.

---

## 분석 파이프라인

```
01_overview.ipynb              데이터 구조 및 기본 현황 파악
        ↓
02_cleaning_preprocessing      결측치 처리, 텍스트 정제, 날짜 파싱
        ↓
03_EDA.ipynb                   탐색적 데이터 분석 및 분포 시각화
        ↓
04_NLP_wordcloud.ipynb         워드클라우드 기반 주요 키워드 시각화
        ↓
05_NLP_topic_modeling.ipynb    LDA 토픽 모델링 및 토픽 할당
        ↓
06_feature_engineering.ipynb   시간/텍스트/토픽 피처 생성
        ↓
07_ab_Test.ipynb               토픽별 게시글 성과 A/B 테스트
        ↓
08_segmentation.ipynb          KMeans 게시글 세그먼트 분류
        ↓
09_recommendation.ipynb        세그먼트 × 토픽 × A/B 결과 기반 추천
```

---

## 주요 분석 내용

### 피처 엔지니어링 (06)

| 피처 | 설명 |
|------|------|
| `hour` / `dayofweek` / `is_weekend` | 게시 시각 기반 시간 피처 |
| `title_length` / `selftext_length` / `total_text_length` | 텍스트 길이 피처 |
| `log_score` / `log_text_length` | 로그 변환 피처 |
| `topic_*` | 토픽 원-핫 인코딩 더미 변수 |

### A/B 테스트 (07)
- 각 토픽과 나머지 그룹을 Welch t-test로 비교
- FDR 다중검정 보정으로 통계적으로 유의한 positive topic 도출
- **positive topic 평균 효과(log_score) = 0.35**

### 세그먼트화 (08)
- topic, log_score, 텍스트 길이, 게시 시간, 주말 여부 기반 KMeans 클러스터링
- Elbow + Silhouette 검토 후 **최적 K = 4개 세그먼트** 선정
- 세그먼트별 평균 score: **1.76 ~ 9.63**

### 추천 시스템 (09)
- 세그먼트별 평균 score 최고 토픽 선정 및 대표 게시글 Top 3 추출
- A/B 테스트 positive topic만 필터링하여 추천 강화

---

## 파일 구조

```
.
├── notebooks/
│   ├── 01_overview.ipynb
│   ├── 02_cleaning_preprocessing.ipynb
│   ├── 03_EDA.ipynb
│   ├── 04_NLP_wordcloud.ipynb
│   ├── 05_NLP_topic_modeling.ipynb
│   ├── 06_feature_engineering.ipynb
│   ├── 07_ab_Test.ipynb
│   ├── 08_segmentation.ipynb
│   └── 09_recommendation.ipynb
├── data/
│   ├── the-reddit-dataset-dataset-posts.csv
│   ├── the-reddit-dataset-dataset-comments.csv
│   └── cleaned_decor.csv
└── output/                         # 분석 결과 (자동 생성)
    ├── posts_cleaned.csv
    ├── posts_with_topics.csv
    ├── posts_feature_engineered.csv
    ├── posts_segmented.csv
    ├── ab_test_results.csv
    └── recommendations_final.csv
```

---

## 기술 스택

- **언어**: Python 3.10
- **데이터 처리**: pandas, numpy
- **NLP**: scikit-learn (TF-IDF, LDA), wordcloud
- **클러스터링**: scikit-learn (KMeans, Silhouette)
- **통계 검정**: scipy (Welch t-test, FDR 보정)
- **시각화**: matplotlib, seaborn
