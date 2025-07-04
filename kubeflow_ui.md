
### Slide 1: Kubeflow Pipelines UI – Key Components

 Pipelines
 Experiments
 Runs
 Recurring Runs
 Artifacts
 Executions

Each plays a vital role in managing end-to-end ML workflows. 
Below is a detailed explanation of each component: what it is, why it's important, and when you use it in the ML workflow.

---

### Slide 2: Pipelines

 What: Orchestrated ML workflows defined via Python SDK and compiled to YAML
 Why: Automate multi-step ML processes (e.g., data prep → training → evaluation)
 When: Used when standardizing and executing end-to-end ML workflows

---

### Slide 3: Experiments

 What: Logical grouping of multiple pipeline runs
 Why: Organizes pipeline executions by version, dataset, model type, etc.
 When: Use when comparing multiple pipeline versions or scenarios

---

### Slide 4: Runs

 What: One complete execution of a pipeline
 Why: Capture logs, outputs, metrics from each execution
 When: Triggered manually or programmatically; used for comparison and validation

---

### Slide 5: Recurring Runs

 What: Scheduled pipeline executions (e.g., hourly, daily)
 Why: Enables periodic retraining, data refresh, or model validation
 When: Ideal for automating workflows in a production environment

---

### Slide 6: Artifacts

 What: Intermediate and final outputs (models, metrics, datasets)
 Why: Enables tracking, sharing, and reproducibility
 When: Used during and after runs to inspect or reuse results

---

### Slide 7: Executions

 What: Backend execution units representing each pipeline step
 Why: Used for detailed inspection, debugging, and auditing
 When: Explore execution logs, inputs/outputs of each step in a run

---














<!-- ---

### Slide 1: Kubeflow Pipelines UI – Key Components

 Pipelines
 Experiments
 Runs
 Recurring Runs
 Artifacts
 Executions

Each plays a vital role in managing end-to-end ML workflows.

---

### Slide 2: Pipelines

## 1. Pipelines

### What:
A Pipeline is a defined workflow of ML tasks, like preprocessing, training, evaluation, and deployment. It's typically created using the Kubeflow SDK (`kfp`) and compiled into a `.yaml` file.

### Why:
It standardizes and automates the end-to-end ML workflow, enabling repeatability and scalability.

### When:
You create a Pipeline when you want to orchestrate multiple steps of an ML workflow and run them as one job — either once or on a schedule.

---

## 2. Experiments

### What:
An Experiment is a logical group or namespace for organizing multiple pipeline Runs. Think of it as a project folder that holds related executions.

### Why:
It helps track and organize experiments based on goals, versions, or ML problems.

### When:
Use it when testing variations of a pipeline (e.g., different models, parameters, datasets) to keep your workflow organized.

---

## 3. Runs

### What:
A Run is a single execution of a pipeline — it may be triggered manually or via a recurring schedule.

### Why:
Each run gives detailed insights: execution logs, outputs, performance metrics, and metadata. You can compare them to identify the best version.

### When:
A Run is created whenever you execute a pipeline, whether once or automatically (via Recurring Runs).

---

## 4. Recurring Runs

### What:
Recurring Runs are scheduled jobs that run a pipeline at defined intervals (e.g., daily, weekly, hourly).

### Why:
Useful for automating retraining, inference, or validation tasks on new data — following a CI/CD pattern in ML.

### When:
Set this up when you need periodic execution of pipelines — like monitoring a model's performance or retraining based on new data.

---

## 5. Artifacts

### What:
Artifacts are the intermediate or final outputs of pipeline steps — like datasets, models, metrics, and files.

### Why:
They are versioned, stored, and tracked to enable reproducibility, debugging, and sharing across pipeline steps.

### When:
Artifacts are created during each Run and are used to pass data between components or evaluate results later.

---

## 6. Executions
## What:
Executions represent the underlying runtime actions of pipeline steps — the actual Kubernetes pods or containers that executed a task.

### Why:
They provide deep-dive information: logs, inputs, outputs, and exact status of each component execution.

### When:
Use Executions to debug, optimize, or audit what exactly happened during a pipeline Run. -->
