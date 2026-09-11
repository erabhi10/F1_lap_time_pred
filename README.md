# 🏎️ Formula One Lap Time Predictor

A hackathon-ready classical ML project for predicting F1 lap times while avoiding train-test leakage.

## Objective

Predict lap times inside one Formula 1 race and test whether **tire age** improves regression performance.

### Models

- Ridge Regression baseline: `grid + lap`
- Random Forest baseline: `grid + lap`
- Ridge enhanced: `grid + lap + tire_age`
- Random Forest enhanced: `grid + lap + tire_age`

### Evaluation

- RMSE
- MAE

### Validation strategy

A random split is intentionally avoided.

For every selected driver:

```text
Earlier stints  -> TRAIN
Final stint     -> TEST
```

This preserves the chronological structure of the race and prevents adjacent laps from leaking between train and test.

## Dataset

Use the Kaggle dataset:

**Formula 1 World Championship (1950 to 2020)** by Rohan Rao.

The notebook expects:

```text
data/
├── lap_times.csv
├── pit_stops.csv
├── results.csv
└── races.csv
```

The CSV files are not bundled with this project.

## Project structure

```text
f1-lap-time-predictor/
├── data/
│   ├── lap_times.csv
│   ├── pit_stops.csv
│   ├── results.csv
│   └── races.csv
├── F1_Lap_Time_Predictor.ipynb
├── README.md
└── requirements.txt
```

## Installation

```bash
pip install -r requirements.txt
```

Then:

```bash
jupyter notebook
```

Open `F1_Lap_Time_Predictor.ipynb` and **Run All**.

## Data cleaning

The notebook removes:

1. Pit-stop laps
2. Laps slower than `1.5 ×` the driver's median lap time
3. Invalid lap times / missing grid values

It reports the number removed by each rule and the total.

## Tire age

```text
tire_age = current_lap - most_recent_pit_lap
```

The counter resets after every pit stop.

## Race selection

The notebook is configured for:

```text
2019 British Grand Prix
```

Change `RACE_YEAR` and `RACE_NAME` near the top if needed.

The historical CSV schema does not contain a clean red-flag boolean, so the notebook does not fabricate one. Verify the selected race had no red flag before final submission.

## Important implementation detail

Do **not** replace the stint-based split with:

```python
train_test_split(...)
```

That would violate the anti-leakage requirement.

## Final deliverables

- Jupyter Notebook
- Model comparison table
- Final-stint actual vs predicted plot
- Concise methodology README

## Hackathon talking points

> "The main challenge was not fitting a regressor. It was designing a validation strategy that respected the race's temporal structure."

> "Tire age gives the model information about stint progression and degradation that grid position and lap number alone cannot represent."

Use the measured RMSE/MAE produced by the notebook in your presentation rather than hard-coding results.
