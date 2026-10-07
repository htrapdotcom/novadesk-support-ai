# Bug Investigator — Evaluation Criteria

## Test Case

Ticket:

ND-10318

Customer:

Acme Analytics

## Expected Investigation Result

Known Issue:

NO

The changelog does not contain a matching known issue for the reported behavior.

The Agent should recognize that the existing Analytics CSV Export changelog entry concerns large text fields taking longer to generate, while the reported issue concerns exports containing more than 10,000 rows causing the dashboard to become unresponsive.

The Agent should not incorrectly classify the reported bug as the known CSV export issue.

## Expected Engineering Report

The result should contain enough information for an engineering team to understand and reproduce the issue.

At minimum, identify:

### Title

A concise description such as:

CSV export freezes dashboard for datasets larger than 10,000 rows

### Reproduction Steps

The Agent should preserve the relevant steps:

1. Open Analytics dashboard.
2. Select a dataset containing more than 10,000 rows.
3. Click Export CSV.
4. Dashboard becomes unresponsive.
5. Page reloads after approximately 30 seconds.

### Expected Behavior

CSV export should complete successfully regardless of dataset size.

### Actual Behavior

The dashboard becomes unresponsive and the export does not complete. The page reloads after approximately 30 seconds.

### Impact

The customer cannot export larger datasets needed for weekly reporting.

### Frequency / Reproducibility

The customer reports that the issue occurred consistently three times on the day of reporting.

## Known-Issue Check

The Agent should explicitly state that no matching known issue was found.

It should distinguish the existing Analytics CSV Export changelog entry from the reported issue rather than treating the feature-name match as sufficient evidence.

## Evidence Discipline

The Agent must not invent:

- Browser type or version
- Operating system
- Dataset format
- Exact dataset size beyond "more than 10,000 rows"
- Backend errors
- Stack traces
- Root cause
- Engineering owner
- Severity unless it is clearly presented as an assessment
- A fix or workaround
- Any information not contained in the bug report or changelog

## Output Quality

The result should:

- Clearly state whether the issue is already known.
- Explain the changelog comparison.
- Produce an engineering-ready report because no matching known issue was found.
- Preserve the customer's reproduction steps.
- Clearly separate expected and actual behavior.
- Include customer impact.
- Be concise and internally oriented.
- Not draft a customer-facing response.
- Not claim that the bug has been created, assigned, or escalated.

## Pass Condition

The Agent passes if it:

1. Determines that no matching known issue is documented.
2. Correctly distinguishes the existing CSV export changelog entry from the reported bug.
3. Produces a well-formed engineering-ready bug report.
4. Includes reproduction steps.
5. Includes expected behavior.
6. Includes actual behavior.
7. Includes impact and reproducibility.
8. Does not invent technical details or claim actions were taken.