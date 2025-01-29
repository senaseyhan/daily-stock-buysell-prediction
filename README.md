# Daily Stock Buy-Sell Prediction (End-to-End ETL & ML Project)

This repository presents a comprehensive pipeline for **daily stock buy-sell predictions** on selected Nasdaq stocks and indices. Developed for a *Computational Data Science* course project, it integrates:

- **Data Extraction** from Yahoo Finance with `yfinance`
- **ETL Processes** using PySpark
- **Relational Storage** in PostgreSQL
- **Technical Analysis & Feature Engineering** (RSI, MACD, Bollinger Bands, etc.)
- **LSTM Modeling** for next-day price forecasting
- **Strategy Backtesting** (Buy-Sell-Hold) vs. a simple Buy & Hold approach

---

## Table of Contents

1. [Project Description](#project-description)  
2. [Project Structure](#project-structure)  
3. [Prerequisites](#prerequisites)  
4. [Installation & Setup](#installation--setup)  
5. [Usage](#usage)  
6. [Exploratory Data Analysis & Modeling](#exploratory-data-analysis--modeling)  
7. [Results](#results)  
8. [Additional Data](#additional-data)  
9. [References](#references)

---

## Project Description

The main objective is to build an **automated pipeline** that fetches historical stock data, transforms it, and loads it into a database. We then train a **deep learning (LSTM)** model to predict future stock prices (focusing on next-day price). Finally, we **simulate** a trading strategy that issues daily **BUY**, **SELL**, or **HOLD** signals based on predicted prices, comparing performance with a **Buy & Hold** baseline.

Key steps:

1. **Data Extraction**  
   - Pull data from Yahoo Finance (`yfinance`) for multiple tickers (stocks, indices, macroeconomic indicators).
2. **Data Transformation**  
   - Use **PySpark** to clean, standardize, and structure data into consistent schemas.
3. **Data Loading**  
   - Store processed data in **PostgreSQL** tables for easy querying and versioning.
4. **EDA & Feature Engineering**  
   - Visualize trends, compute technical indicators, and assess correlations.
5. **Modeling**  
   - Train an **LSTM** neural network to predict prices, performing hyperparameter tuning to optimize performance.
6. **Strategy & Backtesting**  
   - Implement daily buy-sell decisions with transaction fees, calculate net profit, ROI, and more.  
   - Compare strategy outcomes against a basic buy-and-hold investment.

---

## Project Structure

```
project-root/
├── data_loading/
│   ├── extract.py           # Pull data from yfinance
│   ├── transformation.py    # Clean & transform with PySpark
│   ├── load.py              # Insert data into PostgreSQL
│   └── main.py              # Orchestrates the ETL pipeline
├── datas(csv)/              # Contains CSV files with historical data (backup or offline use)
├── project.ipynb            # Jupyter Notebook for EDA, modeling, and backtesting
├── requirements.txt         # List of Python dependencies (optional)
├── README.md                # This README
└── ...
```

**Folders & Files**  
- **data_loading/**: Python scripts for ETL tasks (Extract, Transform, Load).  
- **datas(csv)/**: CSV files if you prefer to use local data instead of API requests.  
- **project.ipynb**: The main notebook containing Exploratory Data Analysis, Feature Engineering, LSTM training, and final backtest steps.

---

## Prerequisites

- **Python** >= 3.8  
- **PySpark**  
- **PostgreSQL**  
- **psycopg2** or **SQLAlchemy** (for PostgreSQL connections)  
- **yfinance** (for data extraction)  
- **TensorFlow/Keras** (for LSTM model)  
- **scikit-learn** (for data preprocessing, metrics)  
- **Matplotlib/Seaborn** (for visualizations)

---

## Installation & Setup

1. **Clone or Download** this repository:
   ```bash
   git clone https://github.com/senaseyhan/daily-stock-buysell-prediction
   cd YourRepoName
   ```

2. **(Optional) Create a virtual environment**:
   ```bash
   python -m venv venv
   source venv/bin/activate      # For Linux/Mac
   # or on Windows:
   venv\Scripts\activate
   ```

3. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
   If `requirements.txt` is missing, manually install each package:
   ```bash
   pip install yfinance pyspark psycopg2 tensorflow scikit-learn matplotlib seaborn
   ```

4. **Configure PostgreSQL**:
   - Install and run PostgreSQL.
   - Create a database (e.g., `finance`).
   - Update `db_config` in `data_loading/load.py` or `main.py`:
     ```python
     db_config = {
       "host": "localhost",
       "port": "5432",
       "user": "postgres",
       "password": "YOUR_PASSWORD",
       "dbname": "finance"
     }
     ```

---

## Usage

1. **Run the ETL Pipeline**:
   ```bash
   python data_loading/main.py
   ```
   This script:
   - **Extracts** data from Yahoo Finance (using `extract.py`)  
   - **Transforms** data via PySpark (`transformation.py`)  
   - **Loads** data into PostgreSQL (`load.py`)

2. **Check Data in PostgreSQL**:
   - Use a client tool like **pgAdmin**, **DBeaver**, or a Python script with `psycopg2`/`SQLAlchemy` to verify data in tables (e.g., `stock_data`, `index_data`, etc.).

3. **Explore & Model with Jupyter**:
   - Launch the notebook to start exploring the data and building the LSTM model:
     ```bash
     jupyter notebook project.ipynb
     ```
   - Inside **project.ipynb**, you’ll see steps for:
     - Reading data back from PostgreSQL  
     - Performing EDA & feature engineering  
     - Training/validating the LSTM model  
     - Running a **backtest** with the model’s price predictions  

---

## Exploratory Data Analysis & Modeling

In **project.ipynb**, we focus on:

1. **EDA**  
   - Plotting timeseries of Adjusted Close, Volume, etc.  
   - Correlation analysis with other stocks, indices, and macro data (gold, oil, etc.).  
   - Generating technical indicators (RSI, MACD, Bollinger Bands, moving averages, etc.).

2. **LSTM Model**  
   - Data splitting by date (Train / Validation / Test).  
   - Hyperparameter tuning (via Keras Tuner or manual search).  
   - Performance metrics: **MSE**, **RMSE**, **MAE**, **R²**.

3. **Backtest**  
   - Implementing a daily strategy to **BUY**, **SELL**, or **HOLD**.  
   - Applying transaction fees and simulating portfolio changes over time.  
   - Comparing final portfolio value with a straightforward **Buy & Hold** approach.

---

## Results

Below is an example of potential outcomes based on the final backtest:

- **Final Portfolio Value**: \$123,872.56  
- **Net Profit**: \$23,872.56 (starting from \$100,000)  
- **ROI**: ~23.87%  
- **Buy & Hold**: \$134,965.79 (34.97% gain)  
- **Success Ratio**: 0.92 (Model vs. Buy & Hold)  

**Trading Metrics**:
- **Win Rate**: ~43% (Days with positive returns vs. total trades)  
- **Sharpe Ratio**: ~0.09  
- **Beta**: ~0.35 (less volatile than the market)  
- **R²**: ~0.57  

**Interpretation**:  
- Although the model made a profit (23.87%), it underperformed the simple buy-and-hold approach (34.97% in the same period).  
- This highlights room for improvement, e.g., refining model features, adjusting thresholds, or optimizing hyperparameters.  

---

## Additional Data

If you prefer offline usage or want direct CSV data, check the [`datas(csv)/`](datas(csv)) folder for historically downloaded files. You can load those directly into your analysis as needed.

---

## References

- [yfinance (PyPI)](https://pypi.org/project/yfinance/)  
- [Yahoo Finance](https://finance.yahoo.com/)  
- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)  
- [PostgreSQL Official Site](https://www.postgresql.org/)  
- [TensorFlow/Keras](https://www.tensorflow.org/)  
- [scikit-learn Documentation](https://scikit-learn.org/stable/)  
- [Matplotlib](https://matplotlib.org/) / [Seaborn](https://seaborn.pydata.org/)  
