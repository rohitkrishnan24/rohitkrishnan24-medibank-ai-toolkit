# ⚡ Excel Copilot Custom Skills
### Industrial-Standard AI Toolkit · Medibank Private × RMIT University

<div align="center">

![Skills](https://img.shields.io/badge/Priority%20Skills-10%20Industrial%20Standard-006D6D?style=for-the-badge)
![Depth](https://img.shields.io/badge/Avg%20Depth-350%2B%20Lines%20Each-success?style=for-the-badge)
![Compliance](https://img.shields.io/badge/PHI%20Act%202007-Compliant-blue?style=for-the-badge)
![Version](https://img.shields.io/badge/Version-3.0-teal?style=for-the-badge)

**Deliverable 2 — Excel Skills Repository**
**Author: Rohit Krishnan · S4116077 · P000280DS · Semester 2, 2026**

</div>

---

## 📌 What Is This?

This repository contains **10 industrial-standard Microsoft 365 Copilot skills** built specifically for Medibank knowledge workers across Finance, Claims, Compliance, Actuarial, and Health Strategy.

These are **not guides or templates**. They are production-ready **SKILL.md files** that any Medibank employee deploys directly into Excel for the web and invokes by typing `@skill-name` — Copilot reads the skill and executes the full workflow automatically.

Each skill was rebuilt to meet the **Medibank industrial standard** — the same benchmark as Avishek Mitra's internal example. That means: 15–19 operating principles, named Excel table architecture, no hardcoded values, quality gates, failure modes, and cited Australian regulatory sources.

> 💬 **Feedback welcome** — Raise a GitHub issue or email s4116077@student.rmit.edu.au

---

## 🚀 How to Deploy — 3 Steps

```
Step 1 →  Upload the skill folder to:
          OneDrive → Copilot → Microsoft Excel → skills → [skill-folder]

Step 2 →  Open Excel for the web
          Copilot pane → ··· → Manage plugins & skills → Custom skills → Toggle ON

Step 3 →  Click inside your data and type @claims-data-cleaner
          Copilot reads the skill and executes the full workflow automatically
```

---

## ✅ The 10 Priority Skills

| # | Skill | Invoke With | Domain | What It Does |
|---|-------|-------------|--------|--------------|
| 01 | Claims Data Cleaner | `@claims-data-cleaner` | Data Quality | Cleans Medibank hospital claims with ABN validation, 3-level deduplication, ICD-10-AM checks, and a Data Quality Certificate. Every change is logged with a reason code. Source data is always preserved. |
| 02 | Data Validator | `@data-validator` | Compliance | Validates claims data across 7 ISO/IEC 25012 dimensions — completeness, accuracy, consistency, uniqueness, timeliness, validity, and reasonableness. Returns PASS / NEEDS REVIEW / FAIL. |
| 03 | Provider Payment Reconciler | `@provider-payment-reconciler` | Finance | Reconciles provider billing against Medibank payments. Classifies discrepancies into 10 types, models Medical Gap Scheme and AGC arrangements, and produces a Recovery Pipeline by probability. |
| 04 | Member Lifetime Value Calculator | `@member-lifetime-value-calculator` | Actuarial | Calculates discounted future value per member using DCF. Community rating compliance (PHI Act 2007) is Principle 1 — health status can never be a variable. Assigns Platinum / Gold / Silver / Bronze tiers. |
| 05 | Anomaly Detector | `@anomaly-detector` | Risk & Fraud | Flags suspicious claims using Z-score, IQR, Peer Group, and Benford's Law. Produces a composite risk score. Never says "fraud detected" — always "flagged for investigation." |
| 06 | Regulatory Submission Checker | `@regulatory-submission-checker` | APRA Compliance | Validates APRA quarterly PHI data before lodgement. Runs 6 critical cross-field checks. Requires 4-party sign-off (Preparer, Reviewer, Actuary, CFO). Returns READY / HOLD / NOT READY. |
| 07 | QoQ Trend Analyser | `@qoq-trend-analyser` | Reporting | Compares current quarter vs prior with statistical significance testing. Direction-coded traffic lights — rising benefit ratio is red, not green. Includes Australian PHI seasonality calendar. |
| 08 | Hospital Gap Fee Analyser | `@hospital-gap-fee-analyser` | Member Experience | Identifies where members pay the highest out-of-pocket costs by hospital and procedure. Flags no-gap violations and MGS non-compliance. Ranks hospitals by a Provider Engagement Priority Score. |
| 09 | Chronic Disease Cost Modeller | `@chronic-disease-cost-modeller` | Health Economics | Projects 5-year cost trajectory for Medibank's chronic disease cohorts. Calculates NPV of managed-care investment. Community rating compliance gate must pass before any output is distributed. |
| 10 | Workforce Productivity Tracker | `@workforce-productivity-tracker` | People & Culture | Tracks Microsoft 365 Copilot adoption and time savings. Uses 3-tier active user definition (Microsoft's 1-use-in-30-days figure is flagged as misleading). Models Cognitive Surrender rework overhead. |

---

## 📂 Repository Structure

```
skills/excel/
│
├── claims-data-cleaner/SKILL.md              ← 411 lines
├── data-validator/SKILL.md                   ← 300 lines
├── provider-payment-reconciler/SKILL.md      ← 320 lines
├── member-lifetime-value-calculator/SKILL.md ← 299 lines
├── anomaly-detector/SKILL.md                 ← 320 lines
├── regulatory-submission-checker/SKILL.md    ← 316 lines
├── qoq-trend-analyser/SKILL.md               ← 271 lines
├── hospital-gap-fee-analyser/SKILL.md        ← 302 lines
├── chronic-disease-cost-modeller/SKILL.md    ← 330 lines
└── workforce-productivity-tracker/SKILL.md   ← 583 lines
```

---

## 🔬 What Each SKILL.md Contains

```
name                    →  The @mention command to invoke the skill
description             →  When Copilot automatically triggers this skill
Purpose                 →  The exact business problem it solves for Medibank
Non-negotiable outcome  →  A checklist of what "complete" means — must all be true
Operating Principles    →  15–19 rules Copilot follows on every run
Workbook Architecture   →  Every sheet, table name, column, and formula
Calculation Standards   →  INDEX/MATCH patterns, no hardcoded values anywhere
Model build workflow    →  8-step guide to building the workbook from scratch
Existing-workbook       →  Rules for updating an already-built workbook
Quality Gates           →  Structural, business logic, and accuracy checks
Failure Modes           →  Explicit "never do this" list with the reason why
Recommended Prompts     →  4 copy-paste examples for each skill
Sources                 →  PHI Act, APRA standards, AIHW data, peer-reviewed literature
```

---

## 🎯 Industrial Standard — What That Means

| Criterion | Standard |
|-----------|----------|
| ✅ Depth | Every skill is 270–583 lines — full workbook specification, not a summary |
| ✅ No hardcoded values | All thresholds reference a configurable Assumptions table via INDEX/MATCH |
| ✅ Regulatory compliance | Every skill cites the relevant Australian legislation and APRA standards |
| ✅ Operating principles | 15–19 non-negotiable rules govern every decision Copilot makes |
| ✅ Quality gates | Structural, business logic, and accuracy gates — all must PASS before distributing |
| ✅ Failure modes | Explicit list of what Copilot must never do, with the reason why |
| ✅ Model build workflow | 8-step guide for building the workbook from scratch |
| ✅ Existing-workbook behaviour | Rules for appending new data without overwriting prior work |
| ✅ Tested | All skills validated using synthetic Medibank-style data — no real member data used |

---

## 🧪 Synthetic Test Dataset

All skills were tested using a synthetic dataset built to mirror real Medibank data structures.
**No real member, provider, or claims data was used at any point.**

📄 **File:** `synthetic-data/Medibank_Synthetic_Dataset_v2.xlsx`

| Sheet | Rows | Skills Tested |
|-------|------|---------------|
| Provider Claims | 485 rows | `@claims-data-cleaner` `@anomaly-detector` `@data-validator` |
| Member Register | 300 members | `@member-lifetime-value-calculator` |
| Quarterly Performance | 20 metrics × 7 quarters | `@qoq-trend-analyser` `@regulatory-submission-checker` |
| Chronic Disease Cohorts | 8 cohorts | `@chronic-disease-cost-modeller` |
| Hospital Admissions | 150 admissions | `@hospital-gap-fee-analyser` |
| Provider Billing | 200 claim lines | `@provider-payment-reconciler` |
| Medibank Payments | 200 payment records | `@provider-payment-reconciler` |
| Copilot Usage | 200 rows | `@workforce-productivity-tracker` |

---

## 💬 Questions for Review

> 1. Do these 10 skill scenarios reflect the highest-impact Excel tasks across Medibank's Finance, Claims, Compliance, and Health Strategy teams?
> 2. Are there specific business units whose workflows should be developed further?
> 3. Does the quality and depth of each SKILL.md match the industrial standard expected for production deployment?
> 4. Are there skills you would like tested against a specific Medibank scenario or dataset?

Raise a GitHub issue on this repository or email **s4116077@student.rmit.edu.au** — response within 24 hours.

---

<div align="center">

**Excel Skills Repository · Deliverable 2 · P000280DS · Semester 2, 2026**
*Rohit Krishnan · Medibank Private × RMIT University*

</div>
