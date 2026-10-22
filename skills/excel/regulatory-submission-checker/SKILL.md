---
name: regulatory-submission-checker
description: Use when asked to validate an APRA private health insurance quarterly return, AIHW data submission, or other regulatory dataset before lodgement. Applies APRA PHI-specific completeness, cross-field consistency, period reconciliation, and year-on-year variance checks, and produces a Submission Readiness Report with READY / HOLD / NOT READY status and an explicit sign-off checklist. Do not use as a substitute for actuarial sign-off or legal compliance review — this skill checks data integrity and format, not legal sufficiency.
---

# Validate Medibank Regulatory Submission Before APRA Lodgement

## Purpose
Apply a structured, rule-based pre-lodgement validation framework to Medibank's quarterly APRA private health insurance (PHI) return or related regulatory dataset, catch errors and inconsistencies before they trigger APRA queries or resubmission penalties, and produce a Submission Readiness Report that documents every check result and enables the submissions officer and CFO to sign off with confidence. The workbook must apply APRA PHI-specific business rules (not generic data quality rules), reconcile quarterly totals against their components, and provide a clear audit trail of every check performed.

## Non-negotiable outcome
Do not stop at colour-coded cells or an informal summary. Build a complete, auditable regulatory submission checker.

The workbook is complete only when:
- every mandatory APRA PHI field for the nominated return type is validated for presence, format, and plausibility;
- internal period consistency checks confirm that quarterly totals equal the sum of monthly breakdowns where both are present;
- cross-field business rules (benefit ratio bounds, membership count non-negativity, premium ≥ claims for most product categories) are checked and each failure is assigned a severity level;
- year-on-year variance analysis flags any metric that has changed by more than the configurable threshold (default 20%) and requests a documented explanation — variance flags are not errors, they are explanation requirements;
- the submission readiness status is one of three: READY (all Critical checks pass, all explanation requirements are documented), HOLD (Critical checks pass but one or more Major findings remain unresolved), NOT READY (one or more Critical checks fail);
- a Sign-Off Checklist is present with named fields for: preparer, reviewer, actuarial sign-off, and CFO approval;
- a complete audit log records: check code, check description, result (PASS / FAIL / WARN), field affected, observed value, expected value or range, and the APRA rule or prudential standard that requires it.

## Operating principles
1. APRA DCP (Data Collection Platform) portal will reject submissions with specific error types: missing mandatory fields, format violations, and internal inconsistency between related data items. These are Critical failures — the submission cannot proceed until they are resolved.
2. APRA DCP may allow submissions to proceed with warnings for non-critical findings, but warnings are reviewed by APRA during their surveillance process and may trigger follow-up queries. Track these as Major findings requiring explanation documentation before lodgement.
3. Private Health Insurance Act 2007 (Cth) requires registered health insurers to lodge quarterly PHI returns with APRA within specified timeframes. Late lodgement attracts financial penalties. The submission checker must be completed in time to allow any identified errors to be corrected and revalidated before the lodgement deadline.
4. APRA PHI returns reference specific table numbers and data items. Use the return type and reference codes supplied by APRA in validation messages. Do not use generic terms like "Field A" or "Column B" — reference the APRA data item identifier (e.g. PHI_QRS_T01_I001 for Table 1, Item 1 of the Quarterly Returns System).
5. Benefit ratio (claims / premium) must be between 0% and 150% for any product category. A benefit ratio above 100% means claims exceeded premium — this is not automatically an error (it can reflect adverse claims experience) but it requires explanation documentation.
6. Membership numbers must be non-negative integers. A negative member count is a data entry error and is Critical.
7. Policy type splits must sum to the total: Individual + Couple + Family + Single Parent = Total Policies. Any discrepancy is Critical.
8. Fund surplus/deficit: the quarterly fund financial position must reconcile: Opening Balance + Premium Revenue − Claims − Admin Expenses − Reinsurance ± Other = Closing Balance. Any imbalance in this equation is a Critical cross-field failure.
9. Year-on-year variance thresholds are not error thresholds — they are explanation triggers. A 25% increase in claims is not necessarily wrong; it requires a documented explanation (e.g. change in membership mix, large-loss event, new benefit offerings). Never automatically reject a variance without allowing for documentation.
10. Premium must be net of the government rebate for some APRA items and gross for others — the difference matters significantly. Check the data dictionary for the nominated return to confirm which convention applies to each data item. Flag items where the convention is unclear as TBC requiring confirmation.
11. Reinsurance arrangements: Medibank may hold reinsurance for catastrophic claims. APRA requires reinsurance ceded and assumed to be reported separately. Flag any submission where reinsurance items are blank if the organisation has active reinsurance arrangements.
12. PHIIB (Private Health Insurance Industry Bulletin) items change periodically. Validate the return against the bulletin version current for the reporting quarter, not the most recent version. Flag any field that APRA added or changed in the bulletin for the current period — these are common submission errors.
13. Prior-period restatements: if the prior period figures in the current return differ from the figures lodged in the prior return, the discrepancy requires an explicit explanation. Do not assume the difference is a correction — it may be an error in the current return.
14. Every check result must cite its source: the APRA rule, data dictionary reference, PHI Act section, or internal Medibank policy that requires the check. A check without a cited source is not auditable.
15. The Sign-Off Checklist must include four distinct sign-offs: preparer (the analyst who ran the validation), independent reviewer (a second analyst or senior finance staff member), actuarial sign-off (for any item that involves an actuarial estimate), and CFO or delegate approval. Do not allow submission without all four being documented.
16. Material threshold: for year-on-year variance analysis, materiality matters. A 25% change in a data item worth AUD 50,000 is less significant than a 5% change in a data item worth AUD 100 million. Include an absolute dollar threshold alongside the percentage threshold.
17. When applying format checks, use the current APRA DCP format specification: integers must be integers (no decimals), date fields must match the APRA-specified format, string fields must not exceed maximum character limits defined in the data dictionary.
18. The checker must be re-runnable: each rerun overwrites only the check results, not the source data. The audit log appends, not overwrites — prior run results are preserved.
19. Never mark a submission as READY on the basis of automated checks alone. READY requires the Sign-Off Checklist to be completed and, for any Major findings, the explanation documentation to be recorded.

