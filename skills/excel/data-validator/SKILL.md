---
name: data-validator
description: Use when asked to validate data quality, produce a Data Quality Score, check a dataset against submission rules, certify data for APRA or AIHW reporting, or confirm a dataset is ready for actuarial, financial, or regulatory use. Produces a scored, rule-referenced validation report with PASS, NEEDS REVIEW, or FAIL status and a field-level breakdown suitable for sign-off. Do not use for data cleaning, deduplication, or any transformation — this skill flags and certifies; use claims-data-cleaner for corrections.
---

# Validate Medibank Dataset Quality for Submission or Analysis

## Purpose
Apply a structured, rule-based validation framework to any Medibank dataset — provider claims, member data, financial reports, or regulatory submissions — and produce a formal Data Quality Report with an overall score, field-level findings, severity-ranked error list, and a sign-off-ready certification status. The validation must be reproducible, rule-referenced, and strong enough to stand up to APRA or actuarial scrutiny. This skill flags issues; it does not fix them.

## Non-negotiable outcome
Do not stop at a colour-coded table or a comment. Deliver a working Excel workbook with a structured validation report, a scored data quality certificate, and a clear statement of what must be resolved before the data can be used.

The workbook is complete only when:
- every validation rule has been applied and every triggered rule has a log entry referencing the rule code, row number, field, value, severity, and recommended fix;
- a Data Quality Score is calculated: rows with zero violations ÷ total rows × 100;
- the overall status is one of three only: PASS (≥ 95%), NEEDS REVIEW (80–94%), FAIL (< 80%);
- mandatory fields are validated for completeness separately from optional fields;
- severity levels (Critical / Major / Minor) are clearly defined and consistently applied — do not use any other severity category;
- the validation framework is configurable: thresholds and rule parameters live in a Rules sheet and can be adjusted without touching any validation formula;
- a Validation Summary sheet presents the findings in a format an APRA submissions officer, actuary, or compliance manager can review and sign off without further explanation;
- the workbook does not modify a single value in the source data — it flags only.

## Operating principles
1. This skill validates; it does not clean, correct, or modify. Every cell in the source data must be identical to its input value when the validation workbook is delivered. Any correction requires a separate cleaning workflow.
2. Validation rules are applied to the data type that column actually contains, not what the column header implies. Before applying any rule, inspect the data in the column and confirm the type. If the column header says "Date" but the column contains mixed types, flag the type inconsistency as a Critical rule failure before applying date-specific rules.
3. Severity must be calibrated to downstream impact, not perceived severity. Critical means the error prevents downstream use (submission, payment, analysis). Major means it reduces reliability or requires investigation before use. Minor means it is cosmetic or low-impact. Never assign Critical to a cosmetic issue or Minor to an issue that blocks submission.
4. The ISO/IEC 25012 data quality dimensions — completeness, accuracy, consistency, timeliness, uniqueness, and validity — are the framework that underlies every rule category. Each rule in the Validation_Rules sheet references which dimension it serves.
5. For APRA PHI reporting: any validation failure in a mandatory field is Critical. The APRA data collection portal will reject submissions with blank mandatory fields, incorrectly formatted dates, or inconsistent period totals. Flag these before submission.
6. For AIHW data collections (NHMD, PHIDB): additional coding rules apply (ICD-10-AM, AR-DRG). Flag records where these codes are absent when the submission type requires them.
7. For actuarial datasets: accuracy of claim amounts and dates is Critical; missing demographic data (age, gender, policy type) is Major because it affects risk analysis; text formatting is Minor.
8. For member data: missing or incorrect Member ID is Critical; incorrect contact details are Major; missing optional preferences are Minor.
9. Statistical outlier detection is part of validation but is a distinct rule category (Reasonableness). Outlier flags are always Major, never Critical, because an extreme value may be legitimate.
10. Do not apply business logic rules (e.g. benefit paid cannot exceed claim amount) unless the relationship is defined in the Validation_Rules sheet by the user or a confirmed Medibank data standard. Do not invent business rules from general industry knowledge.
11. Cross-field consistency rules (e.g. lodgement date ≥ service date) are always more reliable than single-field rules. Run them in addition to, not instead of, single-field checks.
12. Future date flags are Critical for all service or lodgement dates. Future financial values are at minimum Major.
13. Uniqueness: duplicate primary keys are Critical regardless of dataset type. Near-duplicates are Major.
14. Timeliness: data older than the expected reporting window (configurable in Validation_Rules, default 2 years) is flagged as a Minor timeliness issue.
15. Validity: code fields (MBS item numbers, ICD-10-AM, ABN, policy type codes, state codes) must match the approved vocabulary defined in reference tables loaded into the workbook. Codes not in the approved vocabulary are Major; codes that fail a checksum are Critical (e.g. ABN check digit).
16. Produce the validation report without waiting for a full pass. Start from the highest-severity findings and work down. If Critical findings exist, state them at the top of the report before listing Major or Minor findings.
17. A dataset that contains zero Critical and zero Major findings but is below 95% due to Minor issues alone should be presented as NEEDS REVIEW with a specific statement that all Major and Critical checks passed — this is meaningfully different from a dataset with unresolved Major findings.
18. Never declare PASS status while any Critical finding is unresolved, regardless of the overall numeric score.
19. The Validation_Rules sheet must contain every rule applied. A validation run that applies a rule not in Validation_Rules is not auditable.

