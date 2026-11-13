# ITSM Machine Learning Project (ABC Tech, PM-PR-0012)

All the Python is in one Jupyter notebook: `ITSM_ML_Project.ipynb` (already run, so outputs are visible).

## Run it

```bash
pip install -r requirements.txt
jupyter notebook ITSM_ML_Project.ipynb      # Run All
```

Data: the notebook expects the ABC Tech incident file `dataset.csv` (semicolon separated, 46,606 rows). The dataset is company data and is **not included in this repository**. Place it next to the notebook (or in `data/`) and set `DATA_PATH` in the first code cell if needed. The notebook already contains saved outputs, so results can be read without the data.

## What is inside

| Section | Goal | Method |
|---------|------|--------|
| 1 | Clean data and quality checks | Parsing, NA handling, priority matrix rebuilt from data |
| 2 | Predict high-priority (P1/P2) tickets | Random forest on information known at ticket creation |
| 3 | Forecast incident volume | Holt-Winters (weekly seasonality) on daily counts, rolled up to quarters |
| 4 | Auto-tag priority and department | Multi-class random forest vs a lookup baseline |
| 5 | Predict RFC and failure | Binary random forest on proxy labels |

## Data notes

- Dates are day-first (`05-02-2012 13:32`). `Handle_Time_hrs` is corrupted in the file, so handle time is rebuilt from Open_Time and Close_Time.
- Coverage is mainly Oct 2013 - Mar 2014 (about 6 months), not the full 2012-2014 span. Weekends are nearly empty.
- Impact `NS` and Priority `NA` (3% of rows) are treated as missing.

## Assumptions to confirm

- Priority is a fixed function of Impact x Urgency, so Task 1 excludes them to avoid leakage.
- No assignment-group field exists, so `WBS` is the department proxy.
- RFC = ticket has a related change; failure = ticket was reopened.
- Because of the short history, the volume forecast is an indicative run-rate, not a seasonal annual forecast.