## First response and ambiguity handling
Before running checks, confirm:
- return type (e.g. Quarterly Returns System — QRS, Actuarial Returns, AIHW Private Health Insurance Database — PHIDB, other);
- reporting quarter and year (e.g. Q4 FY2026 = April–June 2026);
- whether prior-period lodgement data is available for year-on-year comparison;
- whether reinsurance arrangements are active;
- the lodgement deadline for this return;
- whether this is a first lodgement or an amendment to a prior return.

If the return type is not confirmed, default to the QRS (the most common) and note that APRA table references are approximate until the return type is confirmed.

## Workbook architecture

### 1. Submission_Data
Protected, verbatim copy of the data prepared for APRA lodgement. No modifications. Header block: return type, reporting period, lodgement deadline, prepared by, preparation date.

### 2. Validation_Rules
Excel table `tblRules`. Regulatory basis for every check.

Columns:
- Check Code (e.g. CRIT-PHI-01, MAJOR-VAR-03)
- Category (Completeness / Format / Cross-Field / Period Consistency / Year-on-Year / Business Rule / Sign-Off)
- Severity (Critical / Major / Minor)
- Data Item (APRA data item identifier or field name)
- Description (plain English check description)
- Expected Condition
- APRA Regulatory Reference (PHI Act section / APRA DCP spec / Data Dictionary item)
- Active (Yes/No — can be toggled without deleting rules)
- Return Type Applicability (QRS / Actuarial / PHIDB / All)

### 3. Check_Results
Excel table `tblResults`. One row per check executed.

Columns:
- Check Code
- Category
- Severity
- Data Item
- Description
- Observed Value
- Expected Value or Condition
- Result (PASS / FAIL / WARN / NOT APPLICABLE — RETURN TYPE)
- Explanation Required (Y/N)
- Explanation Documented (Y/N — for user to tick)
- Regulatory Reference
- Run Date

Conditional formatting:
- FAIL: red fill (Critical)
- WARN: amber fill (Major or variance trigger)
- PASS: white (no colour — clean result)
- NOT APPLICABLE: light grey

### 4. Completeness_Check
For each mandatory field:
- Field name and APRA data item reference
- Value present (Y/N)
- Format valid (Y/N)
- Value is within plausible range (Y/N — for numeric fields)
- Result: PASS / FAIL

Table named `tblCompleteness`.

Mandatory fields for QRS (illustrative — update from APRA data dictionary for the specific return):
- Fund identifier (ABN and PHII registration number)
- Reporting period (start date and end date)
- Total insured persons at period end
- Total insured persons — hospital cover
- Total insured persons — general treatment cover
- Total premiums received (gross)
- Total benefits paid — hospital
- Total benefits paid — general treatment
- Total management expenses
- Benefit ratio — hospital
- Benefit ratio — general treatment
- Fund equity at period end
- Reinsurance ceded (0 if no reinsurance, not blank)

### 5. Cross_Field_Checks
Critical business rules that must hold across data items.

