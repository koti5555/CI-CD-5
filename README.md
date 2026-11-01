# Student Result Prediction — Continuous Delivery (Practical 5)

This repository implements Practical 5: Continuous Delivery of an ML Prediction Application using GitHub Actions.

## Workflow

A push or pull request targeting `main`, or a manual workflow dispatch, runs these stages:

1. Install Python dependencies.
2. Train the student result prediction model.
3. Enforce the minimum model accuracy quality gate (0.85).
4. Run seven ML pipeline tests.
5. Run four Flask prediction API tests.
6. Prepare a release candidate containing the Flask application, trained model, metrics, dependencies, and deployment instructions.
7. Upload the `student-result-ml-release` Actions artifact.

Production deployment is intentionally not performed. The workflow prepares a validated release candidate that is ready for a separate deployment step.

## Project files

- `train_model.py` — creates the sample dataset, trains a Logistic Regression model, and saves the model and metrics.
- `quality_gate.py` — fails the workflow if accuracy is below 0.85.
- `test_ml_pipeline.py` — seven ML pipeline tests.
- `app.py` — Flask health endpoint (`GET /`) and prediction endpoint (`POST /predict`).
- `test_app.py` — four application tests.
- `.github/workflows/ml-ci.yml` — complete CI/CD workflow.
- `requirements.txt` — project dependencies.

## Prediction request

Send a JSON request to `POST /predict` with all four model features:

```json
{
  "attendance": 90,
  "internal_marks": 85,
  "assignment_marks": 88,
  "previous_score": 80
}
```

The response contains a `PASS` or `FAIL` prediction and its numeric code.

## Verify the release

Open **Actions → Student Result ML CI-CD**, select a successful run, and download the `student-result-ml-release` artifact. It contains:

```text
release_candidate/
├── app.py
├── student_result_model.pkl
├── metrics.json
├── requirements.txt
└── DEPLOYMENT.txt
```

All training, tests, and packaging run on GitHub-hosted runners. Local Python or Git installation is not required for the CI/CD practical.
