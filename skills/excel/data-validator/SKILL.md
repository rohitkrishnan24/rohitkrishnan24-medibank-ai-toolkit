---
name: data-validator
description: Validate data quality and produce a PASS, REVIEW, or FAIL score. Use when asked to validate data, check data quality, find errors, or confirm a dataset is ready for submission.
---

# Data Validator

## Purpose
Check a Medibank dataset against standard validation rules and produce a Data Quality Score with PASS, REVIEW, or FAIL status.

## Before you begin
- Confirm the dataset type such as provider claims, member data, or financial report
- Confirm any custom rules in addition to standard ones
- Ensure data has a header row in Row 1

## Workflow

### Step 1 - Completeness checks
Flag rows with blank values in required columns: Claim ID, Provider Name, Claim Date, Amount. Flag rows where Amount is blank or zero.

### Step 2 - Format checks
Flag dates not in DD/MM/YYYY format. Flag negative Amount values. Flag Claim IDs with inconsistent format.

### Step 3 - Logic checks
Flag Claim Dates in the future. Flag amounts exceeding 50000 for review. Flag duplicate Claim IDs.

### Step 4 - Assign severity
High for missing required fields, negative amounts, duplicate IDs. Medium for future dates, amounts over 50000. Low for minor format issues.

### Step 5 - Highlight failures
Red border for High severity. Orange border for Medium. Yellow border for Low.

### Step 6 - Validation report
Create new sheet Validation Report with: Row Number, Claim ID, Rule Failed, Failing Value, Severity, Recommended Fix. Sort by severity descending. Calculate Data Quality Score as rows with no failures divided by total rows times 100. Show PASS above 95 percent, REVIEW between 80 and 95, FAIL below 80.

## Output
- Colour coded borders on failing cells
- New Validation Report sheet
- Data Quality Score with PASS, REVIEW, or FAIL status

## Common pitfalls
- Flag only, never delete or modify rows
- Do not apply checks to wrong column types
- Confirm threshold for non-claims datasets