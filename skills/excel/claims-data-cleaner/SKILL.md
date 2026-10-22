---
name: claims-data-cleaner
description: Use when asked to clean, standardise, deduplicate, or prepare a raw Medibank provider claims dataset for analysis, regulatory submission, or payment processing. Produces an audited, formula-protected, fully traceable cleaned workbook with change log, data quality certificate, and original data preserved. Do not use for member demographic updates, financial posting, or any action that writes back to a source system.
---

# Clean and Standardise Medibank Provider Claims Data in Excel

## Purpose
Ingest a raw Medibank provider claims extract, apply a rigorous, auditable cleaning and standardisation pipeline, and deliver a workbook in which the cleaned data is clearly separated from the original, every change is logged with a reason code, and a Data Quality Certificate can be signed off before the data proceeds to analysis, payment processing, or regulatory reporting. The workbook must never silently modify source data, must preserve a verbatim copy of every raw value, and must produce a cleaning summary that an actuary or compliance officer can review independently.

## Non-negotiable outcome
Do not stop at highlighting issues or listing recommendations. Apply the full cleaning pipeline to a working Excel workbook and deliver a file ready for downstream use.

The workbook is complete only when:
- a verbatim, locked, original-data sheet is present and formula-protected before any cleaning is applied;
- every change to a cell is recorded in a Change Log sheet with: row number, column, original value, cleaned value, reason code, and timestamp;
- duplicate detection distinguishes exact duplicates (identical Claim ID) from near-duplicates (same Provider + Member + Service Date + Amount within tolerance);
- all mandatory fields are validated against Australian health insurance data standards before and after cleaning;
- date values are standardised to `DD-MMM-YYYY` and invalid dates are flagged, not silently dropped or defaulted;
- MBS item numbers, provider ABNs, and diagnosis codes (ICD-10-AM where present) are format-validated, not assumed correct;
- a Data Quality Certificate sheet shows pre-cleaning and post-cleaning record counts, error rates, field-level completeness, and overall status (CERTIFIED / NEEDS REVIEW / NOT CERTIFIED);
- a Cleaning Summary sheet shows every rule applied, records affected, and remediation action taken;
- the cleaned dataset is protected against further accidental editing with sheet protection applied after delivery;
- the user can reproduce the cleaning run by re-applying the documented rules in the Change Log.

