---
name: provider-payment-reconciler
description: Use when asked to reconcile provider billing against approved benefits and payments, identify overbilling, underpayment, overpayment, or duplicate payment issues, or produce a financial discrepancy report for Medibank's finance or audit team. Produces a fully formula-driven, auditable reconciliation workbook with discrepancy classification, financial exposure summary, provider-level analysis, and a recoveries pipeline. Do not use for posting payments to a financial system, issuing recovery demands, or modifying payment records.
---

# Reconcile Medibank Provider Billing Against Approved Payments

## Purpose
Ingest Medibank provider billing data and approved payment records, calculate variances at the claim and provider level, classify discrepancies by type and financial exposure, and deliver an auditable reconciliation workbook ready for review by the Finance, Audit, or Claims Operations team. The workbook must separate calculation from data entry, reference all thresholds from a single Assumptions sheet, distinguish confirmed discrepancies from differences explainable by legitimate business rules (e.g. partial payment by agreement, known-gap arrangements, benefit assignment), and produce a Recoveries Pipeline that can feed into a formal recovery workflow.

## Non-negotiable outcome
Do not stop at a variance table or colour-coded summary. Build a complete, formula-driven Excel workbook.

The workbook is complete only when:
- every claim line has a calculated Billing Variance, Benefit Variance, Payment Variance, and Total Exposure figure, all formula-driven from input columns;
- discrepancy type is assigned to each line: Overbilling / Overpayment / Underpayment / Duplicate Payment / Matched / Partial Payment (Legitimate) / Awaiting Documentation;
- known-gap, no-gap, and Medical Gap Scheme arrangements are modelled as a separate rule layer so that legitimate below-schedule payments are not flagged as underpayments;
- duplicate detection uses a three-level hierarchy (exact invoice match; same Provider ABN + Service Date + Amount; same Provider + Date within tolerance);
- a provider-level summary ranks providers by total financial exposure and flags High Discrepancy Providers (configurable threshold, default ≥ 5% discrepancy rate or ≥ AUD 10,000 total exposure);
- a Recoveries Pipeline sheet lists every confirmed overbilling and duplicate payment with status, age (days since service date), responsible owner, and estimated recovery value;
- a financial exposure summary distinguishes: overbilling recovery potential, overpayment recovery potential, underpayment liability, and duplicate payment exposure;
- all thresholds (overbilling tolerance, overpayment threshold, duplicate detection tolerance, high-discrepancy provider threshold) are in a single Assumptions sheet and drive all formulas;
- a Checks sheet confirms the reconciliation closes (sum of all variances reconciles to the net balance due to/from providers).

## Operating principles
1. Preserve source data. Billing records and payment records are input-only. Create `Billing_Raw` and `Payments_Raw` sheets and protect them immediately. All reconciliation work happens on derived sheets.
2. Distinguish confirmed discrepancy from unexplained difference. A variance between billed and approved amounts may be: (a) a known-gap arrangement where the provider has agreed to accept a below-MBS-fee payment; (b) a benefit assignment where the member has paid a gap directly; (c) a partial payment pending documentation; (d) a genuine overbilling requiring recovery; or (e) a data quality issue where amounts do not match due to rounding or system differences. Apply a classification rule to each before calling it a discrepancy.
3. Overbilling threshold: Billed Amount exceeds Approved Amount by more than the configured tolerance (default: 10% or AUD 50, whichever is greater). Do not call a smaller variance overbilling — label it Minor Rounding Difference.
4. Overpayment: Paid Amount exceeds Approved Amount by more than AUD [Overpayment_Tolerance] (default AUD 1.00 accounting for rounding). These are your highest-priority recovery items.
5. Underpayment: Paid Amount is less than Approved Amount by more than AUD [Underpayment_Tolerance] (default AUD 1.00). These represent financial liability to providers. Distinguish between: deliberate underpayments (partial payments by documented agreement) and unintentional underpayments (processing errors).
6. Duplicate payments: any two records where the same provider was paid twice for the same service. Use a three-level hierarchy:
   - Level 1: identical Invoice ID paid twice — definitive duplicate;
   - Level 2: same Provider ABN + Service Date + MBS Item + Amount within AUD [Dup_Tolerance] — near-duplicate, high confidence;
   - Level 3: same Provider + Date (within ±[Dup_Date_Window] days) + Amount within tolerance — suspicious, requires manual review.
