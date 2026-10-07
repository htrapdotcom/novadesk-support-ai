# Customer Response Skill — Evaluation Criteria

## Objective

Evaluate whether the customer-response skill can generate customer-facing
support replies that:

1. Follow the current support policy.
2. Follow the NovaDesk tone of voice.
3. Avoid unsupported commercial claims.
4. Avoid hallucinating pricing or refund information.
5. Handle missing policy information safely.

---

# Test-Level Evaluation

## Test 01 — Duplicate Charge

PASS if the response:

- Acknowledges the duplicate charge.
- States that verification is required.
- Does not guarantee a refund before verification.
- Does not guarantee a $499 refund.

FAIL if the response promises or states that the $499 refund has been
approved, issued, or will definitely be issued.

---

## Test 02 — Growth Pricing

PASS if the response:

- States that the Growth list price is $199/month.
- Does not invent a discount.
- Does not imply that $199 is necessarily the customer's contracted price.

FAIL if the response provides a different Growth list price or invents
customer-specific pricing.

---

## Test 03 — Annual Cancellation

PASS if the response:

- States that refund eligibility depends on contractual terms.
- States that the contract must be verified.
- Does not promise the $2,400 refund.
- Does not claim the refund has already been processed.

FAIL if the response promises or confirms the $2,400 refund.

---

## Test 04 — Angry Customer

PASS if the response:

- Acknowledges frustration.
- Remains calm.
- Is professional and conversational.
- Avoids blaming the customer.
- Provides a useful next step.
- Avoids excessive corporate language.

FAIL if the response is defensive, robotic, excessively apologetic,
or does not provide a useful next step.

---

## Test 05 — Policy Gap

PASS if the response:

- Does not confirm continuation of the 40% discount.
- Does not calculate or invent a renewal price.
- States that customer-specific pricing/discounts require verification.
- Directs the customer toward contract/commercial verification.

FAIL if the response claims the discount will continue or invents a
renewal price.

---

# Global Evaluation Rules

Every response must satisfy all of the following:

## 1. No Unsupported Commercial Claims

The skill must never invent:

- Prices
- Refund amounts
- Discounts
- Credits
- Contractual terms
- Refund eligibility

unless the information is explicitly supported by the current policy.

## 2. Policy Authority

The current policy takes precedence over:

- Customer assumptions
- Previous policy versions
- Model memory
- General knowledge

## 3. Missing Information

When the policy does not provide enough information:

- Do not guess.
- State what needs to be verified.
- Provide an appropriate next step.

## 4. Tone

Responses should be:

- Professional
- Conversational
- Concise
- Calm
- Helpful

## 5. Customer-Facing Output

The final response should normally be a message that could be sent directly
to the customer.

Do not expose internal reasoning, policy analysis, or evaluation criteria
to the customer.

---

# Overall Pass Condition

The skill passes the test package when:

- All five test cases satisfy their individual PASS conditions.
- No test case contains an unsupported commercial commitment.
- The responses consistently follow the NovaDesk tone.
- The skill can be invoked independently using a ticket and the current
  policy/reference documents.

A single hallucinated refund, price, discount, or contractual commitment
should be treated as a critical failure.