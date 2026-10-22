---
name: workforce-productivity-tracker
description: Use when the user asks Copilot in Excel to build, rebuild, complete, review, or update a Microsoft 365 Copilot productivity tracker, AI adoption dashboard, Copilot ROI model, workforce time-saving report, AI usage log, or team adoption scorecard. Produces a working, formula-driven, auditable Excel workbook with usage inputs, adoption analysis, time-saving calculations, financial value estimates, break-even analysis, trend tracking, quality checks, and an executive dashboard. Do not use for individual performance reviews, headcount planning, HR disciplinary records, or personal productivity coaching.
---

# Track Microsoft 365 Copilot Adoption and Productivity Gains in Excel

## Purpose
Create a working Excel workbook that measures Microsoft 365 Copilot adoption, quantifies time saved, estimates the financial value of productivity gains, identifies low-adoption teams, exposes uncertainty in self-reported estimates, and builds an evidence-based ROI narrative for leadership. The workbook must separate usage inputs from calculations, apply conservative and auditable assumptions, distinguish capacity gains from cashable savings, and never present inflated vendor-claimed benchmarks as expected outcomes.

## Non-negotiable outcome
Do not stop at advice, a summary table, or a static chart. Build or update the workbook directly using native, editable Excel features.

The workbook is complete only when:
- usage inputs are editable and separated from calculated outputs;
- adoption rate, time-saving estimates, and financial values are formula-driven, not typed;
- all assumptions (hourly rate, working weeks, realisation rate, licence cost, minimum measurement period) are visible and editable in a single Assumptions sheet;
- capacity gains (hours freed) and cashable savings (confirmed headcount, contractor, or overtime reduction) are shown separately and never combined without a documented realisation mechanism;
- a break-even analysis shows the weekly hours saved per user required to justify the licence and implementation cost;
- industry benchmark ranges are visible alongside the organisation's own figures so leadership can assess whether results are plausible;
- trend data exists for the current and at least one prior period so movement over time is visible;
- a data source quality flag distinguishes system-logged data (Microsoft Viva Insights, Admin Centre) from self-reported survey estimates;
- department and use-case breakdowns are present;
- an adoption league table ranks teams with traffic-light status;
- a measurement maturity flag warns when data is from fewer than 90 days of usage (insufficient to annualise);
- formula, logic, range, and reasonableness checks pass;
- the user can update one input cell and see all dependent outputs refresh.

## Operating principles
1. Preserve any existing workbook content, formulas, styles, and sheet structure unless a change is necessary to fix an error or add a required section.
2. Inspect the workbook before building. Reuse reliable data already present.
3. Never invent usage figures, team names, department names, session counts, or time-saving estimates.
4. If information is missing, create clearly labelled input cells with placeholder text `TBC – input required` in amber fill.
5. Self-reported time savings are estimates, not measurements. Label every time-saving output as indicative unless the source is a Microsoft system report (Viva Insights, Admin Centre, or Copilot Analytics). Include a visible data source column in the usage table and apply amber fill to self-reported rows.
6. Enterprise-wide averages from independent research consistently show 30–60 minutes saved per user per week, not the 1.7 hours per week cited in Microsoft-funded early-adopter surveys. Use 30 minutes per week as the conservative default and 60 minutes as the realistic default when the user provides no figures. Label these clearly and do not use vendor claims as defaults.
7. Do not treat hours saved as a cash saving unless the user provides a confirmed realisation mechanism: documented reduction in contractor spend, avoided overtime, vacancy capture, staff redeployment, or confirmed headcount reduction. Label all unrealised hours as capacity only.
8. Calculate FTE capacity equivalent and display it, but do not label it as headcount reduction without a realisation mechanism documented by the user.
9. Apply a break-even calculation: the weekly minutes saved per user required to justify total cost per user (licence plus training plus governance) at the given hourly rate. Display the break-even threshold prominently alongside actual or modelled savings so leadership can see whether the investment is justified. Use the community-validated formula: `(time saved per task in hours) × (fully loaded hourly cost for that role) vs credit cost per task` for task-level ROI checks.
10. Do not annualise time savings from fewer than 90 days of data. Apply a measurement maturity flag for teams with fewer than 90 days on record. Annualised figures for immature datasets must carry a visible warning.
11. Apply an adoption ramp for teams in their first quarter: do not assume steady-state usage. Use a configurable ramp factor defaulting to 60% of steady state.
12. Distinguish three adoption metrics and never conflate them: (a) Microsoft's official "active user" definition counts any user with at least one interaction in 30 days — the Reddit enterprise community widely flags this as misleading and inflating true adoption; (b) Regular user: meets a minimum frequency threshold, defaulting to at least 3 sessions per week — this is closer to genuine adoption; (c) Daily active user: the strictest definition and what many IT managers consider the real measure. The workbook tracks all three and clearly labels which definition drives each metric.
13. Track Shadow IT as a qualitative flag per team: are staff using Claude, ChatGPT, or other AI tools alongside or instead of Copilot? This signals dissatisfaction and should be captured as an input. A high shadow IT flag alongside low Copilot adoption indicates the problem is the tool, not AI resistance in general.
14. Track "Cognitive Surrender" risk: the community documents a pattern where users pass all writing through Copilot without proofreading, generating verbose, error-filled content. This is a hidden cost — time spent reviewing, correcting, and re-prompting reduces the net time saving. The workbook includes a Rework Overhead input (default 15% of gross time saved) to model this. Label it clearly and link it to the Assumptions table.
15. Data governance readiness is a prerequisite for reliable results, not an afterthought. The community consensus is that Copilot "surfaces the oversharing you already had." Include a Data Governance Readiness flag per team (Ready / In Progress / Not Started) in the Usage Data table. Teams flagged Not Ready carry a warning that their results may surface unintended data access risks rather than productivity gains.
16. Track both fixed licence cost and variable consumption cost (Cowork and agent billing). The Reddit community reports consumption billing reaching £1k/day for 20% of users — a cost that no fixed-licence ROI model captures. Include a Consumption Billing input column in the Usage Data table and add it to the total cost calculation.
17. Do not combine different use cases into a single time-saving figure without showing the breakdown. Meeting summaries, email drafting, document creation, and data analysis have different time and value profiles.
18. Include a Scenarios section with three cases: Conservative (30 min/week saved per user at 0% cashable realisation), Realistic (45 min/week at 20% realisation), and Optimistic (60 min/week at 40% realisation). Clearly label the active scenario and its basis.
19. Use Australian conventions: AUD, `DD-MMM-YYYY` dates, 48-week working year, negative values in parentheses, and loaded hourly rate of AUD 65 as the conservative placeholder unless the user provides an actual rate.

