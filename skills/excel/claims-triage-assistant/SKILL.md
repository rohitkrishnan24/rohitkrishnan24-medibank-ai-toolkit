---
name: claims-triage-assistant
description: Triage and prioritise incoming claims by urgency, complexity, and processing priority. Use when asked to triage claims, prioritise a claims queue, or categorise claims by complexity.
---

# Claims Triage Assistant

## Purpose
Categorise and prioritise a Medibank claims queue by urgency and complexity so claims officers can process the highest priority claims first and reduce backlog.

## Before you begin
- Confirm the sheet contains: Claim ID, Service Type, Claim Amount, Lodgement Date, Member Age, Policy Type, Status
- Confirm whether to triage all claims or only pending ones

## Workflow

### Step 1 - Filter pending claims
Filter all claims where Status is Pending or Under Review. Count total claims to triage.

### Step 2 - Score urgency
Assign Urgency Score: days since lodgement more than 10 adds 3 points, Service Category is Hospital or Emergency adds 3 points, Member Age above 65 adds 2 points, Claim Amount above 5000 adds 2 points.

### Step 3 - Score complexity
Assign Complexity Score: Claim Amount above 10000 adds 3 points, Service Type is Surgery or Specialist adds 2 points, Policy Type is Hospital plus Extras adds 1 point.

### Step 4 - Calculate priority
Total Priority Score equals Urgency Score plus Complexity Score. Assign: High Priority above 7, Medium Priority 4 to 7, Low Priority below 4.

### Step 5 - Apply colour coding
High Priority: red fill. Medium Priority: orange fill. Low Priority: green fill.

### Step 6 - Triage output
Create new sheet Claims Triage with: priority queue sorted by Total Priority Score descending, summary count by priority level, and claims exceeding 10 days since lodgement flagged as Overdue.

## Output
- New Claims Triage sheet sorted by priority
- Colour coded priority levels
- Overdue claims flagged separately
- Summary count by priority level

## Common pitfalls
- Do not triage claims that are already Paid or Rejected
- Urgency takes precedence over complexity for final ordering
- Flag claims lodged more than 10 days ago regardless of priority score