## First response and ambiguity handling
Before validating, inspect the dataset and infer as much as possible from column headers, sample values, and data type distribution.

Resolve these items when available:
- dataset type (provider claims / member data / financial report / APRA submission / AIHW submission / other);
- reporting period and submission deadline;
- mandatory vs optional field classification for this dataset type;
- any custom validation rules beyond the standard set;
- whether reference tables (MBS schedule, approved code vocabularies, ABN list) should be loaded into the workbook;
- the significance threshold for reasonableness checks (default: flag values more than 3 standard deviations from the group mean).

If the user provides only a data file, apply the full standard rule set for the inferred dataset type, document every assumption in the Validation_Rules sheet, and note which rules required assumptions due to missing context.

## Workbook architecture

### 1. Source_Data
An exact copy of the data submitted for validation. Protected. Not modified. Header block states: source, date, row count, submitted by. If the user attaches a file, this sheet is populated from it.

### 2. Validation_Rules
An Excel table named `tblRules`. Every rule applied in this validation run must appear here.

Columns:
- Rule Code (e.g. VCOMP-01, VACC-03)
- Category (Completeness / Accuracy / Consistency / Timeliness / Uniqueness / Validity / Reasonableness)
- ISO 25012 Dimension
- Field(s) Applicable
- Description (plain English statement of what is checked)
- Threshold or Parameter (editable; e.g. outlier threshold in standard deviations)
- Severity if Triggered (Critical / Major / Minor)
- Applies to Dataset Types (comma-separated list)
- Active (Yes / No)
- Regulatory Basis (APRA / AIHW / Medibank Standard / ISO 25012 / None)

### 3. Validation_Results
An Excel table named `tblResults`. One row per rule violation found.

Columns:
- Violation ID (auto-incrementing)
- Rule Code
- Category
- Severity
- Row Number in Source_Data
- Field Name
- Failing Value
- Expected Value or Format (where applicable)
- Rule Description (copied from tblRules)
- Recommended Fix

Sort by Severity (Critical first) then Rule Code. Apply conditional formatting:
- Critical: red fill, white bold text
- Major: amber fill, dark text
- Minor: yellow fill, dark text

### 4. Summary_by_Field
A pivot-style summary table showing, for each field:
- Field name
- Required (Yes/No)
- Total records
- Records with value
- Records blank or null
- Completeness rate
- Critical violations
- Major violations
- Minor violations
- Field status (Pass / Review / Fail)

### 5. Validation_Report
Single-page executive summary. Sections:
- Dataset identification block
- Validation run date and rules version
- Total records, total violations, violations by severity
- Data Quality Score and overall status (PASS / NEEDS REVIEW / FAIL)
- Top 5 Critical findings listed explicitly
- Top 5 Major findings listed explicitly
- Summary of Minor findings (count only)
- APRA / AIHW submission readiness: Ready to Submit / Not Ready (with specific blockers)
- Recommended next steps (ordered by priority)

### 6. Reference_Tables
Read-only reference data used by validation rules:
- Approved Status codes
- Approved State codes (NSW, VIC, QLD, SA, WA, TAS, NT, ACT)
- Approved policy type codes
- MBS item number reference (load from external file if available — note if absent)
- ICD-10-AM code format reference (pattern only, not full codebook)
- ABN check digit validation table

### 7. Data_Quality_Certificate
Single-page, printable sign-off certificate.
- Dataset name and period
- Validation run date and rule count
- Score and overall status with large colour-coded status block
- Field-level completeness summary
- Outstanding Critical findings (explicit list; "None" if all resolved)
- Sign-off block: Validated By (name, role, date, signature space)
- Note: "PASS status is valid for this dataset as at the validation run date. Changes to source data invalidate this certificate."

