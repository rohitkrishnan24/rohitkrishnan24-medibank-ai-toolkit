---
name: qoq-trend-analyser
description: Compare current quarter performance against the previous quarter. Use when asked for QoQ analysis, quarterly trends, or quarter over quarter comparison.
---

# Quarter on Quarter Trend Analyser

## Purpose
Compare current quarter against previous quarter for key metrics. Produces a comparison table and executive commentary for management reporting.

## Before you begin
- Confirm which columns are current and previous quarter
- Confirm which column contains metric names
- Confirm whether higher values are better or worse

## Workflow

### Step 1 - Identify columns
Confirm which column is current quarter and which is previous quarter.

### Step 2 - Calculate changes
For each metric: absolute change, percentage change to 1 decimal place, direction as Improved, Declined, or Unchanged.

### Step 3 - Apply formatting
Green fill for improved metrics. Red fill for declined metrics. Yellow fill for less than 1 percent change.

### Step 4 - Flag significant movements
Flag any metric with more than 10 percent change as Significant Movement.

### Step 5 - Create QoQ table
Create new sheet QoQ Analysis with: Metric Name, Previous Value, Current Value, Absolute Change, Percentage Change, Direction, Significance Flag.

### Step 6 - Write commentary
Write 3 to 5 sentence commentary covering overall trend, top improvements, top declines, and significant movements.

## Output
- Conditional formatting on original sheet
- New QoQ Analysis sheet
- Executive commentary below the table
- Summary line showing count of improved, declined, and unchanged metrics

## Common pitfalls
- Show N/A instead of dividing by zero
- Always show one decimal place for percentages
- Write commentary after the table is complete