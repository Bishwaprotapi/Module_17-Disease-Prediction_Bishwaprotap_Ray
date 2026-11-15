# Heart Disease Prediction (FastAPI + Web App + Docker)

A FastAPI service that serves a Random Forest classifier trained on the
[Heart Disease dataset](https://www.kaggle.com/datasets/johnsmith88/heart-disease-dataset),
with a web form (HTML/CSS/JS in `templates/`) and a REST API.

## Project layout
```
app/                 FastAPI app (main.py) + Pydantic schemas (schemas.py)
templates/           Web app (index.html - all HTML, CSS and JS)
model/               heart_model.joblib (trained model)
data/heart.csv       Dataset used by train.py
train.py             Trains and saves the model
Dockerfile, docker-compose.yml, requirements.txt
```

## URLs
| URL | What |
|---|---|
| `/` | Web app (prediction form) |
| `/docs` | Swagger UI |
| `GET /health` | Health check |
| `GET /info` | Model type + feature list |
| `POST /predict` | Returns `{"heart_disease": true/false, "probability": 0.0-1.0}` |

## How to run

### Option 1: Docker (recommended)
```
docker-compose build
docker-compose up
```
Open http://localhost:8000 (web app) or http://localhost:8000/docs (Swagger).
Stop with `Ctrl+C`, then `docker-compose down`.

### Option 2: Python 3.12 + .venv (Windows)
```
py -3.12 -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```
Linux/macOS: `python3.12 -m venv .venv && source .venv/bin/activate`, then the same pip/uvicorn commands.

### Retrain the model (optional)
```
python train.py
```
This rewrites `model/heart_model.joblib`. Keep the scikit-learn version in
`requirements.txt` the same as the one used for training.

## Test the API
```
curl http://localhost:8000/health
curl -X POST http://localhost:8000/predict -H "Content-Type: application/json" \
  -d '{"age":52,"sex":1,"cp":0,"trestbps":125,"chol":212,"fbs":0,"restecg":1,"thalach":168,"exang":0,"oldpeak":1.0,"slope":2,"ca":2,"thal":3}'
```

## Deploy on Render
1. Push this repo to GitHub.
2. Render -> New -> Web Service -> connect the repo.
3. Language: **Docker**, root directory/build context: repo root (the Dockerfile is there).
4. Deploy. The container listens on Render's `$PORT`.
5. Open the Render URL (web app at `/`, Swagger at `/docs`).

Live URL: https://module-17-disease-prediction.onrender.com (web app at `/`, Swagger at `/docs`)

> Educational demo only, not medical advice.