## Operating principles
1. Preserve the original data verbatim. The first action of every cleaning run is to copy the raw data to a sheet named `Raw_Original` and apply worksheet protection (no password, to allow deliberate override but prevent accidental edits). All subsequent work is on a `Cleaned_Data` sheet copied from the raw.
2. Never delete a row without explicit user confirmation. Flag, quarantine, or mark rows as Pending Deletion; execute deletion only after the user has reviewed and confirmed the quarantine list.
3. Every cleaning rule applied must produce a Change Log entry. Bulk operations (e.g. trim all whitespace) produce one log entry per affected cell, not one per batch. Untraceable bulk changes invalidate the audit trail.
4. Distinguish between critical errors (prevent downstream processing), major errors (reduce analytical reliability), and minor errors (cosmetic or non-critical). Treat them differently in the quality certificate.
5. Australian Private Health Insurance Act 2007 (Cth) and APRA PHI prudential standards require that data submitted to regulators or used in benefit calculations is accurate and complete. Any claims dataset destined for APRA reporting or benefit payment must meet CERTIFIED status before leaving this workbook.
6. MBS item numbers must match the format: numeric, 3–8 digits, in the range listed in the current Medicare Benefits Schedule. Flag item numbers outside this range for manual review — do not delete them, as they may be DVA, compensable, or overseas codes with different validation rules.
7. Provider ABNs: validate the 11-digit Australian Business Number check digit algorithm. An ABN that fails the check digit is definitively invalid. An ABN that passes may still be incorrect — flag it as Format Valid but do not label it Confirmed.
8. ICD-10-AM diagnosis codes (where present): validate format (letter + 2 digits + optional dot + 1–4 additional characters). Flag codes that do not match this pattern; do not attempt to correct them as clinical coding requires a qualified coder.
9. Claim amounts must be positive, must not exceed the MBS schedule fee by more than a configurable tolerance (default 200% of schedule fee), and must be internally consistent: Gap Amount = Claim Amount − Benefit Paid, within a configurable rounding tolerance (default AUD 0.05).
10. Date standardisation applies only to columns explicitly identified as date columns. Do not parse dates from free-text notes or description columns. Do not assume ambiguous dates (e.g. 01/02/2024 could be 1 Feb or 2 Jan depending on the source locale) — flag them for manual resolution when the source locale cannot be confirmed.
11. Deduplication uses a hierarchy of confidence. Level 1: exact Claim ID match — these are definitive duplicates. Level 2: same Provider ABN + Member ID + Service Date + MBS Item Number + Amount within AUD 1 — these are near-duplicates requiring confirmation before action. Level 3: same Provider + Date + Amount but different Claim ID — flag as suspicious, do not act without user direction.
12. Text standardisation (Provider Name, Hospital Name, Specialist Name) uses Title Case as the default. Do not apply UPPER or lower case to proper names without user direction. Apply UPPER CASE only to code fields (Status codes, State codes, MBS codes).
13. Missing values in non-mandatory fields are filled with a structured placeholder: `[NOT PROVIDED – field name]`. Do not use "N/A", blank, zero, or "Unknown" as these are ambiguous to downstream systems.
14. Data source provenance must be recorded: what system the extract came from (HICAPS, Eclipse, internal claims system, manual CSV export), what date the extract was run, and who ran it. If this information is not supplied, mark the Data Source as TBC in the Quality Certificate and flag it as a minor risk.
15. When a field is cleaned or standardised, retain the original value in a hidden column named `ORIG_[ColumnName]` alongside the cleaned column. This allows a downstream analyst to trace any value to its raw form without opening the Change Log.
16. Do not apply any cleaning rule whose effect you cannot describe precisely in plain English. If a cleaning transformation cannot be stated as a rule (e.g. "Provider Name was changed from X to Y because of rule R"), do not apply it.
17. The cleaning pipeline is idempotent: running it twice on the same input must produce the same output. This means every rule must check current state, not assume the prior run's output.
18. Never combine raw and cleaned values in the same column. Raw goes in `Raw_Original`, cleaned goes in `Cleaned_Data`. Analysis, validation, and reporting sheets reference `Cleaned_Data` only.
19. Confirm the target use of the cleaned data before applying rules: payment processing, regulatory submission, actuarial analysis, and fraud detection each have different tolerance for false negatives vs false positives. Default to conservative flagging (flag rather than silently fix).

## First response and ambiguity handling
Before cleaning, inspect the workbook or file and infer as much as possible from column headers, sample values, and sheet names.

Ask questions only when the answer materially changes the cleaning rules applied. Otherwise, proceed with explicit documented assumptions.

Resolve these items when available:
- data source system (HICAPS, Eclipse, internal claims system, manual extract);
- extract date and period covered;
- intended downstream use (payment, APRA submission, actuarial, fraud review);
- whether MBS schedule fee tolerance check is applicable (not all datasets contain MBS items);
- whether ABN validation is required (some datasets use internal provider codes only);
- whether ICD-10-AM codes are present and whether they require format validation;
- the amount tolerance for near-duplicate detection (default AUD 1.00);
- whether prior-period data is available to cross-check for re-submitted or adjusted claims.

If the user provides a raw file with no additional context, apply the full default cleaning pipeline with conservative flags, document every assumption, and note which rules could not be applied due to missing column context.

## Workbook architecture
Use the minimum number of sheets that keeps the pipeline clear and auditable.

