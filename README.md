# Hyper-Local-AI-Weather-Nowcasting-Urban-Flood-Warning-System

## Data Engineering & MLOps Integration – From Raw API to Real‑Time Alert

This project is **not just a machine learning model** – it is a **fully automated, production‑grade data pipeline** that integrates modern **Data Engineering** with **MLOps** practices on AWS.

### Data Engineering Pipeline (ETL)

- **Extract** – Every hour, an AWS Lambda function triggered by Amazon EventBridge fetches live meteorological data from the Open‑Meteo API across **5 spatial nodes** (Khlong Nueng + 4 surrounding provinces). A separate request retrieves **real‑time soil moisture** (3‑layer profile: 0–1cm, 1–3cm, 3–9cm).
- **Transform** – The Lambda performs **in‑memory feature engineering**:
  - Trigonometric transformation of wind direction (Wind_X, Wind_Y) to eliminate cyclical discontinuity.
  - Spatial interpolation to handle missing node data.
  - Rolling window aggregations (3‑hour, 12‑hour cumulative rainfall).
  - Isolation of soil moisture as a separate variable for the 4‑Tier logic.
- **Load** – The cleaned feature vector is written to **Amazon DynamoDB** (low‑latency feature store), while raw JSON payloads are archived in **Amazon S3** (data lake for future retraining).

### MLOps & Real‑Time Inference

- **Model Deployment** – The XGBoost model is serialized and stored in **S3 Model Registry**. Because XGBoost dependencies exceed the standard Lambda size limit, the prediction environment is **containerized with Docker** and hosted on **Amazon ECR**. AWS Lambda pulls this image at runtime – a true serverless MLOps pattern.
- **Inference** – Another Lambda function fetches the latest feature row from DynamoDB, runs the prediction, and evaluates the **4‑Tier Adaptive Alert Logic** (cross‑referencing predicted rainfall with real‑time soil moisture). If a threshold is breached, it instantly sends a **LINE alert** to citizens.
- **CI/CD (GitHub Actions)** – Every code push automatically builds the Docker image, pushes it to ECR, and updates the Lambda function. This eliminates manual deployment errors and ensures **reproducibility**.
- **Concept Drift Monitoring** – A third Lambda runs daily, calculates the MAE over the last 7 days (delayed ground truth), and alerts the engineering team via LINE if the error exceeds 2.0 mm – enabling **automated retraining triggers**.

### 📊 Frontend & Observability

- **Serverless Dashboard** – The prediction results and soil moisture are exposed via **Amazon API Gateway** (with CORS) to a lightweight **static web dashboard** hosted on **Amazon S3**. Anyone can view real‑time flood risk without any backend infrastructure.
- **System Observability** – **Amazon CloudWatch** aggregates Lambda logs, tracks error rates, and triggers **SNS email alerts** to the engineering team on pipeline failures (e.g., API outage, timeout). This ensures **high reliability** for a life‑saving system.

### Why This Matters (Data Engineering + MLOps)

> *“This project transforms raw, messy, multi‑node weather telemetry into an automated, bias‑free flood warning system – all within a **<$0.50/month** budget. It is a textbook example of how **Data Engineering** (ETL, feature stores, spatial interpolation) and **MLOps** (CI/CD, containerization, concept drift monitoring) can be combined to solve a real‑world disaster management problem.”*

**Key integration points:**
- Data Engineering provides **clean, aligned, real‑time features** (DynamoDB) → MLOps consumes them for **low‑latency inference**.
- S3 acts as both **data lake** (raw API logs) and **model registry** (versioned .pkl files).
- EventBridge orchestrates **hourly ETL + daily monitoring** – no cron servers needed.
- The **4‑Tier Alert Logic** is a **deterministic rule engine** that bridges ML predictions and physical ground conditions – a hybrid AI+engineering approach.

---

### What You Can Learn From This Codebase

If you are a data engineer or ML engineer, studying this repository will show you:

- How to build a **serverless ETL pipeline** on AWS without expensive managed services (Glue, SageMaker).
- How to **containerize an ML model** for Lambda (bypassing 250MB limit).
- How to implement **CI/CD for ML** using GitHub Actions + ECR.
- How to create a **low‑cost feature store** with DynamoDB.
- How to **monitor concept drift** in production using delayed ground truth.
- How to design a **real‑time alerting system** (LINE API) that separates user‑facing alerts from system alerts (SNS).


## Overview

**Problem:** Traditional flood warning systems rely on human decision-making and coarse 11×11 km weather grids, leading to delayed evacuations and false alarms (e.g., Hat Yai disaster).

**Solution:** A fully automated, bias‑free early warning system combining:
- XGBoost regression with spatial‑lag features (“Cross‑Provincial Radar”)
- A novel **4‑Tier Adaptive Alert Logic** that cross‑references rainfall predictions with real‑time soil moisture
- Serverless MLOps pipeline on AWS (EventBridge, Lambda, DynamoDB, S3, API Gateway)

**Results:**
- RMSE **0.8331 mm**, MAE **0.2355 mm** (outperforms ARIMA & Random Forest)
- Operational cost **< $0.50/month** (within AWS Learner Lab $50 limit)
- End‑to‑end latency under 5 minutes

---



## Key Features

| Feature | Description |
|---------|-------------|
| **Spatial Lag Matrix** | 5‑node grid (Khlong Nueng + 4 surrounding provinces) tracks storm trajectories. Nonthaburi west node ranked #1 feature. |
| **Trigonometric Wind Transformation** | Converts 0‑360° wind direction into linear vectors (Wind_X, Wind_Y) to eliminate algorithmic discontinuity. |
| **4‑Tier Adaptive Alert Logic** | Dynamically triggers alerts using rainfall predictions + real‑time soil moisture. Prevents false alarms and alert fatigue. |
| **Serverless MLOps** | AWS EventBridge (scheduler), Lambda (ingestion/prediction/monitoring), DynamoDB (feature store), S3 (data lake + model registry + static web). |
| **CI/CD Pipeline** | GitHub Actions builds Docker image → pushes to Amazon ECR → updates Lambda. Fully automated deployment. |
| **Real‑time Alerts** | LINE Messaging API delivers tier‑based flood warnings (RED/ORANGE) directly to citizens. |
| **Concept Drift Monitoring** | Daily MAE evaluation (threshold >2.0 mm) triggers automatic retraining alerts to engineering team. |

---
## AI Use Declaration
During the development of this project, I used AI tools for:

Language translation and sentence refinement

Code suggestions, debugging, and structural guidance

Writing assistance for the report and this README

Brainstorming and conceptual support

However, all core model design, data collection, feature engineering, result interpretation, and final technical decisions were made by the author (Paradorn Khanongsuwan). All AI‑generated outputs have been manually verified and adapted by the author.

This declaration follows the academic integrity guidelines of the Asian Institute of Technology (AIT).

---


##  Acknowledgements
Open‑Meteo API – free historical and real‑time weather data

AWS Learner Lab – cloud computing credits for academic projects

Asian Institute of Technology (AIT) – faculty and staff support
