# Hyper-Local-AI-Weather-Nowcasting-Urban-Flood-Warning-System

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

## 🏗️ Architecture

![Architecture](docs/images/architecture.png)

*Placeholder: Insert your architecture diagram (e.g., using draw.io). Show EventBridge → Lambda Ingestion → S3/DynamoDB → Lambda Prediction → API Gateway → Dashboard / LINE.*

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