```
CRIT-CF-01: Policy splits sum to total
  Individual + Couple + Family + Single Parent = Total Policies
  Tolerance: 0 (no rounding — integers)

CRIT-CF-02: Financial balance equation
  Opening Equity + Premium Revenue − Benefits Paid − Management Expenses ± Reinsurance ± Other = Closing Equity
  Tolerance: AUD 1,000 (rounding across multiple line items)

CRIT-CF-03: Benefit ratio = Benefits Paid / Premium Revenue
  Validate the stated benefit ratio against the calculated ratio
  Tolerance: 0.1% (formula difference due to rounding)

CRIT-CF-04: Benefits paid ≤ Maximum plausible benefit ratio × Premium
  Alert if benefits > 150% of premium (may be valid but requires explanation)
  Severity: WARN if 100–150%; FAIL if > 150%

CRIT-CF-05: All member counts are non-negative integers
  Check: no negative values or decimal values in membership fields

CRIT-CF-06: Period dates are consistent
  Reporting period end − Reporting period start = 89–92 days (quarterly)

MAJOR-CF-07: Hospital admissions per insured person is within plausible range
  Expected: 0.05–0.30 admissions per insured person per quarter
  Flag if outside this range (may be legitimate for unusual mix or period)

MAJOR-CF-08: Average benefit per hospital admission is within plausible range
  Expected: AUD 3,000–AUD 20,000 average
  Flag if outside (configurable in tblConfig)
```

### 6. Period_Consistency_Check
If the submission includes monthly breakdowns alongside quarterly totals:

```
CRIT-PC-01: Monthly premium sum = Quarterly total premium
  SUM(Month1 + Month2 + Month3) = Quarter total
  Tolerance: AUD 0 (exact — integer items); AUD 1,000 (calculated ratios)

CRIT-PC-02: Monthly benefit sum = Quarterly total benefits

CRIT-PC-03: Monthly membership average = Quarterly closing membership
  (if applicable; check APRA specification for averaging methodology)

CRIT-PC-04: No month within a quarter has zero premium (unusual — flag for review)
```

### 7. YOY_Variance_Analysis
Comparison against prior corresponding period.

Calculate for each material data item:
- Prior year value (from prior lodgement or user-supplied)
- Current value
- Absolute change (AUD)
- Percentage change
- Material threshold exceeded (Y/N) — flag if: ABS(% change) > 20% AND ABS(AUD change) > AUD 100,000 (both configurable)
- Explanation Required (Y/N — Yes if material threshold exceeded)
- Explanation Documented (user enters text here)

Items to include:
- Total insured persons (hospital and general)
- Total premium revenue
- Total benefits paid (hospital and general)
- Benefit ratio (hospital and general)
- Management expense ratio
- Fund equity
- Hospital admissions count
- Average benefit per hospital admission

Sort by absolute dollar change descending. Items with Explanation Required = Y and Explanation Documented = N are blocking issues for READY status.

### 8. Submission_Readiness_Report
Single-page executive summary.

Sections:
- Return identification: fund name, ABN, return type, reporting period, lodgement deadline
- Validation run date and check version
- Check results summary: Critical PASS / FAIL count, Major PASS / WARN count, Minor count
- Submission readiness status (large, colour-coded: READY / HOLD / NOT READY) with rationale
- READY conditions: all Critical = PASS, all Major WARN with Explanation Documented = Y, Sign-Off Checklist complete
- HOLD conditions: all Critical = PASS, but one or more Major items without documented explanation
- NOT READY conditions: one or more Critical FAIL
- Outstanding issues list: explicit enumeration of every FAIL and undocumented WARN
- Estimated effort to resolve (based on issue count and type)

### 9. Sign_Off_Checklist
A printable sign-off page for the lodgement file.

Sections:
- Return identification
- Preparer: Name / Role / Date / Signature line
- Independent Reviewer: Name / Role / Date / Signature line
- Actuarial Sign-Off (if actuarial estimates present): Actuary Name / Accreditation / Date / Signature line
- CFO / Delegate Approval: Name / Role / Date / Signature line
- Declaration statement: "I confirm that the data in this regulatory return has been reviewed and, to the best of my knowledge, is accurate, complete, and prepared in accordance with APRA's requirements for the Private Health Insurance Quarterly Returns System."

### 10. Read_Me
- Purpose, scope, and what this skill does not check (legal sufficiency, actuarial methodology)
- How to run the checker on a new submission
- How to add or update a validation rule in tblRules
- How to document a year-on-year variance explanation
- APRA lodgement timeline and consequences of late lodgement
- Sign-off process and who must sign
- Definitions: benefit ratio, management expense ratio, fund equity, reinsurance ceded/assumed
- Limitations: this checker uses the QRS format as a default; APRA data item identifiers must be confirmed against the current data dictionary

