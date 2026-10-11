# IRIS MLOps Pipeline - Week 1 (21f3003066)

End-to-end IRIS training and inference pipeline on Google Cloud, using a Vertex AI Workbench notebook and Google Cloud Storage (GCS).

## Files in this repo
| File / folder | Purpose |
|---|---|
| 21f3003066_MLOps_Assignment_1.ipynb | Full pipeline notebook: data prep, training, inference, two executions, and v1 vs v2 comparison, with outputs from the successful run |
| runs/<timestamp>/metrics.json | Training metrics for that run, copied from the GCS artifacts bucket |
| runs/<timestamp>/inference_metrics.json | Evaluation-set accuracy for that run |
| runs/<timestamp>/train.log | Training log for that run |
| .gitignore | Keeps datasets, splits and model binaries (*.csv, *.joblib) out of the repo |
| README.md | This file |

Model binaries (model.joblib), predictions and data splits are stored only in GCS, not in this repo.

## GCP setup
- GCP free trial activated and a Vertex AI Workbench instance created.
- APIs enabled: Cloud Storage, Compute Engine, Notebooks, and Vertex AI (now Gemini Enterprise Agent Platform).
- Data bucket: gs://mlops_w1_21f3003066_data_bucket (data/v1/, data/v2/)
- Artifacts bucket: gs://mlops_w1_21f3003066_artifacts_bucket (runs/<UTC timestamp>/)
- The Workbench service account was granted Storage Object Admin on both buckets.

## Pipeline steps
1. Data prep (Task 2): reads the raw IRIS CSV from data/<version>/, makes a stratified 60/40 train/eval split (random_state=42), and writes train.csv and eval.csv back to the same GCS folder.
2. Training (Task 3): fetches train.csv from GCS and trains a DecisionTreeClassifier(max_depth=3). It saves model.joblib, metrics.json and train.log to runs/<UTC timestamp>/ in the artifacts bucket.
3. Inference (Task 4): downloads the model for a run from GCS, predicts on eval.csv, and writes predictions.csv and inference_metrics.json into the same run folder.
4. Two executions (Task 5): training and inference were run twice on data v1, giving two timestamped run folders in the artifacts bucket.
5. Multiple data versions (Task 6, optional): the pipeline was run for v1 and v2, and the results compared.

<One or two sentences comparing v1 and v2. Mention that a very small eval set makes accuracy a weak basis for comparison.>

## Issues faced and fixes
- <Describe any error you hit and how you fixed it, for example a NumPy / scikit-learn version mismatch fixed by upgrading scikit-learn, or a 403 on GCS fixed by granting the service account Storage Object Admin on the buckets.>

## How to re-run
1. Open the notebook in a Workbench instance and set PROJECT_ID, DATA_BUCKET and ARTIFACT_BUCKET in the config cell.
2. Run the cells in order. Each execution creates a new runs/<timestamp>/ folder in the artifacts bucket.
