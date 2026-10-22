---
name: qoq-trend-analyser
description: Use when asked to compare current quarter performance against the prior quarter, produce a QoQ trend analysis, generate executive commentary for a quarterly management report, or analyse multi-period trends for Medibank KPIs. Produces a formula-driven, statistically qualified trend workbook with significance testing, directional commentary, seasonality notes, and a board-ready narrative. Do not use for year-end annual reporting (use a YoY structure instead) or for individual claim-level analysis.
---

# Quarter-on-Quarter Trend Analysis for Medibank Management Reporting

## Purpose
Build a formula-driven quarter-on-quarter (QoQ) trend analysis in Excel that compares current quarter performance against the prior quarter and, where data is available, across multiple prior periods to surface genuine trends versus noise. Apply statistical significance testing to distinguish material movements from normal quarter-to-quarter variation, flag seasonality as a confounding factor, and produce board-ready commentary that correctly attributes causes rather than just describing magnitudes. The workbook must be updatable in minutes when a new quarter's data arrives.

## Non-negotiable outcome
Do not produce a static table with hand-typed percentages. Build a complete, formula-driven trend workbook.

The workbook is complete only when:
- all absolute changes, percentage changes, and directional indicators are formula-driven from input data — no manually typed figures in the analysis;
- statistical significance testing is applied to every metric where sample size allows: a 3% change that is within normal variation is labelled Routine Movement, not flagged as Improved or Declined;
- seasonality flags are applied to metrics known to have seasonal patterns in private health insurance (hospital admissions peak winter Q2, mental health claims trend upwards, premium revenue is highest after April premium increase);
- the commentary follows a strict template: size of movement → direction → significance → attribution (cause) → outlook — not just "increased by X%";
- metrics are correctly directional: for some KPIs (e.g. benefit ratio) an increase is unfavourable; for others (e.g. NPS) an increase is favourable. The workbook codes this correctly and applies the correct traffic light;
- a Trend panel shows at least four quarters of data (current plus three prior) in a single view so the current movement is contextualised against the trend;
- metrics are grouped by business domain (Financial, Membership, Claims, Operations, Customer Experience) and each domain has a sub-total summary;
- a Significance Filter allows the user to display only statistically significant or material movements, hiding routine noise;
- the executive commentary section is structured for cut-and-paste into a board pack or management report.

## Operating principles
1. A QoQ change is meaningful only relative to the normal variation range. Report not just the change, but whether it is outside the normal range for that metric. Use historical standard deviation to define the normal variation range where four or more prior periods are available.
2. Direction coding must be correct for each metric. Do not apply green = up, red = down universally. For benefit ratio, claims per member, lapse rate, and complaint count: up is unfavourable. For NPS, membership count, premium revenue: up is favourable. Code each metric with a Direction Convention (Higher is Better / Lower is Better / Neutral) in the Metrics Definition sheet.
3. Percentage change calculation: when the prior period value is zero or negative, percentage change is undefined or misleading. Display `N/A` and note the denominator issue rather than dividing and displaying an erroneous figure.
4. Significance thresholds: for metrics measured on large populations (claims count, membership count), even small percentage changes may be statistically significant. For metrics based on small samples (NPS from 500 surveys, high-cost admission count), a large percentage change may not be significant. Apply the appropriate test for each metric type.
5. Australian private health insurance seasonality patterns (from APRA statistics):
   - Hospital admissions: peak Q2 (Jan–Mar — Southern Hemisphere summer, post-NYE procedures), trough Q4 (Jul–Sep — winter lull effects on elective procedures);
   - General treatment: relatively stable with slight Q1 peak (dental, optical before year-end lapse in benefits);
   - Lapse rate: elevated in Q4 (April–June — post-premium increase period) and after life events;
   - Premium revenue: step-change in Q4 due to the annual April premium increase.
   Apply seasonal flags to any metric that is expected to move in a known direction due to the season, and note whether the current movement is with or against the seasonal trend.
