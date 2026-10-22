---
name: hospital-gap-fee-analyser
description: Use when asked to analyse hospital gap fees, out-of-pocket costs, no-gap coverage rates, or member financial stress from hospital admissions at Medibank. Produces a complete gap fee workbook with hospital and procedure benchmarking, no-gap compliance monitoring, member financial impact analysis, access gap cover review, and a provider engagement prioritisation report. Do not use to identify or name individual surgeons in external documents, or to set or negotiate gap fee limits.
---

# Analyse Hospital Gap Fees and Member Out-of-Pocket Costs for Medibank

## Purpose
Build a comprehensive gap fee analysis workbook from Medibank hospital admissions data that quantifies the financial burden on members from gap fees, identifies the hospitals and procedures generating the most out-of-pocket exposure, monitors compliance with no-gap and known-gap arrangements, and produces a provider engagement report that prioritises which relationships need attention. The workbook must be actionable: it must tell the Member Experience and Provider Relations teams where the highest member impact is and what the levers are to reduce it.

## Non-negotiable outcome
Do not produce a summary table with simple averages. Build a complete, formula-driven analysis workbook.

The workbook is complete only when:
- gap fee is correctly calculated as Claim Amount minus Benefit Paid (not MBS fee minus benefit — this is the member's actual out-of-pocket);
- no-gap, known-gap, and Access Gap Cover (AGC) admissions are clearly classified and analysed separately from gap admissions;
- the Medical Gap Scheme (MGS) maximum permitted gap under the Private Health Insurance Act 2007 is applied as a benchmark against each hospital's reported gaps (where MBS schedule fees are available);
- a hospital-level ranking shows total gap fees paid by Medibank members, average gap fee, no-gap coverage rate, and known-gap compliance rate;
- a procedure-level analysis breaks down by MBS item group (or procedure category if MBS items are not available) and shows average and maximum gaps, with comparisons to industry benchmarks where available;
- a member financial impact analysis identifies financially stressed members (total gap fees exceeding a configurable threshold) and the characteristics of admissions driving their exposure;
- excess payments are modelled separately from gap fees — a member on a high-excess policy may have zero gap fee but still face significant out-of-pocket cost via their excess;
- a Provider Engagement Prioritisation report ranks hospitals by combination of gap fee burden, no-gap compliance rate, and member volume, indicating where negotiation or scheme education would have the greatest member impact;
- all thresholds (gap fee alert levels, no-gap violation definition, financially stressed member threshold, high-gap hospital definition) are configurable in a single Assumptions sheet.

## Operating principles
1. Gap fee = Claim Amount − Benefit Paid. This is what the member actually paid out of pocket. Do not substitute MBS schedule fee minus benefit (that is the theoretical gap, not the actual gap). Always confirm which columns in the supplied data represent these figures before calculating.
2. The Private Health Insurance Act 2007 Part 3-2 defines three gap payment categories: no-gap (member pays nothing beyond their excess), known-gap (member and insurer both know the gap in advance), and unexpected gap (member discovers a gap post-admission). The workbook classifies and counts each type.
3. Access Gap Cover (AGC) is Medibank's specific known-gap scheme. Under AGC, the practitioner agrees to charge no more than an approved fee above the MBS schedule fee, and Medibank covers the difference. Admissions with AGC should show zero or a controlled gap. Flag any AGC-tagged admission with a gap above the AGC-approved level as a potential contract violation.
4. The Medical Gap Scheme (MGS) under the PHI Act sets a maximum permitted known-gap amount (currently a fixed dollar threshold per service, updated periodically by the Department of Health). Flag any known-gap admission that exceeds the MGS maximum as Non-Compliant.
5. Member excess is separate from gap fee. Under the member's policy, an excess (the annual amount the member contributes before benefits are paid) is charged per admission (some policies) or per family per year. Excess payments are not gap fees — they are policy-determined co-payments. The model must track excess separately. Do not add excess and gap fee into a single out-of-pocket figure without labelling them clearly.
6. Surgeon vs hospital gap: the gap fee may arise from the surgeon (who charges above MBS fee), the anaesthetist, the hospital facility fee, or a combination. Where the data includes separate line items for each component, analyse them separately. Where only a total is available, note that surgeon vs hospital attribution is unknown.
7. Zero-gap admission does not mean zero out-of-pocket. A member with a AUD 750 excess per admission pays AUD 750 even on a no-gap procedure. The total out-of-pocket is gap + applicable excess contribution. Model this for financially stressed member identification.
8. Hospital tiers and contract types determine what gap arrangements are available. Public hospitals participating in the fully covered (default) arrangement generate no gap. Private hospitals have contracted rates that may or may not include gap protection. Flag any admission at a hospital with a no-gap agreement where a gap is recorded — this is a potential system error or contract issue.
9. Elective vs emergency admissions have different gap dynamics. Emergency admissions often involve multiple practitioners from outside the hospital's contracted network, generating unexpected gaps. The workbook tracks admission type to contextualise gap patterns.
10. High-gap hospitals vs high-gap specialties: some hospitals are high-gap because of their case mix (complex surgical cases attract higher specialist fees), not because of poor contracting. Do not label a hospital as a problem based on average gap alone without controlling for case mix.
11. Procedure volume matters for the provider engagement prioritisation. A hospital with a 60% gap rate but only 5 admissions per quarter has less impact than one with a 30% gap rate and 500 admissions. The prioritisation report multiplies gap rate by volume to create an impact score.
12. Mental health admissions: under the Private Health Insurance (Benefit Requirements) Rules 2011, psychiatric admissions have specific benefit requirements. Gap patterns in mental health may reflect different billing practices. Track separately.
13. Do not name individual surgeons in any output that could be distributed externally or to the member. The workbook may include surgeon-level analysis for internal provider management use only, clearly marked as such, and must not be included in any member-facing or externally distributed report.
14. Age and demographic analysis: gap fee burden may fall disproportionately on older members or members in specific regions. If demographic data is available, include a breakdown of average gap fee by age group and state — this informs member communications and policy advocacy.
15. The financially stressed member threshold is configurable. Default: a member whose total gap fees across all admissions in the period exceed AUD 2,000 is flagged as Financially Stressed. This threshold should be reviewed annually against CPI and average hospital costs.
16. Currency: all figures in AUD. Do not mix gross and net figures — confirm whether amounts are before or after government rebate adjustments.

## First response and ambiguity handling
Before building, confirm:
- data columns available (minimum: Admission ID, Hospital Name, Procedure/MBS Item, Claim Amount, Benefit Paid, Member ID);
- whether excess amounts are included in the data;
- whether AGC/no-gap tagging is present in the data;
- analysis period (start and end date);
- whether prior period data is available for trend comparison;
- whether MBS schedule fees can be loaded (for MGS compliance check);
- whether surgeon-level data is present (internal use only);
- the financially stressed member threshold (default AUD 2,000 total gap fees in period).

If gap amount is not a separate column but Claim Amount and Benefit Paid are both present, calculate Gap Amount = Claim Amount − Benefit Paid and note this derivation.

## Workbook architecture

### 1. Raw_Data
Protected copy of the admissions input data. Header block: period, source, record count.

Expected columns:
- Admission ID
- Member ID
- Hospital Name
- Hospital ABN (if available)
- State
- Admission Type (Elective / Emergency / Day Procedure)
- Procedure Description or MBS Item Number
- MBS Item Group (if not present, derive from item number)
- Speciality Category
- Admission Date
- Claim Amount (AUD) — what was billed
- MBS Schedule Fee (AUD) — if available
- Benefit Paid (AUD) — what Medibank covered
- Member Excess Applied (AUD) — if available
- Gap Fee (AUD) — if present; else derived
- Gap Arrangement (No-Gap / AGC / Known-Gap / Unexpected Gap / Unknown)
- Policy Type (Hospital-Only / Hospital + Extras)
- Policy Tier (Top Hospital / Medium / Basic)

### 2. Assumptions
Excel table `tblAssumptions`.

| ID | Assumption | Default Value | Unit | Status |
|----|------------|---------------|------|--------|
| A001 | High-gap hospital: average gap threshold | 1,000 | AUD | Estimate |
| A002 | High-gap hospital: gap rate threshold | 40% | % | Estimate |
| A003 | Financially stressed member: total gap threshold | 2,000 | AUD | Estimate |
| A004 | Significant gap admission: individual gap threshold | 500 | AUD | Estimate |
| A005 | MGS maximum permitted known-gap amount | 500 | AUD | Check current PHI Act |
| A006 | No-gap violation: gap above this amount on a no-gap arrangement | 0.05 | AUD | Regulatory |
| A007 | High-volume hospital: minimum admissions for ranking | 10 | count | Estimate |
| A008 | Analysis period start | TBC | Date | TBC |
| A009 | Analysis period end | TBC | Date | TBC |
| A010 | Minimum admissions for case mix adjustment | 20 | count | Estimate |
| A011 | High-cost admission threshold (for separate tracking) | 50,000 | AUD | Estimate |

### 3. Gap_Analysis_Detail
Excel table `tblGapDetail`. One row per admission (derived from Raw_Data).

Columns:
- Admission ID
- Member ID
- Hospital Name
- State
- Speciality Category
- Admission Date
- Admission Type
- Gap Arrangement (from Raw_Data)
- Claim Amount (AUD)
- MBS Schedule Fee (AUD)
- Benefit Paid (AUD)
- Gap Fee (AUD) — derived: Claim − Benefit where not supplied
- Member Excess Applied (AUD)
- Total Out-of-Pocket (AUD) — formula: Gap Fee + Member Excess Applied
- Gap Type (No Gap / Controlled Gap / Unexpected Gap / Excess Only)
- Gap as % of Claim Amount — formula: Gap Fee / Claim Amount
- MGS Compliance Flag — formula: IF(Gap Arrangement = "Known-Gap" AND Gap Fee > A005, "Non-Compliant", IF(Gap Arrangement = "No-Gap" AND Gap Fee > A006, "No-Gap Violation", "Compliant"))
- High-Cost Flag (Y/N) — formula: IF(Claim Amount > A011, "Y", "N")
- Significant Gap Flag (Y/N) — formula: IF(Gap Fee > A004, "Y", "N")

Conditional formatting:
- No-Gap Violations: dark red fill
- MGS Non-Compliant: amber fill
- Unexpected Gap: yellow fill
- No Gap: white fill
- Excess Only: light grey fill

### 4. Hospital_Analysis
Excel table `tblHospitals`. One row per unique hospital.

Columns:
- Hospital Name
- State
- Total Admissions
- Admissions with Gap (count)
- Gap Rate (% of admissions with gap > A006)
- Total Gap Fees (AUD)
- Average Gap Fee per Admission (AUD) — includes zero-gap admissions in denominator
- Average Gap Fee per Gapped Admission (AUD) — gapped admissions only
- Maximum Gap Fee (AUD)
- Members Affected (unique count)
- No-Gap Coverage Rate (% of admissions with gap = 0 or arrangement = No-Gap)
- No-Gap Violations (count)
- MGS Non-Compliant (count)
- Case Mix Complexity Index (average Claim Amount ÷ portfolio average — indicates whether high gaps reflect complex cases)
- Provider Engagement Priority Score (formula: Gap Rate × Total Admissions × ABS(1 − No-Gap Coverage Rate) — higher score = more impact from engagement)
- High-Gap Hospital Flag (Y/N — formula: IF(Average Gap > A001 AND Gap Rate > A002 AND Total Admissions ≥ A007, "Y", "N"))

Sort by Provider Engagement Priority Score descending.

### 5. Procedure_Analysis
Excel table `tblProcedures`. One row per MBS item group (or procedure category).

Columns:
- MBS Item Group / Procedure Category
- Total Admissions
- Total Gap Fees (AUD)
- Average Gap Fee (AUD)
- Maximum Gap Fee (AUD)
- % with Zero Gap
- % with Significant Gap (> A004)
- Speciality Categories involved (comma-separated)
- Most Common Hospital for this Procedure
- Gap Profile: High Volume / Low Gap — High Volume / High Gap — Low Volume / High Gap — Low Volume / Low Gap

Sort by Total Gap Fees descending.

### 6. Member_Impact_Analysis
Excel table `tblMembers`. One row per unique Member ID.

Columns:
- Member ID
- Admissions Count (in period)
- Total Claim Amount (AUD)
- Total Benefit Paid (AUD)
- Total Gap Fees (AUD)
- Total Excess Applied (AUD)
- Total Out-of-Pocket (AUD) — Gap + Excess
- Policy Type
- Policy Tier
- State
- Most Frequent Hospital
- Most Common Procedure Category
- Financially Stressed Flag (Y/N — formula: IF(Total Gap Fees > A003, "Y", "N"))
- Intervention Recommended (dropdown for Member Experience team: Proactive Call / Letter / None)

Summary panel:
- Total members with at least one gapped admission: count and %
- Financially Stressed Members (gap > A003): count and total exposure
- Average out-of-pocket per admission: across all admissions; for gapped admissions only
- Distribution of out-of-pocket: by range (0, 1–500, 501–2000, 2001–5000, 5000+)

### 7. State_Analysis
Summary by state showing: admission count, total gap fees, average gap, no-gap coverage rate, top hospitals. Identifies geographic disparities.

### 8. Provider_Engagement_Report
Prioritised list of hospitals for the Provider Relations team.

For each top-priority hospital (sorted by Engagement Priority Score):
- Hospital name, state, tier
- Key metrics: gap rate, total gap fees, members affected, no-gap violations, MGS non-compliance count
- Priority reason (formula-generated: "High volume + high gap rate" / "No-gap violations" / "MGS non-compliance")
- Recommended engagement action (dropdown: Contract Renewal Review / Scheme Education / AGC Enrolment Discussion / Escalate to Compliance)
- Current contract type (input: No-Gap / AGC / Known-Gap / Not Contracted)
- Last engagement date (input)

### 9. Trend_Comparison
If prior period data is available:
- Quarter-on-quarter change in: total gap fees, average gap fee, no-gap coverage rate, financially stressed members, no-gap violations
- Changes in top 10 high-gap hospitals (movement in ranking, new entrants, exits)
- Trend classification per metric (Improving / Stable / Deteriorating)

### 10. Checks
- Gap Fee = Claim − Benefit for all derived gaps (FAIL if derivation is wrong)
- No negative gap fees (FAIL if any — would mean benefit exceeded claim)
- Total Out-of-Pocket ≥ Gap Fee for all members (FAIL if excess drives it below)
- A008 and A009 (period dates) are populated (WARN if TBC)
- Hospital ranking includes only hospitals with ≥ A007 admissions (WARN if any below threshold)
- Provider Engagement Priority Score uses correct formula (formula audit)

### 11. Read_Me
- Purpose and scope
- Privacy restrictions: surgeon-level data is internal only; not for distribution
- Gap fee definition: Claim Amount minus Benefit Paid
- Difference between gap fee, excess, and total out-of-pocket
- MGS and AGC explanation
- How to update with new period data
- How to adjust thresholds in Assumptions
- How to add a new hospital's contract type to the Provider Engagement Report
- Limitations: case mix affects average gap interpretation; surgeon attribution requires separate data; this workbook does not adjust for excess payments unless the excess column is populated

## Calculation standards
```
[A003] = INDEX(tblAssumptions[Default Value], MATCH("A003", tblAssumptions[ID], 0))

Gap_Fee = IF(Gap_Column_Present, Gap_Column, Claim_Amount − Benefit_Paid)
Total_OOP = Gap_Fee + Member_Excess_Applied
No_Gap_Rate = COUNTIF(tblGapDetail[Gap Fee], 0) / COUNT(tblGapDetail[Admission ID])
MGS_NonCompliant = COUNTIF(tblGapDetail[MGS Compliance Flag], "Non-Compliant")
Priority_Score = Gap_Rate × Total_Admissions × (1 − No_Gap_Rate)
```

## Formatting standard
- No-Gap Violations: dark red
- MGS Non-Compliant: amber
- High-Gap Hospital: light orange fill on tblHospitals row
- Financially Stressed Members: amber flag
- Positive trends (gap reducing): green
- Negative trends (gap increasing): red
- Amounts: `$#,##0.00` at admission level; `$#,##0` at hospital/portfolio level
- Percentages: `0.0%`
- Dates: `DD-MMM-YYYY`

## Quality gates

### Structural gate
- Gap_Analysis_Detail, Hospital_Analysis, Procedure_Analysis, and Member_Impact_Analysis all exist with correct column structures.
- Gap Fee is correctly derived where not supplied.
- MGS Compliance Flag is formula-driven.

### Business logic gate
- No negative gap fees remain unresolved.
- Financially Stressed Flag uses the configurable A003 threshold, not a hardcoded value.
- Hospital ranking excludes hospitals below A007 minimum admissions.

### Accuracy gate
- Provider Engagement Priority Score formula matches the documented formula in Read_Me.
- No-Gap Coverage Rate is calculated as the proportion of admissions with gap = 0, not as the proportion tagged No-Gap (tagging may be incomplete; zero-gap is the observable fact).

## Failure modes to prevent
Never:
- calculate gap as MBS schedule fee minus benefit (that is the theoretical gap; use actual claim minus benefit);
- name individual surgeons in any output intended for member communication or external distribution;
- add gap fees and excess payments into a single "gap fee" figure without labelling;
- rank hospitals by average gap without accounting for volume (one large-gap admission inflates a small hospital);
- call a high-gap hospital a "problem" without checking whether the case mix is genuinely more complex;
- suppress No-Gap Violations — these are potential contract or compliance issues and must always be surfaced.

## Recommended prompts
- `@hospital-gap-fee-analyser Analyse this quarter's hospital admissions for gap fee patterns. Show me which hospitals have the highest burden on members and flag any no-gap violations.`
- `@hospital-gap-fee-analyser I need the Provider Engagement prioritisation for Q4 FY26 — which hospitals should our Provider Relations team contact first to discuss no-gap arrangements?`
- `@hospital-gap-fee-analyser Identify all Financially Stressed Members (total gap > AUD 2,000 this quarter) and give me a summary for the Member Experience team.`
- `@hospital-gap-fee-analyser Compare our gap fee trends this quarter vs last quarter. Has the no-gap coverage rate improved or deteriorated?`

## Optional collaboration with related skills
- Use `@claims-data-cleaner` before building if the admissions data has quality issues — incorrect Claim or Benefit amounts will produce incorrect gap calculations;
- Use `@anomaly-detector` to identify individual admissions where the gap fee is a statistical outlier for the procedure and hospital — these may indicate data errors or unusual billing;
- Use `@qoq-trend-analyser` to produce the QoQ narrative on no-gap coverage rate and average gap fee for the management report.

## Sources and references
**Australian regulatory framework:**
- Private Health Insurance Act 2007 (Cth) — Part 3-2: Gap cover arrangements — definitions of no-gap, known-gap, and AGC; MGS maximum permitted gap amounts.
- Private Health Insurance (Benefit Requirements) Rules 2011: Benefit calculation rules and minimum default benefit requirements.
- Department of Health and Aged Care — Medical Gap Scheme: Current MGS maximum permitted gap amounts (updated periodically).
- APRA Private Health Insurance Circular — Gap Cover Obligations: Compliance monitoring guidance.

**Industry data:**
- APRA Quarterly Private Health Insurance Statistics: Average benefit per hospital admission by state and fund type.
- AIHW Hospital Performance — Private Patients: Hospital-level gap payment statistics.
- Australian Medical Association (AMA) — Informed Financial Consent: Best practice standards for patient communication of expected gaps.
- Private Healthcare Australia (PHA): Industry body guidance on no-gap and known-gap scheme implementation.
