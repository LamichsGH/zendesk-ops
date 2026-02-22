# TRT Refunds

## Core Rule
The agent CANNOT process refunds. ALL refund requests must be escalated.
But the agent CAN gather context to make the escalation efficient.

## Step 1: Detect Refund Type

**Order refund** (medication, blood test kit):
  → Signals: "refund for my order", "want my money back for [product]"
  → Call get_order_history
  → Include in internal_note: order number, item, amount if visible, reason

**Subscription refund** (overcharged, billed after cancellation):
  → Signals: "charged after I cancelled", "double charged", "unexpected payment"
  → Call get_subscription_status AND get_order_history
  → Include in internal_note: subscription status, payment dates, patient's claim

**Damaged/wrong item:**
  → Signals: "damaged", "wrong medication", "received wrong item"
  → Call get_order_history
  → Include in internal_note: what was expected vs what was received

**Wrong medication received:**
  → confidence = 0.35 (URGENT ESCALATE, send_response = false)
  → safety_flags: ["wrong_medication"]
  → Internal note: "URGENT: Patient reports receiving wrong medication. Requires immediate clinical review."

**Service complaint refund:**
  → Signals: "waste of money", "not working", general dissatisfaction
  → Call get_subscription_status
  → Include in internal_note: nature of complaint

## Step 2: Respond and Escalate

ALL refund types (except wrong medication) → confidence = 0.55 (ESCALATE)

Response structure:
1. Empathize: "I'm sorry to hear about this."
2. Acknowledge specifically: "I can see you're requesting a refund for [specific item/reason]."
3. Action: "I've passed this to our patient care team with all the details."
4. SLA: "They'll review this and get back to you within 24–48 hours."

## What NOT to Do
- Do NOT say "I've processed your refund" — you cannot
- Do NOT promise a specific refund amount
- Do NOT ask the patient to choose between refund and replacement — let the team decide
- Do NOT say "we don't do refunds" or imply refusal
- Do NOT ask for bank details

## Returns Policy (Non-Prescription Items Only)

**Non-prescription TRT items:** Blood test kits, injection supplies, sharps bins
**Prescription medication:** Testosterone, HCG, Clomid, Tadalafil, aromatase inhibitors, Statins

IF patient wants to return a non-prescription item:
  → confidence = 0.55 (ESCALATE)
  → Returns may be possible if the item is **unopened** and the request is within **14 days**
  → The agent CANNOT generate return labels or provide return addresses — CS handles logistics
  → "Returns for unopened non-prescription items may be possible within 14 days. I've passed this to our team who will arrange the return process."

IF patient wants to return prescription medication:
  → confidence = 0.55 (ESCALATE)
  → Prescription medication CANNOT be returned or refunded once dispensed/dispatched (regulatory/safety rules)
  → "Unfortunately, prescription medication can't be returned once it's been dispensed — this is due to regulatory and safety requirements. I've passed your query to our team to see if there's anything else we can do."

## Money Back Promise

IF patient mentions the Money Back Promise or says treatment isn't working:
  → confidence = 0.55 (ESCALATE)
  → Provide the link: [Our Money Back Promise](https://manual.co/money-back-promise)
  → "You can find the details of our Money Back Promise, including eligibility criteria and how to apply, on [this page](https://manual.co/money-back-promise). I've also flagged this with our team in case you need any further assistance."
  → Do NOT list or guess the eligibility criteria — just link to the page

## Bank Timeline Reference

When discussing refund timelines or pending charges:
- Always say: "the standard bank timeline of **3–5 working days** (this can vary depending on your bank)"
- Never say "a few days" or other vague language
- If beyond 5 working days: advise patient to check with their bank/card provider first
- If patient confirms they've checked with their bank and it's still unresolved: escalate
