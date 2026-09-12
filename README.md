
# Forecasting Corporate Credit Spreads Using Macroeconomic and Financial Indicators

### An Expanding-Window Regression Framework for One-Month-Ahead Forecasting

This project develops a one-month-ahead forecasting framework for U.S. corporate credit spreads using macroeconomic and financial-market indicators. The analysis focuses on the **Moody's Seasoned Baa Corporate Bond Yield Relative to the 10-Year Treasury Yield** and evaluates whether macro-financial information can improve forecasts beyond simple persistence and autoregressive benchmarks.

A key focus of the project is maintaining temporal integrity. Macroeconomic variables are aligned according to their public release timing where appropriate, and forecasting models are evaluated using an **expanding-window out-of-sample framework** designed to approximate a real-time forecasting environment.

---

## Project Objective

Credit spreads measure the additional yield investors demand for holding corporate bonds relative to lower-risk U.S. Treasury securities. They tend to widen during periods of financial stress and heightened risk aversion and tighten as economic and financial conditions improve.

Because credit spreads are highly persistent, simple forecasts can be difficult to outperform.

The central research question is:

> **Can macroeconomic conditions, financial-market indicators, and recent credit-spread dynamics improve one-month-ahead forecasts relative to naive persistence and autoregressive benchmarks?**

---

## Data

The dataset combines publicly available macroeconomic and financial-market information.

### Target

- **Moody's Seasoned Baa Corporate Bond Yield Relative to the 10-Year Treasury Yield**

### Macroeconomic Indicators

- Consumer Price Index (CPI)
- Unemployment Rate
- Industrial Production
- Real GDP
- Federal Funds Rate

### Financial-Market Indicators

- 10-Year minus 3-Month Treasury Yield Curve
- 10-Year Treasury Yield
- VIX
- S&P 500
- Russell 2000
- WTI Crude Oil

Selected raw series are transformed into economically meaningful forecasting features, including:

- Year-over-year inflation
- Year-over-year industrial production growth
- Year-over-year real GDP growth
- S&P 500 monthly returns
- Russell 2000 monthly returns
- WTI crude oil monthly returns
- Six-month rolling credit-spread volatility
- Lagged credit-spread and macro-financial variables

---

## Release-Aware Data Alignment

One of the main challenges in macroeconomic forecasting is avoiding **look-ahead bias**.

Economic observations are not necessarily available at the end of the period they describe. For variables subject to publication delays or revisions, the analysis accounts for public release timing where appropriate so that forecasting observations better represent the information that would realistically have been available at each forecast date.

All variables are ultimately aligned to a common **monthly, month-end forecasting timeline**.

This allows information available through month *t* to be used to forecast the corporate credit spread in month *t+1*.

---

## Exploratory Analysis

The exploratory analysis examines:

- Historical credit-spread behavior
- Rolling mean and volatility
- Stationarity using the Augmented Dickey-Fuller test
- Autocorrelation and partial autocorrelation
- Contemporaneous correlations
- Lagged relationships between credit spreads and macro-financial indicators

Credit spreads exhibit substantial persistence while also widening sharply during periods of financial and economic stress.

Among the predictors, **VIX and recent credit-spread volatility exhibit particularly strong positive relationships with credit spreads**, while several measures of economic activity and interest rates exhibit negative relationships.

The lag analysis also indicates that recent macro-financial information can contain useful information about subsequent credit-spread conditions.

---

## Forecasting Framework

The forecasting exercise uses a chronological train/test design rather than a random split.

- **Training/development period:** through December 2021
- **Out-of-sample evaluation:** 55 monthly forecasts
- **Forecast horizon:** one month ahead

For the estimated forecasting models, an **expanding-window approach** is used.

At each forecast origin:

1. The model is estimated using all observations available up to that point.
2. A one-month-ahead credit-spread forecast is generated.
3. Once the next observation becomes available, it is added to the training sample.
4. The model is re-estimated for the subsequent forecast.

This framework more closely approximates how the model would be used in a real forecasting environment while preventing future observations from entering the estimation sample.

---

## Models

The project evaluates increasingly informative forecasting specifications:

### Benchmarks

- **Naive Persistence:** assumes next month's spread equals the current spread.
- **AR(1):** estimates next month's spread from its current value.

### Regression Models

- OLS using contemporaneous macroeconomic and financial predictors
- OLS incorporating the current credit spread
- OLS combining selected current and lagged credit-spread and macro-financial predictors

The final specification is evaluated using the expanding-window forecasting procedure.

---

