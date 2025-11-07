# 🌺 **MLOps Iris Classifier — End-to-End CI/CD Deployment (GitLab Edition)**

This repository demonstrates a **complete MLOps workflow** using the classic **Iris dataset**, progressing from data preprocessing and model training to full web deployment through an automated **CI/CD (Continuous Integration and Continuous Deployment)** pipeline built with **GitLab CI/CD** and deployed to **Google Cloud Platform (GCP)**.

<p align="center">
  <img src="img/flask/flask_app.gif" alt="Deployed Flask Iris Classifier Application" style="width:100%; height:auto;"/>
</p>

While the machine learning use case — **Iris species classification** — is intentionally simple, the project’s main objective is to showcase a **modern, production-grade MLOps workflow** using **GitLab pipelines** for automation, containerisation, and cloud deployment through **Google Kubernetes Engine (GKE)**.

## 🧩 **Project Overview**

This project guides the full lifecycle of a machine learning system — from raw data to live deployment — following a modular, reproducible, and production-aligned architecture.
Each stage builds upon the previous one, ensuring traceability, automation, and scalability.

### 🌱 **Stage 00 — Project Setup**

The foundation was established with a structured repository layout (`src/`, `pipeline/`, `artifacts/`, etc.), dependency management via **`uv`**, and environment setup for consistent development.

### 💾 **Stage 01 — Data Processing**

The **`data_processing.py`** module handled:

* Loading the Iris dataset
* Cleaning and handling outliers
* Splitting into training and test sets
* Saving processed artefacts for downstream use

This ensured that all preprocessing steps were fully reproducible and logged.

### 🧠 **Stage 02 — Model Training**

The **`model_training.py`** module trained a **Decision Tree Classifier**, evaluated performance (accuracy, precision, recall, F1), and saved both the model (`model.pkl`) and confusion matrix.
All actions were logged and wrapped with robust exception handling.

### 🌸 **Stage 03 — Flask Application**

A **Flask web app** was developed to serve the trained model through a user-friendly interface.
Users can input sepal and petal measurements and receive real-time species predictions.
This stage introduced:

* A responsive UI (`templates/index.html`)
* Clean styling (`static/style.css`)
* Live model inference served via `app.py`

### ⚙️ **Stage 04 — Training Pipeline**

The **`pipeline/training_pipeline.py`** module unified data processing and model training into a single automated script, ensuring consistent execution and end-to-end logging.
This stage introduced modular orchestration, preparing the groundwork for CI/CD integration.

### 🦊 **Stage 05 — GitLab Integration**

The project was linked to **GitLab** to manage source control and pipeline automation.
This involved:

* Creating a GitLab repository and configuring remotes alongside GitHub.
* Setting global Git identity and adding a secondary remote (`git remote add gitlab …`).
* Enabling seamless code pushes to both GitHub and GitLab.

Every commit pushed to GitLab triggers an automated pipeline, establishing the foundation for full CI/CD automation.

### ☁️ **Stage 06 — Google Cloud Platform (GCP) Setup**

The cloud infrastructure was provisioned within **Google Cloud Platform** to host containerised workloads and manage deployments via **GKE Autopilot**.

Key configuration steps included:

* Enabling essential APIs: Kubernetes Engine, Artifact Registry, Compute Engine, IAM, Cloud Build, and Cloud Storage.
* Creating an **Artifact Registry** repository (`mlops-iris-ii`) in `us-central1`.
* Setting up a **Service Account** with IAM permissions and generating a JSON key.
* Creating a **GKE Autopilot cluster** (`autopilot-cluster-1`) in the same region.

This environment provides a secure, scalable foundation for the CI/CD pipeline.

### 🚀 **Stage 07 — CI/CD Deployment (GitLab → GCP)**

Finally, the project was extended into a **CI/CD pipeline** using **GitLab CI/CD** to automate the build-and-deploy process.
Each push to GitLab (`git add`, `git commit`, `git push gitlab main`) automatically triggers:

1. **Build** — Construct a Docker image for the Flask app using the provided `Dockerfile`.
2. **Push** — Upload the image to **GCP Artifact Registry**.
3. **Deploy** — Apply `kubernetes-deployment.yaml` to the **GKE cluster** to update the live application.

The pipeline logic resides in **`.gitlab-ci.yml`**, which defines three stages — `checkout`, `build`, and `deploy`.
On completion, the application becomes publicly accessible via the external LoadBalancer endpoint provided by GKE.

## 💡 **Why GitLab CI/CD over CircleCI or Jenkins?**

This implementation uses **GitLab CI/CD** instead of CircleCI or Jenkins to demonstrate a tightly integrated, end-to-end DevOps experience.

### ✅ **Key Advantages of GitLab CI/CD**

* **Native integration** with repositories — pipelines trigger automatically on push.
* **Built-in container registry** and easy authentication with GCP Artifact Registry.
* **No external servers required** — everything runs on GitLab’s managed runners.
* **Environment variables** for securely storing service account keys and secrets.
* **Excellent GCP integration** — ideal for deploying to GKE Autopilot clusters.
* **Unified interface** for version control, pipelines, and deployments.

Together, these features make **GitLab CI/CD a powerful, all-in-one platform** for orchestrating MLOps workflows.

## 🗂️ **Final Project Structure**

```text
mlops_iris_classifier/
├── artifacts/                    # Data, processed artefacts, and model outputs
│   ├── raw/
│   ├── processed/
│   └── models/
├── pipeline/
│   └── training_pipeline.py       # Unified orchestration of data processing + training
├── src/
│   ├── data_processing.py
│   ├── model_training.py
│   ├── logger.py
│   └── custom_exception.py
├── templates/
│   └── index.html                 # Flask front-end UI
├── static/
│   ├── style.css
│   └── img/app_background.jpg
├── img/
│   ├── flask/flask_app.gif        # Animated Flask app demonstration
│   ├── cicd/                      # Screenshots for GitLab + GCP setup
│   └── gcp/
├── Dockerfile                     # Container image definition for Flask app
├── kubernetes-deployment.yaml     # Kubernetes Deployment + Service configuration
├── .gitlab-ci.yml                 # GitLab CI/CD pipeline definition
├── app.py                         # Flask application entry point
├── pyproject.toml                 # Project metadata
├── setup.py                       # Editable install support
└── requirements.txt               # Dependencies
```

## 🌐 **End-to-End Workflow Summary**

1. **Data Ingestion & Preprocessing** → clean, split, and save artefacts.
2. **Model Training** → fit, evaluate, and export model artefacts.
3. **Flask Deployment** → serve predictions through a local web interface.
4. **Pipeline Orchestration** → automate full data + training execution.
5. **GitLab Integration** → manage version control and trigger pipelines.
6. **GCP Setup** → provision registry, service account, and cluster.
7. **CI/CD Deployment** → build and deploy automatically to GKE.

## ✅ **In Summary**

This project transforms a simple Iris classification model into a **complete, automated MLOps system**.
It demonstrates how to **operationalise machine learning workflows** using **GitLab CI/CD** and **Google Cloud Platform**, covering every stage — from data to deployment — in a fully reproducible, production-ready pipeline.
