# 🛡️ CyberShield AI

**Explainable AI for Cyber Threat Detection**  
A Streamlit dashboard for detecting malware, phishing, and network intrusions with SHAP + LIME explanations, severity scoring, and downloadable PDF investigation reports.

## 🚀 Features
- Network intrusion, malware, and phishing detection
- Threat classification with severity scores
- SHAP + LIME explainability
- Plain‑English AI narratives
- PDF investigation reports
- REST API integration

## 📂 Project Structure
- `app.py` → Streamlit dashboard
- `src/` → Core modules (training, prediction, explainability, reporting)
- `data/` → Training datasets
- `reports/` → Generated investigation reports

## ⚙️ Installation
```bash
git clone https://github.com/pavithra-l/CyberShield-AI.git
cd CyberShield-AI
pip install -r requirements.txt
streamlit run app.py

# 🛡️ CyberShield AI

**Explainable AI-Powered Cybersecurity Platform for Threat Detection**

CyberShield AI ingests network / malware / phishing telemetry, classifies threats
with tree-based ML models (RandomForest + XGBoost), scores severity, and
explains every prediction using **SHAP** and **LIME** — including a plain-English
narrative and a downloadable PDF investigation report.

---

## Overview

- Detects **Network Intrusions, Malware, and Phishing** events
- Trains and compares **RandomForest** vs **XGBoost**, keeps the best model
- Explains predictions with **SHAP** (global + local) and **LIME** (local)
- Produces **plain-English** AI explanations
- **Streamlit** dashboard + **FastAPI** REST API
- Exports **PDF investigation reports**
- Modular, logged, tested

## Architecture

```
                +--------------------+
   CSV upload -->|  Preprocessing    |
                |  (impute+scale+OHE)|
                +---------+----------+
                          v
                +--------------------+
                |  Training Pipeline |  RF vs XGBoost -> best
                +---------+----------+
                          v
      +---------+---------+---------+---------+
      |         |         |         |         |
      v         v         v         v         v
   Predict   SHAP/LIME  Severity  FastAPI   Streamlit
   (batch)   Explain    Scoring   REST API  Dashboard
                          |
                          v
                     PDF Report
```

## Folder Structure

```
CyberShield-AI/
├── data/            # raw datasets (CSV)
├── models/          # persisted model + preprocessor + metadata
├── reports/         # generated PDFs and diagnostic plots
├── logs/            # cybershield.log
├── notebooks/       # exploration
├── src/
│   ├── preprocessing.py
│   ├── feature_engineering.py
│   ├── train_model.py
│   ├── evaluate_model.py
│   ├── explainability.py
│   ├── predict.py
│   ├── report_generator.py
│   ├── api.py
│   ├── config.py
│   └── utils.py
├── tests/           # pytest unit tests
├── app.py           # Streamlit entry point
├── requirements.txt
├── README.md
└── .gitignore
```

## Installation

Requires **Python 3.12**.

```bash
git clone <your-repo-url> CyberShield-AI
cd CyberShield-AI

python3.12 -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt
```

## Usage

### 1. Train a model

Place a CSV (NSL-KDD / UNSW-NB15 / CICIDS2017 / generic with `label` column) into `data/`, then:

```bash
python -m src.train_model --data data/your_dataset.csv
```

Artifacts land in `models/`. Diagnostic plots (confusion matrix, ROC) land in `reports/`.

### 2. Launch the Streamlit dashboard

```bash
streamlit run app.py
```

Pages: Overview · Train Model · Predict · Explainability · Model Performance.

### 3. Start the FastAPI service

```bash
uvicorn src.api:app --reload --port 8000
```

Interactive docs at `http://localhost:8000/docs`.

## API Documentation

| Method | Path              | Purpose                              |
|--------|-------------------|--------------------------------------|
| GET    | `/health`         | Liveness probe                       |
| GET    | `/model`          | Best model, classes, metrics         |
| POST   | `/predict`        | Score a single JSON record           |
| POST   | `/predict/batch`  | Score a JSON array of records        |
| POST   | `/predict/csv`    | Upload a CSV, get scored rows back   |

Example:

```bash
curl -X POST http://localhost:8000/predict \
  -H "Content-Type: application/json" \
  -d '{"record": {"duration": 0.1, "src_bytes": 512, "dst_bytes": 0, "protocol_type": "tcp"}}'
```

## Testing

```bash
pytest -q
```

## Screenshots

_Placeholders — capture from your local run_

- `docs/screenshot_dashboard.png`
- `docs/screenshot_explainability.png`
- `docs/screenshot_report.png`

## Future Improvements

- Streaming inference from Kafka / syslog
- Deep learning models (Autoencoders, Transformers on flow sequences)
- Active learning loop with analyst feedback
- Threat-intel enrichment (MISP, VirusTotal)
- Role-based access control on the API

## License

MIT

