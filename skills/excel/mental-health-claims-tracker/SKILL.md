---
name: mental-health-claims-tracker
description: Track mental health benefit utilisation against policy limits and flag members approaching session caps. Use when asked to track mental health claims, review psychology benefit usage, or identify members near their limit.
---

# Mental Health Claims Tracker

## Purpose
Track Medibank member mental health benefit utilisation against annual policy limits, identify members approaching session caps, and enable proactive care coordination before members run out of cover.

## Before you begin
- Confirm the sheet contains: Member ID, Policy Type, Service Type, Sessions Used, Annual Session Limit, Benefit Paid
- Confirm the alert threshold for approaching limit (default 80 percent of sessions used)

## Workflow

### Step 1 - Calculate utilisation rate
For each member: Sessions Remaining as Annual Session Limit minus Sessions Used, Utilisation Rate as Sessions Used divided by Annual Session Limit times 100.

### Step 2 - Flag members by status
Limit Exceeded: Sessions Used exceeds Annual Session Limit. Approaching Limit: Utilisation Rate above 80 percent. Active User: 40 to 80 percent. Low User: below 40 percent.

### Step 3 - Apply colour coding
Limit Exceeded: red fill. Approaching Limit: orange fill. Active User: yellow fill. Low User: green fill.

### Step 4 - Benefit spend analysis
Calculate total benefit paid by service type: Psychology, Psychiatry, Counselling. Show average cost per session and average sessions per member.

### Step 5 - Policy type analysis
Group by Policy Type. Show average sessions used, average utilisation rate, and percentage approaching or exceeding limit.

### Step 6 - Tracker dashboard
Create new sheet Mental Health Tracker with: utilisation summary by status, members approaching or exceeding limit sorted by sessions remaining ascending, benefit spend breakdown, and policy type analysis.

## Output
- New Mental Health Tracker sheet
- Limit exceeded members highlighted in red
- Approaching limit members in orange
- Outreach priority list sorted by urgency

## Common pitfalls
- Session limits vary by policy type — do not apply a single limit to all members
- Members who exceeded their limit may have approved extensions — verify before flagging
- Never include clinical notes or diagnosis details in this analysis