6. Health insurance metrics are correlated. Membership growth drives premium revenue growth; a high-cost admission mix shift drives claims ratio deterioration independent of per-admission cost changes. Flag metric correlations in the commentary when one movement explains another — do not treat correlated metrics as independent signals.
7. Prior period comparisons must be like-for-like. If the prior period included a one-time item (e.g. a reserve release, an extraordinary claims event, a large group policy lapse), the comparable figure should be shown both reported and on an underlying basis. Flag any period where a one-time item is known to affect comparability.
8. Materiality thresholds apply alongside significance thresholds. A statistically significant change in a metric worth AUD 50,000 is less material than a non-significant change in a metric worth AUD 100 million. Set both a significance threshold and a materiality threshold (configurable in Assumptions) and flag items that exceed either.
9. The commentary must not be automatically generated from formula outputs alone. The workbook produces structured prompts — size and direction — but the attribution (why the metric moved) requires analyst input. Provide a Commentary Inputs section where the analyst adds the attribution, and the final commentary combines formula-output (size, direction, significance) with analyst-input (attribution, outlook).
10. Multi-period trend panels (four or more quarters) reveal whether the current movement is: (a) a reversal of a prior trend, (b) a continuation of an existing trend, or (c) a spike against a stable baseline. Classify the trend type for each metric.
11. APRA quarterly statistics are published with a lag (typically 6 weeks after quarter-end). Where APRA industry benchmarks are available for the current period, include a Medibank vs Industry comparison alongside the QoQ movement.
12. When data for fewer than four prior periods is available, the trend panel shows what is available and notes the limited history. Do not extrapolate or fill in missing historical data.
13. Rounding and presentation: all percentage changes displayed to one decimal place (0.0%) in the report but calculated to full precision internally. Absolute changes in AUD displayed with appropriate scale ($ k, $ m, or $ bn) — do not mix scales within a table.
14. The metrics definition sheet is the configuration layer. Adding a new metric (a new KPI) requires only a row in the metrics definition table — no formula changes to the analysis engine.
15. Board-pack formatting: the executive commentary and summary table are designed to print on A4 landscape or copy-paste cleanly into PowerPoint. Use table borders, not fill colour, as the primary structure in the printable section.
16. Suppress noise: minor movements (within ± [Minor_Threshold]%, configurable) are shown but not coloured and are marked Routine. The executive commentary does not mention routine movements unless asked.
17. When metrics move in opposite favourable/unfavourable directions simultaneously (e.g. NPS improves while lapse rate deteriorates), flag the tension explicitly in the commentary.

## First response and ambiguity handling
Before building, confirm:
- the metric list (what KPIs are to be included);
- the current quarter and prior quarter data (at minimum; ideally four quarters);
- whether each metric is Higher is Better or Lower is Better;
- whether APRA industry benchmark data is available for this period;
- whether any one-time items affected the current or prior quarter;
- the target audience (board / senior management / operational team) — affects commentary length and technical depth;
- whether seasonality flags are required.

If only two periods are provided (current and prior quarter), build the full workbook with the trend panel showing two quarters and note that significance testing and trend classification require more history.

## Workbook architecture

### 1. Metrics_Definition
Excel table `tblMetrics`. One row per KPI. This is the configuration layer.

Columns:
- Metric ID (e.g. FIN-01, MEM-03)
- Domain (Financial / Membership / Claims / Operations / Customer Experience)
- Metric Name
- Unit (AUD, Count, %, Ratio, Days, Score)
- Direction Convention (Higher is Better / Lower is Better / Neutral)
- Materiality Threshold (absolute change — below this, mark Routine regardless of significance)
- Significance Threshold (% change — above this AND material, flag as Significant)
- Seasonal Pattern (Q1 Seasonal Up / Q2 Seasonal Down / None — per Southern Hemisphere calendar)
- APRA Benchmark Available (Y/N)
- Notes (e.g. "Premium revenue increases in Q4 due to April rate increase — QoQ Q4 vs Q3 will always show a step up")

Minimum metric set for Medibank quarterly reporting:
- Financial: Total Premium Revenue, Total Benefits Paid, Management Expense Ratio, Benefit Ratio (Hospital), Benefit Ratio (General Treatment), Fund Equity, Capital Adequacy Ratio
- Membership: Total Insured Persons (Hospital), Total Insured Persons (General Treatment), Net Membership Growth (QoQ), Lapse Rate, New Policies Written
- Claims: Hospital Admissions Count, Average Benefit per Hospital Admission, General Treatment Claim Count, Average General Treatment Benefit, Mental Health Claims Count, High-Cost Admissions Count (> AUD 50k)
- Operations: Claims Processing Time (average days), Complaint Count, Complaint Resolution Rate, HICAPS Claim Error Rate
- Customer Experience: Net Promoter Score, Customer Satisfaction Score, Digital Adoption Rate (members using online account)

### 2. Period_Data
Excel table `tblPeriods`. Structured for easy updating.

Structure: periods as columns (Q1 FY27, Q4 FY26, Q3 FY26, Q2 FY26, Q1 FY26), metrics as rows. Each cell is an input.

Include a row above each group for: Quarter name, Start Date, End Date, One-Time Item Flag (Y/N), One-Time Item Description.

### 3. QoQ_Analysis
Calculated table. One row per metric.

