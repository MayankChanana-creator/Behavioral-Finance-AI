# Behavioral-Finance-AI
An AI-powered Behavioral Finance tool that analyzes mutual fund NAV trends to predict investor behavior using machine learning and generate psychological explanations with LLMs.
# 🧠 MutualMind — Investor Behaviour Analyser

> **AI-powered mutual fund behaviour analysis tool that predicts investor psychology using real-time NAV data, machine learning, and a local LLM.**

---

## 📌 Overview

**MutualMind** fetches live mutual fund data from the MFAPI, computes key financial metrics over a user-defined holding period, and uses a trained **Random Forest Classifier** to predict investor behaviour patterns such as *Panic Selling*, *Nervous Holding*, *Greedy Overconfidence*, and more.

It then passes those predictions to a **local LLM (via Ollama)** to generate human-readable behavioural explanations — without ever giving financial advice.

---

## ✨ Features

- 📈 **Live NAV Fetching** — Pulls real-time mutual fund data from [mfapi.in](https://api.mfapi.in/mf)
- 🤖 **ML Behaviour Prediction** — Random Forest model trained on 2,000 simulated investor scenarios
- 🧬 **5 Investor Behaviour Labels** — Panic Sell · Nervous Hold · Satisfied Hold · Greedy/Overconfident · Neutral
- 💬 **LLM-Powered Explanations** — Uses a local `phi3` model via Ollama to explain *why* a behaviour pattern emerged
- 📊 **Financial Metrics Engine** — Computes volatility, NAV slope, drawdown triggers, and reaction time
- 🔁 **Auto Model Persistence** — Trains and saves the model on first run; reuses it on subsequent runs via `joblib`

---

## 🏗️ Architecture

```
User Input (Fund + Holding Days)
        │
        ▼
  Live NAV API (mfapi.in)
        │
        ▼
  Feature Engineering
  ┌─────────────────────────────┐
  │  loss_percent               │
  │  volatility (std of returns)│
  │  nav_slope                  │
  │  holding_days               │
  │  reaction_time              │
  └─────────────────────────────┘
        │
        ▼
  Random Forest Classifier
        │
        ▼
  Predicted Behaviour Label
        │
        ▼
  Local LLM (Ollama / phi3)
        │
        ▼
  Human-readable Explanation
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.9+
- [Ollama](https://ollama.ai) installed and running locally
- `phi3` model pulled via Ollama

```bash
ollama pull phi3
ollama serve
```

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/mutualmind.git
cd mutualmind

# Install dependencies
pip install -r requirements.txt
```


### Run

```bash
python model.py
```

You will be prompted to:
1. **Select a fund** — enter the index number from the displayed list
2. **Enter holding period** — number of days you've (or would have) held the fund

---

## 📦 Dependencies

| Package | Purpose |
|---|---|
| `pandas` | Data manipulation and NAV processing |
| `numpy` | Numerical computation and simulation |
| `scikit-learn` | Random Forest model training and prediction |
| `requests` | Fetching live mutual fund data |
| `joblib` | Model serialization and loading |

Install all at once:

```bash
pip install pandas numpy scikit-learn requests joblib python-dotenv
```

---

## 🧠 Behaviour Labels

| Label | Trigger Condition |
|---|---|
| 🔴 **Panic Sell** | Loss > 5% AND reaction time < 2 days |
| 🟠 **Nervous Hold** | Any loss AND negative NAV slope |
| 🟢 **Satisfied Hold** | Positive returns, upward trend, low volatility |
| 🟡 **Greedy / Overconfident** | Gains > 5% |
| ⚪ **Neutral** | No dominant signal |

---

## 📁 Project Structure

```
Behavioral-Finance-AI/
├── model.py                  # Entry point
├── behaviour_model.pkl      # Saved ML model (auto-generated on first run)         
├── requirements.txt
└── README.md
```

---

## 🔮 How the ML Model Works

On **first run**, the model:
1. Generates 2,000 synthetic investor scenarios with randomised financial metrics
2. Labels each scenario using rule-based heuristics (mimicking real investor behaviour)
3. Trains a **Random Forest Classifier** (150 estimators, max depth 6)
4. Saves the model to `behaviour_model.pkl`

On **subsequent runs**, the saved model is loaded directly — no retraining needed.

---

## 🛡️ Disclaimer

> This tool is built for **educational and behavioural research purposes only**.  
> It does **not** provide financial advice, investment recommendations, or trading signals.  
> Always consult a SEBI-registered financial advisor before making investment decisions.

---

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you'd like to change.

---

## 👤 Author

Mayank Chanana
[LinkedIn](https://www.linkedin.com/in/mayank-chanana-4556a9341/) · [GitHub](https://github.com/MayankChanana-creator)

---

*Built with Python · scikit-learn · Ollama · MFAPI*
