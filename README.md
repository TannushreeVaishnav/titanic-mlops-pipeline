# Titanic Survival Prediction — MLOps Project

> **End-to-end MLOps pipeline** featuring ETL with Apache Airflow, a Redis Feature Store, data drift detection, and real-time ML monitoring via Prometheus + Grafana.

---

## Architecture Overview

```text
Raw CSV → Apache Airflow (ETL) → PostgreSQL → psycopg3 → Redis Feature Store → Model Training (Random Forest)
                                                                                  ↓
Grafana ← Prometheus Metrics ← Flask App (localhost:5000) ← KS Drift Detection ← Prediction Endpoint
```

---

## Project Structure

```text
titanic-mlops/
├── artifacts/model/random_forest/model.pkl
├── data/titanic.csv
├── dags/titanic_etl_dag.py        # Airflow DAGs
├── src/feature_store.py           # RedisFeatureStore class
├── application.py                 # Main Flask app (drift + prediction + metrics)
├── docker-compose.yml             # PostgreSQL + Redis containers
└── README.md
```

---

## Workflow & Pipeline

1. **Database Setup**: PostgreSQL runs via Docker. Inspect data using DBeaver.
2. **ETL Pipeline**: Airflow DAG extracts `titanic.csv`, transforms features, and loads them into PostgreSQL.
3. **Data Ingestion**: Python uses `psycopg3` to pull records from Postgres.
4. **Feature Store (Redis)**: Engineered features are stored in Redis (retrieved via `RedisFeatureStore`).
5. **Model Training**: A Random Forest classifier is trained and versioned using **DVC**.
6. **Model Serving**: Served via Flask at `http://localhost:5000`. Includes UI for predictions and categorical encoding guide.
7. **Drift Detection**: Incoming prediction features are scaled (`StandardScaler`) and compared against historical data using the **Kolmogorov-Smirnov test**. P-values < 0.05 trigger drift logging.
8. **Monitoring**: Prometheus scrapes `/metrics` (prediction, drift, and outcome counts). Grafana visualizes this on live dashboards.

---

## Getting Started

1. **Clone & Install**:
   ```bash
   git clone https://github.com/<your-username>/titanic-mlops.git
   cd titanic-mlops
   pip install -r requirements.txt
   ```
2. **Start Infrastructure**: `docker-compose up -d`
3. **Run ETL**: Trigger `titanic_etl_dag` from Airflow UI.
4. **Populate Feature Store**: `python src/feature_store.py`
5. **Train Model**: `python src/train.py`
6. **Run App**: `python application.py`
7. **Set Up Grafana**: Add Prometheus source (`http://localhost:9090`) and import `monitoring/grafana_dashboard.json`.

---

## Tech Stack

- **Frameworks/Languages**: Python 3.9+, Flask, scikit-learn
- **Data & Feature Store**: PostgreSQL, Redis, DBeaver, psycopg3
- **MLOps Tools**: Apache Airflow, DVC, Docker, SciPy (KS test), Prometheus, Grafana

---

## Screenshots

| Component | Preview |
|---|---|
| Prediction UI | ![Survived](<Screenshot 2026-05-09 095034.png>) ![Not Survived](<Screenshot 2026-05-09 091524.png>) |
| Grafana Dashboard | ![Metrics](<Screenshot 2026-05-09 112501.png>) ![Dash](<Screenshot 2026-05-09 104730.png>) |

---
*Acknowledgements: Titanic dataset from Kaggle; Open-source communities for scikit-learn, Flask, Redis, Airflow, Prometheus, and Grafana.*