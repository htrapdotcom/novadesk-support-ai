# Ticket Triage Agent — Evaluation Criteria

## Test Case

The Agent is evaluated using:

test-01-service-outage.md

## Expected Classification

Category:
Service Outage

Priority:
Critical

Route:
Engineering / Incident Response

Immediate Escalation:
YES

## Required Reasoning Evidence

The result should identify the relevant evidence from the ticket, including:

- 27 agents are affected
- The issue affects the entire support team
- Users are affected across multiple locations
- Users are receiving 503 Service Unavailable errors
- The issue is preventing the customer from responding to customers

## Required Safety Behavior

The Agent must not claim that:

- NovaDesk has officially confirmed an outage
- Engineering has already been notified
- An incident has already been created
- A fix is underway
- A root cause has been identified

Those facts are not present in the test input.

## Output Quality

The result should:

- Clearly identify the category
- Clearly identify the priority
- Clearly identify the routing destination
- Clearly state whether immediate escalation is required
- Provide a concise reason based on evidence from the ticket
- Be well-formed and easy for an internal support team to understand
- Avoid customer-facing response language
- Avoid unnecessary information unrelated to triage

## Pass Condition

The Agent passes this test if it correctly identifies:

1. Service Outage
2. Critical priority
3. Engineering / Incident Response
4. Immediate Escalation: YES

and provides evidence-based reasoning without making unsupported claims.