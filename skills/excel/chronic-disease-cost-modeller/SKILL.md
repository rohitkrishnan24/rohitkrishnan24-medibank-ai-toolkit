---
name: chronic-disease-cost-modeller
description: Model projected claims costs for members with chronic conditions. Use when asked to model chronic disease costs, project future claims, or identify members for care management programmes.
---

# Chronic Disease Cost Modeller

## Purpose
Identify Medibank members with chronic conditions based on claims history, model projected future costs, and flag members who would benefit most from proactive care management programmes.

## Before you begin
- Confirm the sheet contains: Member ID, Diagnosis Category, Claims Last 12M, Total Claimed, Age Group, Policy Type
- Confirm which chronic condition categories to include

## Workflow

### Step 1 - Identify chronic condition members
Flag members with 3 or more claims in the same Diagnosis Category within 12 months as Chronic Condition Members.

### Step 2 - Calculate current cost burden
For chronic condition members: average claims per member, average total claimed, and total cost burden per condition category.

### Step 3 - Project future costs
Apply condition-specific cost escalation rates: Cardiovascular 8 percent, Diabetes 6 percent, Mental Health 12 percent, Respiratory 5 percent, Musculoskeletal 4 percent, Oncology 15 percent. Calculate 12 and 36 month projections.

### Step 4 - Care management prioritisation
Score each member: claims above 5000 adds 3 points, age above 55 adds 2 points, 2 or more chronic conditions adds 3 points, satisfaction below 6 adds 2 points. Flag members scoring above 6 as Care Management Priority.

### Step 5 - Savings potential
Estimate potential savings: industry benchmark is 15 to 25 percent reduction in hospitalisation costs for managed chronic condition members. Calculate low and high estimates.

### Step 6 - Chronic disease dashboard
Create new sheet Chronic Disease Model with: summary by condition, 12 and 36 month cost projections, care management priority list, and savings potential estimate.

## Output
- New Chronic Disease Model sheet
- Care management priority members flagged
- 12 and 36 month cost projections
- Savings potential estimate range

## Common pitfalls
- Cost projections are estimates based on historical trends not clinical predictions
- A member with high claims is not automatically a care management candidate
- Validate savings estimates with the actuarial team before presenting externally