Columns:
- Metric ID and Metric Name (from tblMetrics)
- Current Quarter Value
- Prior Quarter Value
- Absolute Change (formula: Current − Prior)
- Percentage Change (formula: IF(Prior = 0, "N/A", (Current − Prior)/ABS(Prior)))
- Direction (Improved / Declined / Unchanged — formula using Direction Convention from tblMetrics)
- Significance Classification (formula: see below)
- Seasonal Flag (from tblMetrics; note if expected directional pattern matches or contrasts actual)
- Traffic Light (Green / Amber / Red — formula using Direction and Significance)
- Commentary Prompt (auto-generated: "[Metric] [direction] by [X]% [significance] [seasonal note]")
- Analyst Attribution (free-text input — analyst enters the 'why')

Significance classification formula:
```
IF ABS(Percentage Change) < Materiality_Threshold → "Routine"
IF four_plus_periods_available AND ABS(Pct_Change) > [SD of prior movements] × 2 → "Statistically Significant"
IF ABS(Percentage Change) ≥ Significance_Threshold → "Flagged — Material"
ELSE → "Notable"
```

### 4. Trend_Panel
For each metric: a four-plus quarter time series view.

Columns: Metric Name, [Q-4], [Q-3], [Q-2], [Q-1], [Current Q], Trend Classification, Chart

Trend Classification (formula):
```
IF all four prior values trending same direction → "Consistent Trend — [Up/Down]"
IF current reverses prior three-quarter trend → "Trend Reversal"
IF current quarter is outlier against stable prior periods → "Spike / Dip"
ELSE → "Mixed"
```

Mini sparkline chart per row (Excel SPARKLINE) showing the four-plus quarters in the correct colour (green or red based on direction convention).

### 5. Domain_Summaries
One summary section per business domain (Financial, Membership, Claims, Operations, Customer Experience).

Each domain section shows:
- Domain name and overall status (Positive / Mixed / Concern)
- Metric count: Improved / Declined / Unchanged / Routine
- Most significant movement (metric name, change, significance)
- Domain-level commentary prompt (structured for analyst to complete)

### 6. Executive_Commentary
Board-pack ready narrative. Structured sections:

**Header**: period, date, author
**Overall Assessment**: two sentences summarising the quarter
**Domain-by-Domain Summary**: bullet points per domain, four metrics maximum per domain, material movements only
**Metrics Requiring Board Attention**: table of any Critical or APRA-reportable metric that moved materially
**Seasonality Note**: any metrics where seasonal effects confound the QoQ comparison
**Prior Period Restatements**: any one-time items in current or prior period affecting comparability
**Outlook**: analyst-written (prompted field)

Formatting: A4 landscape, table borders, 11pt Aptos, ready to copy into board pack.

### 7. Industry_Benchmark_Comparison
If APRA quarterly statistics are available for the current period:

Columns:
- Metric
- Medibank Current Quarter
- Industry Average (from APRA)
- Medibank vs Industry (ABS and % difference)
- Medibank Rank / Position (top quartile / median / bottom quartile if available)
- Movement vs Prior Quarter (Medibank)
- Movement vs Prior Quarter (Industry)
- Relative Performance Change (is Medibank gaining or losing ground vs industry?)

### 8. Significance_Filter_View
Filtered dashboard: user selects filter criteria and sees only items matching:
- Significance: All / Significant Only / Flagged Material / Routine Only
- Direction: All / Improved / Declined
- Domain: All / Financial / Membership / Claims / Operations / Customer

Driven by Excel slicers or dropdown validation. All formulas reference live tblQoQ data.

