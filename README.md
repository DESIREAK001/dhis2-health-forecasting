# Forecasting Malaria Cases by District, with a DHIS2 Data Quality Pipeline

**Status: in progress.** Week 1 (data exploration and baselines) is complete. The machine learning model is next.

## Overview

Routine health data, collected monthly by systems such as DHIS2, is used to plan health services. This project has two parts:

- **Part A: Data quality checks on DHIS2 data.** Pull monthly antenatal care (ANC) data from the public DHIS2 demo instance, then flag missing reports and sudden drops by district.
- **Part B: Forecasting malaria cases.** Use real, open monthly malaria data from 27 districts of Malawi to forecast future case rates, and test whether a machine learning model can beat simple baselines.

## Data

**Part B (main dataset):** Elling, M. T., Karnauskas, K., Kowalcyk, M., Mategula, D., Chirombo, J., Livneh, B., McCann, R., & Buchwald, A. (2025). *Malawi Malaria Incidence 2005-2023* [Dataset]. Zenodo. https://doi.org/10.5281/zenodo.17161438. Licence: CC BY 4.0.

- 27 districts, 228 months (January 2005 to December 2023), 6,156 rows
- Columns used: district ID, date, cases, population, and cases per 1,000 people
- No missing values or duplicate rows
- Notes: districts are identified by number, and ID 10 is absent (27 of Malawi's 28 districts appear). The file name mentions 2023-2025, but the data covers 2005-2023.

**Part A:** The public DHIS2 demo instance (Sierra Leone sample data), indicator "ANC 1st visit". **This is demonstration data, not real health data.** It covers only 2025-2026, the two years are almost identical, and it contains gaps and one sudden drop (Pujehun), so it is used here for pipeline and data quality practice, not for judging forecasts.

The raw data files are not included in this repository. Download the Malawi file from the Zenodo link above.

## Week 1 findings

- The data is complete and consistent: every district has all 228 months, and the case rate matches cases divided by population × 1,000.
- Malaria shows strong seasonality. Average case rates are highest from January to May (about 38-41 cases per 1,000 people) and lowest from July to September (about 22-24).
- National monthly totals vary a lot between years (for example, a low in 2013 and a high in 2010), so the data is a genuine forecasting challenge.
- *[Add 1-2 sentences about what you notice in the charts in the `figures/` folder.]*

### Baselines (test years 2022-2023, trained on 2005-2019)

| Method | Mean absolute error (cases per 1,000 people) |
|---|---|
| Same month last year | 15.15 |
| Average of that month in the training years | 13.39 |

The average case rate in the test period is 25.14, so there is clear room for a model to improve on these.

## Plan

- [x] Load and check the data, plot seasonality, score baselines (`week1_explore.py`)
- [ ] Build lag and seasonal features (previous months, same month last year, rolling averages)
- [ ] Train a gradient boosting model and compare it with the baselines on a time-based split (train 2005-2019, validate 2020-2021, test 2022-2023)
- [ ] Evaluate by district and by forecast horizon (1 to 3 months ahead)
- [ ] Part A: pull DHIS2 data via the API and flag reporting gaps and outliers
- [ ] Write up results, limitations and next steps

## How to run

```bash
pip install pandas matplotlib
python week1_explore.py path/to/malawi_malaria_incidence_mte2025_2023-2025.csv
```

This prints the data checks and baseline scores, and saves charts to a `figures/` folder.

## Repository structure

```
week1_explore.py   # data checks, charts, baselines
figures/           # charts produced by the script
README.md
```

## Limitations

- The model uses past case counts only. Other drivers of malaria (such as rainfall, temperature and interventions) are not included yet.
- Reported cases depend on how many people seek care and how well facilities report, so they are not the same as true infections.
- The DHIS2 demo data in Part A is synthetic and is used only to demonstrate the pipeline.

## Author

Desire Acquilina Kabagyo, Computer Science graduate, Gulu University.
[LinkedIn link] | github.com/DESIREAK001
