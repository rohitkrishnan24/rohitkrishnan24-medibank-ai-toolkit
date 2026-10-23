---
name: workforce-productivity-tracker
description: >
  Triggered when a user asks to track, measure, or report on Microsoft 365
  Copilot adoption, productivity gains, time savings, ROI, or credit consumption
  across Medibank. Also triggered for Copilot usage dashboards, licence
  utilisation reviews, or workforce AI performance reporting.
---

## Purpose
Build a rigorous, evidence-based Microsoft 365 Copilot productivity tracking
workbook for Medibank — one that measures real adoption and genuine time savings,
not inflated vendor metrics. The workbook must expose cognitive-surrender risk,
track credit consumption against labour-cost savings, and give People & Culture
and Finance a defensible ROI figure they can present to the ARMC.

## Non-negotiable outcome
The skill is complete only when all of the following are true:
- [ ] Adoption_Dashboard distinguishes genuine daily-active users from
      Microsoft's misleading "1 use in 30 days" active-user definition.
- [ ] ROI_Model calculates credit cost per task versus fully-loaded hourly labour
      cost saved, at both task level and portfolio level.
- [ ] Cognitive_Surrender_Monitor flags teams where AI-assisted output length,
      error rate, or proofreading bypass has increased since rollout.
- [ ] Credit_Burn_Tracker monitors Cowork consumption-based billing weekly and
      alerts when a team's daily spend exceeds a configurable threshold.
- [ ] All thresholds reference tblConfig via INDEX/MATCH — no hardcoded values.
- [ ] A printable Executive_Summary shows adoption rate, ROI, and cognitive
      surrender risk status on one page.
- [ ] Community-rating equivalent: no metric is presented that would mislead
      leadership into believing adoption or ROI is higher than it is.

## Operating principles
1. **Never use Microsoft's default active-user definition.** M365 Admin portal
   and Viva Insights count 1 use in 30 days as an "active user." This is
   misleading. Medibank's definition: a genuine active user interacts with
   Copilot on at least 10 distinct working days per month. Surface both
   figures side by side so leadership can see the gap.
2. **Track credit burn separately from licence cost.** The base Copilot licence
   ($30/user/month) is sunk cost. The real variable is credit consumption for
   Cowork and agent-based features — this is the meter that nobody has budgeted
   for. Report it weekly per team, not monthly in arrears.
3. **Cognitive Surrender is a real risk — measure it.** Reddit enterprise
   practitioners consistently report that rollouts without behaviour-change
   programs produce teams that stop critical thinking, generate lengthy unread
   AI output, and skip proofreading. Track proxy indicators: output length
   per task, error-report rate post-AI-use, and bypass rate on review steps.
4. **ROI formula: (time saved per task × fully-loaded hourly rate) vs credit
   cost per task.** This is the community-validated formula. Self-reported
   broad time savings are unreliable — measure specific, repetitive tasks
   where manual vs Copilot time can be compared directly.
5. **Data governance readiness gates ROI.** Organisations with clean SharePoint
   permissions, DLP policies active (not simulation mode), and sensitivity
   labelling in place report 3–9× higher ROI than those without. Include a
   Governance_Readiness checklist as a prerequisite tab.
6. **Pilot before fleet rollout.** Set per-user credit caps, run a pilot group
   of 20–50 users for 4 weeks, read consumption reports weekly, and use that
   data to calibrate fleet-wide projections. Do not extrapolate from vendor
   demos — demos run on clean staged data, pilots run on real tenant data.
7. **Shadow IT is a signal, not a problem to suppress.** If users are switching
   to Claude or ChatGPT, that is a signal that Copilot is not meeting their
   needs for those tasks. Surface shadow-IT rate as a metric, not a compliance
   issue.
8. **Adoption benchmarks from enterprise community:** 25–30% of licences go
   unused without structured enablement. With comprehensive training (MVP
   workshops, role-based labs, internal champions, leadership show-and-share),
   organisations have reached ~80% genuine active use within 6 months. Use
   these as calibration benchmarks, not targets.