## First response and ambiguity handling
Before editing, inspect the workbook and infer as much as possible from existing sheets, tables, headings, and data.

Ask questions only when the answer is essential to prevent a materially wrong model. Otherwise, proceed with explicit assumptions and a visible Inputs Needed section in the workbook.

Resolve these items when available:
- reporting period (start and end date; note if fewer than 90 days);
- list of teams and departments in scope;
- number of Microsoft 365 Copilot licences assigned per team;
- number of active users per team (used in the period);
- total eligible staff per team;
- Microsoft 365 app most used (Teams, Outlook, Word, Excel, PowerPoint, other);
- primary use case per team;
- average sessions per week per active user;
- average minutes saved per session;
- data source: Microsoft Viva Insights / Admin Centre, internal survey, or self-reported estimates;
- when each team was onboarded;
- actual loaded hourly rate or confirmation to use AUD 65 placeholder;
- annual licence cost per user;
- any confirmed cashable realisation mechanisms.

If the user supplies only a short request and a data table, build the full workbook using conservative placeholder assumptions and the three scenario defaults. State every assumption clearly, marked TBC where not confirmed.

## Industry benchmarks (for reference and workbook inclusion)
Include these benchmarks in the workbook's Assumptions sheet as a read-only reference table so analysts can compare their own results against them. Do not use them as inputs to calculations unless the user explicitly chooses to adopt one.

| Source | Reported saving | Sample | Credibility notes |
|--------|----------------|--------|-------------------|
| Microsoft early-adopter survey (2024) | ~1.7 hrs/week | Self-selected enthusiasts | Lowest — vendor-funded, cherry-picked users; do not use as a realistic target |
| Enterprise independent average (2025) | 30–60 min/week | Cross-industry | Moderate — reflects realistic range across mixed adoption maturity |
| UK Cross-Government Trial (Q4 2024) | ~26 min/day (~2.2 hrs/week) | 20,000 licences, 7,115 survey responses, 12 govt organisations | High — largest independent trial to date; self-reported but large-scale |
| Lloyds Banking Group (2025) | ~46 min/day (~3.8 hrs/week) | 1,000 surveyed from 30,000 licences | Moderate — survey only, methodology not published |
| UK Government DWP independent study (2026) | ~19 min/day (~1.6 hrs/week) | Civil servants | High — independent government evaluation |
| pladis pilot (Microsoft case study, 2024) | ~12 hrs/month (~2.8 hrs/week) | 48 of 70 trained completers | Low-moderate — trained completers only, excludes drop-outs |
| Insurance sector strong roles | 1.3–1.8 hrs/week | Role-specific | Moderate — role level, not org-wide |
| Insurance sector customer service | ~40 min/week | Role-specific | Moderate — lower-complexity tasks |
| Break-even threshold (AUD 65/hr, AUD 780/user/year licence) | ~45–60 min/week | Calculated | Confirm with actual licence cost |