### 1. Raw_Original
A verbatim, formula-protected copy of the data exactly as received. Nothing is edited here. Sheet protection is applied on creation (no password). If the user later needs to re-run from source, this is the canonical starting point. Include a header block showing: source system, extract date, extracted by (TBC if unknown), row count, column count, and a warning: "This sheet is the unmodified source record. Do not edit."

### 2. Cleaned_Data
The working copy on which all cleaning rules are applied. This is the output delivered to downstream systems. Columns follow this structure:
- All original columns, in original order
- A `ORIG_[ColumnName]` hidden companion column for each column where any cleaning was applied (shows the pre-cleaning value)
- A `Row_Status` column at the far right: Clean / Flagged / Quarantined / Pending Review
- A `Flag_Count` column: count of flags on this row
- A `Priority_Flag` column: Critical / Major / Minor / None

Apply conditional formatting:
- Critical flag rows: light red fill
- Major flag rows: light amber fill
- Minor flag rows: light yellow fill
- Clean rows: white
- Quarantined rows: medium grey with strikethrough

### 3. Change_Log
An Excel table named `tblChangeLog` recording every cleaning action.

Columns:
- Run ID (incrementing integer per cleaning run)
- Timestamp (date and time the rule was applied)
- Sheet (always Cleaned_Data)
- Row Number
- Column Name
- Original Value
- Cleaned Value
- Rule Code (e.g. RULE-DATE-01, RULE-DUP-02)
- Rule Description (plain English)
- Action Taken (Corrected / Flagged / Quarantined / No Action — Confirmed Clean)
- Applied By (Copilot Skill / Manual Override)

Sort descending by Run ID and Row Number. This sheet is not editable by the user — protect it after each run, same as Raw_Original.

### 4. Cleaning_Rules
An Excel table named `tblRules` listing every rule in the cleaning pipeline.

Columns:
- Rule Code
- Category (Deduplication / Date Standardisation / Amount Validation / Code Validation / Text Standardisation / Completeness / Consistency)
- Scope (columns the rule applies to)
- Description
- Default Threshold or Parameter (editable)
- Severity if Triggered (Critical / Major / Minor)
- Action (Fix / Flag / Quarantine / Report Only)
- Active (Yes / No — allows rules to be disabled without deleting them)
- Regulatory Basis (PHI Act / APRA / MBS / Internal Medibank Standard / None)

This sheet is the configuration layer. Analysts who understand the rules can adjust thresholds or disable rules without touching the cleaning engine.

### 5. Duplicate_Review
An Excel table named `tblDuplicates` listing all records flagged at any deduplication confidence level.

Columns:
- Claim ID (primary)
- Duplicate Claim ID(s) (comma-separated)
- Match Level (1 = Exact Claim ID / 2 = Near-Duplicate / 3 = Suspicious)
- Match Basis (field names that matched)
- Match Confidence (High / Medium / Low)
- Billed Amount Difference (AUD)
- Recommended Action (Remove Duplicate / Investigate / Keep Both)
- User Decision (dropdown: Confirm Duplicate — Remove / Confirmed Unique — Keep / Escalate)

Do not remove any row until the User Decision column is set to Confirm Duplicate — Remove. Only then should a cleaning run move the row to quarantine.

### 6. Cleaning_Summary
A summary view of the full pipeline run for sign-off review.

Sections:
- Run metadata: date, source file, extract period, row count before and after
- Rule-by-rule summary: rule code, records affected, action taken
- Field-level completeness table: field name, required (Yes/No), records with value, records blank, completeness %, status
- Error breakdown by severity: Critical count, Major count, Minor count
- Quarantine list: count of rows in Quarantine, link to tblDuplicates
- Recommendation: proceed to downstream use / resolve outstanding flags first

### 7. Data_Quality_Certificate
A single-page, printable (A4 portrait) certificate designed to be signed off by the data owner before the cleaned dataset proceeds.

Include:
- Dataset name and extract date
- Cleaning run date and pipeline version
- Pre-cleaning record count, post-cleaning record count, quarantined count
- Critical errors resolved, Critical errors outstanding
- Field completeness rates for all mandatory fields
- Duplicate detection results (exact and near)
- Overall certification status (CERTIFIED / NEEDS REVIEW / NOT CERTIFIED) with clear colour coding
- Sign-off block: Data Owner name, role, date, signature space (for printing)
- Warning: "This certificate is valid for this dataset and run date only. Re-cleaning invalidates this certificate."

### 8. Read_Me
- Purpose and scope of this workbook
- Pipeline overview (8 rules, what each does)
- How to re-run the pipeline on a new extract
- How to adjust a rule threshold in the Cleaning_Rules sheet
- How to resolve a duplicate in the Duplicate_Review sheet
- How to add a new cleaning rule
- Colour legend: Critical / Major / Minor / Clean / Quarantine
- Definitions: exact duplicate, near-duplicate, MBS format valid, ABN check-digit valid
- Limitations: MBS schedule fee matching requires the current MBS schedule to be loaded separately; ABN validation is format-only, not ATO registry lookup; ICD-10-AM validation is format-only, not clinical codebook lookup
- Change log and version history

## Cleaning pipeline — eight-step workflow

### Step 1: Protect source and create working copy
- Copy raw data to `Raw_Original` sheet. Apply worksheet protection immediately.
- Copy raw data again to `Cleaned_Data` sheet. Add `Row_Status`, `Flag_Count`, `Priority_Flag` columns. Add `ORIG_` companion columns for all fields to be cleaned.
- Record pipeline start: Run ID, timestamp, source file name, row count.

### Step 2: Completeness scan
Apply rule RULE-COMP-01 through RULE-COMP-[N] for every mandatory field.

Mandatory fields and their criticality:
- Claim ID — Critical (no claim can exist without a unique identifier)
- Provider Name or Provider ABN — Critical (payment cannot be issued)
- Service Date — Critical (cannot assign to a benefit period)
- Claim Amount — Critical (cannot calculate benefit or gap)
- Member ID — Critical (cannot link to a policy)
- Benefit Paid — Major (gap calculation fails without this)
- MBS Item Number — Major (if MBS-based dataset; Critical for APRA submissions)
- Claim Status — Minor (can default to Pending Review with a flag)
- ICD-10-AM Code — Minor (required for some APRA returns; check with user)

For each blank mandatory cell: set `Row_Status` to Flagged, increment `Flag_Count`, set `Priority_Flag` to the field's criticality level, and log to Change Log with Action = Flagged.

Fill non-mandatory blank cells: `[NOT PROVIDED – {field name}]` with Major flag and log entry.

### Step 3: Deduplication
Apply deduplication rules in confidence order.

Level 1 — Exact Claim ID:
```
Rule: COUNTIF(tblClaims[Claim ID], [@[Claim ID]]) > 1
Action: Flag the second and subsequent occurrences as Level 1 Duplicate
Status: Quarantine after user confirmation in Duplicate_Review sheet
Log: RULE-DUP-01
```

Level 2 — Near-duplicate:
```
Rule: Same Provider ABN + Member ID + Service Date + MBS Item Number + Amount within AUD [Threshold]
Action: Flag as Level 2 Near-Duplicate
Log: RULE-DUP-02
Threshold: configurable in tblRules, default AUD 1.00
```

Level 3 — Suspicious match:
```
Rule: Same Provider Name (trimmed) + Service Date + Amount (exact) but different Claim ID
Action: Flag as Level 3 Suspicious — Requires Manual Review
Log: RULE-DUP-03
```

Never remove a flagged row in this step. Populate `tblDuplicates`. Await user decision.

### Step 4: Date standardisation
Apply rule RULE-DATE-01 to all columns identified as date columns (Claim Date, Service Date, Lodgement Date, Processing Date).