## Calculation standards
All thresholds reference tblConfig:
```
[YOY_Pct] = INDEX(tblConfig[Value], MATCH("VAR_PCT_THRESHOLD", tblConfig[Config ID], 0))
[YOY_AUD] = INDEX(tblConfig[Value], MATCH("VAR_AUD_THRESHOLD", tblConfig[Config ID], 0))
```

Benefit ratio:
```
Benefit_Ratio = Benefits_Paid / Premium_Revenue
Stated_vs_Calculated_Diff = ABS(Stated_Ratio − Benefit_Ratio)
```

Financial balance:
```
Calculated_Closing = Opening_Equity + Premium_Revenue − Benefits_Paid − Mgmt_Expenses + Reinsurance_Net + Other_Net
Balance_Diff = ABS(Stated_Closing_Equity − Calculated_Closing)
```

## Formatting standard
- NOT READY: dark red, large bold status block
- HOLD: amber, large bold
- READY: dark green, large bold
- FAIL rows: red fill in Check_Results
- WARN rows: amber fill
- PASS rows: white
- Sign_Off_Checklist: no fill, black borders, clean A4 layout for printing
- Dates: `DD-MMM-YYYY` throughout
- Monetary values: `$#,##0;($#,##0);-`
- Percentages: `0.00%` (two decimal places — regulatory precision)

## Quality gates

### Coverage gate
- All mandatory fields for the nominated return type have a completeness check result.
- All six cross-field check groups have been applied.
- YOY analysis covers all material data items.

### Integrity gate
- Source data is unmodified and protected.
- No check result is manually typed — all are formula-driven from check logic.
- Sign-Off Checklist fields are blank by default and must be completed by humans.

### Readiness gate
- READY status cannot be set while any Critical check is FAIL.
- READY status cannot be set while any YOY Explanation Required = Y and Explanation Documented = N.
- Sign-Off Checklist must be complete before submission proceeds.

## Failure modes to prevent
Never:
- mark submission as READY while any Critical check is FAIL;
- apply generic data quality rules in place of APRA PHI-specific business rules;
- use the same materiality threshold for a AUD 50,000 item as for a AUD 50 million item — apply both percentage and dollar thresholds;
- allow variance flags to be dismissed without documented explanations;
- apply checks for a return type other than the one nominated (e.g. applying QRS checks to an Actuarial return);
- omit reinsurance fields from the check — blank reinsurance fields in a fund with active reinsurance are a Critical issue;
- present the Sign-Off Checklist with pre-filled names — it must be signed by the actual named persons;
- submit without all four sign-offs.

## Recommended prompts
- `@regulatory-submission-checker Validate this Q4 FY26 APRA QRS submission. Flag all Critical errors first. The lodgement deadline is in 3 business days.`
- `@regulatory-submission-checker Run the year-on-year variance analysis against our Q4 FY25 lodgement. Our hospital claims increased 28% — I need to document the explanation before APRA asks.`
- `@regulatory-submission-checker Add the period consistency checks for the monthly breakdown I've now added to the submission file.`
- `@regulatory-submission-checker Produce the Sign-Off Checklist for CFO review. All Critical checks pass but two Major variance items need their explanations documented.`

## Optional collaboration with related skills
- Use `@data-validator` before this skill to resolve generic data quality issues (blanks, format errors) that would otherwise generate noise in the regulatory checks;
- Use `@claims-data-cleaner` to prepare the underlying claims data before it is aggregated into the regulatory return;
- Use `@qoq-trend-analyser` to produce the detailed QoQ narrative that supports the YOY variance explanations.

## Sources and references
**Primary regulatory sources:**
- Private Health Insurance Act 2007 (Cth): Obligations for registered health insurers; quarterly reporting requirements (Part 4-3).
- APRA Prudential Standard HPS 310 (Audit and Related Matters): Internal review and sign-off requirements for regulatory returns.
- APRA Data Collection Platform (DCP) — Private Health Insurance: Data dictionary, format specifications, and lodgement instructions for QRS and other PHI returns (current version at time of submission).
- APRA Private Health Insurance Industry Bulletins (PHIIB): Current-quarter amendments to reporting requirements.
- APRA Quarterly Private Health Insurance Statistics: Published aggregate data useful for benchmark comparisons.

**AIHW submissions:**
- AIHW Private Health Insurance Database (PHIDB): Annual data collection requirements and data dictionary.
- AIHW National Hospital Morbidity Database (NHMD): Hospital separation data requirements.

**Industry and actuarial:**
- Institute of Actuaries of Australia (IAAust) Practice Standard PS 400 (Private Health Insurance — Actuary's Report): Actuarial sign-off requirements for PHI regulatory returns.
- Private Healthcare Australia (PHA): Industry body guidance on APRA reporting obligations for health insurers.