**Reddit community-validated adoption benchmarks (r/microsoft_365_copilot, 42K weekly visitors; r/Office365, 142K weekly visitors; r/sysadmin, 834K weekly visitors):**
- **25–30% of licences go unused** where rollout arrived without structured enablement — community-consistent finding across multiple posts
- **80% adoption within 6 months is achievable** with structured enablement: MVP-led training, hands-on workshops, AI champions program, leadership town halls, role-based training, and internal AI community — without these, expect 40–60% at best
- **Microsoft's "active user" definition is misleading** — the M365 Admin portal and Viva Insights count 1 interaction in 30 days as active. The community strongly rejects this: real active adoption means daily or near-daily use
- **Shadow IT is a signal**: users switching to Claude or ChatGPT after trying Copilot indicates the tool is underperforming, not that staff resist AI
- **Governance-first organisations achieve 320% ROI** (4-month payback); organisations with strong governance AND compliance report up to 930% ROI and 85 min/day saved — but these are best-case figures from a whitepaper cited by a Redditor, not independently verified
- **Consumption billing (Cowork) can exceed £1k/day** for only 20% of users — the Reddit community consistently warns: "the licence is fixed, the meter is not, and nobody has budgeted for the meter yet"
- **"Cognitive Surrender" reduces net benefit**: users pass all writing through Copilot without proofreading, generating verbose, error-filled content. Factor in a rework overhead of 10–20% of gross time saved as a cost offset

**Output quality caution (include this as a visible note in the workbook):**
University of Washington enterprise trial found that 38% of Copilot prompts failed to produce usable output, and 57% of users reported faulty results approximately 25% of the time or more. Time saved must be weighed against time spent reviewing, correcting, and re-prompting. The workbook does not currently model correction overhead — if the organisation has data on this, a rework time factor should be added.

**Adoption app split (from UK Cross-Government Trial):**
Active adoption by application is uneven. Teams peaked at 71% adoption. Excel and PowerPoint stayed at 23–24%. This means a single org-wide adoption rate conceals large differences by app. The workbook tracks Primary App per team for this reason.

Instruct the user: if their reported figures significantly exceed the UK Government trial average of 26 min/day, verify whether data is representative or concentrated in power users who completed training programmes.

## Workbook architecture
Use the minimum number of sheets that keeps the model clear. For a new workbook, create sheets in this order.

### 1. Executive Dashboard
Purpose: decision view for leadership, not data entry.

Include:
- model period, scenario in use, currency, hourly rate assumption, and data quality status at the top;
- KPI cards for: Adoption Rate (active ÷ licences), Weekly Hours Saved (ramp-adjusted), Annualised Hours Saved, FTE Capacity Equivalent, Indicative Financial Value, Break-even Minutes/Week;
- break-even status: are actual or modelled savings above or below break-even? Show clearly with green (above) or red (below) indicator;
- adoption status bar: percentage of teams at High / Medium / Low adoption with traffic-light colour coding;
- data quality summary: number of teams on system data vs self-reported;
- measurement maturity warning: number of teams with fewer than 90 days of data;
- top three teams by hours saved and top three by adoption rate;
- top three use cases by annualised hours saved;
- department-level bar chart: adoption rate and hours saved by department;
- scenario comparison table: Conservative, Realistic, Optimistic side-by-side showing hours saved, financial value, and ROI;
- a visible FAIL warning banner if any mandatory check fails.

### 2. Assumptions
Create an Excel table named `tblAssumptions` with these columns:
- Assumption ID
- Category
- Assumption
- Conservative
- Realistic
- Optimistic
- Active Value (formula-driven from scenario selector)
- Unit
- Source
- Source date
- Status (dropdown: Confirmed / Estimate / TBC)
- Notes

Active scenario selector: a validated dropdown cell (Base / Conservative / Realistic / Optimistic) that drives the Active Value column. This feeds all downstream calculations.

Required assumptions (mark Status as shown):

| ID | Assumption | Conservative | Realistic | Optimistic | Unit | Status |
|----|------------|-------------|-----------|------------|------|--------|
| A001 | Productive working weeks per year | 48 | 48 | 48 | weeks | Estimate |
| A002 | Loaded hourly rate (AUD) | 65 | 75 | 85 | AUD/hr | TBC |
| A003 | Capacity realisation rate | 0% | 20% | 40% | % | TBC |
| A004 | First-quarter ramp factor | 50% | 60% | 75% | % | Estimate |
| A005 | Active user definition (Microsoft) | 1 session/30 days | 1 session/30 days | 1 session/30 days | sessions | Note: community flags this as misleading |
| A006 | Regular user threshold (sessions/week) | 3 | 3 | 3 | sessions/week | Estimate |
| A007 | Annual licence cost per user (AUD) | 780 | 780 | 780 | AUD/user/year | TBC |
| A008 | Implementation + training cost per user (AUD) | 230 | 150 | 100 | AUD/user | TBC — Reddit community notes 25–30% of licences go unused without structured enablement; budget accordingly |
| A009 | Low adoption ceiling (Microsoft definition) | 40% | 40% | 40% | % | Community benchmark: 25–30% licences unused without enablement |
| A010 | High adoption floor (Microsoft definition) | 80% | 80% | 80% | % | Community benchmark: 80% achievable in 6 months WITH structured enablement |
| A011 | Default time saved (min/week) — no data supplied | 30 | 45 | 60 | min/week | Industry benchmark — use org data when available |
| A012 | Minimum measurement period for annualising | 90 | 90 | 90 | days | Estimate |
| A013 | Rework overhead (% of gross time saved) | 20% | 15% | 10% | % | Estimate — accounts for reviewing, correcting, re-prompting Copilot output ("Cognitive Surrender" offset) |
| A014 | Model reporting period start | TBC | | | Date | TBC |
| A015 | Model reporting period end | TBC | | | Date | TBC |

