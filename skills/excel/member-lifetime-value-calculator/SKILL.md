---
name: member-lifetime-value-calculator
description: Use when asked to calculate member lifetime value (CLV/LTV) for Medibank health insurance members, prioritise retention investment, identify high-value members at churn risk, or build a member portfolio valuation model. Produces a formula-driven, scenario-tested Excel workbook with discounted future cashflows, risk-adjusted value tiers, churn probability scoring, and a retention investment framework. Do not use for individual premium rating, actuarial pricing, underwriting decisions, or any discriminatory segmentation prohibited under community rating obligations.
---

# Calculate Member Lifetime Value and Prioritise Retention for Medibank

## Purpose
Build an evidence-based Member Lifetime Value (MLV) model in Excel that translates historical premium, claims, and tenure data into a forward-looking view of member value, identifies Platinum and Gold members at churn risk who warrant proactive retention investment, and produces a retention ROI framework. The model must operate within Australian private health insurance community rating obligations, must never use health status as a segmentation variable, and must clearly distinguish historical value (what a member has contributed) from modelled future value (an estimate subject to documented assumptions).

## Non-negotiable outcome
Do not stop at a ranked member list or a simple LTV formula. Build a complete, auditable workbook.

The workbook is complete only when:
- historical value (past premium less past claims) and modelled future value (discounted expected future cashflows) are calculated separately and clearly labelled;
- future value uses a proper discounted cashflow calculation, not simple multiplication of annual premium by estimated remaining years;
- churn probability is scored on observable, non-discriminatory signals (tenure, satisfaction score, payment behaviour, product coverage) and does not use health status, diagnosis, or claims history as a churn predictor;
- all assumptions (discount rate, retention tenure mapping, cost of churn, cost of retention action) are in a single Assumptions sheet and referenced by Assumption ID throughout the model;
- three scenarios are modelled — Conservative, Realistic, Optimistic — allowing leadership to see value under different retention effectiveness assumptions;
- a Retention Investment Framework shows: for each value tier, the recommended maximum spend per member on retention, calculated from break-even on modelled future value;
- a Churn Risk Register lists every High-Value (Platinum/Gold) member with a churn probability above the threshold, their modelled future value, and the retention action category that provides positive ROI;
- the model satisfies community rating obligations — no pricing, no differential service access, and no value tier visible to the member or in any external communication;
- formula and logic checks pass and the model reconciles portfolio total MLV to the sum of individual member values.

