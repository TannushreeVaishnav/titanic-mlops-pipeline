# Titanic Survival Prediction — MLOps Project

<<<<<<< HEAD
> **End-to-end MLOps pipeline** featuring ETL with Apache Airflow, a Redis Feature Store, data drift detection, and real-time ML monitoring via Prometheus + Grafana.
=======
> **End-to-end MLOps pipeline** featuring ETL with Apache Airflow, a Redis Feature Store, data drift detection, and real-time ML monitoring via Prometheus + Grafana — built on the classic Titanic dataset.

---

## Note

Don't skip this just because of the dataset title. The **Titanic dataset is just a vehicle** — the real focus is the MLOps infrastructure built around it:

- Automated ETL pipelines with **Apache Airflow**
- Scalable **Feature Store** using Redis
- **Data Drift Detection** (Kolmogorov-Smirnov test)
- Real-time **ML Monitoring** with Prometheus metrics + Grafana dashboards
>>>>>>> f25522a91c6a4aecea540b7fca278574bcf26aec

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
<<<<<<< HEAD
├── artifacts/model/random_forest/model.pkl
├── data/titanic.csv
├── dags/titanic_etl_dag.py        # Airflow DAGs
├── src/feature_store.py           # RedisFeatureStore class
├── application.py                 # Main Flask app (drift + prediction + metrics)
├── docker-compose.yml             # PostgreSQL + Redis containers
=======
├── artifacts/
│   └── model/
│       └── random_forest/
│           └── model.pkl
├── data/
│   └── titanic.csv
├── dags/                          
│   └── titanic_etl_dag.py
├── notebooks/                     
├── src/
│   ├── feature_store.py           
│   ├── logger.py
│   └── ...
├── static/                        
├── templates/
│   └── index.html                 
├── application.py                 
├── docker-compose.yml             
├── requirements.txt
>>>>>>> f25522a91c6a4aecea540b7fca278574bcf26aec
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

<<<<<<< HEAD
## Screenshots

| Component | Preview |
|---|---|
| Prediction UI | ![Survived](<Screenshot 2026-05-09 095034.png>) ![Not Survived](<Screenshot 2026-05-09 091524.png>) |
| Grafana Dashboard | ![Metrics](<Screenshot 2026-05-09 112501.png>) ![Dash](<Screenshot 2026-05-09 104730.png>) |

---
*Acknowledgements: Titanic dataset from Kaggle; Open-source communities for scikit-learn, Flask, Redis, Airflow, Prometheus, and Grafana.*
=======
## Output
### Prediction UI — Survived
![Prediction UI-Survived](<Screenshot 2026-05-09 095034.png>)
### Prediction UI — Not Survived
![Not Survived](<Screenshot 2026-05-09 091524.png>)
Grafana Dashboard
![Grafana](<Screenshot 2026-05-09 112501.png>)  

---

## Acknowledgements

- Titanic dataset — Kaggle / public domain
- scikit-learn, Flask, Redis, Apache Airflow, Prometheus, Grafana open-source communities
>>>>>>> f25522a91c6a4aecea540b7fca278574bcf26aec