### 8. Read_Me
- Purpose of this workbook and what it does and does not do
- How to run validation on a new dataset (replace Source_Data sheet)
- How to adjust a threshold in Validation_Rules
- How to add a new rule
- Severity definitions
- Score thresholds (PASS/NEEDS REVIEW/FAIL)
- Regulatory basis for key rules
- Limitations: validation is rule-based, not AI-based; unexpected data patterns not covered by a rule will not be caught; reference tables must be kept current

## Validation rule set
Apply these rules in the order listed. Rule codes must match tblRules entries.

### Completeness rules (VCOMP)
```
VCOMP-01: Mandatory fields — flag any blank, null, or placeholder value in mandatory fields. Severity: Critical.
VCOMP-02: Optional fields — flag blank optional fields. Severity: Minor.
VCOMP-03: Completeness rate per field ≥ 99% for Critical fields. Flag if below. Severity: Critical.
VCOMP-04: Completeness rate per field ≥ 90% for Major fields. Flag if below. Severity: Major.
```

### Accuracy rules (VACC)
```
VACC-01: Date fields — must be parseable as a valid calendar date. Severity: Critical.
VACC-02: Date fields — must not be in the future (> today). Severity: Critical.
VACC-03: Date fields — must not be before 01-JAN-1984. Severity: Major.
VACC-04: Amount fields — must be numeric. Severity: Critical.
VACC-05: Amount fields — must be positive (≥ 0 for benefit; > 0 for claim). Severity: Critical.
VACC-06: Amount fields — must not exceed [Max_Amount] threshold (configurable; default AUD 500,000). Severity: Major.
VACC-07: ABN fields — must pass check digit algorithm. Severity: Critical (for payment datasets) / Major (for analysis datasets).
VACC-08: MBS Item — must be numeric, 3–8 digits. Severity: Major.
VACC-09: ICD-10-AM — must match format [A-Z][0-9]{2}(\.[0-9A-Z]{1,4})? where required. Severity: Major.
```

### Consistency rules (VCONS)
```
VCONS-01: Service Date ≤ Lodgement Date (where both present). Severity: Critical.
VCONS-02: Benefit Paid ≤ Claim Amount. Severity: Critical.
VCONS-03: Gap Amount = Claim Amount − Benefit Paid (within AUD 0.05 tolerance). Severity: Major.
VCONS-04: Period totals reconcile to sum of component line items (for summary-level datasets). Severity: Critical.
VCONS-05: Member policy type is consistent with the service type claimed. Severity: Major.
VCONS-06: State of service consistent with provider state (where both present). Severity: Minor.
```

### Uniqueness rules (VUNIQ)
```
VUNIQ-01: Primary key field (e.g. Claim ID) must be unique within the dataset. Duplicates: Critical.
VUNIQ-02: Near-duplicates (same Provider + Member + Service Date + Amount within tolerance). Severity: Major.
VUNIQ-03: Membership numbers must not appear more than [Max_Policies] times (configurable). Severity: Minor.
```

### Timeliness rules (VTIME)
```
VTIME-01: Service Date must be within the reporting period specified in the metadata. Severity: Major.
VTIME-02: Service Date must be within [Lookback_Years] of today (configurable; default 2 years). Severity: Minor.
VTIME-03: Lodgement Date must be ≤ Lodgement_Cutoff for the reporting period (if applicable). Severity: Major.
```

### Validity rules (VVAL)
```
VVAL-01: Status codes must match approved vocabulary in Reference_Tables. Severity: Major.
VVAL-02: State codes must be one of: NSW, VIC, QLD, SA, WA, TAS, NT, ACT. Severity: Major.
VVAL-03: Policy type codes must match approved vocabulary. Severity: Major.
VVAL-04: MBS Item number must be in the approved MBS schedule (if reference table loaded). Severity: Major.
```

### Reasonableness rules (VREASON)
```
VREASON-01: Claim amount > mean + [Outlier_SD] × SD for the same MBS item group. Severity: Major. (Default SD threshold: 3)
VREASON-02: Sessions per member per month > [Max_Sessions] (configurable; default 20). Severity: Major.
VREASON-03: Year-on-year variance on aggregate metrics > [YoY_Threshold] (configurable; default 20%). Severity: Major.
```

