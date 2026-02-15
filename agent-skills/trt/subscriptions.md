# TRT Subscriptions

## What the Agent CAN Do (SOLVE)

**Subscription status inquiry:**
  → confidence = 0.90 (SOLVE)
  → Call get_subscription_status to read current plan details
  → Provide: current plan, next payment date, active/paused status
  → "Your subscription is currently [status]. Your next payment is on [date]."

**"What's included in my subscription?":**
  → confidence = 0.90 (SOLVE)
  → Search KB for TRT subscription details
  → Key inclusions: prescribed medication, FREE monitoring blood tests, clinical oversight, patient portal access
  → "Your subscription includes your prescribed medication, free monitoring blood tests, and ongoing clinical oversight."

**"When is my next blood test?":**
  → confidence = 0.85 (SOLVE)
  → Call get_customer_tasks for pending blood test task
  → Monitoring blood tests are typically every 3–6 months (verify via KB)
  → If no task found: "Your clinical team will schedule your next monitoring blood test based on your treatment plan."

**"How much does my subscription cost?":**
  → confidence = 0.90 (SOLVE)
  → Use subscription data from patient context for THEIR specific cost
  → Do NOT quote prices from KB for other plans unless patient is asking to compare

**"When will my next order ship?":**
  → confidence = 0.85 (SOLVE)
  → Check order history for patterns
  → If upcoming order visible: provide date
  → If not: "Orders typically ship a few days before your next payment date."

## What the Agent CANNOT Do (ESCALATE)

**Cancellation requests:**
  → confidence = 0.55 (ESCALATE)
  → "I've passed your cancellation request to our patient care team. They'll be in touch within 24–48 hours."
  → Do NOT process cancellations
  → Do NOT try to retain the customer

**Pause requests:**
  → confidence = 0.55 (ESCALATE)
  → "I've passed your pause request to our team. They'll arrange this and let you know the details."

**Dose change requests:**
  → confidence = 0.55 (ESCALATE to clinical team)
  → "Dose changes need to be reviewed by your clinical team. I've flagged this with them."

**Payment failures / billing disputes:**
  → confidence = 0.55 (ESCALATE)
  → If patient mentions failed payment: "I've flagged this with our team to look into."
  → Do NOT ask for payment card details
  → Do NOT advise on updating payment methods unless KB has self-serve instructions

**Plan/tier change requests:**
  → confidence = 0.55 (ESCALATE)
  → "I've passed this to our team who can review the options with you."

## Distinguishing Subscription Cancel vs Email Unsubscribe

IF patient says "unsubscribe" or "stop emails":
  → This is likely an EMAIL unsubscribe, not a subscription cancellation
  → Handle using unsubscribe.md rules
  → confidence = 0.90+ (SOLVE)

IF patient says "cancel subscription", "stop treatment", "cancel my plan":
  → This IS a subscription cancellation
  → confidence = 0.55 (ESCALATE)

IF ambiguous:
  → confidence = 0.75 (MONITOR)
  → Ask: "Just to make sure I help you with the right thing — are you looking to unsubscribe from marketing emails, or to cancel your treatment subscription?"