## Operating principles
1. Community rating is a legal requirement under the Private Health Insurance Act 2007 (Cth). Health insurers in Australia cannot charge higher premiums or provide lower service based on health status. The MLV model is for internal resource allocation only and must never be used to determine service levels or communicate tier status to members.
2. Distinguish historical value from modelled future value at every level — member row, summary, and dashboard. Historical value is fact; future value is a model output. Label them accordingly.
3. The discount rate applied to future cashflows must reflect the organisation's weighted average cost of capital or a nominated hurdle rate, not a market interest rate alone. Use the rate supplied by the user; if none is supplied, default to 8% with a TBC flag and a note that this requires CFO or actuarial confirmation.
4. Churn probability scoring uses only non-discriminatory, observable signals. Permitted signals: tenure (longer = lower churn risk), satisfaction score (NPS or CSAT where available), payment behaviour (direct debit vs manual = lower churn risk), product coverage (hospital + extras = lower churn risk than hospital-only), premium affordability indicator (premium ÷ estimated income bracket), prior contact with retention team (Y/N). Prohibited signals: claims frequency, diagnosis, health status, age, gender, family size.
5. Retention tenure mapping converts qualitative renewal intent (if available) or churn probability score to an estimated remaining tenure. If renewal intent data is not available, estimate from tenure-based cohort survival curves. Document the method clearly in the Assumptions sheet.
6. The cost of churn must be modelled at two levels: direct cost (acquisition cost of a replacement member, estimated at the marketing cost per new policy) and indirect cost (value of the departed member's expected future cashflows). Both must be shown separately. Netting them into a single "churn cost" conceals the drivers.
7. Retention investment ceiling: for any member or segment, the maximum economically rational retention spend is: Modelled Future Value × Probability of Successful Retention × (1 − discount for uncertainty). Do not set retention budgets above this ceiling without documented justification.
8. Segment by value tier, not by health status. Value tiers are calculated from MLV: Platinum (top decile), Gold (second decile or > AUD 50,000 MLV), Silver (AUD 15,000–50,000), Bronze (< AUD 15,000). Tier boundaries are configurable in the Assumptions sheet.
9. Policy type matters for value calculation. A hospital-only policy has a different premium and claims profile than a hospital + extras policy. Model them separately and aggregate carefully.
10. Government rebate treatment: Australian private health insurance premiums are partially subsidised by the government rebate (Lifetime Health Cover loading and the income-tested rebate). The net premium to Medibank (after rebate clawback) differs from the gross premium paid by the member. Use net premium in the cashflow model, not gross premium.
11. Medibank Private vs ahm brand: Medibank operates two brands. MLV profiles differ significantly between them due to different member demographics, product mix, and retention economics. If the dataset spans both brands, disaggregate.
12. Family vs individual policies: a family policy's value includes the risk of multiple members leaving simultaneously (family lapse). Model family policies with an adjusted churn probability — family lapse events are rarer but have 2–4× the individual impact.
13. Lifetime Health Cover (LHC) loading affects member retention decisions. Members who joined health insurance after age 30 carry an LHC loading (2% per year after 30, up to 70%). A member who lapses after 10 years with LHC loading faces a financial penalty for rejoining. This makes their retention probability higher than their satisfaction score suggests. Model it as a retention modifier.
14. Claims are not a pure negative in the MLV calculation for a health insurer. High-claiming members drive renewal because they value their coverage. Pure claims-cost-minus-premium thinking produces a misleading ranking. Use the CLV framework: premium income minus claims (the margin) as the value driver, not premium alone.
15. The model outputs are for internal use by retention, marketing, and strategy teams only. No member-facing system should access these tiers or scores. Ensure the Read Me includes an explicit privacy and use restriction statement.
16. Validate model outputs against known portfolio averages. If the portfolio average MLV is significantly different from industry benchmarks or prior actuals, investigate before presenting to leadership.
17. The three scenarios (Conservative, Realistic, Optimistic) differ in: churn probability assumption, retention success rate, and future claims cost escalation. Each must be a complete, standalone scenario, not a sensitivity on a single variable.
18. Never present a single-point MLV figure without confidence bounds or a scenario range. Future value is inherently uncertain; presenting it as precise misleads decision-makers.

## First response and ambiguity handling
Before building, inspect the member data and infer what is available.

Resolve these items when available:
- member dataset: Member ID, annual premium (gross and net of rebate if available), years as member, total claims paid in last 12 months, policy type (hospital/extras/combined), brand (Medibank/ahm), payment method, satisfaction score or NPS;
- renewal intent data (if available from survey);
- discount rate (default 8% if not supplied);
- retention action budget per tier (if known);
- acquisition cost per new member (for churn cost calculation; if unknown, flag as TBC);
- whether LHC loading data is available (affects retention probability modifier);
- tier boundary thresholds (default: Platinum top decile, Gold top quartile, Silver mid, Bronze below);
- whether to model Medibank and ahm separately or combined.

If member-level data is limited (e.g. only aggregate cohort data is available), build the model at cohort level and note that individual-member MLV requires row-level data.

## Workbook architecture

### 1. Executive Dashboard
Decision view for retention leadership.

Include:
- Total portfolio MLV under three scenarios (side-by-side)
- Member count by tier (Platinum / Gold / Silver / Bronze)
- Total MLV at risk (Platinum + Gold members with churn probability above threshold)
- Retention investment ceiling by tier
- Top 10 members by MLV and churn risk (MLV × churn probability = Value at Risk)
- Scenario selector driving all dashboard figures
- Disclaimer: "Internal use only. Not for member communication or external distribution."

### 2. Assumptions
Excel table `tblAssumptions` with Assumption ID, Category, Assumption, Conservative, Realistic, Optimistic, Active Value, Unit, Source, Status.

Required assumptions:

| ID | Assumption | Conservative | Realistic | Optimistic | Unit | Status |
|----|------------|-------------|-----------|------------|------|--------|
| A001 | Discount rate | 10% | 8% | 6% | % p.a. | TBC — confirm with Finance |
| A002 | Platinum MLV threshold | Top 10% | Top 10% | Top 10% | Decile | Estimate |
| A003 | Gold MLV minimum (AUD) | 40,000 | 50,000 | 60,000 | AUD | Estimate |
| A004 | Silver MLV range (AUD lower) | 10,000 | 15,000 | 20,000 | AUD | Estimate |
| A005 | High churn risk threshold (probability) | 30% | 40% | 50% | % | Estimate |
| A006 | Retention programme success rate | 40% | 55% | 70% | % | Estimate |
| A007 | Acquisition cost per new member (AUD) | 800 | 600 | 400 | AUD | TBC |
| A008 | Premium escalation rate p.a. | 2.5% | 3.5% | 4.5% | % | Industry benchmark |
| A009 | Claims cost escalation rate p.a. | 5.0% | 6.5% | 8.0% | % | Industry benchmark |
| A010 | Tenure-based renewal intent mapping — 1 yr | 60% | 65% | 70% | % prob. of renewal | Estimate |
| A011 | Tenure-based renewal intent mapping — 3 yr | 70% | 75% | 80% | % prob. of renewal | Estimate |
| A012 | Tenure-based renewal intent mapping — 5 yr+ | 80% | 85% | 90% | % prob. of renewal | Estimate |
| A013 | LHC loading retention modifier | +5% | +8% | +10% | % added to renewal prob. | Regulatory estimate |
| A014 | Family policy lapse multiplier | 1.5× | 2.0× | 2.5× | × individual churn cost | Estimate |
| A015 | Government rebate rate (average portfolio) | 24% | 24% | 24% | % of premium | Confirmed — PHIA 2007 |

### 3. Member_Data
Excel table `tblMembers`. One row per member policy.

Columns:
- Member ID
- Brand (Medibank / ahm)
- Policy Type (Hospital Only / Extras Only / Hospital + Extras)
- Policy Classification (Individual / Couple / Family / Single Parent)
- Annual Gross Premium (AUD)
- Annual Net Premium (AUD) — formula: Gross Premium × (1 − A015)
- Years as Member
- Claims Last 12M (AUD)
- Annual Net Margin (formula: Net Premium − Claims Last 12M)
- Satisfaction Score (1–10 NPS; leave blank if unavailable)
- Renewal Intent (dropdown: Definite / Likely / Undecided / Likely Leave / Definite Leave / Not Available)
- Payment Method (dropdown: Direct Debit / Credit Card / Manual / PAYG via Employer)
- LHC Loading Flag (Y/N)
- Prior Retention Contact (Y/N)
- Data Completeness (formula: % of non-blank fields in mandatory columns)

### 4. MLV_Calculation
Excel table `tblMLV`. Links from tblMembers; all calculations here.

Columns:
- Member ID
- Historical Value (formula: Annual Net Premium × Years as Member − Total Claims to Date [if available; else Net Margin × Years])
- Churn Probability Score (formula-driven from signals — see below)
- Estimated Remaining Tenure (years) — formula-driven from tenure mapping assumptions
- Future Annual Net Margin (formula: Annual Net Margin × (1 + A008) / (1 + A009) — net of premium and claims escalation)
- Discounted Future Value (DCF formula: sum of annual net margins over estimated remaining tenure, discounted at A001)
- Total MLV (formula: Historical Value + Discounted Future Value)
- Value Tier (formula: Platinum / Gold / Silver / Bronze based on A002–A004)
- Value at Risk (formula: Total MLV × Churn Probability)
- Retention Investment Ceiling (formula: Discounted Future Value × Churn Probability × A006)
- MLV Rank (RANK function within full portfolio)

Churn Probability Score:
```
Base_Prob = IF(Renewal Intent = "Definite Leave", 90%,
           IF(Renewal Intent = "Likely Leave", 70%,
           IF(Renewal Intent = "Undecided", 50%,
           IF(Renewal Intent = "Likely", 25%,
           IF(Renewal Intent = "Definite", 10%,
           1 − INDEX(tblAssumptions[Active Value], MATCH("A010"/"A011"/"A012", tblAssumptions[ID], 0))  [tenure-based if Not Available])))))

Satisfaction_Modifier = IF(Satisfaction Score < 5, +15%,
                        IF(Satisfaction Score < 7, +5%,
                        IF(Satisfaction Score >= 9, −10%, 0%)))

Payment_Modifier = IF(Payment Method = "Manual", +10%, 0%)
LHC_Modifier = IF(LHC Loading Flag = Y, −[A013], 0%)
Family_Modifier = IF(Policy Classification = "Family", −5%, 0%)

Churn Probability = MIN(95%, MAX(5%, Base_Prob + Satisfaction_Modifier + Payment_Modifier + LHC_Modifier + Family_Modifier))
```

Discounted Future Value (DCF):
```
For each year t from 1 to Estimated_Remaining_Tenure:
  Year_t_Margin = Future Annual Net Margin × (1 + A008)^t / (1 + A009)^t
  PV_t = Year_t_Margin / (1 + A001)^t

Discounted Future Value = SUM(PV_t for all t)
```

Use Excel's NPV function or build year-by-year columns in a helper section.

### 5. Churn_Risk_Register
Filtered view: all Platinum and Gold members with Churn Probability ≥ A005.

Columns from tblMLV plus:
- Churn Risk Tier (High / Very High — based on churn probability above A005 or above 60%)
- Value at Risk (AUD)
- Retention Investment Ceiling (AUD)
- Recommended Retention Action Category (Proactive Outreach / Product Review / Financial Assistance / Personalised Contact / Escalate to Retention Specialist)
- Assigned Retention Owner (blank — for the retention team to populate)
- Action Status (dropdown: Not Started / In Progress / Complete / Lost)

Sort by Value at Risk descending.

### 6. Tier_Summary
Portfolio view by value tier.

For each tier:
- Member count
- Total Historical Value (AUD)
- Total Discounted Future Value (AUD)
- Total MLV (AUD)
- Average MLV per member
- Average Churn Probability
- Total Value at Risk (AUD)
- Total Retention Investment Ceiling (AUD)
- Average Satisfaction Score
- Average Tenure (years)

Show three scenarios side-by-side (Conservative, Realistic, Optimistic).

### 7. Scenarios
Full portfolio view under three scenarios. For each scenario show:
- Total portfolio MLV
- MLV by tier
- Total Value at Risk
- Estimated churn losses (Value at Risk × scenario churn probability)
- Estimated retention saves (Churn Losses × A006)
- Estimated retention investment required (members actioned × average spend)
- Net retention ROI = (Retention Saves − Retention Investment) / Retention Investment

### 8. Checks
PASS / WARN / FAIL checks:
- Sum of individual MLV values reconciles to portfolio total (FAIL if difference > AUD 100)
- No member has a negative Discounted Future Value unless their claims exceed premium (WARN)
- Churn Probability is between 5% and 95% for all members (FAIL if outside)
- A001 discount rate is not TBC (WARN if TBC)
- A007 acquisition cost is not TBC (WARN if TBC)
- No member row has a blank Value Tier (FAIL if any)
- Portfolio average MLV is within a plausible range (WARN if < AUD 1,000 or > AUD 500,000 per member — may indicate a data or formula issue)

### 9. Read_Me
- Purpose and legal restrictions (community rating, internal use only)
- How to update with a new member extract
- How to change the active scenario
- Definitions: Historical Value, Discounted Future Value, Churn Probability Score, Retention Investment Ceiling
- Assumptions requiring Finance/Actuarial sign-off (A001, A007, A009)
- Privacy statement: this workbook contains member-identifiable data; access is restricted to authorised retention and strategy staff; member tier information must not be communicated to members or used in service differentiation
- Version and change log

## Calculation standards
All assumption references via INDEX/MATCH on tblAssumptions[ID]:
```
[A001] = INDEX(tblAssumptions[Active Value], MATCH("A001", tblAssumptions[ID], 0))
```

DCF building block:
```
PV_Year_1 = Net_Annual_Margin / (1 + [A001])
PV_Year_2 = Net_Annual_Margin × ((1 + A008)/(1 + A009)) / (1 + [A001])^2
...
DFV = NPV([A001], Year_1_Margin, Year_2_Margin, ..., Year_N_Margin)
```

## Formatting standard
- Platinum: deep purple fill with white text on tier indicator
- Gold: amber fill
- Silver: light grey fill
- Bronze: white fill
- Churn risk: red (Very High), orange (High), yellow (Moderate)
- Financial figures: `$#,##0;($#,##0);-` at member level; `$0.0"m"` or `$0.0"k"` at portfolio level
- Percentages: `0.0%`
- No member names or contact details on dashboard — Member ID only (privacy)

## Quality gates

### Structural gate
- tblAssumptions, tblMembers, tblMLV, tblChurnRisk all exist with correct columns.
- Scenario selector drives Active Value column in tblAssumptions.
- Community rating compliance note is present on every sheet showing member-level data.

### Financial gate
- DCF uses the discount rate from tblAssumptions, not a hardcoded value.
- Historical Value and Discounted Future Value are always shown separately.
- Portfolio MLV reconciles to sum of individual MLVs.

### Compliance gate
- No health status, diagnosis, or claims frequency variable is used in churn scoring.
- Privacy statement is present in Read_Me and Dashboard.

## Failure modes to prevent
Never:
- use health status, diagnosis code, or disease burden as a churn predictor or MLV variable;
- present MLV as a precise figure without scenario range;
- set the discount rate to 0% or use simple arithmetic instead of DCF;
- net historical value and future value into a single opaque number without showing them separately;
- use gross premium without accounting for the government rebate;
- apply a churn probability above 95% or below 5% (model breaks down at extremes);
- communicate member tier status outside authorised internal users;
- use claims history alone to predict churn — high claimers are often the most loyal members;
- present the model to leadership without confirming the discount rate with Finance or Actuarial.

## Recommended prompts
- `@member-lifetime-value-calculator Build a full MLV model from this member extract. Show three scenarios and give me the Churn Risk Register for Platinum and Gold members above 40% churn probability.`
- `@member-lifetime-value-calculator Update the MLV model with this quarter's renewal intent survey results and show me how the Value at Risk figure has changed.`
- `@member-lifetime-value-calculator What is the retention investment ceiling for our top 100 members by Value at Risk? I need this to build a business case for the proactive outreach programme.`
- `@member-lifetime-value-calculator Recalculate the model with a discount rate of 10% (CFO's preferred hurdle rate) and show the impact on total portfolio MLV vs the 8% base case.`

## Optional collaboration with related skills
- Use `@data-validator` to validate the member extract before building the model — data quality directly affects MLV accuracy;
- Use `@qoq-trend-analyser` to compare portfolio MLV quarter on quarter and surface deterioration in the Churn Risk Register;
- Use `@anomaly-detector` to identify members whose claims pattern has changed significantly, as a leading indicator for churn risk.

## Sources and references
**Australian regulatory framework:**
- Private Health Insurance Act 2007 (Cth) — Part 2-2: Community rating obligations; prohibition on discriminatory pricing and service levels.
- Private Health Insurance (Lifetime Health Cover) Rules 2007: LHC loading rules and their effect on member retention economics.
- Private Health Insurance Rebate — Department of Health and Aged Care: Government rebate rates by income tier and age group.

**Actuarial and financial frameworks:**
- Institute of Actuaries of Australia (IAAust): Actuarial practice standards for health insurance modelling (PS 401 and PS 500 series).
- Discounted Cashflow (DCF) methodology: Standard NPV formula per AASB 13 (Fair Value Measurement) for financial modelling conventions.

**Industry and research:**
- APRA Quarterly Private Health Insurance Statistics: Industry-level lapse rates, member growth, and premium trends (benchmark for model validation).
- AIHW Australian Institute of Health and Welfare — Private Health Insurance: Annual datasets on membership levels, benefit payments, and policy mix.
- Medibank Private Limited Investor Presentations (2024–2025): Publicly available data on lapse rates, Net Promoter Score, and retention programme outcomes.
