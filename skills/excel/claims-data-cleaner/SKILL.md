---
name: claims-data-cleaner
description: Clean and standardise a raw provider claims dataset. Use when asked to clean claims data, fix duplicates, standardise dates, or prepare claims data for analysis.
---

# Claims Data Cleaner

## Purpose
Clean and standardise a raw Medibank provider claims dataset so it is ready for analysis, reporting, or submission.

## Before you begin
- Confirm the dataset has a header row in Row 1
- Confirm columns include: Claim ID, Provider Name, Claim Date, Amount, Status
- Save a backup copy before running this skill

## Workflow

### Step 1 - Identify duplicates
Scan the Claim ID column for duplicate values. Show a summary before removing anything. Ask for confirmation before removing.

### Step 2 - Remove duplicates
After confirmation, remove all duplicate rows based on Claim ID. Highlight removed rows in yellow. Report how many rows were removed.

### Step 3 - Fix blank cells
Fill blank Provider Name cells with Unknown Provider. Fill blank Status cells with Pending Review. Report how many cells were filled.

### Step 4 - Standardise dates
Convert all Claim Date values to DD/MM/YYYY format. Highlight in orange any dates that cannot be converted.

### Step 5 - Standardise text case
Convert Provider Name column to Proper Case. Convert Status column to UPPER CASE.

### Step 6 - Remove spaces
Remove all leading and trailing spaces from Provider Code and Provider Name columns.

### Step 7 - Cleaning summary
Create a new sheet named Cleaning Summary showing original count, rows removed, cells filled, dates fixed, and final row count.

## Output
- Cleaning applied to original sheet
- Removed rows highlighted yellow
- Unconvertible dates highlighted orange
- New Cleaning Summary sheet added

## Common pitfalls
- Do not remove rows without user confirmation
- Do not convert dates in non-date columns
- Do not run without a header row