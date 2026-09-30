---
name: regulatory-submission-checker
description: Validate APRA quarterly data submissions against reporting rules before lodgement. Use when asked to check regulatory data, validate an APRA submission, or review compliance reporting.
---

# Regulatory Submission Checker

## Purpose
Validate Medibank quarterly APRA data submission against standard reporting rules to catch errors before lodgement and prevent regulatory breaches or resubmission penalties.

## Before you begin
- Confirm the sheet contains the quarterly reporting data to be submitted to APRA
- Confirm the reporting quarter and year
- Confirm which APRA reporting table this relates to

## Workflow

### Step 1 - Completeness check
Verify all required fields are populated. Flag any blank cells in mandatory reporting fields. Count total mandatory fields and total missing values.

### Step 2 - Numeric range validation
Check all numeric fields fall within expected reporting ranges. Flag any values that appear implausibly high or low. Highlight in orange.

### Step 3 - Period consistency check
Verify reporting period data is internally consistent: totals match sum of components, quarterly figures align with monthly breakdowns where both are present.

### Step 4 - Year on year variance check
If prior period data is available, calculate year on year variance for each key metric. Flag any metric with variance above 20 percent as Requires Explanation.

### Step 5 - Cross field validation
Validate logical relationships: claims paid cannot exceed premium collected, membership numbers cannot be negative, benefit ratios must be between 0 and 100 percent.

### Step 6 - Submission readiness report
Create new sheet Submission Checker with: overall status READY or NOT READY, list of all errors by severity, metrics requiring explanation, and a sign-off checklist.

## Output
- New Submission Checker sheet
- Overall READY or NOT READY status
- Error list by severity
- Sign-off checklist

## Common pitfalls
- Do not mark as READY if any mandatory fields are blank
- Year on year variances above 20 percent need explanation not rejection
- Cross field validation failures are High severity errors