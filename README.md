# DOG API CASE STUDY README

Project ID: sally-pyne-2026

Repository Link: [Insert your GitHub URL here]

## TASK 1: ENVIRONMENT & REPOSITORY

1. Project Initialization

GCP Setup: Created project sally-pyne-2026 and enabled core APIs (BigQuery, Cloud Storage, IAM, and Cloud Scheduler).

Dependency Management: Utilized uv for a high-performance, reproducible Python environment.

Compatibility: Pinned the project to Python 3.12 to ensure stable serialization for dbt-core and its dependencies.

2. Infrastructure & Security (Least-Privilege)

Service Account: Provisioned data-pipeline-runner@sally-pyne-2026.iam.gserviceaccount.com.

Granular RBAC: Assigned the following roles to satisfy security requirements:

roles/storage.objectAdmin: To manage the GCS staging area.

roles/bigquery.dataEditor: To materialize tables in the Bronze and Analytics layers.

roles/bigquery.jobUser: To execute compute workloads.

## TASK 2: DATA INGESTION (DLT)

1. Ingestion Pipeline

Source: The Dog API (Breeds endpoint).

Tooling: Implemented via dlt for automated schema evolution and normalization.

Storage Partitioning: Configured a custom filesystem layout to ensure raw JSON data is partitioned in GCS by run date: gs://sally-pyne-raw-data/bronze/{table_name}/{YYYY}/{MM}/{DD}/

Target: Data is loaded into the bronze dataset in BigQuery.

2. Orchestration Design

Scheduler: Configured to run daily at 02:00 UTC.

Logic: Designed as a Cloud Run Job triggered by Cloud Scheduler to minimize idle compute costs.

TASK 3: DATA TRANSFORMATION (DBT)

1. Modeling Strategy

Staging Layer: Flattens nested JSON structures and casts metrics (weight, height) to numeric types.

Analytics Layer: (In Progress) Calculating breed-specific metrics and health indicators for downstream reporting.

BOOTSTRAP INSTRUCTIONS

1. Clone the Repo:
git clone [your-repo-url]
cd my-dlt-project

2. Install Dependencies:
uv sync

3. Authentication:
Place your gcp-key.json in the root directory (this file is git-ignored).
export GOOGLE_APPLICATION_CREDENTIALS="gcp-key.json"

4. Run Ingestion:
uv run python dog_pipeline.py