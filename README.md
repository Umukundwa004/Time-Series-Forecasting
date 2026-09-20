# Mobile Network Traffic Forecasting

Comparative analysis of three sequential models — **Linear Regression, MLP, and LSTM** —
for **one-step-ahead** forecasting of Internet traffic on the Milan telecom dataset
(Harvard Dataverse `doi:10.7910/DVN/EGZHFV`).

**Research question:** How do different sequential models compare for one-step-ahead
mobile network traffic forecasting, and how does their performance vary across
geographical areas with different traffic characteristics?

---

## What the project does

The notebook runs the full pipeline, in order:

1. **Download** the dataset (62 daily files, ~19.4 GB) using the required Dataverse guestbook flow.
2. **Memory management** — measure one day's memory, then reduce it with smaller data types
   and by loading only needed columns; large steps are processed one file at a time.
3. **Exploratory analysis** — distribution of traffic across the 10,000 areas, the top-3
   busiest areas, and time-series plots for five areas.
4. **Time-series analysis** — daily/weekly patterns, autocorrelation (ACF/PACF),
   stationarity (ADF test), and trend/seasonal/residual decomposition.
5. **Model selection** — three sequential models chosen for comparison.
6. **Hyperparameter tuning** — each model is tuned on a validation week; a before-vs-after
   table shows the effect of tuning.
7. **Forecasting experiments** — the models forecast the week of 16–22 Dec 2013 for the
   top-3 areas, evaluated with MAE, MSE, RMSE, MAPE, and R².

---

## Repository structure

```
.
├── forecasting.ipynb      # main notebook (rename to match your file)
├── requirements.txt       # Python dependencies
├── README.md              # this file
└── data/                  # downloaded dataset (created on first run; not committed)
```

> If your notebook file has a different name (e.g. `forecasting_improved.ipynb`),
> either rename it to `forecasting.ipynb` or update the name above.

---

## Requirements

- Python 3.11 or 3.12 recommended (see the note about TensorFlow below).
- The packages listed in `requirements.txt`:

```
pandas
numpy
matplotlib
scikit-learn
statsmodels
python-dotenv
requests
tensorflow
```

---

## Setup

1. **Get a Dataverse API token.** Log in at https://dataverse.harvard.edu, open your
   account settings, and create an API token.

2. **Create a `.env` file** in the project folder with a single line:

   ```
   DATAVERSE_API_TOKEN=your-token-here
   ```

   The notebook reads the token from this file, so it is never written in the code.

3. **Install the dependencies:**

   ```
   pip install -r requirements.txt
   ```

   The LSTM needs TensorFlow. If TensorFlow will not install on your Python version,
   create a virtual environment with Python 3.11 or 3.12 and run the notebook there.
   Without TensorFlow the notebook still runs, but the LSTM (the third model) is skipped.

---

## How to run

Open the notebook and run the cells **top to bottom**:

```
jupyter notebook forecasting.ipynb
```

Notes on running:

- The **first task downloads ~19.4 GB** into a `data/` folder. The download is
  **resumable** — if your connection drops, just re-run the download cell and it
  continues where it stopped (finished files are skipped).
- **Order matters in Task 4:** run the tuning cells (4.2) **before** the before/after
  table and the final experiment, because tuning sets the values the later cells use.
- To **fine-tune**, you only need to edit the option lists in the settings cell
  (`N_LAGS_OPTIONS`, `MLP_HIDDEN_OPTIONS`, `LSTM_UNITS_OPTIONS`, `LSTM_EPOCHS_OPTIONS`).

---

## Outputs

Running the notebook produces: the memory before/after comparison, the exploratory and
time-series figures, the tuning tables (including before-vs-after), the 9 actual-vs-predicted
plots (3 areas × 3 models), per-area metric tables (MAE, MSE, RMSE, MAPE, R²), a timing
table, and a failure analysis (single-day zoom and error-by-hour).

---

## Notes

- The written report (analysis, interpretation, and conclusions) is submitted separately.
- **AI disclosure:** AI tools were used to assist with the code and debugging. All analysis,
  interpretation, and written content are my own.
