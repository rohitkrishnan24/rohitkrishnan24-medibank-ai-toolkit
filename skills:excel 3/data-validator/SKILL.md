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

## Model build workflow

### Step 1: Profile the dataset
Run a data profile before writing any rule: row count, column count, data type per column (check the actual cell values, not just the header label), completeness rate per column, and the range of any numeric columns. A date rule that fires on a free-text field produces false positives that erode trust in the entire result. Write the profile summary in the Read_Me header block along with the confirmed dataset type.

### Step 2: Classify the dataset and its columns
From the column headers and sample values, determine the dataset type (provider claims, member data, APRA submission, AIHW collection, actuarial dataset, or other). For each column, note whether it is mandatory or optional for this dataset type, and flag columns that contain coded fields (MBS item number, ICD-10-AM code, ABN, state code, status code) — coded fields need validity checks against reference vocabularies, not just completeness checks.

### Step 3: Build Validation_Rules (tblRules)
Populate tblRules with every rule that will run. Work from the standard set (VCOMP, VACC, VCONS, VUNIQ, VTIME, VVAL, VREASON) and deactivate rules that don't apply to this dataset type with Active = No. Add any custom business rules the user has specified. Each rule must have a severity, a source (APRA data item reference, ISO 25012 dimension, or Medibank Standard), and where relevant a threshold value. A validation result that is not traceable to a rule in tblRules cannot be signed off in an audit.

### Step 4: Run completeness and accuracy checks
Apply VCOMP rules first — blanks in mandatory fields invalidate most downstream checks for that row and should appear at the top of the results. Then run VACC rules: confirm date fields parse as valid dates, amount fields are numeric and non-negative, ABN fields pass the check digit algorithm, and code fields match expected formats. Write each violation to tblResults with the row number, field name, failing value, and recommended fix action.

### Step 5: Run consistency and uniqueness checks
Apply VCONS rules for cross-field business logic: Service Date ≤ Lodgement Date, Benefit Paid ≤ Claim Amount, Gap Amount = Claim − Benefit within AUD 0.05 tolerance. Run VUNIQ rules for exact duplicate primary keys (Critical) and near-duplicates (same Provider + Member + Date + Amount within defined tolerance, Major). A duplicate primary key must block the DQ certificate from showing PASS regardless of the overall score — it is a fundamental data integrity failure.

### Step 6: Run timeliness, validity, and reasonableness checks
Apply VTIME rules for records outside the reporting period window or older than the lookback threshold. Run VVAL rules against Reference_Tables: check status codes, state codes, MBS item numbers, and policy type codes against approved vocabularies. Then apply VREASON: flag claims amounts more than [Outlier_SD] SD above the peer group mean as Major. These are investigation candidates, not automatic rejections — the outlier may be legitimate.

### Step 7: Calculate DQ Score and build the certificate
Calculate DQ_Score = Clean_Rows / Total_Rows. Apply status logic: PASS ≥ 95%, NEEDS REVIEW 80–94%, FAIL below 80%. Apply the Critical override before assigning final status. Build the Data_Quality_Certificate with the calculated (never typed) score, outstanding Critical count, and the sign-off block. List the top 5 Critical findings and top 5 Major findings explicitly in the Validation_Report — do not bury Critical items in a long table where they require scrolling to find.

### Step 8: Deliver with Validation_Report
Sort tblResults by Severity then Rule Code. If APRA submission readiness is requested, state in plain language whether the data is Ready to Submit or Not Ready, naming each specific blocker. Protect Source_Data before sending the workbook. The signing analyst must understand the certificate is valid as at the run date — any subsequent change to the source data invalidates it and requires a re-run.

## Formatting standard
- Red for Critical and FAIL status
- Amber for Major and NEEDS REVIEW
- Yellow for Minor
- Green for PASS
- White rows in Validation_Results for rows with no violations
- Aptos or workbook font, 10–11pt data, 14–16pt headings
- Gridlines on Validation_Results; off on Validation_Report and Data_Quality_Certificate
- All columns in Validation_Results print on A3 landscape if needed; key report sheets print on A4 portrait

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

### Decision gate
- The Data_Quality_Certificate clearly states PASS / NEEDS REVIEW / FAIL with a quantified DQS score, so the submitting team knows the outcome without reading the detail sheets.
- Every failing or warning dimension identifies the specific rows and fields affected — the team can fix issues without opening raw data themselves.
- The certificate is printable and self-contained: a reviewer who did not run the validator can read it and understand the data quality status.
- A resubmission path is documented: the certificate states what must be corrected before PASS status can be achieved.

### Usability gate
- Input cells are visually distinct from formula cells — a new user can locate the configurable inputs without a guide.
- Notes and source references are visible in the cell or an adjacent column without opening a cell editor.
- Column widths allow all content to be read without horizontal scrolling on a standard 1920×1080 display.
- No material status is conveyed by colour alone — all colour-coded statuses have an accompanying text label.
- A user who did not build the workbook can change one key assumption and confirm all dependent outputs update within 30 seconds.
- Sheet tab names are plain English and describe the sheet's purpose, not internal code names.

## Existing-workbook behaviour
When rerunning validation on a corrected or updated dataset:
1. Replace Source_Data with the new file. Do not add rows or columns to the existing Source_Data sheet structure. Protect the sheet again immediately after loading.
2. Clear tblResults completely before rerunning. Results from a prior run must not persist alongside new findings — mixing them produces an inflated violation count and makes the DQ Score meaningless. If you want to keep prior results for comparison, copy them to a sheet labelled "Prior Run — [date]" before clearing.
3. Rerun all rules in tblRules with Active = Yes. If a rule was deactivated for the prior run, confirm whether it should remain inactive for this one. Deactivating a rule without noting the reason changes the certification scope silently.
4. After rerunning, cross-check against the prior Critical findings: items the user said were corrected should now show no corresponding row in tblResults. If a previously corrected item still appears, flag this explicitly — either the correction did not take or a new instance of the same violation has appeared in the updated data.
5. Issue a new Data_Quality_Certificate with the current run date and the signing analyst's details. The prior certificate is invalidated the moment the dataset changes. Do not amend or reuse it.
6. Note in Read_Me which version of the source data was validated and what changed since the prior run. This is especially important in APRA submission contexts where the amendment history must be auditable.

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
