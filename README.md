# Data Quality Validator
A reusable Python validation pipeline that runs automated quality
checks on any CSV dataset and produces a structured report.
Built as part of an AI evaluation engineering portfolio.

## What it does

Takes any pandas DataFrame and runs six validation checks:

| Check | What it detects |
|-------|----------------|
| Null analysis | Missing values by column with severity rating |
| Duplicate detection | Exact duplicate rows |
| Schema validation | Column data type mismatches |
| Value range validation | Values outside expected min/max bounds |
| Outlier detection | Statistical outliers using z-score method |
| Category consistency | Unexpected or inconsistent categorical values |

Outputs a structured JSON report and 4-chart visualisation dashboard.

## Why this matters for AI evaluation

In MLE Bench and SWE Bench roles, datasets arrive from production
systems and are almost never clean. This pipeline surfaces problems
systematically before any dataset enters a training or evaluation
pipeline — reducing the risk of corrupted model outputs.

## Results on demo dataset

Dataset: synthetic clinical records (205 rows, 8 columns)

| Check | Result | Issues found |
|-------|--------|-------------|
| Null analysis | FAIL | 3 columns affected |
| Duplicate detection | FAIL | 5 duplicate rows |
| Schema validation | PASS | None |
| Value range validation | FAIL | 2 columns (negative sizes, invalid scores) |
| Outlier detection | FAIL | 1 extreme outlier (999mm tumour size) |
| Category consistency | FAIL | 2 columns (BENIGN vs benign, INVALID codes) |

Overall health: **FAIL** — 5 high severity issues found

## How to use on your own dataset

```python
import pandas as pd

df = pd.read_csv('your_data.csv')

config = {
    'expected_schema' : {'col1': 'float', 'col2': 'object'},
    'range_rules'     : {'col1': {'min': 0, 'max': 100}},
    'numeric_cols'    : ['col1'],
    'category_rules'  : {'col2': ['value_a', 'value_b']},
    'z_threshold'     : 3.0
}

report = run_full_validation(df, config)
print(report['summary'])
```
## Tests

9 pytest tests covering all validation functions:
- Null detection and severity scoring
- Duplicate counting and edge cases
- Value range violation detection
- Category consistency checking

Run with:
