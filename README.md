# NovaDesk Support AI

## Overview

This project evaluates five AI-powered customer support capabilities for **NovaDesk**, a fictional B2B SaaS support platform, and determines whether each capability is best implemented as a **Skill, Agent, or Neither**.

The classification considers the nature of the task, required reasoning, autonomy, tool access, and the assignment constraint that each implemented component must work independently without an orchestration layer.

Four capabilities were implemented as standalone Claude Code components:
- 1 Skill
- 3 Agents

The fifth capability was intentionally classified as **Neither** because it primarily requires scheduling, sampling, aggregation, and orchestration rather than a standalone Skill or Agent.

## Capability Classification

| # | Capability | Classification | Implementation |
|---|---|---|---|
| 1 | New ticket categorization/routing, with immediate escalation for live outage, legal, or security issues | Agent | `ticket-triage` |
| 2 | Customer-facing outbound responses that follow company tone and current commercial policy | Skill | `customer-response-policy` |
| 3 | At-risk customer briefing from billing, product usage, and support history | Agent | `customer-risk-briefing` |
| 4 | Weekly rolling sample of closed tickets graded against a rubric, with recurring-failure summaries | Neither | Not implemented (see below) |
| 5 | Bug report investigation against the changelog, with an engineering report for unknown issues | Agent | `bug-investigator` |

## Architecture

- **Skill** (`customer-response-policy`): a reusable procedure plus reference documents (policy, tone, company background). It gives Claude a consistent method for one task: writing a policy-compliant reply. It lives in `.claude/skills/`.
- **Agents** (`ticket-triage`, `customer-risk-briefing`, `bug-investigator`): each is a subagent definition in `.claude/agents/` with its own system prompt, a read-only tool set (`Read` only), an explicit list of files it may read, and a fixed output format. Each investigates one input and returns an internal result.

Every component is independently invocable. No component calls, depends on, or hands off to another, and each agent prompt explicitly forbids invoking other agents or skills. There is no orchestrator, coordinator, or end-to-end pipeline, because the assignment does not require one.

## Implemented Components

### 1. Customer Response Policy — Skill

**Problem solved:** Drafts a customer-facing reply that sounds like NovaDesk support and makes no commercial claim the policy does not support.

**Reference files** (read on every use), in `.claude/skills/customer-response-policy/references/`:
- `current-support-policy.md`: authoritative source for refunds, list prices, credits, and policy conflicts (version 2026-10-01)
- `tone-of-voice.md`: professional, conversational, concise, calm; acknowledge, answer, next step, close
- `company-background.md`: company and support-model context

**Key constraints:**
- The current policy outranks the ticket, customer assumptions, general knowledge, and model memory.
- Each commercial point is treated as *covered*, *conditional* (needs verification), or *not covered* (do not guess; offer to route to the relevant team).
- Output is only the customer-facing message, with no policy citations or internal notes.

**Preventing unsupported claims:** The skill must not invent or calculate prices, refunds, discounts, credits, contract terms, or timeframes. It quotes only list prices from the policy, and never as a customer's contracted price. It treats amounts and discounts in the ticket as customer claims to acknowledge, not confirm. It never promises or implies a refund, credit, or discount before the policy allows it, and never claims an action has been taken unless the ticket says so.

**Invoke:**
```
/customer-response-policy <ticket text or path to a ticket file>
```

### 2. Ticket Triage — Agent

**What it does:** Takes one new support ticket and returns Category, Priority, Route, Immediate Escalation (YES/NO), and a concise evidence-based Reason.

**What it reads:** The ticket, plus `tests/ticket-triage/routing-rules.md` (categories: Account & Access, Billing, Product Usage, Service Outage, Security, Legal; default routes; Critical/High/Medium/Low priority definitions) and `tests/ticket-triage/escalation-rules.md`.

**Escalation logic:** Immediate escalation is YES for a widespread outage (multiple users, a whole team, or multiple locations), repeated 5xx errors affecting multiple users, a suspected security incident, or a formal legal threat or issue. When YES, the result names the receiving team and the triggering evidence.

