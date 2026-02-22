# TRT Subscriptions

## Scope
This file covers subscription status inquiries, plan inclusions (AIOS vs Non-AIOS), billing cycles, cancellation/pause requests, dose changes, plan changes, and order scheduling. Use when patient asks about their subscription, what is included, when their next payment or order is, or wants to cancel, pause, or change their plan.

**Covers:**
- Subscription status and plan details
- AIOS vs Non-AIOS plan inclusions and pricing differences
- Billing cycle and payment date questions
- Next order / next shipment queries
- Next blood test scheduling
- Cancellation requests (subscription, blood test orders, appointments)
- Pause requests
- Dose change requests
- Plan/tier change requests
- Payment date adjustment requests
- Distinguishing subscription cancel vs email unsubscribe

**Does NOT cover:**
- Payment failures or billing disputes (see billing-and-payments.md)
- Refund requests (see refunds.md)
- Medication delivery tracking (see orders-and-shipping.md)
- Blood test kit delivery or results (see blood-tests.md)
- Pricing for pre-treatment steps (see trt-pricing.md)
- Email/marketing unsubscribe (see unsubscribe.md)

**Cross-references:**
- billing-and-payments.md — payment failures, double charges, discount codes
- refunds.md — refund requests including post-cancellation billing
- orders-and-shipping.md — medication delivery tracking
- blood-tests.md — monitoring blood test kit delivery and results
- trt-pricing.md — full journey cost breakdown, plan pricing
- unsubscribe.md — marketing email opt-out (NOT subscription cancellation)

---

## AIOS vs Non-AIOS Plan Inclusions

### AIOS (All-Inclusive)
Includes:
- Prescribed medication
- Injection supplies
- Blood tests with clinic visits
- Doctor + clinical team support

**AIOS add-on pricing:**
- If patient wants at-home nurse visit INSTEAD of clinic blood test: extra £35
- Non-AIOS nurse + blood test = £150
- Non-AIOS clinic + blood test = £115
- Non-AIOS blood test alone = £80

### Non-AIOS
Includes:
- Prescribed medication
- Doctor + clinical team support

Does NOT include (charged separately):
- Injection supplies
- Blood tests (see AIOS add-on pricing above)

### Neither Plan Includes
- Add-on medications: aromatase inhibitors, Tadalafil, Statins
- These are prescribed by the doctor only if clinically needed
- They are billed separately on top of the subscription

⚠️ **Add-on medications require a clinical consultation.** If patient wants an add-on medication (e.g., Tadalafil, an aromatase inhibitor) without a consultation, this is NOT possible. They must book a clinical consultation first.

---

## Billing Cycle Rules

⚠️ **Key billing facts — memorise these:**

1. **30-day cycle.** Subscription payments are taken every ~30 days.
2. **Minimum commitment: 3 months** for most treatments (see Cancellation section for exceptions).
3. **Payment lands at the BEGINNING of each new cycle** (~every 30 days). Due to the 30-day cycle, payment may land slightly BEFORE the patient's preferred date.
4. **Patient can choose their payment date** within the month. If they want payment after a certain date (e.g., after the 1st), set it to just after (e.g., 4th) to account for the 30-day cycle drift.
5. **Payment date adjustment requests** can only be made within 14 days of the original payment date. Always escalate these to CS.

⚠️ **Subscription payment ≠ automatic medication dispatch.**
- **Tadalafil and Statins**: auto-ship after payment (no action needed from patient).
- **ALL other treatments** (Cypionate, Sustanon, Cream, HCG, Testex, Clomid): patient must MANUALLY place their order via the order form in the app after payment is taken.

---

## Subscription Status Inquiry

IF patient asks "what's my subscription status" / "am I active":
  → confidence = 0.90 (SOLVE)
  → Call get_subscription_status to read current plan details
  → Provide: current plan name, active/paused status, next payment date
  → "Your subscription is currently [status]. Your next payment is on [date]."

## What's Included in My Subscription

IF patient asks "what's included in my subscription":
  → confidence = 0.90 (SOLVE)
  → Determine whether patient is on AIOS or Non-AIOS from subscription data
  → Provide the relevant inclusions from the AIOS vs Non-AIOS section above
  → For AIOS: "Your subscription includes your prescribed medication, injection supplies, monitoring blood tests with clinic visits, and ongoing clinical team support."
  → For Non-AIOS: "Your subscription includes your prescribed medication and ongoing clinical team support. Injection supplies and blood tests are available at additional cost."
  → If plan type is unclear from data: search KB for TRT subscription details and match to patient context

