---
name: hospital-gap-fee-analyser
description: Analyse hospital gap fees and out of pocket costs by hospital, procedure, and surgeon. Use when asked to analyse gap fees, review out of pocket costs, or identify where members pay the most.
---

# Hospital Gap Fee Analyser

## Purpose
Analyse Medibank hospital admissions data to identify where members are paying the highest out-of-pocket gap fees and which hospitals and procedures generate the most financial stress.

## Before you begin
- Confirm the sheet contains: Member ID, Hospital Name, Procedure, Claim Amount, Benefit Paid, Gap Amount
- Confirm the time period to analyse

## Workflow

### Step 1 - Gap fee summary
Calculate: total gap fees paid by members, average gap fee per admission, percentage of admissions with gap fee above 500, and percentage with zero gap.

### Step 2 - Gap fees by hospital
Group by Hospital Name. Calculate total gap fees, average gap fee, and percentage with gap. Flag hospitals where average gap exceeds 1000 as High Gap Hospitals.

### Step 3 - Gap fees by procedure
Group by Procedure Type. Calculate average gap fee, highest gap recorded, and percentage with zero gap. Rank by average gap descending.

### Step 4 - Member impact analysis
Identify members who paid gap fees above 2000 in the period. Flag members with total gap fees above 5000 as Financially Stressed Members.

### Step 5 - No gap coverage rate
Calculate the percentage of admissions with zero gap per hospital. This shows which hospitals have the best no-gap arrangements with Medibank.

### Step 6 - Gap fee dashboard
Create new sheet Gap Fee Analysis with: summary panel, high gap hospitals table, high gap procedures table, financially stressed members list, and no-gap coverage rate by hospital.

## Output
- New Gap Fee Analysis sheet
- High gap hospitals highlighted
- Financially stressed members flagged
- No-gap coverage rate calculated

## Common pitfalls
- Gap Amount is Claim Amount minus Benefit Paid — verify this before running
- Zero gap does not always mean no cost — check for excess payments separately
- Do not identify individual surgeons by name in external reports