7. The Medical Gap Scheme (MGS) and Access Gap Cover arrangements under the Private Health Insurance Act 2007 define maximum permitted gaps. Claims where the gap fee is within the legislated maximum for the hospital tier are compliant — do not flag as overbilling.
8. No-gap arrangements: some providers have contracted to zero-gap billing under Medibank's hospital agreements. Any gap fee from a no-gap provider should be flagged as a contract violation, not a standard overbilling. These carry a higher severity level.
9. Provider flags must be evidence-based. A High Discrepancy Provider flag requires at least [Min_Claims] claims (default 3) in the period with a discrepancy rate above the threshold. Do not flag a provider based on one anomalous claim.
10. All financial exposures must be shown at three levels: claim line, provider total, and portfolio total. The reconciliation summary closes when the portfolio total variance equals the sum of all claim-line variances.
11. Recovery priority must be modelled: overpayment has the highest recovery urgency (Medibank paid too much); overbilling has the next highest (provider billed too much and Medibank may have approved it); underpayment has lower urgency but still creates provider liability risk.
12. Age of discrepancy matters. A duplicate payment identified within 30 days has a high recovery probability. One identified after 180 days has a significantly lower probability — note the age and recovery probability estimate in the Recoveries Pipeline.
13. HICAPS and Eclipse claims have a known processing timeline: HICAPS claims are typically processed same day; Eclipse claims within 2–5 business days. Payment Date should be consistent with these windows. Flag claims where Payment Date is more than [Processing_Window] days after Service Date (configurable; default 60 days) as potentially stale or incorrectly dated.
14. Do not use provider name alone as a matching key — the same provider may operate under multiple entities. Always use Provider ABN as the primary matching key, with Provider Name as a secondary check for anomalies (e.g. ABN and name don't match).
15. Partial payments pending documentation are a common legitimate pattern. These should be classified as Awaiting Documentation, not Underpayment, when there is evidence of a documented instalment or pending information request. Include a Documentation Status field in the reconciliation table.
16. Foreign currency: all amounts in this workbook are AUD. If any billing record contains a non-AUD currency symbol or code, flag it as a Critical data issue before attempting reconciliation.
17. Never net positive and negative variances against each other in a single summary figure without also showing them separately. An organisation may have AUD 500k in overpayments and AUD 450k in underpayments — netting to AUD 50k hides both exposures.
18. The recoveries pipeline is a living document. Mark the status of each recovery item: Identified / Under Review / Recovery Initiated / Recovered / Written Off. Update the pipeline at each reconciliation run.
19. GST treatment: some provider payments include GST components (10%); others are GST-exempt health services. Do not apply GST adjustments unless the user confirms the billing data includes GST amounts. Flag the GST status as TBC in the Assumptions sheet if not confirmed.

## First response and ambiguity handling
Before building, inspect the billing and payment data and infer as much as possible from column headers, amount patterns, and provider identifiers.

Resolve these items when available:
- billing period (start and end date);
- payment system used (HICAPS, Eclipse, manual payment);
- whether MGS and no-gap arrangements are applicable (affects overbilling rules);
- whether the billing file contains GST-inclusive amounts;
- the overbilling tolerance (default 10% or AUD 50);
- the processing window for flagging stale payments (default 60 days);
- whether a provider ABN reference file is available;
- whether prior-period reconciliations exist (to carry forward unresolved items);
- the recoveries pipeline owner (who is responsible for follow-up).

If only a billing file is provided with no payment data, build the workbook structure with the payment columns as blank inputs, flag as incomplete, and produce a partial reconciliation showing billing patterns only.

## Workbook architecture

### 1. Billing_Raw
Protected verbatim copy of provider billing data. Header block: source system, extract date, period, record count. Columns expected:
- Invoice ID
- Provider Name
- Provider ABN
- Service Date
- MBS Item Number
- Billed Amount (AUD)
- Invoice Status
- Member ID

### 2. Payments_Raw
Protected verbatim copy of payment records. Columns expected:
- Payment Reference
- Invoice ID (link key to Billing_Raw)
- Provider ABN
- Service Date
- Approved Amount (AUD)
- Paid Amount (AUD)
- Payment Date
- Payment Method
- Benefit Assignment Flag (Y/N — member paid gap directly)

### 3. Assumptions
Excel table `tblAssumptions` with these rows:

| ID | Assumption | Value | Unit | Status |
|----|------------|-------|------|--------|
| R001 | Overbilling tolerance % | 10% | % | Estimate |
| R002 | Overbilling tolerance minimum (AUD) | 50 | AUD | Estimate |
| R003 | Overpayment tolerance (AUD) | 1.00 | AUD | Estimate |
| R004 | Underpayment tolerance (AUD) | 1.00 | AUD | Estimate |
| R005 | Near-duplicate amount tolerance (AUD) | 1.00 | AUD | Estimate |
| R006 | Near-duplicate date window (days) | 1 | days | Estimate |
| R007 | High discrepancy provider rate threshold | 5% | % | Estimate |
| R008 | High discrepancy provider minimum exposure (AUD) | 10,000 | AUD | Estimate |
| R009 | Minimum claims for provider flag | 3 | count | Estimate |
| R010 | Payment processing window — HICAPS (days) | 1 | days | Confirmed |
| R011 | Payment processing window — Eclipse (days) | 5 | days | Confirmed |
| R012 | Stale payment flag threshold (days) | 60 | days | Estimate |
| R013 | High recovery probability window (days post-payment) | 30 | days | Estimate |
| R014 | Low recovery probability threshold (days post-payment) | 180 | days | Estimate |
| R015 | GST applicable | TBC | Y/N | TBC |
| R016 | Reconciliation period start | TBC | Date | TBC |
| R017 | Reconciliation period end | TBC | Date | TBC |

### 4. Reconciliation_Detail
Excel table `tblRecon`. One row per claim line (matched or unmatched).

Columns:
- Invoice ID
- Provider ABN
- Provider Name
- Service Date
- MBS Item Number
- Billed Amount (AUD) — from Billing_Raw
- Approved Amount (AUD) — from Payments_Raw
- Paid Amount (AUD) — from Payments_Raw
- Payment Date
- Billing Variance (formula: Billed − Approved)
- Benefit Variance (formula: Approved − Paid)
- Total Exposure (formula: Billed − Paid)
- Discrepancy Type (formula-assigned; see classification logic below)
- Discrepancy Severity (Critical / Major / Minor / None)
- Duplicate Level (1 / 2 / 3 / None)
- No-Gap Provider Flag (Y/N — from provider reference)
- MGS Compliance Flag (Compliant / Exceeds Limit / Not Applicable)
- Documentation Status (dropdown: Complete / Awaiting / N/A)
- Payment Age (days) (formula: Payment Date − Service Date)
- Stale Payment Flag (formula: IF(Payment Age > R012, "Stale", ""))
- Recovery Priority (High / Medium / Low / N/A)
- Notes

Discrepancy Type classification logic:
```
IF Duplicate Level = 1 → "Duplicate Payment — Confirmed"
ELSE IF Duplicate Level = 2 → "Duplicate Payment — Near-Match"
ELSE IF Duplicate Level = 3 → "Suspicious — Review Required"
ELSE IF Paid > Approved + R003 → "Overpayment"
ELSE IF No-Gap Provider Flag = Y AND Gap > 0 → "No-Gap Contract Violation"
ELSE IF Billed > Approved × (1 + R001) AND Billed − Approved > R002 → "Overbilling"
ELSE IF Paid < Approved − R004 AND Documentation Status ≠ "Awaiting" → "Underpayment"
ELSE IF Paid < Approved − R004 AND Documentation Status = "Awaiting" → "Partial — Awaiting Docs"
ELSE IF ABS(Billed − Approved) ≤ R002 → "Minor Rounding Difference"
ELSE → "Matched"
```

### 5. Provider_Summary
Excel table `tblProvider`. One row per unique Provider ABN.

Columns:
- Provider ABN
- Provider Name
- Total Claims
- Total Billed (AUD)
- Total Approved (AUD)
- Total Paid (AUD)
- Total Billing Variance (AUD)
- Total Payment Variance (AUD)
- Total Exposure (AUD)
- Discrepancy Rate (Total Exposure / Total Billed)
- Overbilling Count
- Overpayment Count
- Underpayment Count
- Duplicate Count
- No-Gap Violations
- High Discrepancy Flag (formula: IF(Discrepancy Rate ≥ R007 AND Total Claims ≥ R009 AND Total Exposure ≥ R008, "HIGH", ""))
- Provider Risk Rating (High / Medium / Low)

Sort by Total Exposure descending. Apply traffic-light conditional formatting on Provider Risk Rating.

### 6. Recoveries_Pipeline
Excel table `tblRecoveries`. One row per confirmed or probable recovery item.

Columns:
- Recovery ID
- Invoice ID
- Provider ABN
- Provider Name
- Discrepancy Type
- Recovery Amount (AUD)
- Service Date
- Days Since Service Date
- Recovery Probability (High / Medium / Low — driven by age)
- Status (Identified / Under Review / Recovery Initiated / Recovered / Written Off)
- Assigned To
- Date Opened
- Date Resolved
- Notes

Recovery Probability formula:
```
IF(Days Since Service Date ≤ R013, "High",
IF(Days Since Service Date ≤ R014, "Medium", "Low"))
```

Summary panel above the table:
- Total open recoveries (AUD)
- High probability (AUD)
- Medium probability (AUD)
- Low probability (AUD)
- Recovered this period (AUD)
- Written off (AUD)

### 7. Reconciliation_Summary
Organisation-level financial summary. All figures formula-driven from tblRecon.

- Total Billed (AUD)
- Total Approved (AUD)
- Total Paid (AUD)
- Net Billing Variance (Billed − Approved)
- Net Payment Variance (Approved − Paid)
- Net Total Exposure (Billed − Paid)
- By Discrepancy Type (separate lines for each type — DO NOT NET):
  - Overbilling exposure
  - Overpayment exposure
  - Underpayment liability
  - Duplicate Payment exposure
  - No-Gap Contract Violations
  - Minor Rounding Differences
- Reconciliation close check: NET_EXPOSURE = SUM(all type exposures) [PASS/FAIL]
- Provider count: matched / High Discrepancy / Medium / Low
- Recovery pipeline: open items count, total open AUD, recovered AUD

### 8. Checks
Mandatory checks with PASS / WARN / FAIL:
- Reconciliation closes: SUM(all variances) = Net Total Exposure (FAIL if difference > AUD 0.01)
- No tblRecon row is unclassified (FAIL if any Discrepancy Type is blank)
- All Invoice IDs in Payments_Raw matched to a Billing_Raw record (WARN if any orphan payments)
- All Invoice IDs in Billing_Raw matched to at least one payment (WARN if any unmatched billing records)
- No Critical discrepancy row has Documentation Status = blank (WARN if any)
- All Provider ABNs pass format check (11 digits) (WARN if any fail)
- Assumptions R016 and R017 are populated with dates (FAIL if either is TBC)
- GST status R015 is confirmed (WARN if TBC)

### 9. Read_Me
- Purpose and scope (reconciliation only, not payment posting)
- How to update with a new billing and payment extract
- How to adjust tolerance thresholds in Assumptions
- How to mark a recovery item as Recovered in tblRecoveries
- How to add a new no-gap provider to the reference
- Colour legend
- Definitions: Overbilling, Overpayment, Underpayment, Duplicate Payment, MGS, No-Gap
- Limitations: ABN matching is format-only; MGS compliance requires the current approved gap schedule

## Calculation standards
All thresholds reference tblAssumptions via MATCH/INDEX:
```
[R001] = INDEX(tblAssumptions[Value], MATCH("R001", tblAssumptions[ID], 0))
```

Portfolio reconciliation close check:
```
Close_Check = ABS(SUM(tblRecon[Total Exposure]) − tblSummary[Net Total Exposure])
Close_Status = IF(Close_Check <= 0.01, "PASS", "FAIL")
```

## Model build workflow

### Step 1: Inspect and load source files
Before building anything, review both the billing extract and the payment file. Identify the shared matching key — usually Invoice ID, but confirm this. Note the Provider ABN format carefully: HICAPS billing files sometimes include leading zeros that payment files strip, which breaks matching silently. Load Billing_Raw and Payments_Raw and protect both sheets immediately. Record the record counts, billing period, and payment system name in the Billing_Raw header block.

### Step 2: Populate Assumptions (tblAssumptions)
Set R016 and R017 (period dates) before doing anything else — without them, the Recovery Probability age calculation in tblRecoveries will produce wrong figures for any item that is close to the age thresholds. Confirm GST status (R015) with Finance — some provider invoices include GST on facility fees, others don't, and mixing the two distorts variance figures. Set overbilling tolerance (R001, R002) to match the tolerance already embedded in Medibank's payment approval rules so the reconciliation doesn't flag differences that the payment system itself accepted.

### Step 3: Build Reconciliation_Detail (tblRecon)
Match billing records to payment records by Invoice ID. For billing records with no payment match, create rows with blank payment columns and classify as Unmatched Billing. For payment records with no matching invoice, create rows with blank billing columns and classify as Orphan Payment — these are higher-risk items because there is no invoice to justify the payment and they should be reviewed before the reconciliation is complete. Apply the Discrepancy Type classification in the required order: Level 1 duplicates first, Level 2, Level 3, then Overbilling — only reach Overbilling after all duplicate checks have been ruled out.

### Step 4: Apply gap arrangement and contract compliance checks
Check each provider ABN against the no-gap reference list. Any gap fee from a contracted no-gap provider is a No-Gap Contract Violation, which carries higher severity than standard overbilling and may require immediate provider follow-up. For providers under MGS or AGC arrangements, confirm the gap is within the legislated maximum before classifying as overbilling. Skipping these checks produces misleading findings against providers who are operating within their arrangement terms.

### Step 5: Build Provider_Summary (tblProvider)
Aggregate tblRecon to the Provider ABN level. Apply the High Discrepancy Provider flag only when all three conditions are met simultaneously: discrepancy rate ≥ R007, claim count ≥ R009, and total exposure ≥ R008. One anomalous invoice should not brand a provider as high risk. Sort by Total Exposure descending. Confirm with Provider Relations before any external contact is initiated off the back of this output.

### Step 6: Build Recoveries_Pipeline (tblRecoveries)
Filter confirmed Overbilling, Overpayment, Confirmed Duplicate, and No-Gap Contract Violation rows into tblRecoveries. Calculate Recovery Probability from the age formula using R013 and R014. Set all Status fields to "Identified" — do not pre-assign any item to Under Review or a later stage. Add the summary panel above the table showing open recovery totals by probability tier.

### Step 7: Build Reconciliation_Summary and run the close check
Build the summary with each discrepancy type on a separate line — never net overbilling against underpayment in the top-level figures. The close check (SUM(tblRecon[Total Exposure]) = Net Total Exposure within AUD 0.01) is the most important formula in the workbook. If it fails, there is a gap in the classification logic and the reconciliation cannot be distributed. Diagnose before sending anything.

### Step 8: Protect source data and hand over
Apply sheet protection to Billing_Raw and Payments_Raw. Confirm the Checks sheet shows PASS on the close check. Brief the Finance team on the Recoveries_Pipeline Status dropdown options (Identified → Under Review → Recovery Initiated → Recovered / Written Off) so they update the pipeline correctly at each review cycle — it is a living document, not a one-off snapshot.

## Formatting standard
- Deep navy header rows on all tables
- Red fill for Overpayment, No-Gap Violation, Confirmed Duplicate (highest recovery urgency)
- Amber fill for Overbilling, Near-Duplicate
- Yellow fill for Underpayment, Suspicious Match
- Green fill for Matched rows
- Light grey for Minor Rounding Difference
- AUD amounts: `$#,##0.00;($#,##0.00);-`
- Percentages: `0.0%`
- Dates: `DD-MMM-YYYY`
- Negative variances (where Medibank was shortchanged): shown in parentheses `($12,345.00)`

### Visual hierarchy
- Use Medibank teal (#006D6D) or the existing workbook primary colour as the header fill. Reserve red, amber, and green exclusively for status indicators — never for decoration.
- Match any existing Medibank workbook style when available; do not introduce a new design language without reason.
- Use Aptos or the existing workbook font throughout. Use 14–16pt for sheet titles, 11–12pt for section headers, 10–11pt for data.
- Apply clear section headers and whitespace between logical areas of each sheet.
- Freeze panes on all data sheets so column and row headers remain visible when scrolling.
- Keep gridlines off on dashboard and summary sheets; leave them on for data entry and results sheets.

### Cell conventions
- Input cells requiring user entry: light blue fill (#DBEAFE or equivalent).
- Formula cells: white or very light grey — do not apply a fill colour unless grouping requires it.
- Linked or imported data pulled from another sheet: light green fill.
- Placeholder or TBC inputs: light amber fill, with the label `TBC — user input required` in the cell or an adjacent note.
- Failed check or error cells: light red fill.
- Do not encode meaning by colour alone — always include a text label alongside any colour-coded status.

### Number formats
- Currency at record level: `$#,##0.00;($#,##0.00);-` in AUD unless the workbook specifies otherwise.
- Large portfolio or executive totals: `$0.0"m"` or `$0"k"` with units stated in the column heading.
- Percentages: `0.0%` where one decimal place is meaningful; `0%` for rough indicators.
- Dates: `DD-MMM-YYYY` throughout.
- Counts and volumes: whole numbers with thousands separators (`#,##0`).
- Never mix dollar and million-dollar units on the same table without explicit labels on every column.
- Show negative currency in parentheses `($12,345)` on executive-facing sheets, not with a leading minus sign.

### Charts
- Link all charts to model tables or named ranges — never manually entered series.
- Use descriptive titles that state the measure and time period, not the chart type.
- Avoid 3D effects, pie charts with more than four segments, excessive legends, and decorative fills.
- Ensure axis labels, units, time period, and active scenario are visible on or immediately beside every chart.
- Use consistent series colours across all charts in the workbook: one colour per named metric, carried through all sheets.

## Quality gates

### Structural gate
- `Billing_Raw` and `Payments_Raw` are protected and unmodified.
- `tblAssumptions` contains all 17 assumption rows.
- `tblRecon` has no blank Discrepancy Type cells.
- Reconciliation close check is PASS.

### Financial integrity gate
- Overbilling and overpayment figures are shown separately, never netted.
- No-Gap violations are classified distinctly from standard overbilling.
- Recovery probability is formula-driven, not manually assigned.

### Usability gate
- A finance analyst can use the Recoveries Pipeline to manage follow-up without needing to open tblRecon.
- The provider risk rating is formula-driven and updates when new data is entered.

### Decision gate
- The Recovery_Pipeline gives the Finance team a ranked, probability-weighted list of recoveries they can action, with discrepancy type, recommended action, and estimated recovery value for each item.
- The overall reconciliation close check (PASS / FAIL) is visible at the top of the Reconciliation_Summary without scrolling.
- A decision-maker can identify the total underpayment, overpayment, and pending exposure from the executive summary without opening claim-level detail.
- The model distinguishes recoverable amounts from write-off risk, so Finance can report a realistic recovery estimate rather than a gross discrepancy total.

### Usability gate
- Input cells are visually distinct from formula cells — a new user can locate the configurable inputs without a guide.
- Notes and source references are visible in the cell or an adjacent column without opening a cell editor.
- Column widths allow all content to be read without horizontal scrolling on a standard 1920×1080 display.
- No material status is conveyed by colour alone — all colour-coded statuses have an accompanying text label.
- A user who did not build the workbook can change one key assumption and confirm all dependent outputs update within 30 seconds.
- Sheet tab names are plain English and describe the sheet's purpose, not internal code names.

**Reddit community research principle — applies to all Copilot skills:**
- Always apply the Ground → Draft → Check → Decide loop. Enterprise practitioners on r/microsoft_365_copilot consistently find that bad Copilot output results from users skipping the Check or Decide step ("cognitive surrender"). Every output this skill produces must be reviewed by the user before distribution — the skill builds the draft, the human makes the decision.
- Data governance must be in place before Copilot output can be trusted. Organisations running Copilot on messy SharePoint permissions, inactive DLP policies, or poorly labelled data report inconsistent and unreliable results. If the source data is poorly governed, flag this in the workbook before distributing output.
- Copilot demos run on clean staged data; real-tenant results vary. Do not present Copilot-assisted output as equivalent to manually audited output without a stated confidence level or quality gate result.

**Reddit research — reconciliation reliability and billing anomalies:**
- Enterprise finance practitioners on r/microsoft_365_copilot note that Copilot's most reliable use in finance is synthesising narrative context — emails, Teams messages, meeting notes — alongside structured data, not replacing structured reconciliation logic. The provider payment reconciliation calculations in this skill must be formula-driven in Excel tables, not delegated to Copilot's in-context arithmetic. Copilot's role is to summarise the Recovery_Pipeline and draft the discrepancy narrative, not to compute the discrepancy values.
- Billing lag documented by community: "Our Copilot licence cost did not move after we pulled 41 licences off users" — r/microsoft_365_copilot. The same billing lag risk applies to provider payment arrangements — document in Read_Me that reconciliation figures reflect data as of the export date, and flag any known billing cycle lags in the tblRecon notes column.

## Existing-workbook behaviour
When running a new billing period through an existing reconciliation workbook:
1. Load the new period's files into Billing_Raw and Payments_Raw and protect both sheets immediately. Update R016 and R017 for the new period dates before any reconciliation formulas run — Recovery Probability age calculations depend on them.
2. Do not clear tblRecoveries before rerunning. Items from prior periods with Status = Identified or Under Review carry forward as unresolved items and must remain visible alongside the new period's findings.
3. Clear tblRecon before rerunning — the new period's claim-line analysis replaces the prior period's record-by-record results. This is separate from tblRecoveries, which accumulates across periods until items are resolved.
4. If a prior-period item has since been recovered, update its tblRecoveries row to Status = Recovered and add the recovery date and amount. Do not delete the row — the audit trail of what was recovered and when must be preserved for Finance.
5. Do not change tblAssumptions tolerance thresholds without recording the change with the approval date and the name of the person who approved it. If the overbilling tolerance (R001, R002) is revised following a Finance decision, note this clearly — a tolerance change can reclassify findings between periods and makes direct comparisons misleading without that context.
6. After the close check passes, compare the new period's Net Total Exposure to the prior period's figure. A material increase with no obvious new provider or volume change may indicate a systematic billing pattern shift worth flagging separately in the period-end cover note.

## Failure modes to prevent
Never:
- net positive and negative variances in the top-level summary — show overbilling and overpayment separately;
- classify a Minor Rounding Difference as overbilling — this inflates the discrepancy rate;
- flag a documented partial payment as an underpayment without checking Documentation Status;
- use provider name as the sole matching key — use Provider ABN;
- declare the reconciliation closed if the close check FAIL;
- post any variance to a financial system from this workbook — this workbook is analysis only;
- assign recovery probability manually rather than from the age formula;
- mark a Level 2 or Level 3 duplicate as "Confirmed Duplicate" without user review.

## Recommended prompts
- `@provider-payment-reconciler Reconcile this month's billing file against last month's payment run. Flag all overpayments immediately and sort the recovery pipeline by size.`
- `@provider-payment-reconciler This provider has a discrepancy rate of 12%. Build a provider-level breakdown of every variance type and show the total recovery exposure.`
- `@provider-payment-reconciler Run duplicate payment detection on this extract. I think we paid some invoices twice in December.`
- `@provider-payment-reconciler Add our no-gap hospital list and rerun the reconciliation — I want to see which hospitals billed a gap when they shouldn't have.`

## Optional collaboration with related skills
- Run `@claims-data-cleaner` and `@data-validator` on the billing file before reconciliation to remove data quality noise from the variance analysis;
- Use `@anomaly-detector` to identify providers whose billing pattern has changed significantly from prior periods before reconciliation.

## Sources and references
- Private Health Insurance Act 2007 (Cth) — Part 3-2: Gap cover arrangements, no-gap and known-gap billing, Medical Gap Scheme rules.
- Private Health Insurance (Benefit Requirements) Rules 2011: Benefit calculation methodology and payment obligations.
- HICAPS: Processing standards and timelines for health insurance claims.
- ECLIPSE (Electronic Claims Lodgement and Information Processing Service Environment): Hospital billing and payment standards.
- APRA Prudential Standard HPS 310 (Audit and Related Matters): Internal control requirements for payment accuracy.
- Medibank Private Limited Annual Report (2024): Public reference for scale of provider network, claims volumes, and benefit ratios.
