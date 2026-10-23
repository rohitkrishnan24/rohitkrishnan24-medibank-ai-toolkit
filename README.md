
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
