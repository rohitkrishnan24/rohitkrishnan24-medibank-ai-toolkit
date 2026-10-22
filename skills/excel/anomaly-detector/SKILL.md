---
name: anomaly-detector
description: Use when asked to detect anomalies, identify statistical outliers, flag suspicious claim patterns, or review data for potential errors or fraud indicators in a Medibank dataset. Applies a multi-method statistical detection pipeline (Z-score, IQR, Benford's Law, peer group comparison, temporal analysis) and delivers an evidence-ranked Anomaly Report with severity classification, recommended actions, and an audit-ready detection log. Do not use as a fraud determination — this skill flags for investigation, not for enforcement action.
---

# Detect Anomalies and Statistical Outliers in Medibank Data

## Purpose
Apply a structured, multi-method statistical anomaly detection pipeline to any Medibank dataset — provider claims, member data, payment records, or utilisation reports — to identify values that are statistically unusual, internally inconsistent, or consistent with known fraud and error patterns. Deliver a ranked Anomaly Report that enables the Finance, Risk, Audit, or Special Investigations team to prioritise their review effort, with every flag supported by a quantified deviation from baseline and a clear rationale. The workbook flags for investigation — it does not make fraud determinations or recommend enforcement actions.

## Non-negotiable outcome
Do not produce a simple colour-coded highlight. Build a complete anomaly detection workbook with a configurable detection engine, method-by-method evidence log, and a prioritised investigation queue.

The workbook is complete only when:
- at least four detection methods are applied (Z-score, IQR, peer group comparison, and Benford's Law for billing amounts) and each method's findings are separately logged with method name and reference code;
- every flagged item has a deviation score — not just a flag colour — showing quantitatively how far the value deviates from the baseline and what method identified it;
- a Composite Risk Score combines evidence from multiple methods (an anomaly flagged by three methods is higher risk than one flagged by one);
- severity tiers are formula-driven from configurable thresholds: Critical (possible systemic issue or fraud indicator), Major (warrants investigation), Minor (statistical outlier may be legitimate);
- a Detection_Log sheet records every flagged item with: method, baseline used, observed value, deviation score, severity, row reference, and recommended next action;
- a Benford's Law analysis is included for billing amount first digits (applies to datasets with ≥ 100 records); departure from Benford's distribution is a recognised fraud analytics indicator used by major audit firms;
- peer group comparison is done within natural grouping variables (provider specialty, MBS item group, hospital, department) so outliers are identified relative to their peers, not the entire population;
- a temporal analysis flags claims or payments that cluster in unusual time patterns (e.g. high volume on public holidays, late-month spikes, round-dollar clustering);
- a minimum 10-row dataset check is applied — detection methods lose statistical validity below this threshold and must not be applied;
- an Investigation_Queue sheet lists Critical and Major anomalies in priority order with assigned owner, investigation status, and resolution notes.

## Operating principles
1. Anomaly detection is a flagging and prioritisation tool, not a fraud determination. Every flag is a hypothesis to be tested, not a finding. Use clear language: "flagged for investigation", "statistically unusual", "warrants review" — never "fraud detected" or "confirmed error."
2. Apply multiple independent methods. An anomaly flagged by a single method is less significant than one confirmed by two or more. The Composite Risk Score rewards convergence of evidence.
3. Statistical baseline must be appropriate for the data. Use peer group baselines (same MBS item, same provider specialty, same hospital tier) where available — population-wide baselines are appropriate only when no natural grouping variable exists. Document which baseline was used for every flag.
4. Z-score and IQR methods have different strengths. Z-score assumes approximate normality; IQR is robust to extreme skew. Healthcare cost data is typically right-skewed — IQR is often more appropriate. Apply both, and flag discrepancies where only one method triggers.
5. Benford's Law: in large, naturally occurring datasets, leading digits follow a predictable distribution (1 appears ~30% of the time; 9 appears ~5%). Billing amounts that deviate significantly from Benford's distribution (using Chi-square test or Mean Absolute Deviation) are a recognised fraud analytics indicator. Apply only when: dataset has ≥ 100 records; values are not constrained to a narrow range (e.g. fixed-price items would not follow Benford's); values are not rounded to standard fee schedules.
6. Round-number clustering: an unusually high proportion of claim amounts ending in .00 or .50 suggests manual entry rather than service-based billing. Flag when the proportion of round numbers exceeds the configurable threshold (default: 30% is notable, 50% warrants investigation).
7. Temporal patterns: claims clustering on weekends, public holidays, or the last day of a billing period may indicate backdating. Claims with a payment date before the service date are impossible and are Critical flags.
8. Peer group outliers: for each natural group (e.g. all claims for MBS item 23010), flag any individual provider whose average claim amount exceeds the group mean by more than [Peer_SD] standard deviations. For groups with fewer than 5 providers, use IQR method only.
9. Volume anomalies: a provider billing significantly more claims than their peer group in a given period may indicate unbundling (splitting one service into multiple billable items) or inflated service volumes. Flag providers in the top 5% of claim count relative to their peer group.
10. Duplicate detection: exact duplicates (same Claim ID twice) are Critical. Near-duplicates (same provider + same date + same amount ± tolerance) are Major. Include in the anomaly detection pipeline even if a separate reconciliation skill is available.
11. Never delete flagged records. Flag, score, and route to Investigation_Queue. All source data remains intact.
12. Do not apply detection methods to columns with fewer than 10 unique values — categorical columns (status codes, department names) do not support statistical outlier detection.
13. A high Composite Risk Score does not mean the item is fraudulent. It means it warrants investigation. Some high-risk scores will have legitimate explanations (e.g. a complex surgical procedure that legitimately costs more than the peer group average). Document expected rates of false positives in the Read_Me.
14. Provider reputation cannot substitute for evidence. A provider known to the team as "problematic" still requires documented statistical evidence for each flag. The anomaly detector treats all providers equally.
15. Date anomalies: beyond payment-before-service, check: service dates on public holidays (not inherently suspicious but worth noting), service dates in the future, and service dates that predate the provider's known registration or contracting date.
16. MBS unbundling: when multiple MBS items for the same patient on the same date by the same provider appear together in combinations that are prohibited or restricted under MBS rules, flag them. Note: this requires the MBS co-claiming rules reference table; without it, flag the pattern but note the reference table is not loaded.
17. The detection threshold must be configurable. Different audit contexts have different sensitivity requirements. An initial fraud screen may use a 3 SD threshold; a deep-dive investigation may use 2 SD. These thresholds live in the Assumptions sheet.
18. Communicate the expected false-positive rate. At a 3 SD threshold, approximately 0.3% of legitimate records will be flagged. For a dataset of 10,000 claims, that is ~30 false positives at 3 SD, or ~450 at 2 SD. Tell the user this upfront so they can set investigation resources appropriately.

## First response and ambiguity handling
Before detecting, inspect the dataset and confirm:
- primary numeric column(s) for outlier detection;
- natural grouping variable(s) for peer group comparison (Provider ABN, MBS Item Group, Hospital, Department);
- whether this is a billing/claims dataset (Benford's Law applicable), member data (volume anomalies), payment data (temporal patterns), or another type;
- the outlier detection threshold (default 3 SD for initial screen; 2 SD for deep-dive);
- whether a prior-period dataset is available for trend comparison;
- the minimum Benford's Law dataset size check (default 100 records; confirm if dataset is smaller).

If grouping variables are absent, apply population-wide methods only and note this reduces detection specificity. If fewer than 10 records exist in a group, apply IQR only and flag the small sample size.

## Workbook architecture

### 1. Source_Data
Protected copy of the raw input data. No modifications. Header block: source, date, record count. Anomaly detection references this sheet by row number.

### 2. Detection_Config (Assumptions)
Excel table `tblConfig`. All detection parameters configurable here.

| Config ID | Method | Parameter | Default | Unit | Notes |
|-----------|--------|-----------|---------|------|-------|
| C001 | Z-score | Outlier threshold | 3.0 | SD | Lower = more sensitive |
| C002 | IQR | Outlier multiplier | 1.5 | × IQR | Standard Tukey fence |
| C003 | IQR | Extreme outlier multiplier | 3.0 | × IQR | Extreme fence |
| C004 | Benford | Chi-square significance level | 0.05 | p-value | Standard significance |
| C005 | Round-dollar | High proportion threshold | 30% | % | Flag for review |
| C006 | Round-dollar | Suspicious proportion threshold | 50% | % | Warrants investigation |
| C007 | Peer group | Minimum peers for SD method | 5 | count | Below this, use IQR only |
| C008 | Peer group | SD threshold for peer outlier | 3.0 | SD | Configurable |
| C009 | Volume | Peer volume outlier percentile | 95th | % | Top 5% flagged |
| C010 | Temporal | Weekend/holiday flag | Yes | Y/N | Disable for 24×7 providers |
| C011 | Temporal | End-of-period spike window (days) | 3 | days | Last N days of billing period |
| C012 | Duplicate | Near-duplicate amount tolerance | 1.00 | AUD | |
| C013 | Minimum dataset | Minimum rows for SD/Z methods | 10 | rows | Below this, output warning only |
| C014 | Minimum dataset | Minimum rows for Benford's Law | 100 | rows | |
| C015 | Composite score | Method weight — Z-score | 1 | weight | |
| C016 | Composite score | Method weight — IQR | 1 | weight | |
| C017 | Composite score | Method weight — Benford | 2 | weight | Higher weight = stronger signal |
| C018 | Composite score | Method weight — Peer group | 2 | weight | |
| C019 | Composite score | Method weight — Volume | 1 | weight | |
| C020 | Composite score | Method weight — Temporal | 1 | weight | |
| C021 | Composite score | Critical threshold (weighted sum) | 4 | points | |
| C022 | Composite score | Major threshold (weighted sum) | 2 | points | |

### 3. Detection_Log
Excel table `tblLog`. One row per detection event (one item may appear multiple times if flagged by multiple methods).

Columns:
- Flag ID (auto-incrementing)
- Source Row Number
- Claim ID / Record ID
- Detection Method (Z-score / IQR / Benford / Peer Group / Volume / Temporal / Duplicate / Round-Dollar / Cross-Field)
- Method Config Reference (C001, C002, etc.)
- Field Analysed
- Observed Value
- Baseline Value (group mean, median, or expected Benford proportion)
- Deviation Score (Z-score, or × IQR multiple, or Benford MAD score, or % above peer, as appropriate)
- Deviation Direction (High / Low / Pattern)
- Flag Severity (Critical / Major / Minor)
- Flag Description (plain English statement: "Claim amount of AUD 45,000 is 4.2 SD above the MBS item 23010 peer group mean of AUD 8,200")
- Recommended Next Action

### 4. Anomaly_Summary
Per-record summary: one row per unique source record, aggregating all method flags.

Columns:
- Source Row Number
- Claim ID / Record ID
- Provider ABN
- Provider Name
- Service Date
- Claim Amount (or primary metric)
- Methods Triggered (comma-separated list of method codes)
- Method Count (number of methods that flagged this record)
- Composite Risk Score (weighted sum of triggered methods from tblConfig)
- Overall Severity (formula: IF(Composite ≥ C021, "Critical", IF(Composite ≥ C022, "Major", "Minor")))
- Brief Rationale (concatenation of Flag Descriptions)

Sort by Composite Risk Score descending.

### 5. Benford_Analysis
Standalone Benford's Law analysis for the primary billing amount column.

Display:
- Observed frequency of each leading digit (1–9)
- Expected Benford frequency (formula: LOG10(1 + 1/d) for digit d)
- Deviation (Observed − Expected)
- Chi-square contribution per digit
- Total Chi-square statistic
- p-value (approximate, via CHISQ.DIST.RT)
- Mean Absolute Deviation (MAD) from Benford's distribution
- Conclusion: CONSISTENT WITH BENFORD / NOTABLE DEVIATION / SIGNIFICANT DEPARTURE
- Bar chart comparing observed vs expected frequencies
- Note: "Significant departure from Benford's distribution warrants investigation but does not confirm fraud. It indicates the billing amounts do not have the natural distribution expected from organically priced services."

Apply Benford's only if row count ≥ C014. If below threshold, show a warning: "Benford's Law requires at least [C014] records. Dataset has [N] records. Analysis not applied."

### 6. Peer_Group_Analysis
For each natural grouping variable:
- Group name and key
- Group member count
- Group mean, median, SD, min, max
- Each record's deviation from group mean (in SD)
- Records exceeding C008 threshold: highlighted and counted
- Top 10 peer outliers by deviation score

### 7. Temporal_Analysis
Date-pattern analysis:
- Service date distribution by day of week (chart: bar showing volume Mon–Sun)
- Service date distribution by month
- End-of-period spike: count of records in last C011 days of each billing period vs average for the period
- Weekend/public holiday records: count and % of total (flag if > 20% of records are on weekends for a business-hours provider)
- Payment before service: COUNT of records where Payment Date < Service Date (Critical flag — all should be investigated)
- Round-dollar distribution: count and % of amounts ending in .00, proportion ending in .50, histogram of last two digits

### 8. Investigation_Queue
Filtered view of Critical and Major items from Anomaly_Summary, sorted by Composite Risk Score descending.

Columns from Anomaly_Summary plus:
- Date Added to Queue
- Assigned To (blank — for investigation team)
- Investigation Status (dropdown: Not Started / In Progress / Cleared — Legitimate / Confirmed Error / Referred to SIU)
- Investigation Notes
- Date Resolved

Summary panel above queue: total open Critical, total open Major, total resolved, total referred to SIU.

### 9. Statistics_Panel
Baseline statistics calculated from the dataset, used by all detection methods.

For each numeric column:
- Count, Mean, Median, Mode, SD, Variance, Min, Max, Q1, Q3, IQR
- Lower IQR fence (Q1 − 1.5 × IQR)
- Upper IQR fence (Q3 + 1.5 × IQR)
- Lower extreme fence (Q1 − 3.0 × IQR)
- Upper extreme fence (Q3 + 3.0 × IQR)
- Skewness coefficient
- Note: "Right-skewed distributions (skewness > 1.0) — IQR method preferred over Z-score"

For peer groups: same statistics broken down by each grouping variable.

### 10. Read_Me
- Purpose and scope
- Methods applied and their bases
- Expected false-positive rate at current threshold settings
- How to adjust thresholds in Detection_Config
- How to interpret Benford's Law result
- How to mark an investigated item in Investigation_Queue
- Definitions: Z-score, IQR, Benford's Law, Composite Risk Score, Peer Group
- Limitations: this skill detects statistical anomalies, not fraud; investigation is required before any action; Benford's Law is a screening tool only
- Legal note: all flagged items must be investigated by authorised personnel before any recovery, referral, or enforcement action is initiated

## Detection workflow

### Step 1: Dataset inspection
- Record row count; confirm ≥ C013 for SD methods, ≥ C014 for Benford's
- Identify numeric columns, categorical columns, date columns
- Identify natural grouping variable(s)
- Compute Statistics_Panel

### Step 2: Z-score method
- For each numeric value in each column: Z = (Value − Mean) / SD
- Flag: |Z| > C001
- Log to tblLog with Z-score as deviation score

### Step 3: IQR method
- For each numeric value: compute fence distance ((Value − Upper Fence) / IQR or (Lower Fence − Value) / IQR)
- Flag standard outlier: outside 1.5×IQR fences
- Flag extreme outlier: outside 3.0×IQR fences
- Log to tblLog with IQR multiple as deviation score

### Step 4: Peer group comparison
- For each group with ≥ C007 members: apply SD method within group
- For each group with < C007 members: apply IQR method within group
- Log to tblLog with within-group Z-score or IQR multiple

### Step 5: Volume analysis
- Count records per provider per period
- Calculate peer volume percentile for each provider
- Flag providers at or above C009 percentile

### Step 6: Benford's Law (billing amounts only)
- Extract first significant digit from each billing amount
- Calculate observed distribution
- Compare to expected distribution
- Calculate chi-square and MAD
- Log overall result and flag individual digit anomalies

### Step 7: Temporal analysis
- Check payment date < service date (Critical flag, record-level)
- Check service dates on weekends and public holidays
- Check end-of-period clustering
- Check round-dollar proportion

### Step 8: Duplicate detection
- Flag exact duplicates (same Claim ID)
- Flag near-duplicates (same Provider ABN + Service Date + Amount ± C012)

### Step 9: Composite scoring and triage
- Calculate Composite Risk Score per record from tblLog
- Assign Overall Severity
- Populate Investigation_Queue for Critical and Major items
- Sort by Composite Risk Score descending

## Calculation standards
All threshold references via INDEX/MATCH on tblConfig[Config ID]:
```
[C001] = INDEX(tblConfig[Default], MATCH("C001", tblConfig[Config ID], 0))
Z_Score = (Value − Mean) / SD
IQR_Multiple = (Value − Upper_Fence) / IQR   [positive = above fence]
Composite_Score = SUM(method_weight × IF(method_triggered, 1, 0))
```

## Formatting standard
- Critical: dark red fill, white text
- Major: amber fill
- Minor: yellow fill
- Cleared (investigation resolved legitimate): green fill
- Confirmed error: orange fill (resolved but was real)
- Benford's chart: blue observed bars, grey expected line, clear deviation labelling
- Statistics_Panel: light grey fill (read-only reference)

## Quality gates

### Coverage gate
- All four mandatory methods (Z-score, IQR, Benford, Peer Group) have been applied or their non-application is documented with reason.
- Every flagged item has a deviation score, not just a flag.
- tblLog has at least one entry per method (or a "0 records triggered" entry confirming the method ran).

### Statistical gate
- Z-score method was not applied to columns with fewer than C013 records.
- Benford's Law was not applied to columns with fewer than C014 records or to constrained price-list columns.
- Peer group SD was not applied to groups with fewer than C007 members.

### Integrity gate
- Source data is unmodified and protected.
- No record has been deleted from the source based on an anomaly flag.
- Investigation_Queue status is not pre-set — all items start as "Not Started."

## Failure modes to prevent
Never:
- call a flagged item "fraud" — flag for investigation only;
- apply Z-score to heavily skewed distributions without noting the limitation and supplementing with IQR;
- apply Benford's Law to constrained price-list amounts (all from a fixed schedule) — these legitimately deviate from Benford's;
- delete or modify source data based on anomaly flags;
- apply detection methods to categorical text columns (status codes, department names);
- set Composite Risk Score weights equal for all methods — Benford's and peer group are stronger signals than Z-score alone;
- produce an investigation queue without a "Not Started" default status — never pre-assign status;
- omit the false-positive rate estimate from the Read_Me.

## Recommended prompts
- `@anomaly-detector Scan this quarterly claims extract for statistical outliers and billing pattern anomalies. Apply all methods. Show me the Critical items first and give me an investigation queue.`
- `@anomaly-detector Run a Benford's Law analysis on the billing amounts in this extract. Flag any significant departure and tell me which digit ranges are overrepresented.`
- `@anomaly-detector I suspect some providers are unbundling claims. Run the peer group comparison for all MBS item groups and show me which providers are billing significantly more than their peers.`
- `@anomaly-detector Add a temporal analysis to this dataset — I want to see if there's an unusual spike in claims in the last 3 days of each month.`

## Optional collaboration with related skills
- Run `@claims-data-cleaner` before anomaly detection to remove data quality noise that would otherwise generate false positives;
- Run `@data-validator` to confirm data integrity before running statistical methods — outlier detection on dirty data is unreliable;
- Hand confirmed high-risk providers to `@provider-payment-reconciler` for a full reconciliation of their billing history.

## Sources and references
**Statistical methods:**
- Tukey, J.W. (1977). Exploratory Data Analysis. IQR fence method.
- Benford, F. (1938). "The Law of Anomalous Numbers." Proceedings of the American Philosophical Society. Theoretical basis for leading-digit analysis.
- Nigrini, M.J. (2012). Benford's Law: Applications for Forensic Accounting, Auditing, and Fraud Detection. Wiley. Standard reference for Benford's application in audit.

**Australian audit and fraud analytics:**
- Australian National Audit Office (ANAO): Data analytics in audit — guidance on statistical sampling and outlier detection.
- APRA Prudential Standard CPS 234 (Information Security) and related: Fraud risk management obligations for financial services entities.
- Australian Institute of Criminology: Healthcare fraud typologies in Australian health insurance.

**Health insurance-specific:**
- Department of Health and Aged Care — MBS Compliance: Guidance on MBS item co-claiming rules and audit triggers.
- Services Australia — Medicare fraud and non-compliance: Published patterns and red flags for Medicare claim anomalies (applicable to private health insurance audit context by analogy).