## Calculation standards
All rule thresholds reference tblRules:
```
[Outlier_SD] = INDEX(tblRules[Threshold or Parameter], MATCH("VREASON-01", tblRules[Rule Code], 0))
[Max_Amount] = INDEX(tblRules[Threshold or Parameter], MATCH("VACC-06", tblRules[Rule Code], 0))
```

Data Quality Score:
```
Violation_Rows = COUNTIF(tblResults[Row Number], "<>"&"") — distinct rows with at least one violation
Clean_Rows = Total_Rows − Violation_Rows
DQ_Score = Clean_Rows / Total_Rows
Status = IF(DQ_Score >= 0.95, "PASS", IF(DQ_Score >= 0.80, "NEEDS REVIEW", "FAIL"))
```

Override: IF COUNTIF(tblResults[Severity], "Critical") > 0, status is at minimum NEEDS REVIEW regardless of DQ_Score.

## Formatting standard
- Red for Critical and FAIL status
- Amber for Major and NEEDS REVIEW
- Yellow for Minor
- Green for PASS
- White rows in Validation_Results for rows with no violations
- Aptos or workbook font, 10–11pt data, 14–16pt headings
- Gridlines on Validation_Results; off on Validation_Report and Data_Quality_Certificate
- All columns in Validation_Results print on A3 landscape if needed; key report sheets print on A4 portrait

## Quality gates

### Structural gate
- Source_Data is unmodified and protected.
- tblRules contains every rule applied.
- tblResults contains one row per violation (not one per rule).
- Data_Quality_Certificate has a calculated, not manually typed, status.

### Rule gate
- Every active rule in tblRules produced either ≥ 1 violation entry or a log of "0 records affected."
- No rule was applied to a column of the wrong data type.
- Severity assignments match the definitions in tblRules.

### Certificate gate
- DQ_Score is formula-driven, not typed.
- PASS is not shown if any Critical violations remain unresolved.
- APRA/AIHW readiness flag is present and accurate.

### Usability gate
- A submissions officer can read the Validation_Report and know exactly what to fix before submitting.
- The Read_Me explains how to run a new validation without touching any formula.

## Failure modes to prevent
Never:
- modify any value in Source_Data — validation is read-only;
- declare PASS when Critical violations remain outstanding;
- assign severity based on personal judgement rather than the tblRules definition;
- apply a rule not listed in tblRules;
- combine multiple violation types into a single Validation_Results row — one row per violation;
- report a percentage without the denominator (always show score as "X% (Y of Z records clean)");
- invent business logic rules not defined in tblRules or provided by the user — flag the gap and ask;
- apply amount validation rules to ID or code columns.

## Recommended prompts
- `@data-validator Validate this APRA PHI quarterly submission. Flag all Critical errors before anything else. I need a signed Validation Report before the lodgement deadline.`
- `@data-validator Run a full validation on this claims extract against our standard rules. Show me the DQ Score and list every Critical and Major finding with the specific row number and value.`
- `@data-validator Add VREASON-03 (year-on-year variance) to the rule set and rerun the validation against Q3 FY26 vs Q3 FY25.`
- `@data-validator This data failed last time with a FAIL status. I've made corrections — rerun and confirm which previous Critical findings are now resolved.`

## Optional collaboration with related skills
- Use `@claims-data-cleaner` before validation if the data requires cleaning — validation after cleaning gives a fair certification status;
- Use `@regulatory-submission-checker` for APRA-specific cross-field and period-consistency checks that extend beyond general data quality validation;
- Use `@anomaly-detector` after validation for statistical outlier detection within the validated clean population.

## Sources and references
**Data quality standards:**
- ISO/IEC 25012:2008 Data Quality Model: Completeness, accuracy, consistency, timeliness, uniqueness, validity, and eight additional dimensions.
- AIHW Data Quality Framework (2012): Six-dimension framework for Australian health data — institutionality, accessibility, relevance, timeliness, coherence, interpretability.

**Regulatory requirements:**
- APRA Prudential Standard HPS 310 (Audit and Related Matters): Data accuracy requirements for PHI returns.
- Private Health Insurance Act 2007 (Cth): Obligations on registered health insurers to maintain accurate records.
- APRA Data Collection Platform (DCP) — specification documents for PHI returns.
- AIHW National Hospital Morbidity Database (NHMD): ICD-10-AM and AR-DRG validation requirements.

**Industry references:**
- HICAPS transaction standards: Approved code vocabularies for electronic claims.
- Medicare Benefits Schedule: MBS item number validity reference.
- Australian Business Register (ABR): ABN check digit algorithm.