**Boundaries:** Read-only. It does not reply to customers, modify tickets, create incidents, or notify teams. It labels outages as "customer-reported" or "suspected" and never claims official confirmation, notification, an incident, a fix, or a root cause unless the inputs state it.

**Invoke:** In Claude Code, ask for it by name, for example:
```
Use the ticket-triage agent on tests/ticket-triage/test-01-service-outage.md
```

### 3. Customer Risk Briefing — Agent

**Data sources:** Billing data, product usage data, and support history for one customer (`tests/customer-risk-briefing/billing-data.md`, `product-usage-data.md`, `support-history.md`), plus the customer request. All three data files are read every time.

**Signals synthesized:** Payment behavior (overdue status, late payments), usage and engagement trends, and support history including unresolved issues. It looks for patterns across sources and describes them as correlation in time, not proven cause. A single weak signal is not high risk; reinforcing signals raise it.

**Expected output:** Customer, Customer ID, Overall Risk (HIGH/MEDIUM/LOW), Risk Summary, Key Risk Factors (each with cited evidence), and Recommended Next Steps. An "Information Gaps / Conflicts" section appears only when needed.

**Boundaries:** Read-only analysis. It never states or implies that the customer will churn or cancel, never speculates about motives, and never invents figures or contract terms. It does not contact the customer, modify records, create CRM entries, or assign tasks. Recommendations are for the internal team to act on.

**Invoke:**
```
Use the customer-risk-briefing agent on tests/customer-risk-briefing/test-01-acme-analytics.md
```

### 4. Bug Investigator — Agent

**Changelog comparison:** It reads the bug report and the full changelog (`tests/bug-investigator/changelog.md`), then compares the report against every entry, not only those with a similar feature name.

**Known vs. related-but-different:** An entry counts as a known issue only if it materially matches on all three of: affected feature or area, triggering conditions, and symptoms. A feature-name match alone is not enough. For example, the 2026-09-29 Analytics CSV Export entry (large text fields may take longer to generate) differs from a report of the dashboard freezing and reloading when more than 10,000 rows are exported. Related entries are noted in the evidence along with how they differ.

**Output:**
- *No match:* `Known Issue: NO` and an engineering report with Title, Customer, Customer ID, Ticket ID, Reproduction Steps, Expected and Actual Behavior, Impact, Frequency/Reproducibility, and Relevant Evidence/Notes. Unprovided fields say "Not provided".
- *Match:* `Known Issue: YES`, the matching entry, why it matches, and the status exactly as the changelog states it. No engineering report is generated.

**Boundaries:** Read-only. It does not state a root cause, owner, fix, workaround, or severity (unless labeled as its own assessment). It does not file tickets, notify Engineering, contact the customer, or claim a bug was created, assigned, or fixed.

**Invoke:**
```
Use the bug-investigator agent on tests/bug-investigator/test-01-unknown-csv-export-bug.md
```

## Why Capability 4 Was Not Implemented

The weekly rolling sample of closed tickets graded against a rubric was classified as **Neither**. It is mainly scheduled sampling, aggregation across many tickets, and recurring reporting, which is orchestration and scheduling work rather than a single reasoning task suited to one Skill or Agent. The assignment does not require orchestration, so this capability was intentionally left out. Building it would also have meant adding a coordinating layer, which conflicts with the constraint that components stay independent.

## Project Structure

```
.
├── README.md
├── .claude/
│   ├── agents/
│   │   ├── bug-investigator.md
│   │   ├── customer-risk-briefing.md
│   │   └── ticket-triage.md
│   └── skills/
│       └── customer-response-policy/
│           ├── SKILL.md
│           └── references/
│               ├── company-background.md
│               ├── current-support-policy.md
│               └── tone-of-voice.md
└── tests/
    ├── bug-investigator/
    │   ├── changelog.md
    │   ├── evaluation-criteria.md
    │   └── test-01-unknown-csv-export-bug.md
    ├── customer-response-policy/
    │   ├── evaluation-criteria.md
    │   └── test-01 … test-05 (duplicate-charge, current-pricing,
    │       refund-amount, angry-customer, policy-gap)
    ├── customer-risk-briefing/
    │   ├── billing-data.md
    │   ├── evaluation-criteria.md   (empty)
    │   ├── product-usage-data.md
    │   ├── support-history.md
    │   └── test-01-acme-analytics.md
    └── ticket-triage/
        ├── escalation-rules.md
        ├── evaluation-criteria.md
        ├── routing-rules.md
        └── test-01-service-outage.md
```