Include the industry benchmark reference table (from Operating Principle 6) as a read-only section below tblAssumptions, clearly labelled as external reference only.

### 3. Usage Data
Create an Excel table named `tblUsage` with these columns:
- Team ID (stable ID, e.g. T001)
- Team Name
- Department
- Role Category (Knowledge Worker / Customer-Facing / Technical / Executive / Other)
- Onboarded Date
- Days Since Onboarding (formula: Model Period End − Onboarded Date)
- Measurement Maturity Flag (formula: IF(Days Since Onboarding < A012, "Insufficient – < 90 days", "OK"))
- First Quarter Flag (formula: IF(Days Since Onboarding <= 90, "Yes", "No"))
- Licences Assigned
- Active Users — Microsoft Definition (1+ interaction in 30 days, as reported by Viva Insights / Admin Centre)
- Active Users — Regular Definition (meeting frequency threshold; input or derived)
- Daily Active Users (input where available — most reliable metric per community consensus)
- Eligible Staff
- Primary App (dropdown: Teams / Outlook / Word / Excel / PowerPoint / Other)
- Primary Use Case (dropdown: Meeting Summaries / Email Drafting / Document Creation / Data Analysis / Coding Assistance / Other)
- Data Source (dropdown: Viva Insights / Admin Centre / Internal Survey / Self-Reported)
- Weekly Sessions per Active User (input)
- Avg Minutes Saved per Session (gross, before rework)
- Rework Overhead % (input; default 15% per Assumption A015 — time spent reviewing, correcting, and re-prompting Copilot output)
- Net Minutes Saved per Session (formula: Avg Minutes Saved × (1 − Rework Overhead %))
- Monthly Consumption Billing AUD (input; Cowork / agent variable billing — leave 0 if not using consumption features)
- Shadow IT Flag (dropdown: None / Some users on Claude or ChatGPT / Majority on alternative tools)
- Data Governance Readiness (dropdown: Ready / In Progress / Not Started)
- Enablement Status (dropdown: Full programme / Partial / Self-directed only / None)
- Notes

Requirements:
- all numeric input columns use data validation to block text;
- Data Source dropdown drives an amber fill rule on the row (Self-Reported = amber, Viva Insights / Admin Centre = white);
- Measurement Maturity Flag drives a light red fill when "Insufficient";
- table must accommodate at least 100 rows cleanly.

### 4. Adoption Analysis
Calculate for each team and summarise at department and organisation level.

Per-team calculations:
```
Adoption Rate (Active) = Active Users / Licences Assigned
Adoption Rate (Workforce) = Active Users / Eligible Staff
Regular User Rate = Regular Users / Licences Assigned
Licence Utilisation = Active Users / Licences Assigned   [same as Adoption Rate Active]
Adoption Status = IF(Adoption Rate >= A010, "High", IF(Adoption Rate >= A009, "Medium", "Low"))
```

Include:
- team-level adoption table with traffic-light conditional formatting referencing A009 and A010;
- department-level summary: average adoption rate, total active users, total licences assigned;
- adoption league table ranked by Adoption Rate descending;
- a clear flag for teams with fewer than 90 days and a note that their figures cannot be reliably annualised.

