

---

```markdown
# 📈 StockAlpha – Explainable Stock Signals (ML + LLM)

<p align="center">
  API-driven stock movement signals using the Alpha Vantage API, machine learning, and GenAI explanations, visualized in an interactive Streamlit dashboard.
</p>

<p align="center">
  <img alt="Python" src="https://img.shields.io/badge/Python-3.13%2B-blue?logo=python&logoColor=white">
  <img alt="scikit-learn" src="https://img.shields.io/badge/scikit--learn-ML%20models-orange?logo=scikitlearn&logoColor=white">
  <img alt="NumPy" src="https://img.shields.io/badge/NumPy-numerical%20computing-informational?logo=numpy&logoColor=white">
  <img alt="Streamlit" src="https://img.shields.io/badge/Streamlit-interactive%20dashboard-ff4b4b?logo=streamlit&logoColor=white">
  <img alt="GenAI LLM" src="https://img.shields.io/badge/GenAI-LLM%20explanations-8A2BE2">
  <img alt="Editor" src="https://img.shields.io/badge/Editor-VS%20Code-007ACC?logo=visualstudiocode&logoColor=white">
  <img alt="License" src="https://img.shields.io/badge/License-MIT-green">
</p>

> 📚 Educational prototype built for a graduate **Finance Information Processing** course.  
> ⚠️ Not investment advice – built for educational and demonstrative purposes only.

---

## 📌 Table of Contents

1. [Problem Statement](#1-problem-statement)  
2. [High-Level Solution](#2-high-level-solution)  
3. [Tech Stack](#3-tech-stack)  
4. [Project Structure](#4-project-structure)  
5. [Features](#5-features)  
6. [Setup & Usage](#6-setup--usage)  
7. [Limitations & Extensions](#7-limitations--extensions)  
8. [Project Status](#8-status)  

---

## 1. Problem Statement

Given recent daily OHLCV data for large-cap U.S. stocks, predict whether **tomorrow’s closing price will be higher than today’s** (UP vs NOT UP), and provide a **plain-English explanation** of the model’s prediction.

**Constraints:**

- Use real-world data from a financial API (no synthetic/Kaggle datasets).
- Provide a working, documented, and demoable pipeline.
- Ensure the prediction is interpretable by non-technical users.

---

## 2. High-Level Solution

1. **Data Ingestion:**  
   Use Alpha Vantage’s **Time Series (Daily Adjusted)** API to collect stock data.

2. **Feature Engineering:**  
   Generate technical features:
   - Moving Averages (short/long)
   - Lagged Returns
   - Rolling Volatility

3. **Model Training:**  
   Evaluate multiple models:
   - Logistic Regression  
   - Random Forest  
   - XGBoost  
   Select based on **directional accuracy** on a test set.

4. **Explanation Layer (LLM):**  
   Use an LLM to generate **natural-language rationales** from model outputs.

5. **Dashboard (Streamlit):**  
   Interactive UI to explore predictions, explanations, charts, and rankings.

---

## 3. Tech Stack

### 📦 Languages & Runtime
- Python 3.13  
- Conda environment: `finance_stock_env`

### 🔧 Libraries & Tools
- **Data:** `pandas`, `numpy`  
- **ML:** `scikit-learn`, `xgboost`  
- **API Calls:** `requests`  
- **Dashboard:** `streamlit`  
- **Environment Management:** `python-dotenv`  
- **LLM Integration:** `openai` or compatible endpoint

### 🌐 External Services
- **Alpha Vantage:** Stock market data
- **OpenAI or Proxy Endpoint:** LLM explanations (with fallback support)

---

## 4. Project Structure

```

.
├── app/
│   └── dashboard.py            # Streamlit dashboard
├── src/
│   ├── config.py               # Symbols, paths, env vars
│   ├── fetch_data.py           # Alpha Vantage API integration
│   ├── features.py             # Feature engineering
│   ├── models.py               # ML models & training
│   ├── llm_explain.py          # LLM explanations
│   └── **init**.py
├── data/
│   ├── raw/                    # Raw API data (ignored by Git)
│   └── processed/              # Engineered features (ignored by Git)
├── demo_prediction.py          # CLI demo for one prediction
├── .env.example                # Template for API keys
├── .gitignore
└── README.md

````

> 🔐 `.env` and `data/` folders are ignored in version control for security.

---

## 5. Features

### 5.1 🧠 Stock Signal Modeling

- **Universe:** Large-cap U.S. equities: `AAPL`, `MSFT`, `AMZN`, `GOOGL`, `META`
- **Features:**
  - `short_ma`, `long_ma`: Moving averages
  - `return`, `ret_lag1`, `ret_lag2`: Lagged returns
  - `short_vol`, `long_vol`: Rolling volatility
- **Target:**
  - `up_next_day = 1` if next close > today’s close; else `0`
- **Models:**
  - Logistic Regression  
  - Random Forest  
  - XGBoost  
- **Metric:**  
  - **Directional accuracy** (typically 53–55%)

---

### 5.2 💬 Explainable AI (LLM)

- **LLM Input:**
  - Symbol, date, prediction, key feature values
- **Explanation Covers:**
  - Trend (MA crossover), momentum/reversion, volatility, and disclaimers
- **Fallback:**
  - If LLM is unavailable, a **rule-based explanation** is used

---

### 5.3 📊 Streamlit Dashboard

- **Inputs:**
  - Symbol and date selectors
- **Outputs:**
  - UP probability and prediction
  - LLM explanation
  - Feature snapshot
  - **Price chart** (adjusted close)
  - **Recent prediction accuracy** (30 days)
  - **Cross-symbol signal ranking**

---

## 6. Setup & Usage

### 6.1 📥 Clone the Repository

```bash
git clone https://github.com/ritwikm14/stockalpha-explainable-ml-llm.git
cd stockalpha-explainable-ml-llm
````

### 6.2 🛠️ Create and Activate Environment

```bash
conda create -n finance_stock_env python=3.13 -y
conda activate finance_stock_env
```

Install required dependencies:

```bash
pip install pandas numpy scikit-learn xgboost streamlit python-dotenv requests openai
```

### 6.3 🔐 Configure API Keys

Copy the example environment file and add your credentials:

```bash
cp .env.example .env
```

Edit `.env`:

```text
ALPHA_VANTAGE_API_KEY=your_alpha_vantage_key
OPENAI_API_KEY=your_openai_key
OPENAI_BASE_URL=optional_custom_base_url
```

### 6.4 ⚙️ Run Pipeline (Fetch → Features → Models)

```bash
python -m src.fetch_data
python -m src.features
python -m src.models
```

### 6.5 🖥️ Run CLI Demo (Optional)

```bash
python demo_prediction.py
```

### 6.6 🚀 Launch the Dashboard

```bash
streamlit run app/dashboard.py
```

Open `http://localhost:8501` in your browser.

---

## 7. Limitations & Extensions

### ❗ Current Limitations

* Small symbol set and limited history
* Only OHLCV-based technical indicators
* No transaction costs or portfolio simulation

### 🌱 Possible Extensions

* Add fundamentals, macroeconomic data, or sentiment
* Expand universe and history
* Use more advanced time-series models (e.g., LSTM)
* Deploy via **FastAPI + production frontend**
* Integrate RAG-based news summarization

---

## 8. Status

* ✅ Full pipeline operational: API → Features → Model → Dashboard
* ✅ LLM explanations integrated with fallbacks
* ✅ Suitable for **FinTech / Quant / ML / GenAI** portfolio showcase

---

