# ⚡ Excel Copilot Custom Skills
### Enterprise AI Enablement Toolkit · Medibank Private × RMIT University

<div align="center">

![Skills](https://img.shields.io/badge/Excel%20Skills-20%20Total-006D6D?style=for-the-badge)
![Core](https://img.shields.io/badge/Core%20Skills-10%20Live-success?style=for-the-badge)
![Advanced](https://img.shields.io/badge/Advanced%20Skills-10%20Built-006D6D?style=for-the-badge)
![Version](https://img.shields.io/badge/Version-2.0-teal?style=for-the-badge)

**Deliverable 2 — Excel Skills Repository**
**Author: Rohit Krishnan · S4116077 · P000280DS · Semester 2, 2026**

</div>

---

## 📌 What Is This?

This folder contains **20 custom Excel Copilot skills** built specifically for Medibank knowledge workers across 10 different business disciplines.

These are **not guides or documentation**. They are real, working **SKILL.md files** that any Medibank employee can deploy directly into Microsoft 365 Copilot and invoke by typing `@skill-name` inside Excel for the web — like a command.

> 💬 **Feedback welcome** — Do these skills match what your teams actually do in Excel? Are there scenarios to add, change, or prioritise? Please raise a GitHub issue or email s4116077@student.rmit.edu.au

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

## ✅ Core Skills — 10 Live and Tested

| # | Skill | Invoke With | Discipline | What It Does |
|---|---|---|---|---|
| 01 | Claims Data Cleaner | `@claims-data-cleaner` | Data Quality | Removes duplicates, fixes blanks, standardises dates, removes spaces. Creates Cleaning Summary sheet. |
| 02 | Anomaly Detector | `@anomaly-detector` | Risk & Fraud | Flags statistical outliers, negative values, and suspicious duplicates. Produces prioritised Anomaly Report. |
| 03 | QoQ Trend Analyser | `@qoq-trend-analyser` | Reporting | Compares quarters across all metrics. Calculates change. Writes executive commentary automatically. |
| 04 | Executive Summary Generator | `@executive-summary-generator` | Executive Reporting | Reads any data table and writes a 150–200 word professional executive summary. Board and APRA ready. |
| 05 | Provider Benchmarker | `@provider-benchmarker` | Provider Relations | Ranks all providers against the group average. Produces colour-coded league table and performance note. |
| 06 | Data Validator | `@data-validator` | Compliance | Checks data against completeness, format, and logic rules. Produces PASS / REVIEW / FAIL score. |
| 07 | Formula Builder | `@formula-builder` | Self Service | Translates plain English into correct Excel formulas with full explanation. Ideal for non-technical staff. |
| 08 | Member Insights Summariser | `@member-insights-summariser` | Member Experience | Analyses satisfaction by state and product type. Flags at-risk members. Writes insights narrative. |
| 09 | Report Narrative Writer | `@report-narrative-writer` | Communications | Transforms a data table into a polished 150–250 word report narrative. Management and board ready. |
| 10 | Chart Visualisation Adviser | `@chart-visualisation-adviser` | Presentations | Recommends the right chart and builds it with Medibank teal branding and a key insight annotation. |

---

## 🚀 Advanced Skills — 10 Built Across Business Disciplines

| # | Skill | Invoke With | Discipline | What It Does |
|---|---|---|---|---|
| 11 | Waiting List Analyser | `@waiting-list-analyser` | Clinical Quality | Flags members waiting beyond clinical benchmarks by hospital, procedure, and state. |
| 12 | Claims Triage Assistant | `@claims-triage-assistant` | Operations | Scores and prioritises pending claims by urgency and complexity. Reduces processing backlog. |
| 13 | Regulatory Submission Checker | `@regulatory-submission-checker` | Compliance & Legal | Validates APRA quarterly data before lodgement. Returns READY or NOT READY status with error list. |
| 14 | Broker Performance Tracker | `@broker-performance-tracker` | Sales & Distribution | Ranks brokers by member quality, retention, and commission efficiency. Assigns Platinum to Review tiers. |
| 15 | Member Lifetime Value Calculator | `@member-lifetime-value-calculator` | Actuarial | Calculates estimated lifetime value per member. Flags high value at-risk members for retention priority. |
| 16 | Hospital Gap Fee Analyser | `@hospital-gap-fee-analyser` | Member Experience | Identifies where members pay highest out-of-pocket costs by hospital, procedure, and surgeon. |
| 17 | Chronic Disease Cost Modeller | `@chronic-disease-cost-modeller` | Health Economics | Projects 12 and 36 month claims costs for chronic condition members. Prioritises care management. |
| 18 | Provider Payment Reconciler | `@provider-payment-reconciler` | Finance & Accounts | Reconciles provider billing against approved payments. Flags overbilling, underpayments, and duplicates. |
| 19 | Mental Health Claims Tracker | `@mental-health-claims-tracker` | Mental Health | Tracks session utilisation against policy limits. Flags members approaching cap for care coordination. |
| 20 | Workforce Productivity Tracker | `@workforce-productivity-tracker` | People & Culture | Tracks Copilot adoption and time saved by team. Calculates total AI productivity ROI. |

---

## 📂 Repository Structure

```
skills/excel/
│
├── Core Skills (10) ✅
│   ├── claims-data-cleaner/SKILL.md
│   ├── anomaly-detector/SKILL.md
│   ├── qoq-trend-analyser/SKILL.md
│   ├── executive-summary-generator/SKILL.md
│   ├── provider-benchmarker/SKILL.md
│   ├── data-validator/SKILL.md
│   ├── formula-builder/SKILL.md
│   ├── member-insights-summariser/SKILL.md
│   ├── report-narrative-writer/SKILL.md
│   └── chart-visualisation-adviser/SKILL.md
│
└── Advanced Skills (10) ✅
    ├── waiting-list-analyser/SKILL.md
    ├── claims-triage-assistant/SKILL.md
    ├── regulatory-submission-checker/SKILL.md
    ├── broker-performance-tracker/SKILL.md
    ├── member-lifetime-value-calculator/SKILL.md
    ├── hospital-gap-fee-analyser/SKILL.md
    ├── chronic-disease-cost-modeller/SKILL.md
    ├── provider-payment-reconciler/SKILL.md
    ├── mental-health-claims-tracker/SKILL.md
    └── workforce-productivity-tracker/SKILL.md
```

---

## 🔬 What Each SKILL.md Contains

```
name        →  The @mention command to invoke the skill
description →  When Copilot automatically triggers this skill
Purpose     →  What business problem it solves for Medibank
Before you begin  →  What to check before running
Workflow    →  Step-by-step instructions Copilot executes
Output      →  Exactly what the workbook looks like after
Pitfalls    →  Common mistakes to avoid
```

---

## 🎯 Quality Framework — All 6 Criteria Met

| # | Criterion | Standard |
|---|---|---|
| ✅ 1 | Specific Scenario | Each skill addresses a named, real Medibank business task |
| ✅ 2 | Clear Instructions | Plain language, no jargon, executable by any employee |
| ✅ 3 | Defined Output | Sheet name, content, and format explicitly stated |
| ✅ 4 | Pitfalls Documented | Common mistakes section in every SKILL.md |
| ✅ 5 | Tested | Core skills validated using synthetic Medibank-style data |
| ✅ 6 | Versioned | Version 1.0.0 — all changes tracked in Git commit history |

---

## 🧪 Synthetic Test Dataset

All core skills were tested using a synthetic dataset built to mirror real Medibank data structures. **No real member, provider, or claims data was used at any point.**

📄 **File:** `synthetic-data/Medibank_Synthetic_Dataset_v2.xlsx`

| Sheet | Rows | Skills Tested |
|---|---|---|
| Provider Claims | 485 rows | `@claims-data-cleaner` `@anomaly-detector` `@data-validator` |
| Member Register | 300 members | `@member-insights-summariser` `@member-lifetime-value-calculator` |
| Quarterly Performance | 20 metrics × 7 quarters | `@qoq-trend-analyser` `@executive-summary-generator` |
| Provider Benchmarking | 15 providers × 10 metrics | `@provider-benchmarker` |
| Member Satisfaction Survey | 200 responses | `@member-insights-summariser` |
| Fraud Risk Register | 80 cases | `@anomaly-detector` |
| Hospital Admissions | 150 admissions | `@hospital-gap-fee-analyser` `@report-narrative-writer` |
| Premium Revenue | 21 months × 5 policy types | `@qoq-trend-analyser` |

---

## 💬 Questions for Review

> 1. Do these 20 skill scenarios reflect the most common Excel tasks performed across Medibank teams?
> 2. Are there specific business units — Provider Relations, Member Services, Finance, Analytics, Clinical — whose workflows should be prioritised further?
> 3. Does the `@skill-name` invocation method feel practical and intuitive for everyday employee use?
> 4. Are there any skills you would like developed further or tested against a more specific Medibank scenario?

Raise a GitHub issue on this repository or email **s4116077@student.rmit.edu.au** — response within 24 hours.

---

<div align="center">

**Excel Skills Repository · Deliverable 2 · P000280DS · Semester 2, 2026**
*Rohit Krishnan · Medibank Private × RMIT University*

</div>