### 9. Checks
PASS / WARN / FAIL:
- All formula cells in tblQoQ contain formulas (FAIL if any are manually typed values)
- All metrics in tblMetrics have a Direction Convention (WARN if any blank)
- Current quarter values are all populated (WARN for any TBC)
- Percentage change denominator check: no division by zero without N/A handling (FAIL if any #DIV/0!)
- Trend classification has at least three prior periods to be valid (WARN if fewer)
- Commentary Inputs section has analyst attribution for all Significant / Flagged Material items (WARN if any blank)

### 10. Read_Me
- How to add a new quarter (add a column to Period_Data)
- How to add a new metric (add a row to tblMetrics)
- How to update the APRA benchmark data
- How to write the commentary (explain the template: size → direction → significance → attribution → outlook)
- Seasonality calendar for Australian PHI (which quarters to expect which movements)
- Direction convention coding and why it matters
- Significance vs materiality distinction
- Limitation: QoQ analysis cannot distinguish seasonal effects from genuine trend changes without longer history

## Calculation standards
Percentage change:
```
Pct_Change = IF(Prior_Value = 0, "N/A", (Current_Value − Prior_Value) / ABS(Prior_Value))
```

Traffic light:
```
IF Significance = "Routine" → Grey (no colour)
IF Direction = "Improved" AND Significance ≠ "Routine" → Green
IF Direction = "Declined" AND Significance ≠ "Routine" → Red
IF Direction = "Unchanged" → Yellow
```

Standard deviation of prior period movements (for significance):
```
SD_Prior = STDEV(prior_period_QoQ_changes)   [requires 4+ data points]
Sig_Test = IF(ABS(Current_Pct_Change) > 2 × SD_Prior, "Statistically Significant", "Within normal variation")
```

All threshold references via INDEX/MATCH on tblMetrics[Metric ID].

## Formatting standard
- Green / Amber / Red traffic lights are cell fill on the Traffic Light column only — not applied broadly across tables
- Sparkline charts: green for Higher is Better metrics trending up; red for trending down (inverted for Lower is Better)
- Executive_Commentary sheet: table borders, no fill colour (for printing)
- Numbers: percentage changes to `0.0%`; AUD to `$#,##0` (thousands) or `$0.0"m"` (millions) depending on scale; Counts to `#,##0`
- Dates in Trend_Panel headers: `QN FYxx` format (e.g. Q4 FY26)

## Quality gates

### Formula gate
- No percentage change, absolute change, or direction figure is manually typed.
- All traffic lights are formula-driven from Direction Convention and Significance.
- N/A is shown wherever the denominator is zero or absent — no #DIV/0! errors.

### Commentary gate
- Every Significant or Flagged Material metric has an analyst attribution in Commentary Inputs.
- Commentary follows the template structure (size → direction → significance → attribution → outlook).
- No routine movements are included in the executive section without a specific reason.

### Trend gate
- Trend_Panel contains at least two periods.
- Trend Classification is present for all metrics.

## Failure modes to prevent
Never:
- apply green = up, red = down universally — direction convention must match each metric's nature;
- type percentage changes or absolute changes manually — all must be formula-driven;
- divide by zero for period changes without N/A handling;
- call every movement Significant — reserve that label for statistically or materially significant changes;
- omit the seasonality caveat for Q4 metrics that are expected to move due to the April premium increase;
- write executive commentary that only states "increased by X%" without attribution;
- show a trend panel with fewer than four periods and call the trend confirmed — note limited history.

## Recommended prompts
- `@qoq-trend-analyser Build a QoQ analysis for Q4 FY26 vs Q3 FY26 using this data. I need the executive commentary section ready for the board pack by Friday.`
- `@qoq-trend-analyser Our hospital benefit ratio deteriorated by 4.2% this quarter. Explain the significance, add the seasonality context, and prepare the attribution section for me to complete.`
- `@qoq-trend-analyser Add Q1 FY27 data to the existing trend model and update the trend classification for all metrics — I want to see which metrics have now reversed their prior trend.`
- `@qoq-trend-analyser Load the APRA industry benchmarks for Q4 FY26 and show me where Medibank is gaining or losing ground relative to industry.`

## Optional collaboration with related skills
- Use `@regulatory-submission-checker` to ensure APRA submission data is consistent with the QoQ trends before lodgement — a large QoQ variance in the submission data that is not mirrored in the management report warrants investigation;
- Use `@anomaly-detector` before trend analysis to confirm that extreme metric movements are genuine period trends, not caused by data quality issues in one period.

## Sources and references
**Statistical methods:**
- Box, G.E.P. and Jenkins, G.M. (1970). Time Series Analysis. Standard reference for trend vs. noise distinction in time series.
- Campbell, J.Y., Lo, A.W., MacKinlay, A.C. (1997). The Econometrics of Financial Markets. Significance testing framework.

**Australian health insurance industry benchmarks:**
- APRA Quarterly Private Health Insurance Statistics: Published quarterly — provides industry aggregate data for benefit ratios, membership counts, and premium levels.
- APRA Annual Private Health Insurance Statistics: Full-year benchmarks for MER, lapse rates, and capital adequacy.
- Private Healthcare Australia (PHA) quarterly reports: Industry body aggregate performance data.
- AIHW Australian Institute of Health and Welfare — Australian Hospital Statistics: Seasonal patterns in hospital admissions.

**Medibank-specific public data:**
- Medibank Private Limited Half-Year and Full-Year Results Presentations (ASX: MPL): Published KPI definitions and historical data for benchmark validation.
- Medibank Annual Reports (2022–2025): Long-run trend data for portfolio-level metrics.