## Out-of-Sample Results

| Model | MAE | RMSE | R² |
|---|---:|---:|---:|
| Naive Persistence | 0.0931 | 0.1184 | 0.7666 |
| AR(1) Benchmark | 0.0948 | 0.1185 | 0.7664 |
| **Final Expanding-Window OLS** | **0.0768** | **0.0993** | **0.8359** |

The final OLS model achieved the strongest overall out-of-sample performance.

Relative to naive persistence, the model reduced:

- **MAE by approximately 17.5%**
- **RMSE by approximately 16.1%**

These results suggest that macro-financial information and recent credit-spread dynamics provide useful incremental forecasting information beyond spread persistence alone.

---

## Directional Forecasting

Forecast accuracy was also evaluated based on whether the model correctly anticipated **credit-spread widening or tightening**.

| Model | Directional Accuracy |
|---|---:|
| AR(1) Benchmark | 43.64% |
| **Final Expanding-Window OLS** | **65.45%** |

The final OLS model correctly predicted the direction of the subsequent spread movement in **36 of 55 months**.

Unlike the AR(1) benchmark, which predicted widening throughout the test period, the OLS model predicted both widening and tightening, demonstrating greater ability to distinguish between changing credit-market conditions.

---

## Statistical Evaluation

Two statistical tests were used to determine whether the forecasting results were meaningful beyond the raw performance metrics.

### Pesaran–Timmermann Test

The Pesaran–Timmermann test evaluates whether predicted and actual spread directions are statistically associated rather than matching simply because of the underlying frequencies of widening and tightening.

**Final result:**

- PT statistic: **2.9166**
- p-value: **0.0035**

The result is statistically significant at the **1% level**, providing strong evidence that the model contains meaningful information about the direction of subsequent credit-spread movements.

### Diebold–Mariano Test

The Diebold–Mariano test compares the squared forecast errors of the final OLS model with those of the naive persistence benchmark.

**Final result:**

- DM statistic: **-1.8142**
- p-value: **0.0697**

The result is not statistically significant at the conventional 5% level but is significant at the **10% level**, providing moderate evidence that the final OLS model improves point-forecast accuracy relative to naive persistence.

---

## Key Findings

- Credit spreads exhibit strong persistence, making naive persistence a challenging benchmark.
- Incorporating selected macroeconomic, financial-market, and lagged spread information improves one-month-ahead forecasting performance.
- The final expanding-window OLS model achieved an **R² of 0.8359**.
- The final model reduced **MAE by approximately 17.5%** and **RMSE by approximately 16.1%** relative to naive persistence.
- Directional accuracy improved to **65.45%**, compared with **43.64% for the AR(1) benchmark**.
- The Pesaran–Timmermann test provides strong statistical evidence of directional forecasting ability.
- The Diebold–Mariano test provides moderate evidence of improved point-forecast accuracy.

---

## Limitations and Future Work

The project is subject to several limitations.

First, macroeconomic and financial variables have different historical coverage, frequencies, and publication schedules. Maintaining a release-aware dataset limits the number of predictors that can be incorporated without shortening the historical sample or introducing potential look-ahead bias.

Second, the out-of-sample evaluation contains only **55 monthly observations**, which limits the statistical power of forecast-comparison tests.

Finally, several model specifications and feature combinations were explored during model development using the same test period. A more rigorous extension would introduce separate **training, validation, and final untouched holdout periods** for model and feature selection.

Future work could therefore focus on expanding the release-aware dataset, incorporating additional economically motivated predictors, and evaluating the forecasting framework across a longer independent holdout sample and multiple credit-market regimes.

---

## Tools and Libraries

- Python
- pandas
- NumPy
- Matplotlib
- Seaborn
- statsmodels
- scikit-learn
- SciPy
- FRED / ALFRED economic data

---

## Repository

The repository contains the complete project analysis and supporting materials:

- **Jupyter Notebook:** `forecasting_corporate_credit_spreads.ipynb` — complete analysis, including data preparation, exploratory analysis, feature engineering, forecasting models, expanding-window evaluation, directional forecasting, and statistical testing.

- **Presentation:** `Corporate_Credit_Spread_Forecasting.pdf` — summary of the research methodology, forecasting results, and conclusions.

- **Project Report:** `Corporate_Credit_Spread_Forecasting_Report.pdf` — detailed discussion of the research question, methodology, results, limitations, and conclusions.

---

## Author

**Albina Gumennaia**

Springboard Data Science Career Track

M.S. Business Analytics  
Baruch College, Zicklin School of Business
