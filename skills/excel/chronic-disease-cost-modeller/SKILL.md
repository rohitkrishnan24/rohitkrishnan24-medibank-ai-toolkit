---
name: chronic-disease-cost-modeller
description: Use when asked to model the long-term cost of chronic disease cohorts, project future claims spend for members with chronic conditions, build a disease burden analysis, evaluate preventive health investment returns, or forecast the financial impact of chronic disease management programs at Medibank. Produces a population health economics workbook with cohort-level cost trajectories, care management ROI, and a preventive investment prioritisation model. Do not use for individual member health assessments or for any analysis that identifies individual members by health condition in an externally distributed output.
---

# Model Chronic Disease Costs and Preventive Health Investment Returns for Medibank

## Purpose
Build a population health economics workbook that models the long-term cost trajectory of Medibank's chronic disease member cohorts, quantifies the financial burden of each condition relative to the non-chronic member baseline, projects future claims spend under business-as-usual and managed-care scenarios, and produces a disease management return-on-investment analysis that helps Medibank allocate its preventive health investment to the cohorts and interventions with the greatest financial and member welfare impact.

The model serves two audiences: the Finance team (who need a credible, auditable cost projection for reserving and pricing) and the Health Strategy team (who need a prioritisation tool for care management program investment).

## Non-negotiable outcome
Do not produce a single-year cost summary or a disease prevalence table. Build a complete, multi-year cost projection and ROI model.

The workbook is complete only when:
- a cohort model shows at minimum the five major chronic condition groups relevant to Australian health insurers (diabetes, cardiovascular disease, musculoskeletal, mental health, respiratory) plus a combined multi-morbidity cohort;
- per-member per-year (PMPY) costs are calculated for each cohort and benchmarked against the Medibank non-chronic baseline and published AIHW/APRA industry data;
- a disease progression model projects the likely deterioration of each cohort's cost trajectory under business-as-usual — reflecting typical condition progression, comorbidity accumulation, and hospitalisation rate increases — over five years as the default projection horizon;
- a managed-care scenario applies configured intervention assumptions (reduction in hospitalisation rate, improvement in medication adherence, reduction in emergency presentations) to produce an alternative cost trajectory;
- the ROI model calculates net present value of each intervention investment using discounted cash flow with a configurable discount rate;
- a Preventive Investment Prioritisation Matrix ranks conditions by: cost burden per member, projected cost growth rate, estimated intervention effectiveness, and proportion of the Medibank book affected;
- all clinical parameters (hospitalisation rates, comorbidity progression rates, mortality rates) are sourced from AIHW or peer-reviewed literature and are configurable in the Assumptions sheet with the source cited;
- premium impact analysis shows the estimated premium revenue implication of not managing chronic disease claims growth, expressed in percentage points added to the claims ratio;
- the Community Rating Constraint section confirms that intervention targeting is based on cost and clinical need, not on excluding members from benefit or differentiating premium, consistent with the Private Health Insurance Act 2007.