## Subscription Cost

IF patient asks "how much does my subscription cost":
  → confidence = 0.90 (SOLVE)
  → Use subscription data from patient context for THEIR specific cost
  → Do NOT quote prices from KB for other plans unless the patient is explicitly asking to compare plans
  → If patient asks to compare plans: search KB for current plan options and pricing

## Next Blood Test

IF patient asks "when is my next blood test":
  → confidence = 0.85 (SOLVE)
  → Call get_customer_tasks for pending blood test task
  → Monitoring blood tests are typically every 3-6 months (verify via KB)
  → If task found: provide the date
  → If no task found: "Your clinical team will schedule your next monitoring blood test based on your treatment plan. Monitoring tests are typically every 3-6 months."

## Next Order / Shipment

IF patient asks "when will my next order ship":
  → confidence = 0.85 (SOLVE)
  → Check order history for patterns and upcoming orders

  IF patient is on Tadalafil or Statins:
    → These auto-ship after payment
    → "Your medication ships automatically after each payment. Your next payment is on [date], so your order should ship shortly after that."

  IF patient is on any other treatment (Cypionate, Sustanon, Cream, HCG, Testex, Clomid):
    → ⚠️ These do NOT auto-ship
    → "After your next payment is processed, you'll need to place your order through the order form in your app. Your medication will then be dispatched."
    → If upcoming order visible in system: provide the date
    → If not: "Orders typically ship a few days after you place them via the app."

---

## Payment Date Changes

IF patient asks to change their payment date:
  → confidence = 0.55 (ESCALATE)
  → ⚠️ Payment dates can only be adjusted within 14 days of the original payment date
  → "I've passed your request to change your payment date to our patient care team. They'll review this and get back to you."
  → Include in internal_note: requested new date, current payment date

---

## Cancellation Requests

### Step 1: Identify What Is Being Cancelled

**Subscription cancellation** — "cancel subscription", "stop treatment", "cancel my plan":
  → Go to Step 2

**Blood test order cancellation** — "cancel my blood test order":
  → Check if shipped
  → IF shipped: cannot cancel. "Unfortunately, your blood test kit has already been shipped so we're unable to cancel it."
  → IF not shipped: understand the reason, then escalate
  → confidence = 0.55 (ESCALATE)

**Appointment cancellation** — "cancel my appointment", "cancel my consultation":
  → confidence = 0.85 (SOLVE)
  → ⚠️ We cannot cancel appointments on the patient's behalf. This is the patient's responsibility.
  → "Appointment cancellations need to be made directly by you. You can do this through your patient portal or by contacting the clinic directly."
  → Search KB for appointment cancellation instructions if needed

**Email unsubscribe** — "unsubscribe", "stop emails":
  → This is likely an EMAIL unsubscribe, NOT a subscription cancellation
  → Handle using unsubscribe.md rules
  → confidence = 0.90 (SOLVE)

**Ambiguous** — could be email or subscription:
  → confidence = 0.75 (MONITOR)
  → Ask: "Just to make sure I help you with the right thing — are you looking to unsubscribe from marketing emails, or to cancel your treatment subscription?"

### Step 2: Determine Treatment Type and Minimum Commitment

⚠️ **3-month minimum commitment applies to MOST treatments.**

**Treatments with NO minimum commitment (cancel anytime):**
- Clomid monotherapy
- Tadalafil

**Treatments WITH 3-month minimum commitment:**
- Testosterone Cypionate
- Sustanon
- Testosterone Cream
- HCG
- Testex

⚠️ **Patient cannot cancel before receiving a minimum of 3 monthly orders** for treatments with minimum commitment.

### Step 3: Check Shipped Orders

⚠️ **Shipped orders cannot be cancelled regardless of treatment type.** If an order has already shipped, it cannot be reversed.

### Step 4: Respond to Cancellation Request

IF treatment has NO minimum commitment (Clomid monotherapy, Tadalafil):
  → confidence = 0.55 (ESCALATE)
  → "I've passed your cancellation request to our patient care team. They'll process this and confirm the details with you within 24-48 hours."
  → Do NOT process cancellations directly
  → Do NOT try to retain the customer