## Testing

Each component was exercised independently with its own realistic input, with no cross-component runs. The repository stores test inputs and pass criteria, **not recorded run outputs**, so the Result column below does not claim a pass.

| Component | Test | Expected behavior | Result |
|---|---|---|---|
| customer-response-policy | test-01 Duplicate charge | Acknowledge; require verification; no promised $499 refund | No recorded result in repo |
| customer-response-policy | test-02 Current pricing | State Growth list price $199/month; no invented discount | No recorded result in repo |
| customer-response-policy | test-03 Refund amount | Eligibility depends on contract verification; no $2,400 promise | No recorded result in repo |
| customer-response-policy | test-04 Angry customer | Calm, brief acknowledgement, human tone, concrete next step | No recorded result in repo |
| customer-response-policy | test-05 Policy gap | Do not confirm the 40% discount or invent a renewal price; route to contract verification | No recorded result in repo |
| ticket-triage | test-01 Service outage | Service Outage, Critical, Engineering / Incident Response, Escalation YES, no unsupported claims | No recorded result in repo |
| customer-risk-briefing | test-01 Acme Analytics | Risk briefing from billing, usage, and support data (`evaluation-criteria.md` is empty, so there are no defined pass criteria) | No recorded result in repo |
| bug-investigator | test-01 Unknown CSV export bug | `Known Issue: NO`; distinguish from the CSV changelog entry; complete engineering report | No recorded result in repo |

## How to Run

1. Open this project folder in Claude Code (CLI or VS Code extension) so `.claude/` is picked up.
2. Skill: `/customer-response-policy <ticket text or file path>`, for example:
<text>/customer-response-policy tests/customer-response-policy/test-01-duplicate-charge.md

3. Agents: ask Claude Code to use the required agent by name and provide the relevant ticket, customer request, or bug report as pasted text or a file path.
Examples:
- Use the ticket-triage agent on tests/ticket-triage/test-01-service-outage.md
- Use the customer-risk-briefing agent on tests/customer-risk-briefing/test-01-acme-analytics.md
- Use the bug-investigator agent on tests/bug-investigator/test-01-unknown-csv-export-bug.md

4. Compare the output with the matching `evaluation-criteria.md` under `tests/`.

## Design Principles / Boundaries

- **Read-only:** Agents have only the `Read` tool and write no files.
- **No unsupported claims:** No invented prices, refunds, discounts, root causes, incidents, or confirmations.
- **Current policy first:** The active policy overrides customer assumptions and model memory.
- **Facts vs. inference:** Outputs separate what the sources state from what is concluded, using hedged wording such as "customer-reported" or "indicates potential risk".
- **No unauthorized actions:** Components do not contact customers, notify teams, create tickets or incidents, or edit records.
- **No orchestration:** Each Skill and Agent stands alone.

## Assignment Deliverables

- Skill: `customer-response-policy`
- Agents: `ticket-triage`, `customer-risk-briefing`, `bug-investigator`
- Supporting reference files: policy, tone, and background documents; routing and escalation rules; changelog; billing, usage, and support-history data
- Test cases and evaluation criteria under `tests/`
- This README

## Notes

- NovaDesk is a fictional company (stated in `company-background.md`). All tickets, customers, and data in this repository are synthetic test inputs.
- The agent definitions read their rules and data from files under `tests/`, so the agents are scoped to this sample dataset.
- The customer-risk-briefing `evaluation-criteria.md` is empty.
- Capability 4 is intentionally not implemented.