### 5. Productivity Analysis
Per-team calculations (all referencing tblAssumptions by Assumption ID):
```
[A013] = INDEX(tblAssumptions[Active Value], MATCH("A013", tblAssumptions[Assumption ID], 0))
         [Rework Overhead %; default 15% in Realistic scenario]

Net Minutes Saved per Session = Avg Minutes Saved per Session × (1 − [A013])
         [This is the formula-driven column in tblUsage; all downstream calculations use NET, not gross]

Weekly Minutes Saved (Net) = Active Users × Weekly Sessions per Active User × Net Minutes Saved per Session

Weekly Hours Saved (Net) = Weekly Minutes Saved (Net) / 60

Ramp Factor = IF(First Quarter Flag = "Yes",
              INDEX(tblAssumptions[Active Value], MATCH("A004", tblAssumptions[Assumption ID], 0)),
              1)

Ramp-Adjusted Weekly Hours Saved = Weekly Hours Saved (Net) × Ramp Factor

Annualised Hours Saved = IF(Measurement Maturity Flag = "OK",
                         Ramp-Adjusted Weekly Hours Saved × [A001 Active Value],
                         Ramp-Adjusted Weekly Hours Saved × [A001 Active Value])
                         [Flag output with "UNCONFIRMED – insufficient measurement period" note when maturity flag = Insufficient]

FTE Capacity Equivalent = Annualised Hours Saved / ([A001 Active Value] × 37.5)

Indicative Financial Value = Annualised Hours Saved × [A002 Active Value]

Cashable Saving = Indicative Financial Value × [A003 Active Value]

Total Variable Cost = Monthly Consumption Billing AUD × 12   [per team, from tblUsage]

Net Value = Cashable Saving − (Licences Assigned × ([A007 Active Value] + [A008 Active Value])) − Total Variable Cost
```

Show in separate columns:
- Gross Time Saved (before rework; labelled "Gross – indicative only");
- Net Time Saved (after rework overhead; this is the figure used in all financial calculations);
- Gross Capacity Value (labelled "before rework");
- Net Capacity Value (used in ROI narrative);
- Cashable Saving (only if A003 > 0 and realisation mechanism is documented);
- Total Fixed Cost (licence + implementation per team);
- Total Variable Cost (consumption billing per team);
- Total Cost (fixed + variable);
- Net Value.

Show in separate columns:
- Gross Capacity Value (indicative, label as such);
- Cashable Saving (only if A003 > 0 and realisation mechanism is documented);
- Total Cost (licence + implementation per team);
- Net Value.

### 6. Break-Even Analysis
Calculate at organisation level and per team.

```
Total Cost per Active User per Year = [A007 Active Value] + [A008 Active Value]

Break-Even Minutes per Week = (Total Cost per Active User per Year / [A002 Active Value]) / [A001 Active Value] × 60

Break-Even Status per Team = IF(Actual or Modelled Min/Week >= Break-Even Minutes, "Above break-even", "Below break-even")
```

Include a one-way sensitivity table showing break-even minutes at hourly rates from AUD 50 to AUD 100 (in AUD 10 steps) using an Excel Data Table. This updates automatically when assumptions change.

Show a summary on the Executive Dashboard: a traffic-light indicator per team — green if above break-even, red if below, amber if data is self-reported (uncertain).

### 7. Use Case Breakdown
Group by Primary Use Case. For each calculate:
- number of teams using it;
- total active users;
- average minutes saved per session;
- average weekly sessions per active user;
- total annualised hours saved;
- indicative financial value;
- most common Primary App.

Sort descending by total annualised hours saved.

Include use case profile classification:
- High Frequency / Low Minutes (e.g. Email Drafting, Meeting Summaries) — value comes from volume
- Low Frequency / High Minutes (e.g. Document Creation, Data Analysis) — value comes from depth

Note in the sheet: these profiles require different success strategies and should not be compared on minutes saved per session alone.

### 8. Scenarios
Build a three-column comparison table sourced from the Productivity Analysis using the three scenario definitions in tblAssumptions.