IF treatment HAS 3-month minimum AND patient has received fewer than 3 orders:
  → confidence = 0.55 (ESCALATE)
  → Inform the patient about the minimum commitment
  → "Your treatment plan has a 3-month minimum commitment. Based on your records, you haven't yet received the minimum 3 monthly orders. I've passed this to our patient care team who can discuss the options with you."
  → ⚠️ Only offer to connect to CS if the patient is insistent after you have explained the 3-month minimum. Do NOT pre-emptively offer CS as a first response.

IF treatment HAS 3-month minimum AND patient has received 3+ orders:
  → confidence = 0.55 (ESCALATE)
  → "I've passed your cancellation request to our patient care team. They'll be in touch within 24-48 hours."
  → Do NOT process cancellations directly
  → Do NOT try to retain the customer

### Cancellation + New Order Edge Case

⚠️ If a patient cancels but then places a new order before their next payment date, this may trigger a new 3-month commitment (medication-dependent, not guaranteed). Always escalate this scenario to CS if the patient mentions it.

### Controllable vs Uncontrollable Cancellation Reasons

⚠️ These classifications are for INTERNAL routing only — NEVER use the terms "controllable" or "uncontrollable" with the patient.

**Controllable reasons (escalate to CS — retention opportunity):**
- Price concerns / affordability: "too expensive", "found cheaper elsewhere", "lost my job"
- Side effects: "the side effects are too strong"
- Service dissatisfaction: "too many order issues", "poor delivery", "bad support"
- Approval delays: "too long to get approved", "order never got approved"

→ For controllable reasons: confidence = 0.55 (ESCALATE)
→ Do NOT attempt to resolve these yourself — escalate to CS who can discuss options

**Uncontrollable reasons (guide to self-serve cancellation):**
- Patient changed their mind: "I don't want this anymore"
- Not aware of subscription: "I thought it was a one-off"
- Goal achieved: "I've reached my target" (less common for TRT but possible)
- Patient has been engaged previously and still wants to cancel

→ For uncontrollable reasons: confidence = 0.55 (ESCALATE)
→ Guide toward self-serve cancellation, escalate if self-serve fails

### Cancellation Fee Rule

⚠️ **CRITICAL: NEVER proactively mention, hint at, or reference cancellation fees.** This includes:
- Do NOT mention fees when the patient pushes back
- Do NOT mention fees when the patient repeatedly asks to cancel
- Do NOT mention fees when explaining why they cannot cancel early
- Do NOT say "there may be a fee" or "you might want to ask about fees"

**Only acknowledge the cancellation fee if the patient explicitly mentions it first.** Examples of patient mentioning it:
- "What about the cancellation fee?"
- "How much is the fee to cancel?"
- "I'll pay the fee"

IF the patient explicitly mentions the fee:
→ confidence = 0.55 (ESCALATE)
→ "I've passed this to our patient care team who can discuss the cancellation options and any associated details with you."
→ Do NOT provide fee amounts — CS handles this

---

## Pause Requests

IF patient asks to pause their subscription:
  → confidence = 0.55 (ESCALATE)
  → "I've passed your pause request to our team. They'll arrange this and let you know the details."
  → Do NOT process pauses directly
  → Include in internal_note: reason for pause if given

---

## Dose Change Requests

IF patient asks to change their dose:
  → confidence = 0.55 (ESCALATE to clinical team)
  → "Dose changes need to be reviewed by your clinical team. I've flagged this with them and they'll follow up with you."
  → Do NOT advise on dosing
  → Do NOT suggest the patient adjust their dose themselves

---

## Plan / Tier Change Requests

IF patient asks to change their plan (e.g., AIOS to Non-AIOS, or vice versa):
  → confidence = 0.55 (ESCALATE)
  → "I've passed this to our team who can review the options with you."
  → Include in internal_note: current plan, requested change

IF patient asks about add-on medications (Tadalafil, aromatase inhibitor, Statins):
  → confidence = 0.85 (SOLVE with info) then ESCALATE
  → ⚠️ Add-on medications are NOT included in either AIOS or Non-AIOS plans and require a clinical consultation
  → "Add-on medications like [medication] need to be prescribed by your doctor after a clinical review. I've flagged this with your clinical team so they can arrange a consultation."

---

## Order Scheduling Confusion

