# TRT Restart Subscription After Cancellation

## Scope
This file covers patients who previously cancelled their TRT subscription and now want to restart treatment. Use when a patient asks to come back, reactivate, resume, or restart their TRT subscription.

**Covers:**
- Restart / reactivation requests from former patients
- Determining time off treatment and flagging clinical requirements
- Blood work requirements for returning patients
- Dose reset safety rules
- Discount eligibility for returning patients

**Does NOT cover:**
- Pause requests or resuming from a pause (see subscriptions.md)
- New patient sign-ups who have never been on TRT (see eligibility.md)
- Active subscription changes (see subscriptions.md)
- Billing or payment issues (see billing-and-payments.md)
- Blood test kit delivery or results (see blood-tests.md)

**Cross-references:**
- subscriptions.md — active subscription management, pause, cancellation
- blood-tests.md — monitoring blood test rules, timing, biomarkers
- billing-and-payments.md — payment failures, discount codes
- trt-pricing.md — pricing for consultations and blood tests
- clinical-routing.md — clinical team escalation rules

---

## CRITICAL: All Restart Requests Require Escalation

⚠️ **The email agent CANNOT reactivate subscriptions.** All restart requests must be escalated to CS/clinical team. Your role is to:
1. Acknowledge the patient warmly — they are choosing to come back
2. Gather context from order history and subscription data
3. Flag any clinical requirements (dose reset, blood work) in the internal note
4. Escalate with a detailed internal note so the team can act quickly

---

## Decision Tree

### Step 1: Confirm Intent and Gather Data

IF patient asks to restart / reactivate / resume / come back:
  → Call `get_subscription_status` to confirm subscription is cancelled/inactive
  → Call `get_order_history` to determine:
    - Date of last order (to calculate time off treatment)
    - Last medication and dose prescribed
    - Date of last blood test (if visible in order history)

### Step 2: Determine Time Off Treatment

Calculate the number of weeks since the patient's last order/treatment.

**IF <= 4 weeks off treatment:**
  → Patient may be able to resume from their previous dose
  → Clinical team review still required
  → confidence = 0.55 (ESCALATE)
  → Flag in internal note: "Patient off treatment for [X] weeks (<=4 weeks). May be eligible to resume at previous dose — clinical review required."

**IF > 4 weeks off treatment:**
  → ⚠️ Dose reset to lowest level will be required for safety
  → Clinical team review required
  → confidence = 0.55 (ESCALATE)
  → Flag in internal note: "Patient off treatment for [X] weeks (>4 weeks). Dose reset to lowest level required per safety protocol — clinical review required."

**IF time off treatment cannot be determined:**
  → confidence = 0.55 (ESCALATE)
  → Flag in internal note: "Unable to determine time off treatment from order history. Clinical team to review patient file."

### Step 3: Check Blood Work Requirement

Determine the date of the patient's last blood test from order history.

**IF > 6 months since last blood test:**
  → New blood work will likely be needed before restarting
  → Add to internal note: "Last blood test was [date] (>6 months ago). Updated blood work likely required before restarting."

**IF <= 6 months since last blood test:**
  → Blood work may still be valid — clinical team to confirm
  → Add to internal note: "Last blood test was [date] (<=6 months ago). Clinical team to confirm if current bloods are sufficient."

**IF blood test date cannot be determined:**
  → Add to internal note: "Unable to determine date of last blood test. Clinical team to review."

### Step 4: Check for Discount/Promo Mentions

**IF patient asks about first-order discount or returning patient discount:**
  → ⚠️ Returning patients are NOT eligible for first-order discounts
  → Do NOT offer or promise any discount
  → IF the patient mentions a specific promo code: include it in the internal note for CS to validate
  → "Welcome back! I should mention that first-order discounts are for new patients only, but if you have a promo code, our team can check if it's applicable to your order."

**IF patient does NOT mention discounts:**
  → Do NOT proactively mention discounts or pricing

### Step 5: Respond and Escalate

→ confidence = 0.55 (ESCALATE)
→ Be welcoming — the patient is choosing to return, which is positive
→ Acknowledge their request and set expectations
→ Close with: "I've passed this to our patient care team. They'll be in touch within 24-48 hours."

---

## Response Templates

### Standard Restart (No Discount Mention)
"Welcome back — it's great to hear you'd like to restart your TRT treatment! I've passed your request to our patient care team. They'll review your file and let you know if anything is needed before getting you started again, such as updated blood work or a clinical review. They'll be in touch within 24-48 hours."

### Restart With Dose Reset Flag (>4 Weeks Off)
"Welcome back — it's great to hear you'd like to restart your treatment! As it's been a little while since your last treatment, our clinical team will need to review your file before restarting. This may include starting at a lower dose for safety reasons, and possibly updated blood work. I've passed this to our patient care team and they'll be in touch within 24-48 hours to go through the next steps with you."

### Restart With Blood Work Flag (>6 Months Since Last Test)
"Welcome back — it's great to hear you'd like to restart your TRT treatment! As some time has passed since your last blood test, our clinical team may need updated blood work before restarting your treatment. I've passed your request to our patient care team. They'll review everything and be in touch within 24-48 hours."

