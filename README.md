# Telco Customer Churn Prediction — End-to-End ML Project

An end-to-end Machine Learning and MLOps project for predicting customer churn in a telecommunications company.

The project covers the complete workflow from data validation and preprocessing to model training, experiment tracking, API development, Docker containerization, automated testing, and CI/CD deployment to Docker Hub.

## Project Overview

Customer churn is a major challenge for telecommunications companies. This project uses historical customer information to predict whether a customer is likely to churn.

The machine learning model is exposed through both a REST API and an interactive web interface.

### Main Technologies

- Python
- Pandas & NumPy
- Scikit-learn
- XGBoost
- Optuna
- Great Expectations
- MLflow
- FastAPI
- Pydantic
- Gradio
- Pytest
- Docker
- GitHub Actions
- Docker Hub

## Machine Learning Workflow

The project follows this pipeline:

```text
Raw Telco Customer Data
        |
        v
Data Validation
(Great Expectations)
        |
        v
Data Preprocessing
        |
        v
Feature Engineering
        |
        v
Train / Test Split
        |
        v
Hyperparameter Optimization
(Optuna)
        |
        v
XGBoost Model Training
        |
        v
Model Evaluation
        |
        v
Experiment Tracking
(MLflow)
        |
        v
Model Packaging
        |
        v
FastAPI + Gradio
        |
        v
Docker Container
        |
        v
GitHub Actions CI/CD
        |
        v
Docker Hub
```

## Model

The project uses an **XGBoost Classifier** for customer churn prediction.

Optuna is used for hyperparameter optimization.

The trained model is tracked with MLflow and packaged with the files required for inference.

During the verified pipeline run, the model achieved approximately:

| Metric | Score |
|---|---:|
| Precision | 0.497 |
| Recall | 0.832 |
| F1 Score | 0.622 |
| ROC AUC | 0.837 |

The project places particular emphasis on recall to identify customers who may be at risk of churning.

## Project Structure

```text
Telco_Customer_Churn/
|
├── data/
│   ├── raw/
│   └── processed/
|
├── notebooks/
│   └── EDA.ipynb
|
├── scripts/
│   ├── check_pipeline_phase1_data_features.py
│   ├── check_pipeline_phase2_modeling.py
│   ├── prepare_processed_data.py
│   └── run_pipeline.py
|
├── src/
│   ├── app/
│   │   ├── app.py
│   │   └── main.py
│   │
│   ├── data/
│   │   ├── load_data.py
│   │   └── preprocess.py
│   │
│   ├── features/
│   │   └── build_features.py
│   │
│   ├── models/
│   │   ├── evaluate.py
│   │   ├── train.py
│   │   └── tune.py
│   │
│   ├── serving/
│   │   ├── inference.py
│   │   └── model/
│   │
│   └── utils/
│       ├── utils.py
│       └── validate_data.py
|
├── tests/
│   └── test_fastapi.py
|
├── .github/
│   └── workflows/
│       └── ci.yml
|
├── .dockerignore
├── .gitignore
├── Dockerfile
├── requirements.txt
└── README.md
```

## Installation

Clone the repository:

```bash
git clone https://github.com/Maik-25/Telco_Customer_Churn.git
cd Telco_Customer_Churn
```

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it in Git Bash on Windows:

```bash
source .venv/Scripts/activate
```

Install the project dependencies:

```bash
python -m pip install -r requirements.txt
```

## Run the ML Pipeline

Run the complete training pipeline with:

```bash
python scripts/run_pipeline.py --input data/raw/Telco-Customer-Churn.csv --target Churn
```

The pipeline performs data validation, preprocessing, feature engineering, model training, evaluation, and MLflow model logging.

## Run the Tests

Run:

```bash
python -m pytest -v
```

The FastAPI tests verify the health endpoint and prediction endpoint.

## Run the Application Locally

Start FastAPI using Uvicorn:

```bash
python -m uvicorn src.app.main:app --host 127.0.0.1 --port 8000
```

Then open:

```text
http://127.0.0.1:8000/
```

### Swagger API Documentation

```text
http://127.0.0.1:8000/docs
```

### Gradio Interface

```text
http://127.0.0.1:8000/ui
```

## API Endpoints

### Health Check

```http
GET /
```

Example response:

```json
{
  "status": "ok"
}
```

### Churn Prediction

```http
POST /predict
```

Example request:

```json
{
  "gender": "Female",
  "Partner": "No",
  "Dependents": "No",
  "PhoneService": "No",
  "MultipleLines": "No phone service",
  "InternetService": "DSL",
  "OnlineSecurity": "No",
  "OnlineBackup": "Yes",
  "DeviceProtection": "No",
  "TechSupport": "No",
  "StreamingTV": "No",
  "StreamingMovies": "No",
  "Contract": "Month-to-month",
  "PaperlessBilling": "Yes",
  "PaymentMethod": "Electronic check",
  "tenure": 1,
  "MonthlyCharges": 29.85,
  "TotalCharges": 29.85
}
```

Example response:

```json
{
  "prediction": "Likely to churn"
}
```

## Docker

Build the image locally:

```bash
docker build -t telco-churn-api .
```

Run the container:

```bash
docker run --name telco-churn-container -p 8000:8000 telco-churn-api
```

Then open:

```text
http://localhost:8000
```

## Docker Hub

A Docker image is published to Docker Hub through the GitHub Actions workflow.

Pull the latest image:

```bash
docker pull emkay25/telco_customer_churn:latest
```

Run the Docker Hub image:

```bash
docker run --name telco-churn-app -p 8000:8000 emkay25/telco_customer_churn:latest
```

The application will then be available at:

```text
http://localhost:8000
```

## CI/CD

The project uses **GitHub Actions** for Docker CI/CD.

When changes are pushed to the `main` branch, the workflow:

1. Checks out the repository
2. Sets up Docker Buildx
3. Authenticates with Docker Hub using GitHub repository secrets
4. Builds the Docker image
5. Pushes the latest image to Docker Hub

Docker Hub credentials are stored securely using:

```text
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
```

No Docker Hub credentials are stored directly in the source code.

## Data Validation

Great Expectations is used to validate the Telco customer dataset before model training.

The validation stage checks the expected dataset structure and data quality before allowing the training pipeline to continue.

## Experiment Tracking

MLflow is used for experiment tracking and model packaging.

The project records model training information and stores the trained model artifacts required for inference.

A stable packaged model is stored under:

```text
src/serving/model/
```

This allows the FastAPI application and Docker image to load the model consistently.

## Interactive Web Interface

Gradio provides a user-friendly interface where customer information can be entered without manually sending API requests.

The interface communicates with the same inference pipeline used by the FastAPI prediction endpoint.

## Deployment Status

The application has been successfully verified through:

```text
Local ML Pipeline
        ↓
FastAPI
        ↓
Gradio
        ↓
Pytest
        ↓
Docker
        ↓
GitHub
        ↓
GitHub Actions
        ↓
Docker Hub
        ↓
Docker Hub Image Runtime Test
```

Cloud deployment to AWS was intentionally not included because the project is designed to remain usable without requiring paid cloud infrastructure.

## Repository

GitHub:

https://github.com/Maik-25/Telco_Customer_Churn

Docker Hub:

https://hub.docker.com/r/emkay25/telco_customer_churn

## Author

**Abdullah Ilyas**

End-to-end Machine Learning / MLOps project demonstrating model development, validation, experiment tracking, API serving, containerization, automated testing, and CI/CD.