9. **The billing anomaly: removing licences does not immediately reduce cost.**
   M365 billing for removed licences lags by up to one full billing cycle.
   Document this in the workbook so Finance is not surprised.
10. **Separate fixed and variable cost in every report.** Fixed: M365 suite +
    Copilot add-on licence. Variable: Cowork credit consumption. One Redditor
    reported £1,000+ per day for 20% of users in a single month. Medibank
    Finance must see these as separate lines, not a blended per-user figure.
11. **Three-tier active user definition:** Tier 1 — Power Users (daily use,
    10+ interactions/day); Tier 2 — Regular Users (10+ days/month); Tier 3 —
    Occasional Users (1–9 days/month). Microsoft's reported figure conflates
    Tier 3 with Tier 1. Report all three tiers.
12. **ROI is only valid for specific task categories.** Reliable ROI evidence
    from the enterprise community: meeting preparation (30–40 min manual →
    ~2 min Copilot, credit cost $4–6); transcript search (30–45 min manual →
    2 min); status report drafting ("no more blank-page Friday afternoons").
    Broad claims like "saves 56 min/day" require governance-first baseline data
    to be credible.
13. **Governance-first organisations see 3× to 9× higher ROI.** One benchmark
    (enterprise whitepaper cited on r/microsoft_365_copilot): average ROI of
    320% with 4-month payback for standard rollout; 930% ROI with 85 min/day
    saved for governance-first rollout. Present these as range, not point
    estimate.
14. **Cognitive Surrender loop: Ground → Draft → Check → Decide.** Train and
    track whether users are completing all four steps. Bad Copilot output
    almost always comes from skipping Check or Decide. Monitor bypass rate
    on the Check step as a leading indicator of cognitive surrender.
15. **For Excel specifically:** Copilot in Excel is most reliable for formula
    generation and data cleaning via natural language, not for calculations
    that belong in an ERP. Flag any attempt to use Copilot for P&L or
    financial close calculations — those must stay in the source system.
16. **Context-rich tenants outperform data-poor tenants.** Teams with
    transcript-enabled meetings, well-maintained SharePoint, and extensive
    email archives consistently report higher Copilot usefulness. Include a
    context-richness score in the Governance_Readiness tab.
17. **Never inflate adoption figures for reporting.** Presenting Microsoft's
    1-use-in-30-days figure as "active user rate" to the ARMC or board is
    misleading. Always present the Medibank-defined active rate alongside the
    vendor-reported figure with a clear label distinguishing them.
18. **Document the "demo vs pilot" gap.** When presenting Copilot results to
    leadership, always note that vendor demos use clean staged data. Actual
    pilot results in Medibank's real tenant are the only credible baseline.

## Workbook architecture

### Sheet list
| Sheet | Table name | Purpose |
|-------|-----------|---------|
| Read_Me | — | Methodology, definitions, governance readiness status, and glossary |
| Governance_Readiness | tblGovReadiness | Checklist: SharePoint permissions, DLP (active not simulation), sensitivity labels, AI governance policy |
| Usage_Raw | tblUsageRaw | Raw Copilot usage export: user, date, feature, interaction count, credit consumption |
| Adoption_Dashboard | tblAdoption | Three-tier active user breakdown; Microsoft definition vs Medibank definition side by side |
| Task_ROI | tblTaskROI | Task-level ROI: task name, manual minutes, Copilot minutes, credit cost, labour cost saved, ROI ratio |
| Credit_Burn_Tracker | tblCreditBurn | Weekly credit consumption by team; daily burn rate; threshold alert column |
| Cognitive_Surrender | tblCSurrender | Output length trend, error-report rate, review bypass rate, overall CS risk score per team |
| Shadow_IT_Log | tblShadowIT | Logged instances of users switching to Claude/ChatGPT; task type; reason if captured |
| Executive_Summary | — | One-page printable: adoption rate (both definitions), ROI, credit burn status, CS risk, governance readiness |
| Config | tblConfig | All thresholds: active-user day minimum, credit alert threshold, CS risk thresholds, hourly rate by band |

### Named table columns