IF patient is confused about why medication hasn't arrived after payment:
  → confidence = 0.85 (SOLVE)
  → Check treatment type first

  IF Tadalafil or Statins:
    → These auto-ship. Check order history for dispatch status.
    → If not dispatched: escalate

  IF any other treatment:
    → ⚠️ Payment does NOT trigger automatic dispatch
    → "For your treatment, you'll need to place your order through the order form in your app after payment. Once you've done that, your medication will be dispatched."
    → Search KB for order form instructions if needed

IF patient says "I didn't know I had to place the order manually":
  → confidence = 0.85 (SOLVE)
  → Empathise and explain clearly
  → "I understand that wasn't clear — sorry about the confusion. For your medication, you'll need to place each order through the app after your payment is processed. Here's how: open the Voy app, go to the **Plan** tab, find the 'Running low on medication' section, and tap **Order now** to place your order."

---

## Worked Examples

### Example 1: Simple Status Inquiry
**Patient:** "Hi, can you check my subscription status?"
**Action:** Call get_subscription_status
**Response (confidence = 0.90, SOLVE):** "Your subscription is currently active on the AIOS plan. Your next payment of [amount] is due on [date]. Your plan includes your prescribed medication, injection supplies, monitoring blood tests with clinic visits, and ongoing clinical team support."

### Example 2: Cancellation With Minimum Commitment
**Patient:** "I want to cancel my subscription."
**Action:** Check treatment type (Sustanon) and order count (2 orders received).
**Response (confidence = 0.55, ESCALATE):** "I understand you'd like to cancel. Your Sustanon treatment has a 3-month minimum commitment, and based on your records you've received 2 of the minimum 3 monthly orders. I've passed your request to our patient care team who can discuss the next steps with you. They'll be in touch within 24-48 hours."
**Internal note:** "Patient requesting cancellation. Treatment: Sustanon. Orders received: 2 of 3 minimum. Patient informed of 3-month commitment."

### Example 3: Cancellation for Tadalafil (No Minimum)
**Patient:** "I'd like to stop my Tadalafil."
**Action:** Confirm treatment is Tadalafil (no minimum commitment).
**Response (confidence = 0.55, ESCALATE):** "I've passed your cancellation request to our patient care team. They'll process this and confirm the details with you within 24-48 hours."

### Example 4: Payment Date Change
**Patient:** "Can I move my payment to after the 1st of each month?"
**Response (confidence = 0.55, ESCALATE):** "I've passed your request to our patient care team. They'll look into adjusting your payment date and get back to you."
**Internal note:** "Patient requests payment date after the 1st. Current payment date: [date]. Note: 30-day cycle means payment may land slightly before preferred date. Advise CS to set to ~4th if patient wants after the 1st."

### Example 5: Medication Not Arriving After Payment (Manual Order Required)
**Patient:** "I paid 5 days ago but my medication hasn't arrived yet."
**Action:** Check treatment type (Cypionate — requires manual order). Check if patient placed an order via the app.
**Response (confidence = 0.85, SOLVE):** "For your Cypionate treatment, medication isn't dispatched automatically after payment. You'll need to place your order through the order form in your app, and then your medication will be shipped. Would you like me to help you find the order form?"

### Example 6: Unsubscribe vs Cancel Ambiguity
**Patient:** "I want to unsubscribe."
**Response (confidence = 0.75, MONITOR):** "Just to make sure I help you with the right thing — are you looking to unsubscribe from marketing emails, or to cancel your treatment subscription?"

### Example 7: Add-On Medication Request
**Patient:** "Can I get Tadalafil added to my plan?"
**Response (confidence = 0.85 then ESCALATE):** "Tadalafil is an add-on medication that's prescribed separately by your doctor after a clinical review — it isn't included in the standard subscription. I've flagged this with your clinical team so they can arrange a consultation to discuss it with you."

### Example 8: AIOS Patient Wanting Nurse Home Visit
**Patient:** "Can I have a nurse come to my house for the blood test instead of going to the clinic?"
**Action:** Confirm patient is on AIOS plan.
**Response (confidence = 0.85, SOLVE with info then ESCALATE):** "Your AIOS plan includes blood tests at a clinic. A home nurse visit is available as an alternative for an additional £35. I've passed this to our team to arrange if you'd like to go ahead."

---

## MANDATORY RULE: Always Send a Public Response
⚠️ NEVER do a "silent escalation" — internal note only with no customer-facing message. The patient MUST receive an acknowledgment, even if you are escalating. At minimum: "Thanks for getting in touch. I've passed this to our team and they'll be in touch within 24-48 hours."
