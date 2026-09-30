---
name: broker-performance-tracker
description: Track broker performance by member quality, retention rate, and commission efficiency. Use when asked to analyse broker performance, review distribution channel results, or assess commission spend.
---

# Broker Performance Tracker

## Purpose
Analyse Medibank broker performance data to identify which brokers generate the highest value members, which generate the highest churn, and whether commission spend is delivering proportionate returns.

## Before you begin
- Confirm the sheet contains: Broker Name, Members Referred, Active Members, Lapsed Members, Average Premium, Average Satisfaction Score, Commission Paid
- Confirm the analysis period

## Workflow

### Step 1 - Retention rate by broker
Calculate retention rate per broker as Active Members divided by Total Members Referred times 100. Rank brokers by retention rate descending.

### Step 2 - Revenue per commission dollar
Calculate Revenue Efficiency as total premium generated divided by commission paid. Flag brokers below 5 times as Low Efficiency.

### Step 3 - Member quality score
Calculate Member Quality Score per broker: average member satisfaction score times retention rate divided by 100.

### Step 4 - Commission vs value analysis
For each broker: total commission paid, total premium generated, net value as premium minus commission. Flag brokers where commission exceeds 20 percent of premium as High Cost.

### Step 5 - Broker tier ranking
Assign tier: Platinum top 20 percent, Gold next 30 percent, Silver next 30 percent, Review bottom 20 percent — all ranked by Member Quality Score.

### Step 6 - Broker dashboard
Create new sheet Broker Performance with: broker ranking table sorted by Member Quality Score, tier assignments, revenue efficiency table, and high cost brokers flagged.

## Output
- New Broker Performance sheet
- Broker tier assignments
- High cost brokers highlighted
- Revenue efficiency ranking

## Common pitfalls
- Do not penalise brokers for low volume if their quality score is high
- Commission percentage should be calculated against gross premium not net
- Review tier brokers need improvement plans not automatic termination