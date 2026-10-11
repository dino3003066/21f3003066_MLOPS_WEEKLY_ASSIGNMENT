# Iris MLOps Pipeline (Vertex AI Workbench + GCS)

Trains a scikit-learn Decision Tree on the Iris dataset, with data, models, logs and predictions stored in Google Cloud Storage. Each training run gets its own `runs/<UTC timestamp>/` folder.

- `mlops_wk1_21f3003066.ipynb`: runs the full pipeline (data split and upload, training, inference, two runs, data-version comparison). Set `DATA_BUCKET` in the setup cell, then run all cells.
- `train.py` / `infer.py`: training and separate inference scripts, also written by the notebook.
- `data/`: Iris dataset files (add other versions here for the optional Task 6).