Standardisation logic:
1. Attempt parsing as DD/MM/YYYY (Australian default)
2. If fails, attempt DD-MM-YYYY
3. If fails, attempt YYYY-MM-DD (ISO 8601 — common in system exports)
4. If all fail, flag as RULE-DATE-PARSE-FAIL and leave value unchanged
5. If parsed successfully, write `DD-MMM-YYYY` to cleaned column, write original to `ORIG_[Column]`
6. Flag future dates (> today) as RULE-DATE-FUTURE (Major)
7. Flag dates before 01-JAN-1984 (earliest Australian PHI scheme) as RULE-DATE-IMPLAUSIBLE (Major)
8. Flag ambiguous dates where day and month are both ≤ 12 and source locale is not confirmed as RULE-DATE-AMBIGUOUS (Major)

Log every changed cell.

### Step 5: Amount and fee validation
Apply rules RULE-AMT-01 through RULE-AMT-05.

```
RULE-AMT-01: Claim Amount ≤ 0 → Critical flag
RULE-AMT-02: Benefit Paid > Claim Amount → Critical flag (benefit cannot exceed claim)
RULE-AMT-03: Gap Amount ≠ Claim Amount − Benefit Paid (within AUD 0.05) → Major flag
RULE-AMT-04: Claim Amount > [MBS_Schedule_Fee] × [Tolerance] → Major flag (if MBS fee loaded; else skip)
RULE-AMT-05: Benefit Paid < 0 → Critical flag
```

MBS schedule fee tolerance: configurable in tblRules, default 200%. Note: providers may legitimately charge above MBS schedule fee; this is a flag, not an error.

### Step 6: Code and identifier validation
Apply rules RULE-CODE-01 through RULE-CODE-04.

```
RULE-CODE-01: MBS Item Number — must be numeric, 3–8 digits. Flag non-conforming values.
RULE-CODE-02: Provider ABN — 11 digits, must pass ABN check digit algorithm. Flag failures as Critical (if payment column) or Major.
RULE-CODE-03: ICD-10-AM Code (if present) — must match pattern: [A-Z][0-9]{2}(\.[0-9A-Z]{1,4})?. Flag non-conforming values.
RULE-CODE-04: Claim Status — must be from approved vocabulary: Approved / Rejected / Pending / Adjusted / Withdrawn. Flag any value not in this list.
```

Do not attempt to correct code values — flag only. Clinical and regulatory codes require qualified human review.

### Step 7: Text standardisation
Apply rules RULE-TEXT-01 through RULE-TEXT-04.

```
RULE-TEXT-01: All text fields — remove leading and trailing whitespace (TRIM equivalent)
RULE-TEXT-02: Provider Name, Hospital Name — convert to Title Case
RULE-TEXT-03: Claim Status, State Code, MBS Group Code — convert to UPPER CASE
RULE-TEXT-04: Provider Name — standardise common abbreviations: PTY LTD, DR, PROF, ASSOC — use consistent form from tblRules
```

For every cell changed, log to Change Log.

### Step 8: Consistency and cross-field checks
Apply rules RULE-CONS-01 through RULE-CONS-05.

```
RULE-CONS-01: Service Date ≤ Lodgement Date (if both present) — flag inversion as Critical
RULE-CONS-02: Claim Amount = SUM(itemised amounts) where itemisation is present — flag mismatch
RULE-CONS-03: Provider ABN matches Provider Name (where ABN lookup table is available) — flag mismatch as Major
RULE-CONS-04: Member Policy Type is consistent with service type claimed (e.g. Extras claim on Hospital-only policy) — flag as Major
RULE-CONS-05: State of Service consistent with Provider State (if both present) — flag mismatch as Minor
```