**tblConfig**
AssumptionID | Label | Value | Unit | Source | LastReviewed

**tblUsageRaw**
UserID | Department | Date | Feature | InteractionCount | CreditsConsumed | TaskCategory

**tblAdoption**
Department | LicencedUsers | MSActiveUsers | MedibankActiveUsers | PowerUsers | RegularUsers | OccasionalUsers | UnusedLicences | MSAdoptionRate | MedibankAdoptionRate

**tblTaskROI**
TaskID | TaskCategory | ManualMinutes | CopilotMinutes | TimeSavedMinutes | FullyLoadedHourlyRate | LabourCostSaved | CreditsConsumed | CreditCost | ROIRatio | ROIVerdict

**tblCreditBurn**
Week | Team | FixedLicenceCost | VariableCreditCost | TotalCost | DailyBurnRate | AlertThreshold | AlertStatus

**tblCSurrender**
Team | AvgOutputLength | OutputLengthTrend | ErrorReportRate | ReviewBypassRate | CheckStepBypassRate | CSSScore | CSRisk

## Calculation standards
- Active user rate (Medibank): `=COUNTIF(tblUsageRaw[UserID], [UserID] criteria with COUNTIFS on date range ≥ tblConfig threshold days) / total licensed`
- ROI ratio per task: `= [LabourCostSaved] / [CreditCost]` — flag as "Positive" if > 1.0, "Break-even" if 0.8–1.0, "Negative" if < 0.8
- Daily credit burn rate: `= SUM(tblCreditBurn[VariableCreditCost] for week) / 5`
- CS Score: weighted average of OutputLengthTrend (30%), ErrorReportRate (40%), ReviewBypassRate (30%) — all pulled via INDEX/MATCH from tblConfig weights
- All rate thresholds pulled from tblConfig using `=INDEX(tblConfig[Value], MATCH([AssumptionID], tblConfig[AssumptionID], 0))`
- No hardcoded dollar amounts, day counts, or percentage thresholds anywhere in the workbook

## Model build workflow
1. **Populate tblConfig first** — enter all thresholds: active-user day minimum (default 10), credit alert threshold per team per day, CS risk thresholds, fully-loaded hourly rates by job band, and credit cost per unit.
2. **Import usage data into tblUsageRaw** — paste or connect the M365 Copilot activity export. Ensure date range covers at least one full calendar month.
3. **Complete Governance_Readiness tab** — mark each checklist item PASS / FAIL / IN PROGRESS. The Executive_Summary will show a governance readiness score derived from this tab.
4. **Build tblAdoption** — calculate both Microsoft and Medibank active-user rates. Flag the gap between the two definitions prominently.
5. **Complete tblTaskROI** — for each tracked task category, enter manual minutes from baseline measurement (not self-report), Copilot minutes from pilot data, and credit cost from consumption report.
6. **Build Credit_Burn_Tracker** — enter weekly consumption by team. Set alert thresholds from tblConfig. Highlight teams exceeding daily burn threshold.
7. **Complete Cognitive_Surrender tab** — enter output length, error rate, and review bypass data. Calculate CS Score per team. Flag any team scoring above the High risk threshold.
8. **Build Executive_Summary** — link all summary KPIs from the tabs above. Do not enter any values manually on this sheet — every cell must be a formula linking to source tables.

## Formatting standard

