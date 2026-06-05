# Dynon Flight Data EDA

## Overview

This notebook performs Exploratory Data Analysis on real flight data
exported from a Dynon avionics system. The goal was to explore
relationships between cylinder head temperatures (CHT) and indicated
airspeed across a full flight recording.

## Dataset

- **Source:** Dynon avionics system export
- **File:** `data/raw/dynon_data.csv`
- **Rows:** 14,133 total, 14,122 after cleaning
- **Columns:** 104 total
- **Grouping Variable:** GPS Fix Quality (0.0 = no lock, 1.0 = basic lock, 2.0 = full lock)

## Columns Analyzed

| Column | Description |
|--------|-------------|
| CHT 1 (deg C) | Cylinder Head Temperature - Cylinder 1 |
| CHT 2 (deg C) | Cylinder Head Temperature - Cylinder 2 |
| CHT 3 (deg C) | Cylinder Head Temperature - Cylinder 3 |
| CHT 4 (deg C) | Cylinder Head Temperature - Cylinder 4 |
| Indicated Airspeed (knots) | Aircraft indicated airspeed |

## Data Quality

Only 9 rows were dropped during cleaning — less than 0.1% of the
dataset. The Dynon system records data consistently, resulting in
very clean data across the key columns analyzed.

## Key Findings

### Correlation Matrix

All four CHT cylinders are nearly perfectly correlated with each
other (0.98-1.0), confirming the engine runs balanced with all
cylinders heating and cooling together.

CHT shows a strong positive correlation with indicated airspeed:

| Column | Airspeed Correlation |
|--------|----------------------|
| CHT 1 | 0.80 |
| CHT 2 | 0.82 |
| CHT 3 | 0.70 |
| CHT 4 | 0.72 |

The front cylinders (1 and 2) correlate more strongly with airspeed
than the rear cylinders (3 and 4), which may reflect differences in
cooling airflow across the engine.

### Scatter Plot — CHT 1 vs Indicated Airspeed

The scatter plot reveals three distinct flight phases:

- **Ground operations (0 knots)** — engine running but stationary,
  CHT ranging from cold start temperatures up to taxi temps
- **Low speed (20-25 knots)** — takeoff roll or slow flight at
  lower CHT values
- **Cruise flight (75-160 knots)** — the main flight envelope,
  showing CHT climbing steadily with airspeed as power demand increases

### Box Plot — CHT 1 by GPS Fix Quality

GPS fix quality acts as a natural proxy for flight phase:

- **Quality 0.0** — no GPS lock, wide temperature spread from cold
  engine to warm taxi temperatures
- **Quality 1.0 and 2.0** — GPS locked and airborne, median CHT
  around 150-160°C with low outliers during initial climb out

### Histogram — CHT 1 Distribution by GPS Fix Quality

The histogram clearly shows two distinct temperature populations —
ground operations clustered at lower temperatures and in-flight
operations clustered around cruise CHT values.

## Suggested Next Steps

- Model CHT ~ Indicated Airspeed using linear regression (Module 7)
- Investigate why front cylinders correlate more strongly with airspeed
- Analyze EGT columns alongside CHT for a more complete engine health picture
- Compare multiple flights to identify trends over time

## Notebook

The full analysis is available in `notebooks/eda_jcarne_dynon.ipynb`.
