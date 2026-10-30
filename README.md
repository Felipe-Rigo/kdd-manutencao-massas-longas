# Predictive Maintenance on a Pasta Production Line (KDD + Classification)

Notebook that applies the **KDD (Knowledge Discovery in Databases)** process to machine stoppage records from a food manufacturing line, built for my Production Engineering undergraduate thesis (UCS, 2025): *"Aplicação do processo KDD para estudo de processos produtivos na indústria alimentícia"*.

From the stoppage events, the notebook computes the **time until the next maintenance** and uses it to build two classification problems that support maintenance planning.

> **All data in this repository is synthetic.** It was randomly generated to demonstrate the code, with the same table structure as the original study. The real data belongs to a company and is not public. Metrics produced on this sample say nothing about the real process.

## What the notebook does

| Step | Description |
|---|---|
| **Preparation** | Reads stoppage events, flags maintenance events, computes the hours until the next maintenance and creates time features (hour, month, weekday) plus one-hot encoded categories |
| **5 time windows** | Predicts whether the next maintenance falls in 0-12h, 12-24h, 24-36h, 36-48h or >48h. Models: Multinomial Logistic Regression, Random Forest, XGBoost (class weights for imbalance) |
| **Binary risk (≤ 24h)** | Predicts whether maintenance will happen within 24 hours. Models: Logistic Regression and XGBoost, evaluated with confusion matrix, classification report and ROC-AUC |

All splits are 70/30, stratified, with `random_state=42`.

## Data format

`data/paradas_exemplo.xlsx`, sheet `Paradas`, one row per stoppage event:

| Column | Meaning |
|---|---|
| `ESTACAO` | Station that stopped |
| `DATA_OCORRENCIA_INICIAL` | Start date and time of the stoppage |
| `TOTAL_TEMPO_SEGUNDOS` | Duration in seconds |
| `ULTIMA_DESCRICAO` | Stoppage reason (the word "MANUTEN..." marks maintenance) |
| `TURNO` | Work shift |

## How to run

```bash
pip install -r requirements.txt
jupyter notebook manutencao_preditiva.ipynb
```

Run it from the repository folder so the relative path `data/paradas_exemplo.xlsx` resolves.

## Tech stack

Python · Pandas · NumPy · Matplotlib · scikit-learn · XGBoost
