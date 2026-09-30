---
name: provider-payment-reconciler
description: Reconcile provider billing against approved payments and flag discrepancies. Use when asked to reconcile provider payments, check billing accuracy, or identify payment discrepancies.
---

# Provider Payment Reconciler

## Purpose
Reconcile what Medibank providers billed against what was approved and paid, identify discrepancies, and flag potential overbilling, underpayment, or duplicate payment issues.

## Before you begin
- Confirm the sheet contains: Invoice ID, Provider Name, Service Date, Billed Amount, Approved Amount, Paid Amount, Status
- Confirm the reconciliation period

## Workflow

### Step 1 - Calculate variances
For each record: Billing Variance as Billed minus Approved, Payment Variance as Approved minus Paid, Total Variance as Billed minus Paid.

### Step 2 - Flag discrepancy types
Overbilling: Billed exceeds Approved by more than 10 percent. Underpayment: Paid is less than Approved. Overpayment: Paid exceeds Approved. Matched: all three within 1 percent.

### Step 3 - Check for duplicates
Flag records where Provider Name, Service Date, and Billed Amount are identical as Potential Duplicate Payment. Highlight in red.

### Step 4 - Calculate financial impact
Sum total overbilling, underpayment, overpayment, and duplicate payment exposure.

### Step 5 - Provider level summary
Group by Provider Name. Calculate total billed, approved, paid, net variance, and discrepancy rate. Flag providers with discrepancy rate above 5 percent as High Discrepancy Providers.

### Step 6 - Reconciliation report
Create new sheet Payment Reconciliation with: financial impact summary, discrepancy type breakdown, duplicate payment flags, and high discrepancy providers table.

## Output
- New Payment Reconciliation sheet
- Duplicate payments highlighted in red
- Overbilling flagged in orange
- High discrepancy providers identified
- Total financial impact calculated

## Common pitfalls
- A 1 percent tolerance is applied to account for rounding differences
- Duplicate flags require manual review before any recovery action
- Underpayments may be intentional partial payments pending documentation