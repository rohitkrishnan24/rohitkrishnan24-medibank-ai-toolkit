# Medibank AI Toolkit — Excel Copilot Skills

<div align="center">

![Skills](https://img.shields.io/badge/Priority%20Skills-10%20Industrial%20Standard-006D6D?style=for-the-badge&logo=microsoftexcel&logoColor=white)
![Lines](https://img.shields.io/badge/Avg%20Skill%20Depth-350%2B%20Lines-success?style=for-the-badge)
![Compliance](https://img.shields.io/badge/PHI%20Act%202007-Compliant-blue?style=for-the-badge)
![Version](https://img.shields.io/badge/Version-3.0-teal?style=for-the-badge)

**Deliverable 2 — Excel Skills Repository**  
**Rohit Krishnan · S4116077 · P000280DS · RMIT × Medibank Private · Semester 2, 2026**

</div>

---

## What Is This?

This repository contains **10 industrial-standard Microsoft 365 Copilot skills** built for Medibank knowledge workers. Each skill is a production-ready `SKILL.md` file that Medibank staff invoke directly inside Excel for the web using `@skill-name` — Copilot reads the skill and executes the full workflow automatically.

These are not guides or templates. They are complete, formula-driven workbook builders with:

- **15–19 operating principles** per skill — non-negotiable rules that govern every decision Copilot makes
- **Named Excel table architecture** — specific sheet structures, column definitions, and table names (`tblClaims`, `tblAssumptions`, etc.)
- **INDEX/MATCH on a configurable Assumptions table** — no hardcoded values anywhere in any formula
- **Quality gates** — structural, business logic, and accuracy checks that must all pass before a workbook is distributed
- **Failure modes** — explicit list of what Copilot must never do, with the reason why
- **Australian regulatory compliance** — every skill cites the relevant legislation (PHI Act 2007, APRA HPS standards, MBS, ICD-10-AM, ABN algorithm)
- **Cited sources** — peer-reviewed literature and government data sources for every benchmark and threshold

> Each skill was developed to meet or exceed the [Avishek Mitra](mailto:avishek.mitra@medibank.com.au) industrial standard benchmark (480+ lines, full workbook specification).

---

## The 10 Priority Skills

| # | Skill | Invoke | Domain | Key Depth |
|---|-------|--------|--------|-----------|
| 01 | [Claims Data Cleaner](#01-claims-data-cleaner) | `@claims-data-cleaner` | Data Quality | ABN check digit · 3-level deduplication · DQ Certificate |
| 02 | [Data Validator](#02-data-validator) | `@data-validator` | Compliance | ISO/IEC 25012 · 7 rule categories · Benford reasonableness |
| 03 | [Provider Payment Reconciler](#03-provider-payment-reconciler) | `@provider-payment-reconciler` | Finance | Medical Gap Scheme · AGC · 10-category discrepancy typing |
| 04 | [Member Lifetime Value Calculator](#04-member-lifetime-value-calculator) | `@member-lifetime-value-calculator` | Actuarial | PHI Act community rating · DCF/NPV · LHC loading |
| 05 | [Anomaly Detector](#05-anomaly-detector) | `@anomaly-detector` | Risk & Fraud | Benford's Law · Z-score · IQR · Composite risk scoring |
| 06 | [Regulatory Submission Checker](#06-regulatory-submission-checker) | `@regulatory-submission-checker` | APRA Compliance | APRA cross-field checks · 4-party sign-off · READY/HOLD/NOT READY |
| 07 | [QoQ Trend Analyser](#07-qoq-trend-analyser) | `@qoq-trend-analyser` | Reporting | Direction-coded traffic lights · PHI seasonality · Significance testing |
| 08 | [Hospital Gap Fee Analyser](#08-hospital-gap-fee-analyser) | `@hospital-gap-fee-analyser` | Member Experience | MGS compliance · Gap = Claim−Benefit · Provider Engagement Priority Score |
| 09 | [Chronic Disease Cost Modeller](#09-chronic-disease-cost-modeller) | `@chronic-disease-cost-modeller` | Health Economics | 5-year cost projection · NPV of managed care · Comorbidity multipliers |
| 10 | [Workforce Productivity Tracker](#10-workforce-productivity-tracker) | `@workforce-productivity-tracker` | People & Culture | Reddit-informed · 3-tier Copilot adoption · Cognitive Surrender modelling |

---

## Skill Detail

### 01 Claims Data Cleaner
**`@claims-data-cleaner`** · 411 lines · 8-sheet workbook

Builds a complete, traceable data cleaning pipeline for Medibank hospital claims. Every change is logged with a reason code. The source data is never modified — a protected `Raw_Original` copy is always preserved.

**Workbook:** `Raw_Original` → `Cleaned_Data` → `Change_Log` → `Cleaning_Rules` → `Duplicate_Review` → `Cleaning_Summary` → `Data_Quality_Certificate` → `Read_Me`

**What makes it industrial:**
- Three-level deduplication: L1 = exact Claim ID · L2 = Provider ABN + Member + Date + Item + Amount ±AUD1 · L3 = same provider/date/amount, different Claim ID
- ABN check digit validation (Australian Business Number 11-digit algorithm)
- MBS item format check (3–8 digit numeric) and ICD-10-AM pattern validation (A00–Z99)
- `ORIG_[ColumnName]` hidden companion columns preserve original value before cleaning
- DQ Score: **CERTIFIED** ≥95% · **NEEDS REVIEW** 80–94% · **NOT CERTIFIED** <80%

**Sources:** PHI Act 2007 · APRA HPS · MBS · ABR ABN Algorithm · ICD-10-AM (AIHW) · HICAPS · ECLIPSE · ISO/IEC 25012

---

### 02 Data Validator
**`@data-validator`** · 300 lines · 8-sheet workbook

Validates health insurance data against 7 ISO/IEC 25012 dimensions. Validates only — never modifies source data.

**Workbook:** `Source_Data` → `Validation_Rules` → `Validation_Results` → `Summary_by_Field` → `Validation_Report` → `Reference_Tables` → `Data_Quality_Certificate` → `Read_Me`

**Rule categories:**
| Code | Dimension | Example Check |
|------|-----------|---------------|
| VCOMP | Completeness | No nulls in mandatory fields |
| VACC | Accuracy | ABN check digit · MBS format · Future date flag |
| VCONS | Consistency | Benefit ≤ Claim · Gap = Claim − Benefit |
| VUNIQ | Uniqueness | Duplicate Claim ID detection |
| VTIME | Timeliness | Admission date within valid policy period |
| VVAL | Validity | Approved code vocabularies |
| VREASON | Reasonableness | Statistical outliers ≥3 SD |

**Sources:** ISO/IEC 25012 · AIHW Data Quality Framework · APRA HPS 310 · PHI Act · APRA DCP

---

### 03 Provider Payment Reconciler
**`@provider-payment-reconciler`** · 320 lines · 9-sheet workbook

Reconciles Medibank provider billing against approved payments. Never nets overbilling and underpayment — each is tracked separately. Models the Medical Gap Scheme, Access Gap Cover (AGC), Known-Gap, and No-Gap arrangements.

**Workbook:** `Billing_Raw` → `Payments_Raw` → `Assumptions` → `Reconciliation_Detail` → `Provider_Summary` → `Recoveries_Pipeline` → `Reconciliation_Summary` → `Checks` → `Read_Me`

**10-category discrepancy classification:** Confirmed Duplicate · Near-Duplicate · Suspicious · Overpayment · No-Gap Violation · Overbilling · Underpayment · Partial Awaiting Docs · Minor Rounding · Matched

**Recovery Pipeline:** High probability (≤R013 days) · Medium (≤R014 days) · Low (>R014 days) — all thresholds configurable via `tblAssumptions`

**Sources:** PHI Act Part 3-2 · PHI Benefit Requirements Rules · HICAPS · ECLIPSE · APRA HPS 310

---

### 04 Member Lifetime Value Calculator
**`@member-lifetime-value-calculator`** · 299 lines · 9-sheet workbook

Calculates the discounted future value of each member's relationship with Medibank. Community rating compliance is **Principle 1** — health status can never be used as a variable.

**Workbook:** `Executive Dashboard` → `Assumptions` → `Member_Data` → `MLV_Calculation` → `Churn_Risk_Register` → `Tier_Summary` → `Scenarios` → `Checks` → `Read_Me`

**Churn scoring — non-discriminatory signals only:**
- Tenure, satisfaction score, payment method, LHC loading flag, family type
- ❌ Health status, diagnosis, claims frequency — **prohibited by PHI Act 2007 Part 2-2**

**Value tiers:** Platinum (top decile) · Gold (top quartile or ≥AUD 50k MLV) · Silver (AUD 15k–50k) · Bronze (<AUD 15k)

**Formula:** `PV_t = Year_t_Margin ÷ (1 + Discount_Rate)^t` — uses Excel `NPV()` function, not simple multiplication

**Sources:** PHI Act 2007 Part 2-2 (community rating) · LHC Rules · APRA QPHIs · AIHW PHI data · Medibank ASX presentations

---

### 05 Anomaly Detector
**`@anomaly-detector`** · 320 lines · 10-sheet workbook

Flags statistically unusual claims for investigation using four mandatory detection methods. Never says "fraud detected" — always "flagged for investigation."

**Workbook:** `Source_Data` → `Detection_Config` → `Detection_Log` → `Anomaly_Summary` → `Benford_Analysis` → `Peer_Group_Analysis` → `Temporal_Analysis` → `Investigation_Queue` → `Statistics_Panel` → `Read_Me`

**Four mandatory methods:**
| Method | Trigger | Condition |
|--------|---------|-----------|
| Z-score | >3 SD from mean | Requires ≥30 records |
| IQR (Tukey) | Outside 1.5×IQR fences | Robust to outliers |
| Peer Group | >2 SD from peer mean | ≥5 peers → SD; <5 → IQR only |
| Benford's Law | Chi-square + MAD | ≥100 records · not price-list-constrained |

**Composite Risk Score:** `SUM(method_weight × IF(method_triggered, 1, 0))` — configurable weights in `tblConfig`

**Expected false-positive rate disclosed:** ~0.3% at 3 SD threshold (~450 per 10,000 records at 2 SD)

**Sources:** Tukey (1977) · Benford (1938) · Nigrini (2012) · ANAO Data Analytics · Services Australia Medicare fraud typologies

---

### 06 Regulatory Submission Checker
**`@regulatory-submission-checker`** · 316 lines · 10-sheet workbook

Validates APRA quarterly PHI data before lodgement. Returns **READY**, **HOLD**, or **NOT READY** — any Critical failure blocks submission.

**Workbook:** `Submission_Data` → `Validation_Rules` → `Check_Results` → `Completeness_Check` → `Cross_Field_Checks` → `Period_Consistency_Check` → `YOY_Variance_Analysis` → `Submission_Readiness_Report` → `Sign_Off_Checklist` → `Read_Me`

**Critical cross-field checks:**
- `CRIT-CF-01` Policy counts sum to total
- `CRIT-CF-02` Financial balance equation
- `CRIT-CF-03` Benefit ratio reconciliation
- `CRIT-CF-04` Benefit ratio within APRA bounds
- `CRIT-CF-05` Non-negative member counts
- `CRIT-CF-06` Quarterly period length correct

**4-party sign-off required:** Preparer · Independent Reviewer · Actuary · CFO/Delegate

**Sources:** PHI Act Part 4-3 · APRA HPS 310 · APRA DCP format spec · APRA PHIIBs · IAAust PS 400

---

### 07 QoQ Trend Analyser
**`@qoq-trend-analyser`** · 271 lines · 10-sheet workbook

Compares current quarter performance against prior quarter with statistical significance testing. Direction-coded traffic lights — rising benefit ratio is **red**, not green.

**Workbook:** `Metrics_Definition` → `Period_Data` → `QoQ_Analysis` → `Trend_Panel` → `Domain_Summaries` → `Executive_Commentary` → `Industry_Benchmark_Comparison` → `Significance_Filter_View` → `Checks` → `Read_Me`

**5 business domains:** Financial · Membership · Claims · Operations · Customer Experience

**Significance classification:**
```
Statistically Significant  →  ABS(Pct_Change) > 2 × SD(prior movements)
Flagged Material           →  ABS(Pct_Change) ≥ configurable threshold
Routine                    →  Within normal variation — not highlighted
```

**Australian PHI seasonality flags built in:**
- Hospital admissions: peak Q2 (Jan–Mar)
- Lapse rate: elevated Q4 (post-April premium increase)
- Premium revenue: step-change Q4 (annual rate increase)

**Sources:** Box-Jenkins (1970) · Campbell et al. (1997) · APRA QPHIs · AIHW hospital stats · Medibank ASX results

---

### 08 Hospital Gap Fee Analyser
**`@hospital-gap-fee-analyser`** · 302 lines · 11-sheet workbook

Quantifies member out-of-pocket costs from hospital gap fees, identifies the highest-burden hospitals and procedures, and produces a provider engagement prioritisation report.

**Workbook:** `Raw_Data` → `Assumptions` → `Gap_Analysis_Detail` → `Hospital_Analysis` → `Procedure_Analysis` → `Member_Impact_Analysis` → `State_Analysis` → `Provider_Engagement_Report` → `Trend_Comparison` → `Checks` → `Read_Me`

**Critical definition:** Gap Fee = Claim Amount − Benefit Paid (actual member out-of-pocket, **not** MBS schedule fee minus benefit)

**Provider Engagement Priority Score:**
```
Priority Score = Gap Rate × Total Admissions × (1 − No-Gap Coverage Rate)
```
Higher score = greater member impact from provider engagement

**Financially Stressed Member flag:** Total gap fees > AUD 2,000 in period (configurable A003 threshold)

**Sources:** PHI Act 2007 Part 3-2 · Medical Gap Scheme (DoH) · PHI Benefit Requirements Rules 2011 · AMA Informed Financial Consent · AIHW Hospital Performance

---

### 09 Chronic Disease Cost Modeller
**`@chronic-disease-cost-modeller`** · 330 lines · 11-sheet workbook

Projects 5-year cost trajectory for Medibank's chronic disease cohorts and calculates NPV of managed-care investment. Community rating compliance gate is mandatory before any output is distributed.

**Workbook:** `Population_Overview` → `Assumptions` → `Cohort_Data` → `Cost_Projection (BaU)` → `Cost_Projection (Managed Care)` → `ROI_Analysis` → `Prioritisation_Matrix` → `Premium_Impact_Analysis` → `Community_Rating_Check` → `Checks` → `Read_Me`

**5 modelled cohorts:** Diabetes · Cardiovascular · Musculoskeletal · Mental Health · Respiratory · Multi-morbidity

**PMPY cost multipliers (from AIHW):**
| Condition | Multiplier vs Non-Chronic |
|-----------|--------------------------|
| Cardiovascular | 3.4× |
| Diabetes | 2.8× |
| Mental Health | 2.3× |
| Musculoskeletal | 2.1× |
| Respiratory | 1.9× |
| Multi-morbidity (2 conditions) | 1.8× combined |
| Multi-morbidity (3+ conditions) | 2.6× combined |

**NPV formula:** Excel `NPV()` function with configurable discount rate — not simple multiplication

**Community Rating Check:** 5-item compliance panel — must show COMPLIANT before any output is distributed (PHI Act 2007 Part 2-2)

**Sources:** AIHW Burden of Disease 2022 · AIHW Chronic Conditions and Comorbidity · APRA QPHIs · PHI Act 2007 · MBS Chronic Disease Management items · IAAust Health Insurance Practice Notes

---

### 10 Workforce Productivity Tracker
**`@workforce-productivity-tracker`** · 583 lines · 11-sheet workbook

Tracks Microsoft 365 Copilot adoption and time savings across Medibank teams. Accounts for Cognitive Surrender (rework overhead), Shadow IT risk, and consumption billing cost — informed by Reddit community findings from r/sysadmin (834K members) and r/Office365 (142K members).

**Workbook:** `Usage_Log` → `Assumptions` → `Team_Summary` → `Productivity_Analysis` → `Cost_Benefit` → `Adoption_Dashboard` → `Shadow_IT_Register` → `Data_Governance_Readiness` → `Benchmark_Comparison` → `Checks` → `Read_Me`

**3-tier active user definition (operating principle P12):**
| Tier | Definition | Source |
|------|-----------|--------|
| Microsoft Definition | 1+ use in 30 days | ⚠️ Flagged as misleading — inflates adoption |
| Regular User | ≥3 sessions/week | Reddit community standard |
| Daily Active User | ≥1 session/day | Gold standard for ROI claims |

**Cognitive Surrender (operating principle P14):** Users passing all output through Copilot without critical review — defaults to 15% rework overhead on claimed time savings

**Net Minutes Saved** = Gross Minutes Saved × (1 − Rework Overhead %) — never uses gross savings for ROI

**Sources:** Microsoft 365 Copilot documentation · r/sysadmin · r/Office365 · r/microsoft_365_copilot · Forrester TEI (2023) · Gartner AI Adoption (2024)

---

## Repository Structure

```
rohitkrishnan24-medibank-ai-toolkit/
│
├── README.md                          ← You are here
│
├── skills/
│   └── excel/
│       ├── claims-data-cleaner/
│       │   └── SKILL.md              ← 411 lines
│       ├── data-validator/
│       │   └── SKILL.md              ← 300 lines
│       ├── provider-payment-reconciler/
│       │   └── SKILL.md              ← 320 lines
│       ├── member-lifetime-value-calculator/
│       │   └── SKILL.md              ← 299 lines
│       ├── anomaly-detector/
│       │   └── SKILL.md              ← 320 lines
│       ├── regulatory-submission-checker/
│       │   └── SKILL.md              ← 316 lines
│       ├── qoq-trend-analyser/
│       │   └── SKILL.md              ← 271 lines
│       ├── hospital-gap-fee-analyser/
│       │   └── SKILL.md              ← 302 lines
│       ├── chronic-disease-cost-modeller/
│       │   └── SKILL.md              ← 330 lines
│       └── workforce-productivity-tracker/
│           └── SKILL.md              ← 583 lines
│
└── synthetic-data/
    └── Medibank_Synthetic_Dataset_v2.xlsx
```

---

## What Makes These Industrial Standard

Each SKILL.md contains all of the following — the same structure as Medibank's internal gold standard:

| Component | What It Does |
|-----------|-------------|
| **Purpose** | Single-sentence statement of the business problem solved |
| **Non-negotiable outcome** | The exact definition of "complete" — a checklist of what must be true |
| **Operating principles (15–19)** | Rules Copilot follows on every run — regulatory constraints, calculation standards, data handling |
| **First response & ambiguity handling** | Questions to confirm before building — prevents building the wrong thing |
| **Workbook architecture** | Every sheet, every table (`tblName`), every column, with types and formulas |
| **Calculation standards** | Exact INDEX/MATCH patterns, Excel function choices, formula templates |
| **Formatting standard** | Cell formats, colour codes, conditional formatting rules |
| **Quality gates** | Structural · Business logic · Accuracy — all must PASS before distribution |
| **Failure modes** | Explicit "never do this" list with the reason |
| **Recommended prompts** | 4 copy-paste `@skill-name` examples for each skill |
| **Collaboration** | Which other skills to run before or after |
| **Sources** | Australian legislation, APRA standards, AIHW data, peer-reviewed literature |

---

## How to Deploy

```
Step 1 →  Upload the skill folder to:
          OneDrive → Copilot → Microsoft Excel → skills → [skill-folder]

Step 2 →  Open Excel for the web
          Copilot pane → ··· → Manage plugins & skills → Custom skills → Toggle ON

Step 3 →  Click inside your data and type:
          @claims-data-cleaner   (or any of the 10 skill names)
          Copilot reads the skill and executes the full workflow automatically
```

---

## Australian Regulatory Framework

All 10 skills are built against the Australian private health insurance regulatory environment:

| Legislation / Standard | Relevance |
|------------------------|-----------|
| Private Health Insurance Act 2007 (Cth) | Community rating · Gap cover · Benefit requirements · APRA reporting obligations |
| APRA HPS 310 | Capital adequacy · Data quality · Reporting standards |
| APRA Data Collection Platform (DCP) | Quarterly submission format and validation rules |
| Medicare Benefits Schedule (MBS) | Item numbering · Schedule fees · Chronic Disease Management items |
| ICD-10-AM (AIHW) | Diagnosis coding in claims data |
| Australian Business Number (ABN) algorithm | Provider identity validation |
| Private Health Insurance (Benefit Requirements) Rules 2011 | Minimum benefit standards · Gap payment classification |
| HICAPS / ECLIPSE | Claims transmission and payment standards |

---

## No Real Member Data

All skills were tested using synthetic data built to mirror real Medibank data structures.  
**No real member, provider, or claims data was used at any point in development.**

---

<div align="center">

**Medibank AI Toolkit · Deliverable 2 · P000280DS · Semester 2, 2026**  
*Rohit Krishnan · S4116077 · Medibank Private × RMIT University*

</div>
