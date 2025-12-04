# ai-data-pipeline-sagemaker

## Developer: Andrew Kremmidas — AWS Solutions Architect Associate

AI Data Pipeline — SageMaker Model Training & Inference
End-to-End Machine Learning Workflow on AWS



A fully managed AI data pipeline that handles:
#1 Data ingestion
#2 Data preprocessing + feature engineering
#3 Model training and tuning (SageMaker)
#4 Model deployment with API inference
#5 MLOps automation + monitoring
This project demonstrates modern cloud-native machine learning engineering — great for enterprise ML workloads.

Real-World Architecture
Raw Data Source (CSV/JSON/API)
           │
           ▼
  S3 Data Lake (raw zone)
           │
   Trigger / Schedule
           ▼
    Lambda or SageMaker Processing
       (clean + feature engineering)
           │
           ▼
 S3 Data Lake (processed zone)
           │
           ▼
    SageMaker Training Job
           │
           ▼
    Model Registry + Artifacts
           │
           ├────────► Batch Inference (SageMaker)
           │
           ▼
   SageMaker Endpoint (Real-Time API)
           │
           ▼
     API Gateway + Cognito Auth

Features & Capabilities
Capability	Demonstrates
Automated ETL in cloud	Data engineering fundamentals
Managed model lifecycle	Production-ready ML
Infra-as-Code (future)	DevOps + reproducibility
Scalable endpoints	Global AI applications
Cloud-native observability	Logs, metrics, cost control
The goal: automated AI delivery pipeline — no training on laptops.

AWS Services Used
Amazon S3 — Data ingestion & lake storage
AWS Lambda — Trigger data preprocessing
Amazon SageMaker
Processing Jobs: Data cleaning
Training Jobs: Managed compute
Endpoints: Real-time inference serving
Model Registry: Versioning + rollbacks
Amazon CloudWatch — Monitoring model drift + latency
Amazon API Gateway — Public API for inference
Cognito (optional) — Authentication & access control



Training Script Example
(Python — packaged for SageMaker)
import argparse
import os
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
import joblib

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--data-dir", type=str)
    parser.add_argument("--model-dir", type=str)
    args = parser.parse_args()

    train = pd.read_csv(os.path.join(args.data_dir, "train.csv"))
    X = train.drop("label", axis=1).values
    y = train["label"].values

    model = RandomForestClassifier(
        n_estimators=200,
        max_depth=20
    )
    model.fit(X, y)

    joblib.dump(model, os.path.join(args.model_dir, "model.joblib"))
More scripts coming soon in /src directory.

Deployment Steps (High-Level)
Step	Action
1	Push raw dataset to S3
2	Run Processing Job to transform data
3	Execute Training Job → produce model artifact
4	Register model in SageMaker Model Registry
5	Deploy Model Endpoint
6	Query endpoint via API Gateway
I can generate CLI + CDK scripts for one-click automation.
🔍 Example Inference Request
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"data":[5.1, 3.5, 1.4, 0.2]}' \
  "$INVOKE_URL/predict"
Response:
{ "prediction": "Iris-setosa" }

Security Best Practices
✔ Private subnets and VPC endpoints
✔ IAM role separation of duties
✔ CloudWatch anomaly monitoring
✔ S3 + EBS encryption enabled
✔ Endpoint throttling + API auth

Repo Structure
project-3/
├─ src/
│  ├─ processing/
│  ├─ training/
│  ├─ inference/
├─ diagrams/
│  └─ sage-ai-pipeline.png
├─ pipeline-config/
└─ README.md

Future Enhancements
Hyperparameter Tuning (SageMaker HPO)
Canary or Blue/Green endpoint deployments
Model drift detection with SageMaker Monitor
EMR or Glue data engineering integration
CI/CD with CodePipeline (MLOps)

## Overall Project Goal
Create a production-grade AI pipeline that continuously improves using scalable, automated cloud services — the foundation of real ML businesses.