## Operating principles
1. Community rating is non-negotiable. The Private Health Insurance Act 2007 Part 2-2 prohibits health insurers from setting premiums based on health status or from refusing membership. Disease management programs are cost-management and member welfare tools — they must be designed to improve outcomes for members, not to discourage high-cost members from continuing their membership. Any intervention in this model must be validated against the community rating principle before inclusion.
2. Separate actuarial inputs from clinical inputs. The actuarial parameters (claims cost, hospitalisation rate, benefit per admission) come from Medibank's own data and APRA statistics. The clinical parameters (disease progression rates, intervention effectiveness, comorbidity risk multipliers) come from peer-reviewed health literature and AIHW. Both are configurable and source-cited. Do not mix the two or allow clinical assumptions to be changed without a note that a health professional or actuary should review the change.
3. PMPY cost is the primary financial metric. Per-member-per-year cost allows comparisons across cohorts of different size, normalised for the time-in-fund effect. Always calculate PMPY, not total cost, as the primary comparison metric, and present total cost as a derived figure (PMPY × cohort size).
4. The chronic disease burden is driven by hospitalisation, not just general treatment. For most chronic conditions, hospital admissions represent 60–80% of total claims cost. Model hospital claims and general treatment claims separately.
5. Comorbidity multiplies cost non-linearly. A member with two chronic conditions does not cost twice as much as a member with one. The multi-morbidity cost multiplier (default: 1.8× for two conditions, 2.6× for three or more, sourced from AIHW) must be built into the model because co-morbidity cost interactions are the dominant driver of high-cost member clusters.
6. Medication adherence is a key modifiable lever. For chronic conditions, medication non-adherence is a primary driver of avoidable hospital admissions. The managed-care scenario must include a medication adherence lever, with a configurable assumption about the relationship between adherence improvement and hospital admission rate reduction (default: 10% adherence improvement → 7% hospital admission reduction for cardiovascular and diabetes cohorts, from published literature).
7. Mental health comorbidity deserves separate modelling. Members with a primary chronic disease who also have a mental health condition show materially higher claims costs and hospital admission rates than those without. The model includes a mental health comorbidity flag in each cohort and applies a cost uplift where data supports it.
8. Aging-in-book effect: as the cohort ages, hospitalisation rates increase. Project an annual aging factor per cohort (configurable; default: 2% cost increase per year purely from aging, independent of disease progression) to reflect that Medibank's existing chronic disease members are a year older in each projection year.
9. New entrant effect: the model separates the existing chronic disease cohort (members already identified) from projected new chronic disease diagnoses (members who will be identified in future periods based on Australian prevalence trends and Medibank's age-sex profile). Prevalence-based entry rates are configurable per condition.
10. Lapse rate adjustment: chronic disease members have a lower voluntary lapse rate than the general book (they are more dependent on their insurance coverage). The default lapse rate for chronic cohorts is 4% vs the general book 8% (configurable). This affects the cohort size projection and the revenue model.
11. Government initiatives: the Australian Government's Chronic Disease Management items (MBS items 721, 723, 729, 731) and the Pharmaceutical Benefits Scheme (PBS) co-payments affect the out-of-pocket cost to members and the net cost to the insurer. The model must flag where MBS and PBS offsets apply and note the impact on Medibank's net cost exposure.
12. Benefit ratio impact: the primary financial concern for Medibank's Finance team is the impact of chronic disease cost growth on the hospital benefit ratio. The model calculates: hospital benefit ratio with and without the chronic disease cohort cost growth, and the premium increase required to hold the benefit ratio flat.
13. Preventive health programs funded by Medibank (e.g. Gap payments for care planning, dietitian visits, telehealth navigation) are general treatment benefits. Their cost is an investment in avoiding future hospital admissions. The ROI calculation must include both the benefit cost of the preventive program and the avoided hospital cost.
14. Five-year projection is the standard horizon. Ten-year projections are available but require explicit disclaimer about increasing uncertainty. Do not present 10-year projections without noting the confidence interval widens significantly past year 5.
15. Privacy and aggregation: the model operates on cohort-level data, not individual member-level data. Output reports must show aggregated cohort statistics only. Any member-level data used in building the cohort is input data only and must not appear in any distributable output. Read_Me must explicitly state this requirement.
16. All cost projections in AUD. All dates in Australian financial year format (FYxx). All per-member figures are per enrolled member per year (PMPY), not per admission or per episode.

## First response and ambiguity handling
Before building, confirm:
- data available: does Medibank have cohort-level chronic disease data (member counts per condition, hospital admission rates, average benefits per admission, general treatment cost PMPY) or will the model start from AIHW/APRA benchmarks?
- analysis date / projection start year;
- which chronic conditions to include (default: diabetes, cardiovascular disease, musculoskeletal, mental health, respiratory, multi-morbidity);
- whether the model should include a preventive program ROI analysis, or just the cost projection;
- whether the output is for Finance (reserving/pricing) or Health Strategy (intervention prioritisation) — this affects the level of actuarial rigour required;
- whether managed-care intervention assumptions are available or should use literature defaults.

If Medibank-specific cohort data is not available, build the model from AIHW and APRA benchmarks, clearly labelling all inputs as industry estimates to be replaced with Medibank actuals once available.

## Workbook architecture

### 1. Population_Overview
Summary of the modelled population. Header block: analysis date, projection horizon, data source (Medibank actuals / industry benchmarks), review date.

Panel:
- Total Medibank members (hospital cover)
- Chronic disease prevalence by condition (member count, % of book): diabetes, cardiovascular, musculoskeletal, mental health, respiratory, multi-morbidity
- Members in two or more chronic condition cohorts (overlap count)
- Chronic disease members as % of total book
- Average age of chronic disease cohort vs total book

Reference: AIHW chronic disease prevalence rates by age group to support estimation when Medibank-specific data is unavailable.

### 2. Assumptions
Excel table `tblAssumptions`. Two sub-tables: Clinical/Epidemiological and Financial.

**Financial assumptions (FA001–FA020):**

| ID | Assumption | Default | Unit | Source |
|----|------------|---------|------|--------|
| FA001 | Discount rate for NPV calculations | 7.0% | % | Medibank WACC estimate |
| FA002 | Annual hospital cost inflation | 4.5% | % | APRA historical |
| FA003 | Annual general treatment cost inflation | 3.5% | % | APRA historical |
| FA004 | Non-chronic member PMPY hospital cost | 800 | AUD | APRA benchmark |
| FA005 | Non-chronic member PMPY general treatment cost | 400 | AUD | APRA benchmark |
| FA006 | Chronic member base lapse rate | 4.0% | % | Estimate |
| FA007 | Non-chronic member lapse rate | 8.0% | % | Medibank historical |
| FA008 | Government rebate rate | 24.3% | % | Dept of Health current |
| FA009 | Target hospital benefit ratio | 82.0% | % | Medibank internal |
| FA010 | Analysis period start (financial year) | FY26 | FY | TBC |

**Clinical/Epidemiological assumptions (CA001–CA020):**

| ID | Assumption | Default | Unit | Source |
|----|------------|---------|------|--------|
| CA001 | PMPY hospital cost multiplier — Diabetes | 2.8× | ratio | AIHW AODS 2024 |
| CA002 | PMPY hospital cost multiplier — Cardiovascular | 3.4× | ratio | AIHW AODS 2024 |
| CA003 | PMPY hospital cost multiplier — Musculoskeletal | 2.1× | ratio | AIHW MUSSD 2023 |
| CA004 | PMPY hospital cost multiplier — Mental Health | 2.3× | ratio | AIHW MHSAR 2022 |
| CA005 | PMPY hospital cost multiplier — Respiratory | 1.9× | ratio | AIHW COPD report 2023 |
| CA006 | Multi-morbidity cost multiplier (2 conditions) | 1.8× | ratio | AIHW comorbidity analysis |
| CA007 | Multi-morbidity cost multiplier (3+ conditions) | 2.6× | ratio | AIHW comorbidity analysis |
| CA008 | Annual disease progression cost uplift — Diabetes | 3.5% | % | AIHW |
| CA009 | Annual disease progression cost uplift — Cardiovascular | 2.8% | % | AIHW |
| CA010 | Annual disease progression cost uplift — Musculoskeletal | 2.2% | % | AIHW |
| CA011 | Annual disease progression cost uplift — Mental Health | 4.0% | % | AIHW MHSAR |
| CA012 | Annual disease progression cost uplift — Respiratory | 3.0% | % | AIHW |
| CA013 | Aging cost inflation factor (all cohorts, per year) | 2.0% | % | APRA aging tables |
| CA014 | Medication adherence → hospital admission reduction rate (CVD/Diabetes) | 7.0% | % | Cutler et al. (2018) |
| CA015 | Mental health comorbidity cost uplift | 35% | % | AIHW 2022 |
| CA016 | New chronic disease entrant rate (new diagnoses as % of non-chronic book) | 2.5% | % | AIHW prevalence trends |
| CA017 | Managed care: hospitalisation reduction — high-intensity program | 18% | % | Medibank pilot / literature |
| CA018 | Managed care: hospitalisation reduction — standard program | 9% | % | Medibank pilot / literature |
| CA019 | Managed care: general treatment investment PMPY | 450 | AUD | Estimate |
| CA020 | Minimum cohort size for ROI calculation | 200 | count | Estimate |

### 3. Cohort_Data
Excel table `tblCohorts`. One row per condition cohort (plus non-chronic baseline).

Columns:
- Cohort ID (COHORT-01 through COHORT-07)
- Condition Name
- Member Count (FY26 — input or benchmark)
- % of Medibank Book
- Average Member Age
- % with Mental Health Comorbidity
- % with a Second Chronic Condition
- Hospital PMPY Cost (AUD) — input or: FA004 × CA00x multiplier
- General Treatment PMPY Cost (AUD) — input or: FA005 × General Treatment Multiplier
- Total PMPY Cost (AUD) — formula: Hospital PMPY + General Treatment PMPY
- vs Non-Chronic Baseline (premium) — formula: Total PMPY / (FA004 + FA005)
- Annual Disease Progression Uplift (% — from CA008-CA012)
- Lapse Rate — FA006 (chronic) for chronic cohorts; FA007 for non-chronic

### 4. Cost_Projection (Business as Usual)
Five-year projection under no-intervention assumptions.

Structure: one block per cohort, years as columns (FY26–FY30), plus summary row.

For each cohort in each year:
- Cohort Size (previous year × (1 − Lapse Rate) + New Entrants from CA016)
- Hospital PMPY Cost (previous year × (1 + Disease Progression Uplift + FA002 + CA013))
- General Treatment PMPY Cost (previous year × (1 + FA003))
- Total Cohort Hospital Cost (AUD) — formula: Cohort Size × Hospital PMPY
- Total Cohort General Treatment Cost (AUD) — formula: Cohort Size × General Treatment PMPY
- Total Cohort Claims Cost (AUD) — sum of hospital + general treatment
- % of Total Medibank Claims (calculated against projected total Medibank premium × FA009)

Summary: total chronic disease cost across all cohorts per year, as a % of total Medibank hospital claims.

### 5. Cost_Projection (Managed Care)
Parallel projection applying intervention assumptions.

For each cohort with a managed-care program applied:
- Intervention Type (High-Intensity / Standard / None — dropdown)
- Hospital PMPY Cost Reduction (formula: IF(High, CA017, IF(Standard, CA018, 0)))
- Managed Care Investment PMPY (CA019 for cohorts with intervention — covers Medibank's investment in care management)
- Adjusted Hospital PMPY Cost — formula: BaU Hospital PMPY × (1 − Hospitalisation Reduction)
- Net PMPY Cost (AUD) — Adjusted Hospital PMPY + Managed Care Investment + General Treatment PMPY
- Cohort Size (same as BaU — intervention does not affect lapse rate in base case)
- Total Cohort Net Cost (AUD) — Cohort Size × Net PMPY Cost

For each year and cohort: Saving vs BaU (AUD) = BaU Total Cost − Managed Care Total Cost − Managed Care Investment Cost

### 6. ROI_Analysis
Excel table `tblROI`. One row per condition cohort with a managed-care intervention.

Columns:
- Cohort Name
- Intervention Type
- Annual Managed Care Investment (AUD) — Cohort Size × CA019
- Projected Annual Gross Saving (AUD) — hospitalisation reduction in dollar terms
- Net Annual Saving (AUD) — Gross Saving − Managed Care Investment
- Payback Period (years) — Investment / Net Annual Saving
- 5-Year Cumulative Net Saving (AUD)
- NPV of 5-Year Savings (AUD) — using Excel NPV function with FA001 discount rate
- NPV formula reference: `=NPV(FA001, Year1_Net_Saving, Year2_Net_Saving, Year3_Net_Saving, Year4_Net_Saving, Year5_Net_Saving) − Initial_Investment`
- ROI Multiple — NPV / 5-year Investment
- Confidence Level (High / Medium / Low — based on evidence quality of CA017/CA018 for this condition)
- Recommended (Y/N — formula: IF(NPV > 0 AND Payback < 3 years AND Cohort Size ≥ CA020, "Y", "N"))

### 7. Prioritisation_Matrix
Four-quadrant visual prioritisation framework for the Health Strategy team.

Axes: Cost Burden per Member (x-axis) vs Estimated Intervention Effectiveness (y-axis).
Bubble size: cohort size (number of members).

Supporting table `tblPriority`:
- Cohort Name
- Cost Burden Score (normalised: 1–10 based on PMPY cost vs non-chronic baseline)
- Intervention Effectiveness Score (normalised: 1–10 based on CA017/CA018 and confidence level)
- Cohort Size (count)
- 5-Year NPV of Intervention (AUD)
- Priority Quadrant (formula): High Burden / High Effectiveness → Priority 1 (Act Now); High Burden / Low Effectiveness → Priority 2 (Monitor + Research); Low Burden / High Effectiveness → Priority 3 (Quick Win); Low Burden / Low Effectiveness → Priority 4 (Maintenance)
- Recommended Action (from Priority Quadrant)

### 8. Premium_Impact_Analysis
Translates claims cost growth into premium and benefit ratio implications for the Finance team.

Rows:
- Total Medibank projected members (FY26–FY30)
- Total claims cost projection: BaU (from Cost_Projection BaU, summed across all cohorts and non-chronic members)
- Total claims cost projection: Managed Care
- Projected premium revenue: at current premium level × membership growth (configurable growth rate)
- Benefit ratio: BaU (Total BaU Claims / Premium Revenue)
- Benefit ratio: Managed Care (Total Managed Care Claims / Premium Revenue)
- Benefit ratio differential (BaU minus Managed Care)
- Equivalent premium increase required to hold benefit ratio at FA009 under BaU: formula: (BaU Claims − Target Claims at FA009 ratio) / Projected Members / 52 weeks (approximate weekly premium)
- Premium increase saved by managed care: BaU premium increase required − Managed Care premium increase required

### 9. Community_Rating_Check
A mandatory review panel included on every output to ensure compliance with the PHI Act 2007.

Checklist (formula-driven where possible, analyst sign-off required):
- All managed-care programs are voluntary member participation programs (Y/N sign-off)
- No premium differential applies to chronic vs non-chronic members as a result of this model (Y/N sign-off)
- No benefit restriction applies to members based on chronic condition status (Y/N sign-off)
- Intervention targeting is based on clinical need and cost efficiency, not on reducing the insurer's exposure to a specific member (Y/N sign-off)
- Model outputs are for internal strategic use only and will not be used to inform decisions about individual member benefits or membership status (Y/N sign-off)

Status: COMPLIANT (all five Y) / REQUIRES REVIEW (any N — stops the report from being distributed)

### 10. Checks
PASS / WARN / FAIL:
- All FA and CA assumptions populated (FAIL if blank)
- Multi-morbidity cost multiplier is being applied where % with second condition > 0 (FAIL if not)
- NPV calculations use FA001 from INDEX/MATCH (FAIL if hardcoded)
- Community_Rating_Check: all five sign-offs complete (WARN if any incomplete)
- Cohort sizes are ≥ CA020 for ROI calculations (WARN if any cohort ROI is calculated below threshold)
- Year 1 projection is within ±5% of last known actuals (where actuals exist) — validation check
- Mental health comorbidity uplift (CA015) applied to rows where mental health comorbidity % > 0 (FAIL if not)

### 11. Read_Me
- Purpose and scope of the model
- Community rating compliance statement (mandatory)
- How to update with new cohort data (replace inputs in Cohort_Data)
- How to change projection horizon (extend column structure in Cost_Projection)
- How to change discount rate (update FA001 in Assumptions — all NPV formulas will recalculate)
- How to add a new condition cohort (add row to tblCohorts, add CA parameters, extend Cost_Projection)
- Distinction between BaU and Managed Care scenarios
- Limitation: disease progression rates are population averages — individual cohorts may differ based on age mix, policy tier, and geographic location
- Limitation: 5-year projection involves material uncertainty; treat year 4–5 figures as directional, not precise
- Privacy restriction: this model uses aggregated cohort data only. It must not be used to make decisions about individual members

## Calculation standards
```
Hospital PMPY (Cohort) = INDEX(tblAssumptions[Default], MATCH("FA004",tblAssumptions[ID],0))
                         × INDEX(tblAssumptions[Default], MATCH("CA00x",tblAssumptions[ID],0))

Cohort Size Year t = Cohort Size Year (t-1)
                     × (1 − Lapse Rate)
                     + (Non-Chronic Book Size × CA016)

Hospital PMPY Year t (BaU) = Hospital PMPY Year (t-1)
                              × (1 + Disease Progression Uplift + FA002 + CA013)

NPV = NPV(FA001, Year1_NetSaving, Year2_NetSaving, ..., Year5_NetSaving)
      [Excel NPV function, not manual calculation]
```

No clinical or financial parameter may be hardcoded in a formula. All must be referenced via INDEX/MATCH on tblAssumptions.

## Formatting standard
- Priority 1 cohorts (Act Now): orange fill in Prioritisation_Matrix table
- Positive NPV: green indicator
- Negative NPV: red indicator
- Community Rating Check non-compliant: dark red, bold "REQUIRES REVIEW"
- AUD figures at cohort level: `$#,##0` (no decimals for large numbers)
- PMPY figures: `$#,##0.00`
- Percentages: `0.0%`
- Multipliers: `0.0"×"`
- Financial year labels: `"FY"YY` format
- Projection figures clearly distinguished from actuals (blue font for projected; black for actuals)

## Quality gates

### Structural gate
- All 11 sheets present with correct table names.
- tblAssumptions contains all FA001–FA020 and CA001–CA020 rows, each with ID, Default Value, Unit, and Source.
- Community_Rating_Check is on a separate sheet, not buried in assumptions.

### Financial gate
- Total BaU claims in year 1 is within ±10% of last known Medibank hospital claims total (where actuals are available).
- NPV of managed care programs is calculated using the Excel NPV function with FA001, not as a simple sum of savings.
- Benefit ratio in year 1 cross-checks against Medibank's published benefit ratio.

### Compliance gate
- Community_Rating_Check sheet shows COMPLIANT status before any output is distributed.
- Read_Me privacy restriction statement is present.

## Failure modes to prevent
Never:
- use health condition cohort membership to suggest a member should be encouraged to lapse or depart — this is both illegal under PHI Act and antithetical to Medibank's member obligations;
- treat a PMPY cost multiplier as fixed over time — it must grow with the disease progression uplift annually;
- calculate NPV by multiplying annual saving by 5 — use the Excel NPV function which applies time-discounting correctly;
- model managed-care savings without including the managed-care investment cost — the net saving is the gross saving minus the program investment;
- apply the multi-morbidity multiplier on top of the base cost and then separately apply the individual condition multipliers — the multi-morbidity multiplier replaces the stacking of individual condition multipliers for members in multiple cohorts;
- present 10-year projections without a wide confidence-band caveat;
- distribute any output before the Community_Rating_Check panel is COMPLIANT.

## Recommended prompts
- `@chronic-disease-cost-modeller Build a 5-year cost projection for our chronic disease cohorts using AIHW benchmarks. I need the benefit ratio impact for the Finance team's half-year review.`
- `@chronic-disease-cost-modeller What is the NPV of investing in a high-intensity managed care program for our diabetes and cardiovascular cohorts over the next 5 years?`
- `@chronic-disease-cost-modeller Which condition cohorts should Health Strategy prioritise for care management investment this financial year? Give me the priority matrix.`
- `@chronic-disease-cost-modeller Update the projection with the FY27 cohort data I've attached and show how the benefit ratio outlook has changed since the FY26 model.`

## Optional collaboration with related skills
- Use `@member-lifetime-value-calculator` alongside this model to understand the revenue retention value of chronic disease cohorts — high-cost members who are also long-tenured have a different retention economics profile;
- Use `@qoq-trend-analyser` to monitor whether quarterly chronic disease claims trends are tracking within the projection range or diverging;
- Use `@anomaly-detector` on claims data before building the cohort — outlier admissions can distort PMPY averages if not identified and appropriately handled.

## Sources and references
**Australian health economics and epidemiology:**
- Australian Institute of Health and Welfare (AIHW) — Australian Burden of Disease Study 2022: Disease-specific cost burden and hospitalisation rates, comorbidity analysis.
- AIHW — Chronic Conditions and Comorbidity 2022: Multi-morbidity cost multipliers and prevalence data.
- AIHW — Australia's Health 2024: Chronic disease prevalence by age group and state.
- AIHW — Mental Health Services in Australia 2022–23: Mental health comorbidity cost uplift data.
- AIHW — COPD in Australia 2023; Cardiovascular Disease in Australia 2023; Diabetes in Australia 2024; Musculoskeletal Disease in Australia 2023.

**Australian regulatory framework:**
- Private Health Insurance Act 2007 (Cth) — Part 2-2 (Community rating), Part 3-2 (Benefit payment obligations).
- Medicare Benefits Schedule (MBS) — Chronic Disease Management items (721, 723, 729, 731): GP care planning items offsetting specialist costs.
- Pharmaceutical Benefits Scheme (PBS): Co-payment structure and PBS-funded medication offsets.
- Department of Health and Aged Care — Private Health Insurance Circular — Chronic Disease Prevention Programs (guidance on permissible insurer preventive health investment).

**Financial/actuarial:**
- APRA Quarterly Private Health Insurance Statistics: PMPY cost benchmarks by insurer size.
- APRA Annual PHI Statistics: Long-run benefit ratio and MER trends.
- Institute of Actuaries of Australia (IAAust) — Health Insurance Practice Notes: Actuarial standards for projection and reserving in private health insurance.

**Clinical intervention effectiveness:**
- Cutler, D.M., et al. (2018). "Reducing Preventable Hospitalizations through Better Chronic Disease Management." Journal of Health Economics 60: 1–15. Medication adherence → hospitalisation reduction relationship.
- Lorig, K.R., et al. (1999). "Evidence suggesting that a chronic disease self-management program can improve health status while reducing hospitalisation." Medical Care 37(1): 5–14.
- Medibank Health Outcomes Reports (ASX disclosures, 2023–2024): Published data on Medibank's own disease management program outcomes.
