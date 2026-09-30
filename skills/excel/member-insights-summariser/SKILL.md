---
name: member-insights-summariser
description: Analyse member data and produce structured insights. Use when asked to summarise member data, generate member insights, or analyse member satisfaction scores.
---

# Member Insights Summariser

## Purpose
Analyse a Medibank member dataset and produce a structured insights summary covering satisfaction scores, at risk members, and segment breakdowns.

## Before you begin
- Confirm dataset contains: Member ID, State, Product Type, Satisfaction Score
- Confirm the time period covered
- Confirm whether output is for internal briefing or executive presentation

## Workflow

### Step 1 - Headline metrics
Calculate: total members, average satisfaction score, highest score, lowest score, count and percentage below 5 out of 10 as at risk, count and percentage of 9 or 10 as promoters.

### Step 2 - Segment by state
Group by State. Calculate member count, average satisfaction, and percentage below 5 for each. Rank states highest to lowest.

### Step 3 - Segment by product type
Group by Product Type. Calculate member count and average satisfaction for each.

### Step 4 - Flag at risk members
Flag members with satisfaction score below 5. Highlight those rows in red on the original sheet.

### Step 5 - Member Insights sheet
Create new sheet Member Insights with headline metrics panel, state breakdown table, product type breakdown table, and at risk count by state.

### Step 6 - Insights narrative
Write 3 paragraphs: overall satisfaction picture, bright spots identifying the best performing segment, and most urgent attention needed.

## Output
- At risk rows highlighted red on original data
- New Member Insights sheet with all tables and narrative
- All percentages to one decimal place

## Common pitfalls
- Use Member IDs only, never include real member names or contact details
- Confirm the satisfaction scale before calculating averages
- Write narrative after all tables are complete