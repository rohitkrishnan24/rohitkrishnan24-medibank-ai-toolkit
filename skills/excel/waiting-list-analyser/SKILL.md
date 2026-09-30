---
name: waiting-list-analyser
description: Analyse hospital waiting times and flag members waiting beyond clinical benchmarks. Use when asked to review waiting lists, analyse hospital wait times, or identify members waiting too long.
---

# Waiting List Analyser

## Purpose
Analyse hospital waiting time data to identify members waiting beyond clinically recommended timeframes, flag high risk states and procedures, and support proactive member outreach.

## Before you begin
- Confirm the sheet contains: Member ID, Hospital, State, Procedure, Wait Start Date, Wait Days, Clinical Benchmark Days
- Confirm the clinical benchmark thresholds to apply

## Workflow

### Step 1 - Calculate wait status
For each member compare Wait Days against Clinical Benchmark Days. Assign: Within Benchmark, Approaching Benchmark within 20 percent, or Exceeded Benchmark.

### Step 2 - Flag critical waiters
Flag all members who have exceeded their clinical benchmark as Critical. Highlight in red. Flag members approaching benchmark in orange.

### Step 3 - Analyse by procedure
Group by Procedure Type. Calculate average wait days, percentage exceeding benchmark, longest current wait. Rank by percentage exceeding benchmark descending.

### Step 4 - Analyse by state
Group by State. Calculate average wait days and percentage exceeding benchmark per state. Identify worst performing states.

### Step 5 - Analyse by hospital
Group by Hospital. Rank by average wait days descending. Flag hospitals where more than 30 percent of patients exceed benchmark.

### Step 6 - Waiting list dashboard
Create new sheet Waiting List Analysis with: summary by wait status, critical waiters sorted by wait days descending, procedure breakdown, state comparison, and hospital ranking.

## Output
- New Waiting List Analysis sheet
- Critical waiters highlighted in red
- Approaching benchmark in orange
- Hospital and procedure rankings

## Common pitfalls
- Clinical benchmarks vary by procedure — do not apply a single threshold to all procedures
- Calculate wait days from Wait Start Date to today if no end date is present
- Do not include members who have already been admitted