# Weight Loss Refunds

## Scope

Handles refund requests, refund status queries, returns, Money Back Guarantee questions, payment confusion, and billing discrepancies for weight loss patients.

## Tool Requirement

**MANDATORY:** Call `get_order_history` BEFORE writing any patient-facing guidance.

**Tool usage rules:**

- Use output to determine which decision tree path to follow
- Reference order status (pending, approved, dispatched) to avoid repeated questions
- Only call when needed for the question being asked
- Never imply you changed anything based on the tool
- When referencing output, say: "From the order details available, I can see..."
- Never say "refunded about X hours/days ago" (tool does not provide refund timing)
- Only say: "...the cancelled order was created about X hours/days ago and shows as refunded."

**If tool fails:**

- Do not imply you have details
- Use neutral wording: "To help me point you in the right direction..."

---

## Item Type Classification

Classify items as **prescription medication** vs **non-prescription** to determine the correct refund/return path.

### Prescription Medication

**Definition:** Medications requiring clinical approval (e.g. Mounjaro, Wegovy).

**Policy:**

- NO returns or refunds once approved, dispensed, or dispatched (regulatory/safety rules)
- Faulty or incorrect items → ESCALATE at 0.55
- Treatment not working → direct to Money Back Guarantee (Path 4)

### Non-Prescription Items

**Examples:** Needles, rescue pack.

**Policy:**

- Returns/refunds may be possible if **unopened** and requested/returned **within 14 days**
- Return logistics are handled by the patient care team — agent cannot generate labels or provide return addresses
- If eligible and patient wants to proceed → ESCALATE at 0.55

**Clarifying question (if item type is unclear from the ticket):**

> "Could you let me know whether the item is prescription medication (e.g. Mounjaro, Wegovy) or a non-prescription item (e.g. needles, rescue pack)?"
> → MONITOR at 0.70, awaiting_customer_input = true

---

## Bank Timeline Reference

Use this whenever discussing pending authorisation holds or refunds appearing in the patient's bank.

| Situation | Response | Route |
| --- | --- | --- |
| Standard timeline | "Refunds typically take **3-5 working days** to appear, depending on your bank." | SOLVE at 0.85 |
| Still showing after 5 working days | Advise patient to check with their bank/card provider first | SOLVE at 0.85 |
| Patient confirmed they checked with bank and charge is still unresolved | ESCALATE at 0.55 | ESCALATE at 0.55 |

**Language rule:** Always say "3-5 working days". Never say "a few days", "shortly", or similar vague language.

---

## Decision Tree

Select exactly ONE path based on the patient's request:

1. Cancel / refund request for a current order
2. Refund status / where is my money?
3. Returns (non-prescription items only)
4. Money Back Guarantee (treatment not effective)
5. Payment confusion (pending vs processed)
6. Charged wrong amount / charged twice

---

### Path 1: Cancel / Refund Request for Current Order

Use `get_order_history` to determine order status, then follow the matching branch.

#### Branch A: Not yet clinically approved

The patient CAN cancel their order. A pending charge is reversed (not shown as a separate refund).

- Inform patient: "Since your order hasn't been clinically approved yet, you can cancel it directly through your account. The pending charge typically reverses within 3-5 working days."
- Note: Cancelling their subscription does NOT cancel a pending order — these are separate actions.
- **Route:** SOLVE at 0.85

#### Branch B: Approved but not yet dispatched

Cancellation/refund may not be possible if the order is already marked for dispatch.

- Inform patient of the policy and escalate for the team to review.
- **Route:** ESCALATE at 0.55

#### Branch C: Dispatched or received (prescription medication)

Refunds/returns for dispensed prescription medication are not available (regulatory/safety rules).

- Explain the policy clearly and empathetically.
- If the patient still wants the team to review → ESCALATE at 0.55
- If treatment is not working → direct to Path 4 (Money Back Guarantee)
- **Route:** ESCALATE at 0.55

#### Branch D: Subscription misunderstanding ("I didn't realise it was recurring" / "I forgot to cancel")

- If order is still pending approval → ESCALATE at 0.55 for the team to review cancellation
- If order is approved/dispatched → explain policy clearly, ESCALATE at 0.55 for review (no guarantees)
- **Route:** ESCALATE at 0.55

---

### Path 2: Refund Status / Where Is My Money?

#### Scenario A: Pending charge / reversal expected

- Explain it is an authorisation hold that should disappear when released
- State: "This typically takes 3-5 working days, depending on your bank."
- **Route:** SOLVE at 0.85

#### Scenario B: Refund was actioned

- Explain refunds go back to the **original payment method**
- State timeline: 3-5 working days (bank-dependent)
- **Route:** SOLVE at 0.85

