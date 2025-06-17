## #################### Setup Virtual Environment and install dependencies ###############

# Step 1: Create and activate virtual environment
python3.10 -m venv kfp-env
source kfp-env/bin/activate

# Step 2: Upgrade pip + required build tools
pip install --upgrade pip setuptools wheel

# Step 3: Pre-install Cython < 3.0 (build requirement for PyYAML 5.4.1)
pip install "Cython<3.0.0"

# Step 4: Build and install PyYAML==5.4.1 manually
pip install --no-build-isolation PyYAML==5.4.1

# Step 5: Install all remaining dependencies from pyproject.toml
pip install \
  kfp==1.8.1 \
  urllib3<2.0 \
  requests-toolbelt<1.0.0 \
  google-auth==1.35.0 \
  protobuf==3.20.3


# ################################################################################

# Ice Cream Price Prediction Pipeline with Kubeflow

This project demonstrates a modular, production-ready **Kubeflow Pipeline** that predicts ice cream prices based on temperature data using Scikit-learn. It includes data preprocessing, model training, evaluation, and optional deployment logic via KServe.

---

## Project Structure

```bash
.
├── components/
│   ├── preprocess.py     # Clean and prepare data
│   ├── train.py          # Train linear regression model
│   ├── evaluate.py       # Evaluate trained model
│   └── deploy.py         # (Optional) Deploy model with KServe
├── data/
│   └── ice_cream.csv     # Raw dataset for training
├── notebooks/
│   └── run_pipeline.ipynb  # Upload and trigger pipeline run
├── utils/
│   └── client.py         # Connect to Kubeflow Pipelines API
├── ml_pipeline.py        # Pipeline definition and compilation
├── ice_cream_pipeline.yaml # Compiled pipeline (auto-generated)
├── Dockerfile            # Base image for components
└── README.md             # Project documentation
```

---

## Pipeline Overview

### Steps:
1. **Preprocess**  
   Loads `ice_cream.csv`, cleans it, and saves the result to a shared volume.
2. **Train**  
   Trains a `LinearRegression` model on temperature vs price.
3. **Evaluate**  
   Calculates MAE, MSE, RMSE, R², and outputs `eval.json` for Kubeflow UI.
4. **(Optional) Deploy**  
   Placeholder for deploying the model using KServe.

---

## Requirements

- Python ">=3.10,<3.11"
- Docker
- Kubeflow Pipelines v1.8.1
- A running Kubeflow instance (locally or on a cluster)

---

## Getting Started

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd kubeflow_pipelines/icecream_pipeline
```

---

### 2. Build Docker image

You must build a shared Docker image for pipeline components:

```bash
docker build -t <image_name>:<tag> .
for example: 
  docker build -t sklearn:v1 .
```

Push to a registry if needed (e.g., DockerHub or your private registry):

```bash
for example:
  docker tag sklearn:v2 <your-repo>/sklearn:v2
  docker push <your-repo>/sklearn:v2
```

> Make sure Kubeflow can pull this image.

---

### 3. Compile the pipeline

This generates `ice_cream_pipeline.yaml`:

```bash
python ml_pipeline.py
```

---

### 4. Connect to Kubeflow Pipelines

Edit `utils/client.py` to match your Kubeflow endpoint:

```python
host_url = "http://<your-kubeflow-host>:<nodeport>"
```
---

### 5. Upload and Run the Pipeline

Run the notebook or script:

```python
from utils.client import get_kfp_client

client = get_kfp_client()

# Upload pipeline
client.upload_pipeline(
    pipeline_package_path="ice_cream_pipeline.yaml",
    pipeline_name="IceCreamPipeline"
)

# Run pipeline
client.create_run_from_pipeline_package(
    pipeline_file="ice_cream_pipeline.yaml",
    arguments={},  # You can pass test_size, random_state here if needed
    run_name="IceCream-Run"
)
```

---

## Input & Output

### Input: `data/ice_cream.csv`
A CSV file with at least two columns:
- `temp`: Temperature
- `price`: Ice cream price

### Output:
- `/data/icecream_clean.csv`: Cleaned data
- `/data/icecream_model.joblib`: Trained model
- `/data/eval.json`: Evaluation metrics

These are stored via `dsl.VolumeOp`.

---

## Visualizing Metrics

Kubeflow UI can automatically render metrics from the JSON format written in:

```python
/data/eval.json
```

Format:

```json
{
  "metrics": [
    {"name": "mae", "numberValue": 2.1},
    {"name": "rmse", "numberValue": 1.4},
    ...
  ]
}
```

---

## Optional: Deployment with KServe

You can enable `deploy.py` and add this step in `ml_pipeline.py`:

```python
deploy_step = deploy_op(model_input=train_step.outputs["model_output"]).after(evaluate_step)
```

This can be extended to generate and apply a KServe YAML.

---