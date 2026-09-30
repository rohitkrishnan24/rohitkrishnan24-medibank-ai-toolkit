---
name: member-lifetime-value-calculator
description: Calculate estimated member lifetime value based on tenure, premium, and claims history. Use when asked to calculate member lifetime value, estimate member worth, or prioritise retention spend.
---

# Member Lifetime Value Calculator

## Purpose
Calculate an estimated lifetime value for each Medibank member to prioritise retention investment, identify highest value members at risk of churning, and guide customer experience decisions.

## Before you begin
- Confirm the sheet contains: Member ID, Annual Premium, Years as Member, Total Claims Paid, Satisfaction Score, Renewal Intent
- Confirm the discount rate to apply (default 8 percent)

## Workflow

### Step 1 - Calculate historical value
Historical Value equals Annual Premium times Years as Member minus Total Claims Paid.

### Step 2 - Estimate remaining tenure
Based on Renewal Intent: Definitely Renew maps to 8 years, Likely Renew to 5 years, Undecided to 2 years, Likely Leave to 1 year, Definitely Leave to 0 years.

### Step 3 - Calculate future value
Future Value equals Annual Premium times Estimated Remaining Years at the discount rate.

### Step 4 - Calculate total lifetime value
Total Lifetime Value equals Historical Value plus Future Value. Assign tier: Platinum above 50000, Gold 20000 to 50000, Silver 5000 to 20000, Bronze below 5000.

### Step 5 - Identify at risk high value members
Flag Platinum or Gold members with Satisfaction Score below 6 or Renewal Intent of Undecided or worse as High Value At Risk.

### Step 6 - Lifetime value dashboard
Create new sheet Lifetime Value Analysis with: member lifetime value table sorted descending, tier distribution, high value at risk members in red, and total portfolio value.

## Output
- New Lifetime Value Analysis sheet
- Members ranked by lifetime value
- High value at risk members flagged in red
- Total portfolio value calculated

## Common pitfalls
- Lifetime value is an estimate not a guarantee
- Do not use lifetime value as the sole basis for service decisions
- High claims members may still have positive lifetime value if premium is high