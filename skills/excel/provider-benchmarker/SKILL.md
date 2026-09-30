---
name: provider-benchmarker
description: Benchmark and rank providers against the group average. Use when asked to benchmark a provider, compare provider performance, or create a provider league table.
---

# Provider Benchmarker

## Purpose
Compare Medibank providers against the group average across key performance metrics. Produces a ranked league table and performance commentary.

## Before you begin
- Confirm which column contains provider names
- Confirm which columns contain the performance metrics
- Confirm whether higher values are better or worse

## Workflow

### Step 1 - Calculate group averages
Calculate the average across all providers for each numeric metric column.

### Step 2 - Score each provider
For each provider and metric: calculate difference from average. Assign Above Average, At Average within 5 percent, or Below Average.

### Step 3 - Rank providers
Rank all providers for each metric. Calculate overall rank based on average position.

### Step 4 - Apply formatting
Green for above average. Red for below average. Yellow for within 5 percent of average.

### Step 5 - Create benchmark table
Create new sheet Provider Benchmark with one row per provider, columns showing value, vs average percentage, and rank. Add overall rank column and benchmark average row.

### Step 6 - Write performance note
For the specified provider write 3 sentences: overall ranking and strength, area below average with specific figure, recommended focus area.

## Output
- Conditional formatting on original data
- New Provider Benchmark sheet
- Benchmark average row clearly labelled
- Performance note below the table

## Common pitfalls
- Do not include text columns in the calculation
- Confirm whether higher is better for each metric
- Do not rank on a single metric unless specifically asked