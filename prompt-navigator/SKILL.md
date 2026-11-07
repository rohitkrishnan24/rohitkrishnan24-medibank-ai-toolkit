---
name: prompt-navigator
description: Use when a Medibank staff member wants to find the best prompt from the approved Prompt Library for their task, optimise a rough prompt into a full CRAFT-structured prompt, or get a ready-to-use personalised version of a library prompt. The user describes what they want to do in plain language and the skill matches it to the closest prompt across all 50 approved prompts covering Communication, Meetings, Planning, Documents, Analysis, Research, and Quality Review categories. Do not use for Excel data analysis tasks - use the dedicated Excel skills instead.
---

# Prompt Navigator - Find and Optimise the Best Prompt from the Medibank Prompt Library

## PURPOSE
This skill helps any Medibank staff member find, match, and optimise the best prompt from the approved Prompt Library for their specific task - without needing to know what CRAFT is or how to write prompts themselves.

The user describes what they want to do in plain language. The skill does three things:
1. Maps their request to the best matching prompt(s) from the library
2. Explains why that prompt fits
3. Outputs an optimised, ready-to-use version personalised for their context

---

## PROMPT LIBRARY REFERENCE
The library contains 50 approved CRAFT prompts across 6 categories:

### COMMUNICATION & EMAIL (Kaavyaa - PL-COMM)
- PL-COMM-01 | Stakeholder update email
- PL-COMM-02 | Rewrite for clarity and tone
- PL-COMM-03 | Professional email from notes
- PL-COMM-04 | Executive email summary
- PL-COMM-05 | Project delay communication
- PL-COMM-06 | Request information or action
- PL-COMM-07 | Audience-adapted message
- PL-COMM-08 | Clarify an ambiguous request

### MEETINGS (Kaavyaa - PL-MEET)
- PL-MEET-01 | Meeting agenda builder
- PL-MEET-02 | Meeting summary and action register
- PL-MEET-03 | Decision log extractor
- PL-MEET-04 | Meeting pre-read
- PL-MEET-05 | Workshop plan
- PL-MEET-06 | Follow-up reminder
- PL-MEET-07 | Stakeholder meeting questions

### PROJECT PLANNING & STATUS (Kaavyaa - PL-PLAN)
- PL-PLAN-01 | Project plan from rough notes
- PL-PLAN-02 | Weekly project status report
- PL-PLAN-03 | RAID register builder
- PL-PLAN-04 | Work breakdown structure
- PL-PLAN-05 | Milestone and dependency map
- PL-PLAN-06 | Project kickoff checklist
- PL-PLAN-07 | Scope statement draft
- PL-PLAN-08 | Stakeholder communication plan
- PL-PLAN-09 | Project health review
- PL-PLAN-10 | Retrospective and improvement actions

### DOCUMENTS & SUMMARISATION (Evita - PL-DOC)
- PL-DOC-01 | Executive brief from a long document
- PL-DOC-02 | Translate technical content for a non-expert audience
- PL-DOC-03 | Extract decisions, actions and accountability
- PL-DOC-04 | Compare documents or versions
- PL-DOC-05 | Synthesise multiple documents into one view
- PL-DOC-06 | Create a first draft from notes or an outline
- PL-DOC-07 | Traceable question answering from a document
- PL-DOC-08 | Convert a document into an FAQ

### ANALYSIS & DECISION SUPPORT (Evita - PL-AN)
- PL-AN-01 | Stress-test a recommendation before sharing
- PL-AN-02 | Compare options against decision criteria
- PL-AN-03 | Identify risks and trade-offs before a decision
- PL-AN-04 | Translate findings into business implications
- PL-AN-05 | Convert findings into a decision brief
- PL-AN-06 | Develop possible explanations and hypotheses
- PL-AN-07 | Build best, expected and worst-case scenarios
- PL-AN-08 | Prioritise requests or initiatives

### RESEARCH (Evita - PL-RES)
- PL-RES-01 | Build a focused research plan
- PL-RES-02 | Synthesise findings from supplied sources
- PL-RES-03 | Compare evidence across sources
- PL-RES-04 | Create a source-based competitor or market snapshot
- PL-RES-05 | Assess evidence strength before a decision

### QUALITY & REVIEW (Evita - PL-QR)
- PL-QR-01 | Pre-send document quality review
- PL-QR-02 | Check a document against requirements
- PL-QR-03 | Check claims and numbers against evidence
- PL-QR-04 | Cross-document consistency check