#### Scenario C: Beyond 5 working days and still not received

- Advise patient to check with their bank/card provider
- If patient confirms they have already checked with their bank and it is still unresolved → ESCALATE at 0.55
- **Route:** SOLVE at 0.85 (initial advice) or ESCALATE at 0.55 (if bank already checked)

**Timing rules:**

- For "how long ago" language: rely ONLY on the order's created-at timestamp from `get_order_history`
- NEVER repeat the patient's claimed timing (e.g. "you were refunded 6 days ago")
- NEVER say "your refund was processed X days/hours ago"
- If the patient's timing conflicts with the order data, gently correct using the order timestamp:
  - Example: "From the order details available, I can see the cancelled order was created about X days ago and shows as refunded."

---

### Path 3: Returns (Non-Prescription Items Only)

1. Classify the item using the Item Type Classification above
2. If **prescription medication** → explain it cannot be returned or refunded once dispensed/dispatched (regulatory/safety rules). Route: SOLVE at 0.85 (policy explanation)
3. If **non-prescription item** (e.g. needles, rescue pack) → explain returns/refunds may be possible if the item is **unopened** and the request is made **within 14 days**
4. Agent cannot generate return labels or provide return addresses — the patient care team handles return logistics
5. If the patient wants to proceed with a return → ESCALATE at 0.55

---

### Path 4: Money Back Guarantee