On completion: recalculate `Flag_Count` and `Priority_Flag` for every row. Populate Cleaning_Summary. Update Data_Quality_Certificate. Apply sheet protection to `Change_Log` and `Raw_Original`.

## Calculation standards
All flag calculations reference `tblRules` for threshold values. Do not hardcode thresholds in formulas.

```
[ABN_Tolerance] = INDEX(tblRules[Default Threshold], MATCH("RULE-CODE-02", tblRules[Rule Code], 0))
[Amount_Tolerance] = INDEX(tblRules[Default Threshold], MATCH("RULE-AMT-04", tblRules[Rule Code], 0))
[NearDup_Tolerance] = INDEX(tblRules[Default Threshold], MATCH("RULE-DUP-02", tblRules[Rule Code], 0))
```

Data Quality Score:
```
Clean_Records = COUNTIF(tblCleaned[Row_Status], "Clean")
Total_Records = COUNTA(tblCleaned[Claim ID])
DQ_Score = Clean_Records / Total_Records
Status = IF(DQ_Score >= 0.95, "CERTIFIED", IF(DQ_Score >= 0.80, "NEEDS REVIEW", "NOT CERTIFIED"))
```

Completeness rate per field:
```
Completeness_Rate = (Total_Records − COUNTIF(column, "")) / Total_Records
```

## Formatting standard
Produce a clean, professional workbook suitable for review by Medibank's Finance, Risk, or Compliance team.

### Visual hierarchy
- One dark colour (deep navy or dark teal) for sheet titles and key headings.
- Red for Critical flags and NOT CERTIFIED status.
- Amber for Major flags and NEEDS REVIEW status.
- Yellow for Minor flags.
- Green for Clean rows and CERTIFIED status.
- Grey for Quarantined rows and read-only reference sheets.

### Cell conventions
- Input cells (tblRules thresholds, Duplicate_Review user decisions): light blue fill.
- Formula cells: white or very light grey.
- Flagged cells: fill colour matching severity (red / amber / yellow).
- Original companion (`ORIG_`) columns: hidden by default; light purple fill when unhidden.
- Data Quality Certificate: no gridlines, larger font (12pt), executive-ready layout.

### Number formats
- Claim amounts: `$#,##0.00;($#,##0.00);-` (AUD, two decimal places).
- Percentages: `0.0%`.
- Row counts: `#,##0` (no decimals).
- Dates: `DD-MMM-YYYY` throughout.
- Timestamps in Change Log: `DD-MMM-YYYY HH:MM:SS`.

## Quality gates
Do not deliver the workbook until all applicable gates pass.

### Structural gate
- `Raw_Original` is present, protected, and unmodified from the source.
- `Cleaned_Data` contains all original columns plus `ORIG_`, `Row_Status`, `Flag_Count`, `Priority_Flag`.
- `Change_Log` has an entry for every cleaned cell and is protected.
- `Duplicate_Review` has an entry for every flagged duplicate.

### Rule gate
- Every cleaning rule in tblRules has been applied to the relevant columns.
- Every rule has produced at least one Change Log entry or a "zero records affected" log entry confirming it ran.
- No rule was applied to a column type it was not designed for (e.g. date rules not applied to amount columns).

### Certificate gate
- `Data_Quality_Certificate` shows a status.
- Critical outstanding errors are listed explicitly (not summarised as a count).
- The certificate is NOT CERTIFIED if any Critical flag remains unresolved.

### Usability gate
- A user who has not used this workbook before can identify which rows have problems, why, and what to do next, without opening the Change Log.
- The `Read_Me` explains all steps clearly with no assumed prior knowledge.
- The Cleaning_Rules sheet allows a rule threshold to be changed in one cell with the effect flowing through all dependent formulas.

