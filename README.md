# Data Analysis Portfolio

A collection of end-to-end data projects covering ETL pipelines, SQL analysis, machine learning, and statistical modeling.

---

## Projects

| Project | Description | Stack |
|---------|-------------|-------|
| [Reddit](#reddit) | Reddit post/comment trend analysis | Python, SQL |
| [Churn Prediction](#churn-prediction) | E-commerce customer churn classification model | Python, SQL, scikit-learn |
| [NYC Taxi Trip](#nyc-taxi-trip) | NYC taxi trip duration prediction and EDA | R |
| [Urban Life Analysis](#urban-life-analysis) | Multi-city urban pattern analysis (mobility, consumption, weather) | MySQL |

---

## Reddit

> Reddit post and comment data analysis — trend extraction and pattern exploration.

**Key highlights**
- (Add brief description of what this project does)

**Tech stack**: Python 3.10, SQL

---

## Churn Prediction

> Binary classification model to predict e-commerce customer churn, with SQL-based risk flag engineering and customer segmentation.

**Key highlights**
- Built risk flags (is_inactive, has_complaint, is_new, etc.) using SQL aggregation
- Compared Logistic Regression vs Random Forest; selected Random Forest (AUC 0.958)
- Segmented customers into Low / Medium / High risk groups
- Identified top churn drivers: Tenure, CashbackAmount, DaySinceLastOrder

**Tech stack**: Python 3.10, scikit-learn, SQL

---

## NYC Taxi Trip

> Exploratory data analysis and trip duration regression modeling on NYC taxi data.

**Key highlights**
- Feature engineering: Haversine distance, hour-of-day, weekend flag
- Compared Linear Regression, Decision Tree, and Random Forest with cross-validation
- Analyzed temporal patterns (peak hours, weekday vs weekend) and geographic density
- `distance_km` ranked as the most important feature across all models

**Tech stack**: R, ggplot2, randomForest, caret

---

## Urban Life Analysis

> SQL-based analysis of mobility, consumption, weather, events, and leisure across 7 Korean cities.

**Key highlights**
- Designed a relational DB schema with 6 tables and foreign key constraints
- Analyzed per-capita mobility and spending, weather impact on movement, event effects on consumption
- Applied window functions (RANK, DENSE_RANK, SUM OVER) for city-level rankings and cumulative metrics
- Built integrated KPI queries combining all data sources

**Tech stack**: MySQL 8.0, SQL (Window Functions, Subqueries, Multi-table JOIN)

---

## Tech Stack Overview

| Category | Tools |
|----------|-------|
| Languages | Python 3.10, R, SQL |
| ML / Modeling | scikit-learn, randomForest, caret, rpart |
| Data Processing | pandas, numpy, data.table, lubridate |
| Visualization | matplotlib, seaborn, ggplot2 |
| Database | MySQL, SQLite |
| ETL | SQLAlchemy, pymysql |
