# NovaDesk Immediate Escalation Rules

## Purpose

These rules define when a support ticket requires immediate escalation rather than normal support routing.

## Immediate Escalation Required

A ticket must be marked for immediate escalation when any of the following conditions are present:

### 1. Widespread Service Outage

Immediately escalate when:
- Multiple users are unable to access NovaDesk
- The issue affects an entire customer team or organization
- The issue affects users across multiple locations
- The customer reports a platform-wide or widespread availability problem

### 2. Platform Availability Errors

Immediately escalate when:
- Multiple users report repeated 5xx server errors
- The ticket reports errors such as 500, 502, 503, or 504 affecting multiple users
- The reported behavior strongly indicates a platform availability problem

### 3. Security Incident

Immediately escalate when:
- Unauthorized access is suspected
- Credentials may have been compromised
- A customer reports a possible security breach

### 4. Legal Escalation

Immediately escalate when:
- The customer makes a formal legal threat
- The customer reports a regulatory or legal issue requiring legal review

## Escalation Output

When immediate escalation is required, the triage result must explicitly state:

- Immediate Escalation: YES
- The team or function receiving the escalation
- The specific evidence from the ticket that triggered escalation

## No Unsupported Claims

The triage agent must not claim that:
- An outage has been officially confirmed
- Engineering has already been notified
- An incident has already been created
- A fix is in progress
- A specific root cause is known

unless that information is explicitly present in the ticket or another authorized source.

The agent should distinguish between:
- A customer-reported or suspected outage
- An officially confirmed outage