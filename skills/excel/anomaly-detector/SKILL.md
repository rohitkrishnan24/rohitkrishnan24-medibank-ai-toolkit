---
name: anomaly-detector
description: Find anomalies, flag unusual values, and detect outliers in a dataset. Use when asked to find suspicious values, flag unusual claims, or detect outliers.
---

# Anomaly Detector

## Purpose
Scan a Medibank dataset and flag values that are statistically unusual, potentially erroneous, or suspicious.

## Before you begin
- Confirm which column contains the primary numeric metric
- Confirm which column contains the grouping variable
- Ensure data has a header row in Row 1

## Workflow

### Step 1 - Calculate baseline statistics
Calculate mean, standard deviation, minimum, maximum, Q1, Q3, and IQR for the primary numeric column. Report in a summary table.

### Step 2 - Flag statistical outliers
Flag values more than 2 standard deviations from the mean. Flag values outside 1.5 times the IQR. Highlight in red.

### Step 3 - Flag zero and negative values
Flag any zero or negative values in amount columns. Highlight in orange.

### Step 4 - Flag duplicates
Flag rows where both Claim ID and Amount are identical. Highlight in yellow.

### Step 5 - Group level anomalies
For each unique provider, flag any row where the value exceeds 3 times that provider average.

### Step 6 - Anomaly report
Create a new sheet named Anomaly Report with: Row Number, Claim ID, Provider, Flagged Value, Flag Type, Severity, Recommended Action. Sort by severity descending.

## Output
- Red highlights on statistical outliers
- Orange highlights on zero or negative values
- Yellow highlights on potential duplicates
- New Anomaly Report sheet sorted by severity

## Common pitfalls
- Do not delete flagged rows, only flag them
- Do not apply statistical checks to text columns
- Warn if dataset has fewer than 10 rows