## Existing-workbook behaviour
When updating a previously cleaned dataset:
1. Increment the Run ID in the Change Log.
2. Do not overwrite the prior run's Change Log entries — append only.
3. Preserve prior `ORIG_` column values (they capture the original raw value, not the prior run's cleaned value).
4. Add a new column `Run_{N}_Status` to track the cleaning state at each run for any cell that changes across runs.
5. Update the Data Quality Certificate to show "Run N of M" and compare improvement from prior run.
6. Do not re-apply a rule that is marked Inactive in tblRules.

## Failure modes to prevent
Never:
- delete any row, cell, or value from `Raw_Original` or `Cleaned_Data` without a Change Log entry and user confirmation;
- silently correct an MBS item number, provider ABN, or ICD-10-AM code — flag only;
- assume an ambiguous date (e.g. 01/02/2024) is in DD/MM/YYYY format without confirming the source locale;
- combine or concatenate raw and cleaned values in a single column;
- apply a cleaning rule to a column whose data type cannot be confirmed from the header row and sample values;
- report CERTIFIED status while any Critical flag is still outstanding;
- omit the `ORIG_` companion column for any field where a cleaning transformation was applied;
- apply amount validation rules to non-amount columns (e.g. Member ID may be numeric but is not an amount);
- call a Level 2 or Level 3 match a "duplicate" without user confirmation — these are near-duplicates or suspicious matches only;
- produce a cleaning run that is not reproducible from the rules in tblRules and the entries in the Change Log.

## Recommended prompts for using this skill
- `@claims-data-cleaner Clean this raw HICAPS claims extract. Flag all Critical errors first, then apply the full pipeline. Do not remove any rows until I review the Duplicate_Review sheet.`
- `@claims-data-cleaner Run the pipeline on this file and produce a Data Quality Certificate for the compliance team to sign before APRA submission.`
- `@claims-data-cleaner This dataset has a near-duplicate problem — the same claim appears to have been submitted twice under different Claim IDs. Identify all Level 2 and Level 3 matches and populate the Duplicate_Review sheet for my decision.`
- `@claims-data-cleaner Validate provider ABNs and MBS item numbers in this extract and flag anything that fails format validation. Do not modify values — flag only.`

## Optional collaboration with related skills
When available and useful:
- use `@data-validator` after cleaning to produce a formal data quality score against submission rules — cleaning prepares data, validation certifies it;
- use `@anomaly-detector` after cleaning to identify statistical outliers in claim amounts that cleaning rules do not catch;
- use `@provider-payment-reconciler` after validation to reconcile the clean claims dataset against payment records.

These optional skills complement but do not replace the quality gates in this skill.

## Sources and references
**Australian regulatory requirements:**
- Private Health Insurance Act 2007 (Cth): Defines requirements for claims records, benefit calculations, and data accuracy obligations for registered health insurers.
- APRA Prudential Standard HPS 001 (Definitions) and related HPS standards: Data quality and reporting accuracy requirements for private health insurers.
- Medicare Benefits Schedule (MBS) — Department of Health and Aged Care: Defines valid MBS item numbers, schedule fees, and benefit rules. Updated quarterly.
- Australian Business Number (ABN) check digit algorithm — Australian Business Register (ABR): Standard algorithm for ABN format validation.
- ICD-10-AM (International Statistical Classification of Diseases, Australian Modification) — AIHW: Clinical coding standard for Australian health datasets.

**Data quality standards:**
- ISO/IEC 25012:2008 Data Quality Model: Defines 15 data quality characteristics; this skill prioritises completeness, accuracy, consistency, and uniqueness.
- AIHW Data Quality Framework (2012): Specific guidance for Australian health data collections — provides the basis for the six-dimension quality assessment in the Data Quality Certificate.

**Industry and technical references:**
- HICAPS — Health Industry Claims and Payments Service: Point-of-service claims processing standard used by most Australian health insurers including Medibank.
- ECLIPSE (Electronic Claims Lodgement and Information Processing Service Environment): Australian government digital health service for hospital claims.
- Australian Digital Health Agency: Standards for electronic health record data formats and interchange.