### Restart With Both Flags (>4 Weeks Off AND >6 Months Since Last Bloods)
"Welcome back — it's great to hear you'd like to restart! Since it's been a while since your last treatment, our clinical team will need to review your file. This will likely involve updated blood work and starting at a lower dose for safety. I've passed this to our patient care team and they'll be in touch within 24-48 hours to walk you through the next steps."

---

## Internal Note Guidance

Every restart escalation MUST include an internal note with the following (where available):

```
RESTART REQUEST — Returning Patient

Last treatment details:
- Last medication: [medication name and dose]
- Last order date: [date]
- Time off treatment: [X weeks/months]

Clinical flags:
- Dose reset required: [Yes (>4 weeks off) / No (<=4 weeks off) / Unknown]
- Blood work needed: [Likely (>6 months since last test) / Possibly (<=6 months) / Unknown]
- Last blood test date: [date or "unable to determine"]

Additional context:
- [Any reason for cancellation if mentioned]
- [Any promo code mentioned]
- [Any other relevant patient context]
```

---

## MANDATORY ESCALATION TRIGGERS

Set confidence_score <= 0.55 and escalate if ANY of these are present:
- Any restart/reactivation request (ALL require escalation)
- Patient mentions clinical concerns about restarting
- Patient asks about changing medication or dose from what they were previously on
- Patient mentions side effects they experienced before cancelling

---

## Worked Examples

### Example 1: Simple Restart, Short Gap
**Patient:** "Hi, I cancelled my subscription about 2 weeks ago but I'd like to come back."
**Action:** Call `get_subscription_status` (confirmed cancelled). Call `get_order_history` (last order 14 days ago, Testosterone Cypionate 0.3ml, last blood test 2 months ago).
**Response (confidence = 0.55, ESCALATE):** "Welcome back — it's great to hear you'd like to restart your TRT treatment! I've passed your request to our patient care team. They'll review your file and let you know if anything is needed before getting you started again. They'll be in touch within 24-48 hours."
**Internal note:** "RESTART REQUEST — Returning Patient. Last medication: Testosterone Cypionate 0.3ml. Last order date: [date]. Time off treatment: ~2 weeks. Dose reset required: No (<=4 weeks off). Blood work needed: Possibly (last test ~2 months ago — clinical team to confirm). Last blood test date: [date]."

### Example 2: Restart After Long Gap
**Patient:** "I stopped my TRT about 3 months ago. I want to start again."
**Action:** Call `get_subscription_status` (confirmed cancelled). Call `get_order_history` (last order ~12 weeks ago, Sustanon 250mg, last blood test 8 months ago).
**Response (confidence = 0.55, ESCALATE):** "Welcome back — it's great to hear you'd like to restart! Since it's been a while since your last treatment, our clinical team will need to review your file. This will likely involve updated blood work and starting at a lower dose for safety. I've passed this to our patient care team and they'll be in touch within 24-48 hours to walk you through the next steps."
**Internal note:** "RESTART REQUEST — Returning Patient. Last medication: Sustanon 250mg. Last order date: [date]. Time off treatment: ~12 weeks. Dose reset required: Yes (>4 weeks off — must restart at lowest dose). Blood work needed: Likely (last test ~8 months ago, >6 months). Last blood test date: [date]."

### Example 3: Restart With Discount Request
**Patient:** "I want to restart my TRT. Do I get the new patient discount since I'm basically starting over?"
**Action:** Call `get_subscription_status` (confirmed cancelled). Call `get_order_history` (last order 6 weeks ago).
**Response (confidence = 0.55, ESCALATE):** "Welcome back — it's great to hear you'd like to restart your treatment! I should mention that first-order discounts are for new patients only, but our team can discuss the available options with you. As it's been a few weeks since your last treatment, our clinical team will also need to review your file before restarting. I've passed this to our patient care team and they'll be in touch within 24-48 hours."
**Internal note:** "RESTART REQUEST — Returning Patient. Last medication: [medication]. Last order date: [date]. Time off treatment: ~6 weeks. Dose reset required: Yes (>4 weeks off). Patient asked about new patient discount — informed not eligible. No promo code provided."

### Example 4: Restart With Promo Code
**Patient:** "I'd like to restart my subscription. I have a code RETURN20 — does that work?"
**Action:** Call `get_subscription_status` (confirmed cancelled). Call `get_order_history`.
**Response (confidence = 0.55, ESCALATE):** "Welcome back — it's great to hear you'd like to restart! I've noted your code RETURN20 and passed it to our team so they can check if it's applicable to your order. They'll review your file and be in touch within 24-48 hours to get you set up."
**Internal note:** "RESTART REQUEST — Returning Patient. Last medication: [medication]. Last order date: [date]. Time off treatment: [X weeks]. Patient provided promo code: RETURN20 — CS to validate. [Include dose reset and blood work flags as per decision tree.]"

---

## MANDATORY RULE: Always Send a Public Response
⚠️ NEVER do a "silent escalation" — internal note only with no customer-facing message. The patient MUST receive a welcoming acknowledgment, even if you are escalating. At minimum: "Welcome back! I've passed your request to our patient care team and they'll be in touch within 24-48 hours."
