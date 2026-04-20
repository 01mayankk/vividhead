# VividHead Backend (Hugging Face Space)

FastAPI service that analyzes uploaded `.mp4` videos and maps head motion (pitch/yaw/roll) to ISL grammatical modifiers.

## API Endpoints

### `GET /health`
Returns service health.

### `GET /dataset-index`
Returns detected phrase class folders from `../dataset`.

### `POST /analyze`
Accepts multipart `file` (`.mp4 | .webm | .mov`) and optional `source_phrase` (string), then returns:
- `modifier`: `affirmation | negation | question_uncertainty | neutral | undetermined`
- `confidence`: confidence score in `[0,1]`
- `reason`: explanation text
- `fps`: measured source FPS
- `total_frames`: analyzed frame count
- `frames`: sampled frame-level pose and mesh points
- `source_phrase`: resolved phrase label used by frontend semantic summaries

## Local Development

```bash
cd backend
python -m venv venv
# Windows
venv\Scripts\activate
# macOS/Linux
# source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
uvicorn app:app --reload --host 0.0.0.0 --port 7860
```

## Evaluation Pipeline (Baseline First)

1. Generate a label template from dataset folders:

```bash
python evaluate_dataset.py --dataset-root ../dataset --labels-csv dataset_labels.csv --write-template
```

2. Fill `dataset_labels.csv` with expected labels per folder (`affirmation`, `negation`, `question_uncertainty`, `neutral`).

3. Run baseline evaluation and export full metrics:

```bash
python evaluate_dataset.py --dataset-root ../dataset --labels-csv dataset_labels.csv --output-json artifacts/baseline_metrics.json
```

This reports:
- accuracy
- precision (macro)
- recall (macro)
- F1 (macro and weighted)
- confusion matrix
- full classification report

## Hybrid Temporal Classifier (If Baseline Is Weak)

Train a lightweight calibrated temporal classifier on pitch/yaw/roll sequences:

```bash
python train_hybrid.py --dataset-root ../dataset --labels-csv dataset_labels.csv --output-dir artifacts/hybrid
```

Then evaluate with calibrated hybrid predictions:

```bash
python evaluate_dataset.py --dataset-root ../dataset --labels-csv dataset_labels.csv --use-hybrid --hybrid-model-dir artifacts/hybrid --output-json artifacts/hybrid_metrics.json
```

## Hugging Face Space Deployment

1. Push this `backend/` folder to your Docker Space repository.
2. Ensure `Dockerfile` and `requirements.txt` are committed.
3. Use port `7860`.
4. Backend URL target: <https://huggingface.co/spaces/01mayankk/computervision-backend>
