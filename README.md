# The Science of the Perfect Pit Stop
### F1 Race Strategy Analytics with SQL, Python, and Machine Learning

This project explores Formula 1 race strategy using **101,371 lap-by-lap records** from the 2022–2025 seasons. The goal was to clean raw race data, analyze tyre degradation and pit timing patterns with SQL, and build a machine learning model that predicts whether a driver will pit on the next lap.[page:1]

## Project Overview

Race strategy in Formula 1 depends on balancing tyre life, lap-time drop-off, track position, and pit timing. In this project, I built an end-to-end workflow that:
- cleans and validates raw lap-level F1 data,
- performs SQL-based strategy analysis,
- engineers features for pit-stop prediction,
- compares machine learning models, and
- exports results for dashboard-ready visualization.[page:1]

## Dataset

The dataset contains **101,371 lap records** across **31 drivers**, **28 race weekends** including some pre-season sessions, and the **2022–2025 seasons**.[page:1]  
After cleaning, the final analytical dataset contains **96,242 rows** and **18 columns** with no missing values.[page:1]

## Data Cleaning

The cleaning process was evidence-based rather than using generic missing-value rules. Key decisions included:
- Dropping 66 rows with missing `Compound` values because they formed complete missing stint blocks that could not be safely inferred.[page:1]
- Dropping 1 row with missing `PitNextLap` because it was the final recorded lap, so the label was structurally undefined.[page:1]
- Filling 1,848 missing `Prev_TyreLife` values with 0 because they occurred on each driver’s first lap and were missing by construction.[page:1]
- Adding an `Is_Racing_Lap` flag so lap 1 outliers were excluded from pace/degradation analysis without deleting strategically useful rows.[page:1]
- Removing 5,062 pre-season rows so strategy analysis only reflects competitive race sessions.[page:1]

## SQL Analysis

The SQL section answers four race-strategy questions:
1. Average lap time by tyre compound adjusted for tyre age.[page:1]
2. Most common pit-stop lap by race.[page:1]
3. Tyre degradation slope by compound.[page:1]
4. Whether earlier or later first pit stops correlate with position gain.[page:1]

The analysis showed that among slick tyres, **SOFT degrades fastest**, **MEDIUM is in the middle**, and **HARD holds pace best**, which matches expected tyre behavior.[page:1]  
It also found that **mid-race pit stops (34–66% of race distance)** had the best typical outcome, with a median gain of **+2 positions**, while early and late first stops had median gains of 0.[page:1]

## Machine Learning

The target variable is **`PitNextLap`**, a binary label indicating whether a driver pits on the next lap.[page:1]  
Because only about **2.9% of laps** are followed by a pit stop, this is a highly imbalanced classification problem, so ROC-AUC was used instead of plain accuracy.[page:1]

Two models were compared:
- **Logistic Regression:** ROC-AUC **0.7836**[page:1]
- **Random Forest:** ROC-AUC **0.8887**[page:1]

The Random Forest performed best and identified the most important features as:
- `LapTime_Delta`
- `TyreLife`
- `Cumulative_Degradation`
- `Stint`
- `Position`[page:1]

At the default threshold, the model achieved:
- **Precision:** 0.14
- **Recall:** 0.74
- **F1-score:** 0.23 for the positive class (`PitNextLap = 1`).[page:1]

This means the model works best as an **early-warning strategy tool** rather than an automatic pit-call system, since it catches most real pit windows but also produces many false alarms.[page:1]

## Dashboard

The project also includes a dashboard-style visualization built from the cleaned dataset. The dashboard highlights:
- lap-time trend by tyre age,
- average cumulative degradation by compound,
- pit-stop lap distribution,
- median net position gain by pit timing bucket.[page:1]

## Key Insights

- Worn **SOFT** tyres were the fastest slick compound on average, followed by **MEDIUM** and **HARD**.[page:1]
- **SOFT** degraded much faster than **HARD**, with roughly a 4x larger degradation slope in the SQL analysis.[page:1]
- **Mid-race first pit stops** showed the strongest typical position gain.[page:1]
- The Random Forest model reached **ROC-AUC 0.889**, clearly outperforming Logistic Regression.[page:1]
- `LapTime_Delta` and `TyreLife` together drove most of the model’s predictive power.[page:1]

## Tech Stack

- **Python** for data cleaning, feature engineering, visualization, and machine learning.[page:1]
- **Pandas / NumPy** for analysis and preprocessing.[page:1]
- **SQLite / SQL** for strategy queries.[page:1]
- **Matplotlib / Seaborn** for charts.[page:1]
- **Scikit-learn** for model training and evaluation.[page:1]

## Files

- `F1_Strategy_Analysis.ipynb` — complete notebook workflow.[page:1]
- `dashboard.png` — 2x2 strategy dashboard preview.[page:1]
- `ml_results.png` — confusion matrix and feature importance chart.[page:1]
- `pr_curve.png` — precision-recall tradeoff visualization.[page:1]

## Future Improvements

Possible next steps include:
- adding gap-to-car-ahead/behind features for better strategy context,[page:1]
- testing boosted tree models such as XGBoost or LightGBM,[page:1]
- improving handling of retirement and incident-driven position outliers in race-level position analysis.[page:1]
