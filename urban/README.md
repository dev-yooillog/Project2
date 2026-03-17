# Urban Life Analysis (SQL Analysis)

> 국내 주요 도시의 이동, 소비, 날씨, 이벤트, 여가 데이터를 통합한 도시 생활 패턴 SQL 분석 프로젝트

---

## 프로젝트 개요

MySQL 기반의 `urban_life` 데이터베이스를 설계하고, 서울·인천·부산 등 7개 도시의 이동량, 소비, 날씨, 이벤트, 여가 데이터를 분석합니다. 기본 집계부터 윈도우 함수, 서브쿼리, 다중 테이블 JOIN을 활용한 통합 KPI 산출까지 단계적으로 구성되어 있습니다.

---

## 데이터베이스 스키마

```
urban_life DB
├── cities              # 도시 기본 정보
├── mobility_logs       # 시간대별 교통수단별 이동 로그
├── consumption_logs    # 소비 카테고리별 결제 로그
├── weather_logs        # 날씨 및 기온 로그
├── city_events         # 도시 이벤트 정보
└── leisure_logs        # 여가 활동 참여 로그
```

**테이블 상세**

| 테이블 | 주요 컬럼 | 설명 |
|--------|-----------|------|
| `cities` | city_id, city_name, population, region | 7개 도시 기본 정보 |
| `mobility_logs` | city_id, base_date, hour, transport_type, movement_count | 시간대·교통수단별 이동량 |
| `consumption_logs` | city_id, base_date, category, spend_amount, transaction_count | 소비 카테고리별 금액 및 건수 |
| `weather_logs` | city_id, base_date, temperature, weather_type, precipitation | 날씨 및 강수 정보 |
| `city_events` | city_id, event_date, event_type, expected_visitors | 도시별 이벤트 및 예상 방문자 |
| `leisure_logs` | city_id, base_date, activity_type, participant_count | 여가 활동 유형별 참여 인원 |

**분석 대상 도시**

| 도시 | 인구 | 권역 |
|------|------|------|
| 서울 | 9,500,000 | 수도권 |
| 인천 | 3,000,000 | 수도권 |
| 부산 | 3,300,000 | 영남 |
| 대구 | 2,400,000 | 영남 |
| 광주 | 1,500,000 | 호남 |
| 대전 | 1,450,000 | 충청 |
| 울산 | 1,100,000 | 영남 |

---

## 파일 구조

```
.
├── 01_urban_life.sql     # DB/테이블 생성 및 샘플 데이터 삽입
├── 02_urban_query.sql    # 기본 분석 쿼리
├── 03_urban_sql.sql      # 심화 쿼리 (CASE, 윈도우 함수, 서브쿼리)
└── 04_urban_advanced.sql # 통합 KPI 및 주제별 고급 분석
```

---

## 파일 설명

### 01_urban_life.sql — DB 설계 및 데이터 삽입

- `urban_life` 데이터베이스 및 6개 테이블 생성
- 외래 키 제약 조건 및 인덱스 설정 (city_id + base_date 복합 인덱스)
- 7개 도시 기초 데이터 및 각 로그 테이블 샘플 데이터 삽입

### 02_urban_query.sql — 기본 분석 쿼리

| 분석 주제 | 쿼리 예시 |
|-----------|-----------|
| 도시/인구 | 지역별 도시 수, 수도권 도시 목록 |
| 이동 분석 | 도시별 총 이동량 및 순위, 출근 시간대 이동량 |
| 소비 분석 | 도시별 총 소비액, 카테고리별 평균 결제 금액 |
| 날씨 분석 | 날씨 유형별 평균 기온, 강수 도시 목록 |
| 이벤트/여가 | 누적 방문자 수, 여가 참여 인원 |
| 비교 분석 | 수도권 vs 비수도권 이동량, 1인당 소비 금액 |

### 03_urban_sql.sql — 심화 쿼리

- `CASE WHEN`으로 시간대 구분 (Morning / Evening / Other), 강수 플래그, 고/저소비 도시 분류
- `RANK()`, `DENSE_RANK()` 윈도우 함수로 도시별 이동량·소비 순위 산출
- `PARTITION BY`로 교통수단별, 도시 내 카테고리별 세부 순위
- 서브쿼리로 평균 이상 도시 필터링
- `LEFT JOIN`으로 이벤트 없는 도시, 비 오는 날 소비 미존재 도시 탐지
- 데이터 검증: 이동량 0 이하 이상치 확인, 로그 테이블 보유 여부 체크

### 04_urban_advanced.sql — 통합 KPI 및 고급 분석

| 분석 카테고리 | 주요 쿼리 |
|---------------|-----------|
| 이동 분석 | 인구 대비 이동량, 교통수단별 비중, 시간대별 이동량 |
| 소비 분석 | 트랜잭션당 평균 금액, 도시 내 카테고리 순위 |
| 날씨 영향 | 날씨 유형별 평균 이동량 (날씨 × 이동량 교차 분석) |
| 이벤트 분석 | 이벤트 날짜와 가장 가까운 소비 기록 매칭 |
| 통합 KPI | 도시별 1인당 이동량·소비액·여가 유형 수·이벤트 수 종합 |
| 시간 분석 | 주간 이동량 집계, 주말/평일 소비 비교 |

---

## 주요 인사이트

- 서울이 이동량, 소비액 모두 압도적 1위 (인구 규모 반영)
- 교통수단 중 지하철과 버스가 이동량의 대부분을 차지
- 강수 여부와 소비 패턴 간 관계 분석 가능 (광주 - 비 오는 날 소비 미기록)
- 이벤트 예상 방문자 10만 이상은 Tech Expo(서울), Beach Festival(부산) 2건

---

## 실행 방법

```sql
-- 1단계: DB 및 테이블 생성, 샘플 데이터 삽입
source 01_urban_life.sql;

-- 2단계: 기본 분석 쿼리
source 02_urban_query.sql;

-- 3단계: 심화 쿼리
source 03_urban_sql.sql;

-- 4단계: 통합 KPI 분석
source 04_urban_advanced.sql;
```

---

## 기술 스택

- **데이터베이스**: MySQL 8.0 이상
- **주요 SQL 기법**: JOIN, 서브쿼리, CTE, 윈도우 함수(RANK / DENSE_RANK / SUM OVER), CASE WHEN, HAVING