### Visual hierarchy
- Use Medibank teal (#006D6D) as the header fill for all sheet headers. Reserve red, amber, and green exclusively for status indicators.
- Use Aptos or the existing workbook font throughout. 14–16pt for sheet titles, 11–12pt for section headers, 10–11pt for data.
- Freeze panes on all data sheets so column and row headers remain visible when scrolling.
- Keep gridlines off on Adoption_Dashboard and Executive_Summary; leave them on for data entry and tracking sheets.

### Cell conventions
- Input cells requiring user entry: light blue fill (#DBEAFE).
- Formula cells: white or very light grey.
- Linked data pulled from another sheet: light green fill.
- Placeholder or TBC inputs: light amber fill with label `TBC — user input required`.
- Alert or failed-check cells: light red fill.
- Do not encode meaning by colour alone — always include a text label alongside any colour-coded status.

### Number formats
- Currency: `$#,##0.00;($#,##0.00);-` in AUD.
- Large totals: `$0.0"m"` or `$0"k"` with units in the column heading.
- Percentages: `0.0%` for adoption and ROI rates; `0%` for rough indicators.
- Dates: `DD-MMM-YYYY` throughout.
- Counts: whole numbers with thousands separators (`#,##0`).
- Show negative currency in parentheses `($12,345)` on executive-facing sheets.

### Charts
- Link all charts to model tables or named ranges — never manually entered series.
- Use descriptive titles stating the measure and time period.
- Avoid 3D effects, pie charts with more than four segments, and decorative fills.
- Adoption trend chart: line chart showing Microsoft-definition and Medibank-definition adoption rates as two separate series over time — so the gap is always visible.
- Credit burn chart: column chart by team, with a horizontal reference line at the alert threshold.

## Quality gates

### Coverage gate
- tblUsageRaw contains at least one full calendar month of data.
- All licensed users appear in tblAdoption, including those with zero usage.
- Every task in tblTaskROI has a recorded manual baseline — no self-reported estimates without a note.

### Metric integrity gate
- Microsoft active-user rate and Medibank active-user rate are both visible on Adoption_Dashboard with labels clearly distinguishing them.
- ROI ratio is calculated at task level before being aggregated — no blended averages masking negative-ROI tasks.
- Credit burn figures match the official M365 consumption report export — no manual estimates.

### Decision gate
- The Executive_Summary answers four questions without the reader opening any other sheet: What is Medibank's genuine Copilot adoption rate? Is the credit spend generating a positive ROI? Which teams are at cognitive surrender risk? Is the tenant governance-ready?
- A People & Culture or Finance leader can take the Executive_Summary directly into an ARMC presentation without reformatting.
- No metric on any executive-facing sheet uses Microsoft's 1-use-in-30-days active-user definition without a clearly labelled caveat.
- Credit burn alert status is visible without filtering or scrolling.

### Usability gate
- Input cells are visually distinct from formula cells — a new user can locate configurable inputs without a guide.
- The definition difference between Microsoft active-user and Medibank active-user is explained in a visible note on the Adoption_Dashboard, not only in Read_Me.
- Column widths allow all content to be read without horizontal scrolling on a 1920×1080 display.
- A user who did not build the workbook can update the credit alert threshold in tblConfig and confirm all dependent charts and alerts update within 30 seconds.

## Existing-workbook behaviour
- When appending a new month's usage data: add rows to tblUsageRaw only — do not modify existing rows. All dashboard calculations will update automatically.
- When credit rates change: update tblConfig only. Do not edit formula cells.
- When a new task category is tracked: add a row to tblTaskROI with manual and Copilot baselines before the first month of data is entered.
- When a team is added or removed: update tblAdoption and tblCreditBurn to include or exclude that team — do not delete historical rows, mark them as "Inactive" in a status column.
- Never overwrite the Shadow_IT_Log — append only. This log is a longitudinal record of where Copilot is failing to meet user needs.
- Always preserve prior-period figures in Credit_Burn_Tracker — do not replace weekly rows, add new rows for each new week.

## Failure modes
- **Never present Microsoft's 1-use-in-30-days figure as the adoption rate** without labelling it as the vendor definition and showing Medibank's definition alongside it.
- **Never blend fixed licence cost and variable credit consumption into a single per-user figure** — this hides the unpredictable billing risk of Cowork.
- **Never accept self-reported time savings as the only ROI evidence** — require at least one task category with a measured manual baseline.
- **Never mark the Cognitive_Surrender risk as Low** if the review-bypass rate is above the tblConfig threshold, regardless of output length or error rate.
- **Never remove or hide the Shadow_IT_Log** — shadow AI tool use is a legitimate signal about Copilot's gaps, not a compliance problem to suppress.
- **Never extrapolate vendor demo ROI figures to Medibank's tenant** without noting that demo data is staged and real-tenant results vary significantly.
- **Never present governance readiness as complete** if any DLP policy is still in simulation mode rather than active enforcement.

## Recommended prompts
- `@workforce-productivity-tracker Build the full Copilot productivity tracking workbook from this month's usage export. Show me both the Microsoft and Medibank active-user rates and flag the gap.`
- `@workforce-productivity-tracker Calculate ROI for our meeting-preparation and transcript-search task categories using the pilot data I've pasted. Show me credit cost versus labour cost saved per task.`
- `@workforce-productivity-tracker Run the cognitive surrender analysis on this month's data. Which teams have the highest review-bypass rate and what does their CS risk score look like?`
- `@workforce-productivity-tracker Update the Credit_Burn_Tracker with this week's consumption report and flag any teams exceeding the daily threshold.`

## Optional collaboration with related skills
- Use `@data-validator` to validate the M365 usage export before importing into tblUsageRaw — malformed exports cause incorrect adoption calculations.
- Use `@qoq-trend-analyser` to produce a quarter-on-quarter view of adoption and ROI trends for the ARMC quarterly report.
- Use `@regulatory-submission-checker` pattern as a template for the Governance_Readiness checklist tab — consistent structure across compliance tools.

## Sources and references

**Reddit community research (r/microsoft_365_copilot, r/Office365, r/GithubCopilot, r/msp):**
- Active user definition gap: M365 Admin portal counts 1 use in 30 days as active. Enterprise practitioners define genuine active use as daily interaction (10+ days/month).
- Unused licence benchmark: 25–30% of Copilot licences go unused where rollout arrives without structured enablement.
- Adoption with enablement: ~80% genuine active use within 6 months achievable with MVP workshops, role-based labs, internal champions, and leadership show-and-share programs.
- ROI formula validated by community: `(time saved per task in hours) × (fully-loaded hourly cost for that role) > credit cost per task`.
- Governance-first ROI benchmark: 320% average ROI / 4-month payback (standard rollout); 930% ROI / 85 min/day saved (governance-first rollout). Source: enterprise whitepaper cited on r/microsoft_365_copilot.
- Meeting preparation ROI example: 30–40 min manual → ~2 min Copilot, credit cost $4–6 per meeting. Clear positive ROI at $40–50/hr loaded rate.
- Cowork billing risk: one enterprise reported £1,000+ per day for 20% of users in a single month. Variable billing is the primary financial risk, not the fixed licence.
- Licence removal billing lag: removing licences does not immediately reduce the M365 bill — lag of up to one full billing cycle documented by community.
- Cognitive Surrender: consistent finding across enterprise practitioners — rollouts without behaviour-change programs produce teams that stop critical thinking, generate lengthy unread AI output, and skip proofreading. Tracking recommended: output length trend, error-report rate, review-bypass rate.
- Ground → Draft → Check → Decide loop: community-validated prompting framework. Bad Copilot output almost always results from skipping the Check or Decide step.
- Shadow IT signal: users switching to Claude or ChatGPT indicate Copilot is not meeting task needs — reported as a signal for workflow redesign, not a compliance issue.
- Context-rich tenants: SharePoint-maintained, transcript-enabled, email-rich environments report significantly higher Copilot usefulness than data-poor tenants.
- Demo vs pilot gap: demos run on clean staged data; actual pilot programs in real tenants reveal data governance issues and inconsistent performance.

**Microsoft documentation:**
- Microsoft Viva Insights: Copilot usage reporting methodology and active-user definition.
- Microsoft 365 Copilot admin documentation: credit consumption reporting, per-user cap configuration.
- Microsoft Purview: DLP policy enforcement vs simulation mode — data loss prevention for AI-assisted workflows.

**Australian regulatory context:**
- Privacy Act 1988 (Cth): obligations governing employee productivity data and AI-assisted performance monitoring.
- APRA Prudential Standard CPS 234 (Information Security): requirements for AI tool governance and data access controls in financial services.
- Fair Work Act 2009 (Cth): relevant to any workforce productivity monitoring program — data collected must be disclosed to employees and used consistently with enterprise agreements.