---

## WORKFLOW

### Step 1 - Understand the user's request
Read what the user typed. Do not ask clarifying questions unless the request is completely ambiguous (fewer than 5 words with no context). Extract:
- What they are trying to produce or achieve
- Who the audience is (if mentioned)
- What data or input they are working with (if mentioned)
- What app or tool they are working in (if mentioned)

### Step 2 - Match to the library
Using the category and prompt list above, identify the 1-3 best matching prompts. Score the match mentally:
- STRONG match = the prompt title directly describes the task
- PARTIAL match = the prompt covers the same category but needs adaptation
- WEAK match = only the category aligns; flag this honestly

Always prefer a STRONG match. If two prompts are equally strong, list both.

### Step 3 - Optimise for the user's context
Take the best matching prompt and personalise it by:
- Replacing placeholders like [AUDIENCE], [PURPOSE], [INSERT HERE] with what the user told you
- Adjusting the tone section if the user mentioned their audience (e.g. "my manager", "the board", "the claims team")
- Adding any domain specifics they mentioned (e.g. "claims data", "provider payments", "Q3 report")
- Keeping all CRAFT components intact - do not remove Context, Role, Action, Format or Tone

### Step 4 - Output the result
Produce the following structured output:

---

**PROMPT NAVIGATOR - RESULTS**

**Your request:** [restate what the user asked in one sentence]

**Best match from library:**
> [Prompt ID] - [Prompt Title]
> Category: [Category]
> Why this fits: [1-2 sentences]

**Additional match (if applicable):**
> [Prompt ID] - [Prompt Title]
> Why this also applies: [1 sentence]

**Optimised prompt - ready to use:**
[Full CRAFT prompt with placeholders filled in using the user's context. Keep all 5 CRAFT components. Mark any placeholder you could not fill as [FILL IN: what is needed].]

**How to use this:**
- Copy the optimised prompt above
- Paste it into [recommended Copilot app] chat
- Review the output before sharing or acting on it - human review is required

**Data reminder:**
Use only approved, de-identified or synthetic data. Do not paste live member, claims, health or financial records into Copilot.

---

## STOP CONDITIONS
Stop and tell the user if:
- Their request involves real patient names, real member IDs, or identifiable personal data - do not proceed; ask them to use synthetic or de-identified data first
- Their request is completely outside all 6 categories - tell them no matching prompt exists and suggest the closest category
- They ask you to skip the human review requirement - remind them it is mandatory for all outputs

## GUARDRAILS
- Never invent a prompt ID that does not exist in the library above
- Never skip the Data reminder in the output
- Never remove the CRAFT structure from the optimised prompt
- Always include "Human review is required" in the output
- If the user asks which prompts exist, list all 50 by ID and title - do not summarise or abbreviate the library

---

## EXAMPLE

**User input:** "I need to write an email to my manager about a project being delayed"

**Output:**

**PROMPT NAVIGATOR - RESULTS**

**Your request:** Write an email to your manager explaining a project delay.

**Best match from library:**
> PL-COMM-05 - Project delay communication
> Category: Communication & email
> Why this fits: This prompt is specifically designed to draft an update explaining a project delay and the proposed response.

**Optimised prompt - ready to use:**

Context: You are supporting a communication task at Medibank. The audience is my direct manager. The purpose is to inform them of a project delay and outline the proposed response. Use only approved, de-identified or synthetic information.

Role: Act as a professional business writer drafting a clear, honest internal communication.

Action:
(a) Acknowledge the delay directly and state the new expected timeline if known.
(b) Explain the reason for the delay without assigning blame.
(c) Outline what has been done and what will be done to get back on track.
(d) State what decision or support is needed from the manager.

Format: Email with subject line. Body: delay summary (2 sentences), reason (2 sentences), mitigation plan (3 bullets), decision needed (1 sentence), close.

Tone: Professional, honest and solution-focused. Do not be defensive.

Task: Draft the email. [FILL IN: project name and original deadline] [FILL IN: reason for delay] [FILL IN: new expected completion date]

**How to use this:**
- Copy the optimised prompt above
- Fill in the three [FILL IN] fields with your project details
- Paste into Outlook Copilot or Copilot Chat
- Review the output before sending - human review is required

**Data reminder:**
Use only approved, de-identified or synthetic data. Do not paste live member, claims, health or financial records into Copilot.
