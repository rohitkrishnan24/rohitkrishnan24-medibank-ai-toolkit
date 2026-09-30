---
name: formula-builder
description: Build Excel formulas from plain English descriptions. Use when asked to write a formula, create a calculation, or explain how to calculate something in Excel.
---

# Formula Builder

## Purpose
Translate a plain English calculation description into a correct, ready to use Excel formula with full explanation.

## Workflow

### Step 1 - Understand the request
Ask: what is the calculation, which column contains the input data, what should the output look like.

### Step 2 - Choose the function
Identify the most appropriate Excel function or combination. Prefer simpler functions where possible.

### Step 3 - Write the formula
Write the complete formula using actual column letters and row numbers from the dataset, starting with equals sign.

### Step 4 - Explain the formula
Explain what each part does, what it returns, and any important assumptions.

### Step 5 - Usage instructions
State which cell to enter the formula in and how to copy it down the column.

### Step 6 - Add error handling
Wrap in IFERROR to return a meaningful message instead of error codes.

## Output
Formula starting with equals sign, plain English explanation, usage instructions, and error handling version.

## Common pitfalls
- Confirm which columns contain the relevant data before writing
- Use XLOOKUP instead of VLOOKUP where appropriate
- Use dynamic ranges rather than fixed row counts
- Provide formula only solutions, no VBA or macros