For each scenario show at organisation level:
- total active users;
- adoption rate;
- weekly hours saved;
- annualised hours saved;
- FTE capacity equivalent;
- indicative financial value;
- cashable saving;
- total licence and implementation cost;
- net value;
- ROI = (Cashable Saving − Total Cost) / Total Cost [show as N/A if A003 = 0%;
- payback period in months.

Note clearly: Conservative and Realistic scenarios assume 0% and 20% cashable realisation respectively. The organisation must document a specific realisation mechanism before reporting any cashable saving.

### 9. Trend (prior-period comparison)
If the user provides data for more than one period, calculate period-on-period change:
```
Adoption Rate Change = Current − Prior
Active Users Change = Current − Prior
Hours Saved Change = Current Annualised − Prior Annualised
New Teams Onboarded = COUNT of teams with Onboarded Date in current period
```

Visualise with a line chart for adoption rate trend across periods and a bar chart for hours saved by period.

If only one period exists, create the Trend sheet with placeholder columns labelled `Prior Period – enter data here`.

### 10. Checks
Create a visible control sheet with PASS / WARN / FAIL and colour coding. Show a summary banner on the Executive Dashboard if any check is FAIL.

Mandatory checks:
- Active Users ≤ Licences Assigned for every team row (FAIL if breached);
- Active Users ≤ Eligible Staff for every team row (FAIL if breached);
- Adoption Rate between 0% and 100% for all rows (FAIL if breached);
- Minutes Saved per Session > 0 for all active teams (FAIL if zero or blank);
- Weekly Sessions per Active User > 0 for all active teams (FAIL if zero or blank);
- All teams have a Department and Primary Use Case assigned (WARN if any missing);
- All teams have a Data Source selected (WARN if any missing);
- Capacity realisation rate between 0% and 100% (FAIL if breached);
- If Cashable Saving is non-zero, a realisation mechanism must be noted in Assumptions (WARN if missing);
- Dashboard KPIs reconcile to Productivity Analysis totals (FAIL if difference > 0.1%);
- No `#REF!`, `#VALUE!`, `#N/A`, `#DIV/0!`, or circular references in model outputs (FAIL if any found);
- Count of teams with fewer than 90 days noted (WARN with count);
- Count of self-reported rows noted (WARN with count and reminder);
- Scenario selector has a value (FAIL if blank);
- All TBC assumptions noted with count for the user to resolve.

### 11. Read Me
Keep this short. Include:
- workbook purpose and scope;
- how to update with a new reporting period;
- how to add a new team row;
- how to change the active scenario;
- colour legend (input, formula, TBC, self-reported, maturity warning);
- definitions: adoption rate, FTE capacity, indicative financial value, cashable saving, break-even;
- data quality and self-report caveats;
- industry benchmark reference (point to Assumptions sheet);
- limitations (self-reported estimates, no direct Viva Insights API link, annualisation assumptions);
- version and change log.

## Calculation standards
Use formulas that reference tblAssumptions by Assumption ID via INDEX/MATCH. Do not hardcode any rate, threshold, cost, or multiplier in a formula.

### Referencing Assumptions
```
[A002 Active Value] = INDEX(tblAssumptions[Active Value], MATCH("A002", tblAssumptions[Assumption ID], 0))
```

Apply this pattern for every assumption reference throughout the workbook. A single change to any assumption in tblAssumptions flows through the entire model.

### Rounding
- Adoption rates: `0.0%` for display; full precision in formulas.
- Hours: `#,##0.0` for display; full precision in formulas.
- FTE: `0.00`.
- Financial values (team level): `$#,##0;($#,##0);-`.
- Financial values (org totals, dashboard): `$0.0"k"` or `$0.0"m"` with label in heading.
- Do not round inputs.

### Monthly discount rate (if monthly periods are used)
```
Monthly Rate = (1 + Annual Discount Rate) ^ (1/12) - 1
```

## Model build workflow

### Step 1: Inspect and map
- Review all existing sheets, tables, named ranges, formulas, charts, and comments.
- Identify existing usage data, any errors, and the user's intended output.
- If an existing tracker is present, preserve and improve it rather than replacing it without reason.

### Step 2: Define scope and period
Record in tblAssumptions:
- reporting period start and end;
- teams in scope;
- active scenario;
- data source quality status.

Note if the reporting period is fewer than 90 days and apply measurement maturity flags accordingly.

### Step 3: Build structured inputs
- Create tblAssumptions with all required rows including industry benchmark reference table.
- Create tblUsage and populate from user-supplied data.
- Apply data validation, colour conventions, derived flags, and data source rules.
- Mark missing cells TBC in amber.

### Step 4: Build calculations
- Populate Adoption Analysis from tblUsage.
- Populate Productivity Analysis with all formulas referencing tblAssumptions by Assumption ID.
- Build Break-Even Analysis including the sensitivity table.
- Build Use Case Breakdown.
- Build Scenarios comparison.
- Create Trend sheet even if prior data is absent.

### Step 5: Build checks sheet
- Write all mandatory checks with PASS / WARN / FAIL outputs.
- Link a summary warning banner to the Executive Dashboard.

### Step 6: Create the executive view
- Pull all KPIs from calculated cells, never manually entered.
- Create charts linked to analysis tables.
- Display scenario selector, period, currency, hourly rate, and data quality status prominently.
- Ensure break-even status is visible alongside headline financial value.

### Step 7: Verify
- Change one material input (e.g. hourly rate) and confirm all dependent outputs update.
- Reconcile dashboard KPIs to Productivity Analysis totals.
- Confirm all checks show PASS or explain any WARN.
- Do not present the workbook as finished if any mandatory check is FAIL.

### Step 8: Handover
At completion, tell the user:
- which sheets were created or changed;
- the active scenario and all key assumptions;
- headline results (adoption rate, hours saved, indicative financial value, break-even status);
- which inputs are still TBC;
- data quality status (proportion of data from system sources vs self-reported);
- measurement maturity status (any teams with fewer than 90 days);
- suggested next steps: connect to Microsoft Viva Insights or Admin Centre for system-logged data; confirm realisation mechanism if any cashable saving is to be reported; measure for at least 90 days before presenting annualised figures.

## Formatting standard
Produce a clean, restrained workbook presentable to a Medibank executive.

### Visual hierarchy
- One dark primary colour (deep navy or dark teal), one accent, neutral greys for structure.
- Red, amber, green only for adoption traffic lights, break-even status, check results, and data source quality.
- Aptos or existing workbook font at 10–11pt for data, 14–16pt for sheet titles.
- Freeze top rows on input sheets.
- Gridlines off on Executive Dashboard and Read Me; on for input and calculation sheets.

### Cell conventions
- Input cells: light blue fill.
- Formula cells: white or very light grey.
- TBC / missing inputs: amber.
- Self-reported data rows: very light amber.
- Measurement maturity warning rows: very light red.
- Failed checks: light red.
- Industry benchmark reference table: light grey (read-only indicator).

### Number formats
- Adoption rate: `0.0%`.
- Hours: `#,##0.0` with "hrs" in header.
- FTE: `0.00`.
- Financial (team level): `$#,##0;($#,##0);-`.
- Financial (dashboard totals): `$0.0"k"` or `$0.0"m"`.
- Dates: `DD-MMM-YYYY`.

### Charts
- All chart series link to named table columns, not manually typed values.
- Chart titles state the measure and period (e.g. "Copilot Adoption Rate by Department – Q2 FY27").
- Avoid 3D charts, pie charts with more than five categories, and decorative fills.
- Adoption chart: horizontal bar, sorted descending, traffic-light series colours matching A009 and A010 thresholds.
- Hours saved chart: clustered bar by department, with Capacity and Cashable Saving as distinct stacked series.
- Break-even sensitivity chart: line chart with break-even minutes at each hourly rate from AUD 50 to AUD 100.
- Charts update automatically when data changes.

## Quality gates
Do not finish until all applicable gates pass.

### Structural gate
- Every sheet has a clear single purpose. No blank or placeholder sheets remain.
- Inputs, calculations, outputs, and checks are separated.
- Table and named-range names are unique and meaningful.

### Formula gate
- All adoption rates, hours, FTE, financial values, and break-even figures are formula-driven.
- No number other than zero and one appears as a literal in a formula.
- Formula patterns are consistent across all rows.

### Data integrity gate
- Active users ≤ licences assigned for every row.
- Active users ≤ eligible staff for every row.
- Adoption rate between 0 and 1 for every row.
- Minutes saved per session is positive for all active rows.
- Every row has a Data Source value.

### Communication gate
- Dashboard answers: How many people are using Copilot? How much time is being saved? Is it above break-even? What does it cost? What is it worth? What don't we know yet?
- Capacity and cashable savings are visibly separated.
- Self-reported data is labelled as such.
- Measurement maturity warnings are visible without opening a separate sheet.
- Industry benchmarks are available for comparison but are not used as default inputs.
- The workbook does not present indicative value as a confirmed saving without a realisation mechanism.

### Usability gate
- Editable cells are distinguishable by colour at a glance.
- The Read Me explains how to add a team, change the scenario, and update to a new period without editing any formula.
- No column requires horizontal scrolling to read its header on a standard 1920×1080 display.
- All charts update automatically on data entry.

## Existing-workbook behaviour
When updating an existing productivity tracker:
1. Preserve all team data, department names, onboarded dates, and prior-period figures.
2. Match the existing colour scheme and font unless asked to restyle.
3. Add sheets or columns only when required.
4. Do not overwrite user-entered usage figures.
5. If an existing calculation conflicts with these standards, retain it in a clearly labelled "Prior Calculation" column and add a corrected column beside it. Do not silently replace a formula the user may rely on.
6. Document material changes in the Read Me change log.

## Failure modes to prevent
Never:
- present hours saved as cash savings without a confirmed realisation mechanism;
- use the 1.7 hours/week vendor-survey figure as a default or expected outcome — this is from self-selected early adopters;
- hardcode the hourly rate, working weeks, ramp factor, or realisation rate in a formula rather than referencing tblAssumptions;
- double count time savings across overlapping use cases for the same user population;
- calculate adoption as active users divided by eligible staff and call it the standard enterprise metric — the standard is active users divided by licences assigned;
- annualise savings without a measurement maturity check when data covers fewer than 90 days;
- show FTE capacity as headcount reduction without a documented realisation mechanism;
- type KPI figures directly into dashboard cells rather than pulling from calculation sheets;
- omit the break-even analysis — without it, leadership cannot assess whether the investment is justified;
- set realisation rate to non-zero by default without explicit user confirmation;
- present self-reported figures without a visible data source label;
- apply the first-quarter ramp without flagging that figures are not yet representative;
- present the workbook as finished if any mandatory check is FAIL;
- omit the rework overhead from financial calculations and call gross time saved the net benefit — this overstates value by 10–20% and ignores the Cognitive Surrender pattern documented by the enterprise community;
- report Microsoft's "1 interaction in 30 days" figure as the headline adoption metric without caveating it — use Regular User Rate (3+ sessions/week) as the primary operational indicator and note the Microsoft definition in parentheses as context only;
- omit consumption billing (Cowork / agent variable billing) from the total cost calculation when the organisation uses these features — the licence is fixed but the meter is not, and community experience shows it can exceed £1k/day for 20% of users without budget controls.

## Recommended prompts for using this skill
- `@workforce-productivity-tracker Build a Copilot adoption and productivity tracker from this usage table. Flag all self-reported data and show the break-even threshold alongside the headline financial value.`
- `@workforce-productivity-tracker Update the tracker with this quarter's figures and show what has changed since last quarter. Flag any teams still below 90 days of measurement.`
- `@workforce-productivity-tracker Review this workbook for hours being called cash savings without a realisation mechanism, self-reported data that isn't labelled, and any hardcoded assumptions. Fix the issues.`
- `@workforce-productivity-tracker Add a scenario comparison showing Conservative (30 min/week saved, 0% realisation), Realistic (45 min/week, 20% realisation), and Optimistic (60 min/week, 40% realisation), and show which scenario is above the break-even threshold.`

## Optional collaboration with related skills
When available and useful:
- use `@anomaly-detector` before building if the usage data contains suspicious outliers — for example, reported minutes per session that significantly exceed industry benchmarks;
- use `@data-validator` to confirm the input table is complete and consistent before calculating any metrics;
- use `@qoq-trend-analyser` to produce a formal quarter-on-quarter comparison once two full periods of data exist.

These optional skills complement but do not replace the quality gates and data integrity checks in this skill.

## Sources and references
The industry benchmarks and measurement recommendations in this skill draw on the following authoritative sources, listed from highest to lowest credibility for enterprise use:

**Government and independent studies (highest credibility):**
- UK Cross-Government Microsoft 365 Copilot Experiment Findings Report (2024–25): 20,000 licences across 12 organisations; 26 min/day saved; 80% adoption rate; Teams 71% adoption, Excel 23%. gov.uk official publication.
- UK Government DWP independent evaluation of Microsoft 365 Copilot trial (2026): ~19 min/day saved. Independent government assessment, not Microsoft-funded.

**Enterprise IT risk assessments:**
- University of Washington IT — CoPilot Executive Recommendations and Risks (2024): 57% of prompts passed, 38% failed; 57% of users reported faulty results ~25% of the time; licensing at USD 475/user/year; ROI listed as a medium risk.

**Independent technology journalism:**
- The Register: "Return on investment for Copilot? Microsoft has work to do" (Sep 2025). Reports Microsoft's own executive (Jared Spataro) stated "it is hard to make the ROI argument for it."
- The Register: "Lloyds Banking Group claims Microsoft Copilot saves staff 46 minutes a day" (Oct 2025). Notes caveats: survey-only, methodology not published, output quality concerns.

**Industry analysis:**
- redresscompliance.com (2025): Break-even framework; enterprise-wide averages of 30–60 min/week; true cost per user USD 500–600 year one including training and governance; 30–40% of licences show minimal use after 90 days.
- lemonlearning.com: Three-layer scorecard (enablement activity, process quality, business outcomes); adoption targets of 60% at 30 days and 80% at 90 days.
- State of Microsoft Copilot in the Enterprise (2026): Distinction between logo adoption, paid penetration, and active valuable use.

**Vendor case studies (use with caution — not independent):**
- pladis Microsoft case study (2024): 12 hrs/month for 48 trained completers; excludes drop-outs; self-reported; Microsoft-published.
- Microsoft early-adopter survey (2024): 1.7 hrs/week; self-selected enthusiasts; not representative of typical enterprise outcome.

**Enterprise practitioner communities (qualitative — highest signal for real-world implementation risk):**
- r/microsoft_365_copilot (42K weekly visitors): Practitioner-reported finding that 25–30% of licences go unused without structured enablement; Microsoft's 1-interaction-in-30-days definition is widely flagged as misleading; shadow IT signals (staff switching to Claude or ChatGPT) indicate tool dissatisfaction, not AI resistance. Community-consistent across multiple threads (2024–2025).
- r/Office365 (142K weekly visitors): Adoption benchmarks consistent with above; strong consensus that governance and data hygiene must precede Copilot deployment — "it surfaces the oversharing you already had." 80% adoption achievable in 6 months with structured enablement: MVP training, AI Champions programme, role-based workshops (community-validated).
- r/sysadmin (834K weekly visitors): Cognitive Surrender pattern documented — users pass all writing through Copilot without proofreading, generating verbose, error-filled output. Rework overhead of 10–20% of gross time saved is the community consensus estimate. Consumption billing (Cowork) reported reaching £1k/day for 20% of users by multiple independent posters; no fixed-licence ROI model captures this.

*Note: Reddit community findings are qualitative practitioner consensus, not peer-reviewed research. Use as signal for risk identification and model design, not as numerical inputs. Where community figures are used in assumptions (e.g. rework overhead default 15%), they are marked as Estimate in tblAssumptions and should be replaced with organisation-specific data when available.*
