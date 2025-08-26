# SurfCast Analytics

## 1. Project Vision & Introduction

SurfCast Analytics is a data engineering and machine learning project designed to analyze and predict historical surf quality. The core objective is to use publicly available weather and oceanographic data to build a reliable surf quality score for various surf spots around the world.

This project serves two main purposes:
1.  **To create a valuable tool for surfers**, allowing them to explore the detailed historical conditions of a surf spot without relying on fragmented or non-existent reports.
2.  **To serve as a practical, real-world project** for honing skills in data engineering, MLOps, cloud architecture, and machine learning.

The project is just starting, and this repository documents its evolution from a research prototype to a production-grade system.

---

## 2. Current Status: Sprint 1 Complete

This initial phase of the project, **Sprint 1: Research & Modeling**, is now complete. The entire sprint was conducted within a Jupyter Notebook (`sprint_1_analise.ipynb`) to rapidly test our core hypothesis and validate the feasibility of the project.

### 2.1. Sprint 1 Methodology

The research phase involved a methodical, iterative process:

1.  **Data Collection:** We extracted historical data from two primary sources for our pilot location (Praia Mole, Florianópolis):
    *   **Open-Meteo API:** Provided our core feature set, including detailed wave, swell, wind, and weather data.
    *   **Surfline:** Provided our ground truth target variable (surf quality rating) and data for cross-validation.

2.  **Data Auditing & Remediation:** A rigorous audit revealed significant gaps and data corruption issues. We successfully developed a targeted extraction pipeline to find and fill missing data, and a surgical replacement process to correct corrupted wind data, resulting in a complete, trustworthy dataset.

3.  **Exploratory Data Analysis (EDA):** We performed critical validation by correlating Open-Meteo features against Surfline's data. This proved that Open-Meteo's wave data was a strong proxy (**0.73 correlation**) and its wind data was a useful proxy (**0.51 correlation**).

4.  **Feature Engineering & Preparation:** We engineered cyclical time-based features (hour, day, month) and integrated tides data. Critically, we identified and removed features that would cause data leakage to ensure the model's validity.

5.  **Baseline Modeling:** We trained an XGBoost Classifier to predict the Surfline rating using only the Open-Meteo and tides data.

### 2.2. Key Findings from Sprint 1

*   **Strong Baseline Model:** The model achieved **69.1% accuracy**, a very strong result that validates the predictive power of our chosen feature set.
*   **Identified Key Drivers:** The model's feature importance revealed the most critical factors for surf quality at this spot:
    1.  **Local Wind Wave Height:** The single most important factor.
    2.  **Swell Period:** The power of the incoming swell.
    3.  **Swell Direction:** The angle of the incoming swell.

---

## 3. Repository Structure (End of Sprint 1)

*   `01_feature_exploration.ipynb`: The main Jupyter Notebook containing all the research, cleaning, and modeling code for this sprint.
*   `landing_zone/`: Directory containing all the raw, cleaned, and complete CSV data files.
*   `modelo_xgboost.pkl`: The serialized, trained baseline XGBoost model.
*   `README.md`: This file.
*   `.gitignore`: Specifies files and directories to be excluded from version control.

---

## 4. Project Roadmap: The Path Forward

With the research phase successfully concluded, the project now transitions to engineering and productionalization.

### Phase 1: Local Production Architecture (Sprint 2)
Build an automated, production-grade data pipeline for a single spot.
-   **Orchestration:** Implement **Apache Airflow**.
-   **Data Lake:** Use **MinIO** for S3-compatible local object storage.
-   **Transformation:** Integrate **dbt** for robust, SQL-based data transformation.

### Phase 2: Scalability & Feature Enrichment
Expand the system's capabilities to handle multiple spots.
-   **Automated Spot Onboarding:** Develop logic to automatically define a spot's ideal wind conditions (offshore, etc.).
-   **Frontend Delivery:** Create a web interface for users to select a spot and visualize its historical data.

### Phase 3: Advanced Modeling
Continuously improve the model's intelligence.
-   **Feature Engineering:** Introduce **lag features** to capture time-series dynamics.
-   **Model Iteration:** Experiment with different algorithms and hyperparameter tuning.

### Phase 4: Cloud Migration & MLOps
Move the local architecture to a scalable and cost-effective cloud environment.
-   **Cloud Infrastructure:** Migrate to **AWS** (S3, MWAA, etc.) with a focus on cost optimization.
-   **MLOps:** Implement data quality monitoring, model performance tracking, and automated reprocessing strategies.