- Direct patient to the Money Back Guarantee page: [Money Back Guarantee](https://www.manual.co/money-back-guarantee)
- Say the page explains eligibility criteria and how to submit a request
- Do NOT list or guess the criteria — the link covers everything
- **Route:** SOLVE at 0.85

---

### Path 5: Payment Confusion (Pending vs Processed)

1. Explain that pending payments can be authorisation holds that typically drop off once released
2. Advise patient to check whether the transaction shows as **pending** or **completed** in their banking app
3. If **completed** and they cannot match it to an order after checking their account/confirmation email → ESCALATE at 0.55
- **Route:** SOLVE at 0.85 (initial explanation) or ESCALATE at 0.55 (if unresolved)

---

### Path 6: Charged Wrong Amount / Charged Twice

Patient reports being charged an incorrect amount or charged multiple times.

1. Use `get_order_history` to review recent orders and charges
2. Acknowledge the concern empathetically
3. ESCALATE at 0.55 — billing discrepancies require the patient care team to investigate and resolve
- **Route:** ESCALATE at 0.55

---

## Replacement Kit Requests

- Acknowledge with empathy
- Explain free replacement policy from KB
- Use honest language: "I've requested a replacement — our team will arrange this"
- **Route:** ESCALATE at 0.55

---

## Mandatory Rules

- All refund/billing actions → ESCALATE at 0.55 (agent cannot process refunds)
- Do NOT claim to have processed a refund, cancellation, or return
- Do NOT promise a refund outcome — always frame as "the team will review"
- Always call `get_order_history` before responding to refund or order-related queries
- Always use "3-5 working days" for bank timelines — never vague language
- All URLs must use markdown hyperlink format
- No multi-turn patterns: do not ask the patient to do something and wait for a response. Provide all guidance in one reply.

---

## Worked Examples

### Example 1: Refund request — order not yet approved

**Patient email:** "I want to cancel my order and get a refund"

**Tool call:** `get_order_history` → order status: pending (not yet approved)

```json
{
  "patient_reply": "Hi [First Name],\n\nSince your order hasn't been clinically approved yet, you can cancel it directly through your account. Once cancelled, the pending charge typically reverses within 3-5 working days, depending on your bank.\n\nPlease note that cancelling your subscription is a separate action — it won't cancel a pending order.\n\nI hope this helps! If you have any other questions, just reply to this email.\n\nBest,\nManual\n\n---\n*This response was generated by the AI Assistant.*",
  "internal_note": "Issue: Patient wants to cancel and refund a pending weight loss order\nAction: Advised self-cancel via account; order not yet approved so reversal applies",
  "related_articles": [],
  "confidence_score": 0.85,
  "confidence_reasoning": "Order not yet approved — patient can self-cancel and pending charge reverses per policy",
  "sources_used": ["Weight Loss Refunds skill"],
  "safety_flags": [],
  "questions_detected": 1,
  "questions_answered": 1,
  "questions_escalated": 0,
  "awaiting_customer_input": false
}
```

### Example 2: Refund request — order dispatched

**Patient email:** "I received my Mounjaro but I want a refund"

**Tool call:** `get_order_history` → order status: dispatched, item: Mounjaro (prescription medication)

```json
{
  "patient_reply": "Hi [First Name],\n\nI understand you'd like a refund for your Mounjaro order. Unfortunately, once prescription medication has been dispensed and dispatched, it cannot be returned or refunded due to regulatory and safety rules.\n\nIf your treatment isn't working as expected, you may be eligible for the [Money Back Guarantee](https://www.manual.co/money-back-guarantee) — the page explains the eligibility criteria and how to submit a request.\n\nI've passed this to our patient care team so they can review your case. We're currently experiencing high demand, so please allow 24-48 hours for a response.\n\nBest,\nManual\n\n---\n*This response was generated by the AI Assistant.*",
  "internal_note": "Issue: Patient wants refund for dispatched Mounjaro\nAction: Explained prescription medication refund policy; mentioned Money Back Guarantee; escalated for team review",
  "related_articles": [{"title": "Money Back Guarantee", "url": "https://www.manual.co/money-back-guarantee"}],
  "confidence_score": 0.55,
  "confidence_reasoning": "Dispatched prescription medication — refund not possible per policy, escalated for team review",
  "sources_used": ["Weight Loss Refunds skill"],
  "safety_flags": [],
  "questions_detected": 1,
  "questions_answered": 1,
  "questions_escalated": 1,
  "awaiting_customer_input": false
}
```

### Example 3: Charged twice

**Patient email:** "I've been charged twice for my order this month"

**Tool call:** `get_order_history` → shows order details

```json
{
  "patient_reply": "Hi [First Name],\n\nI'm sorry to hear about the duplicate charge — I completely understand the concern. I've passed this to our patient care team so they can investigate and resolve this for you. We're currently experiencing high demand, so please allow 24-48 hours for a response.\n\nBest,\nManual\n\n---\n*This response was generated by the AI Assistant.*",
  "internal_note": "Issue: Patient reports being charged twice for weight loss order\nAction: Billing discrepancy — escalated to patient care team for investigation",
  "related_articles": [],
  "confidence_score": 0.55,
  "confidence_reasoning": "Billing discrepancy (charged twice) — requires team investigation, cannot resolve via agent",
  "sources_used": ["Weight Loss Refunds skill"],
  "safety_flags": [],
  "questions_detected": 1,
  "questions_answered": 1,
  "questions_escalated": 1,
  "awaiting_customer_input": false
}
```

### Example 4: Money Back Guarantee inquiry

**Patient email:** "Wegovy isn't working for me, can I get my money back?"

```json
{
  "patient_reply": "Hi [First Name],\n\nI'm sorry to hear the treatment hasn't been working as expected. You may be eligible for our Money Back Guarantee — you can find all the details, including eligibility criteria and how to submit a request, here: [Money Back Guarantee](https://www.manual.co/money-back-guarantee)\n\nI hope this helps! If you have any other questions, just reply to this email.\n\nBest,\nManual\n\n---\n*This response was generated by the AI Assistant.*",
  "internal_note": "Issue: Patient says Wegovy is not effective, wants money back\nAction: Directed to Money Back Guarantee page",
  "related_articles": [{"title": "Money Back Guarantee", "url": "https://www.manual.co/money-back-guarantee"}],
  "confidence_score": 0.85,
  "confidence_reasoning": "Treatment effectiveness concern — directed to Money Back Guarantee page per policy",
  "sources_used": ["Weight Loss Refunds skill"],
  "safety_flags": [],
  "questions_detected": 1,
  "questions_answered": 1,
  "questions_escalated": 0,
  "awaiting_customer_input": false
}
```

### Example 5: Return request for non-prescription item

**Patient email:** "I want to return the needles I ordered, they're still sealed"

**Tool call:** `get_order_history` → confirms needle order

```json
{
  "patient_reply": "Hi [First Name],\n\nThank you for reaching out. Since needles are a non-prescription item and they're still unopened, a return and refund may be possible provided the request is within 14 days of delivery.\n\nI've passed this to our patient care team — they'll arrange the return logistics and next steps for you. We're currently experiencing high demand, so please allow 24-48 hours for a response.\n\nBest,\nManual\n\n---\n*This response was generated by the AI Assistant.*",
  "internal_note": "Issue: Patient wants to return unopened needles (non-prescription)\nAction: Eligible per returns policy (unopened, within 14 days). Escalated for return logistics.",
  "related_articles": [],
  "confidence_score": 0.55,
  "confidence_reasoning": "Non-prescription return request — policy allows if unopened and within 14 days, but team must handle return logistics",
  "sources_used": ["Weight Loss Refunds skill"],
  "safety_flags": [],
  "questions_detected": 1,
  "questions_answered": 1,
  "questions_escalated": 1,
  "awaiting_customer_input": false
}
```
