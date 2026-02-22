# TRT Orders & Shipping (Medication)

## Scope
⚠️ **This file covers MEDICATION orders ONLY** (testosterone, HCG, ancillaries).

**BLOOD TEST KITS ARE NOT MEDICATION.** If the patient is asking about a blood test kit delivery, STOP — do NOT use this file's decision tree. Use **blood-tests.md** instead. The SLA thresholds are different.

How to tell the difference:
- Order contains "Blood Test", "Test Kit", "Randox" → use **blood-tests.md**
- Order contains "Testosterone", "Sustanon", "HCG", "Anastrozole" → use **this file**

## Order Tracking
- All medication orders have outbound tracking
- Use tracking data from patient context — NEVER fabricate tracking codes
- Format tracking as clickable link: [Track your delivery](URL)

## ⚠️ CRITICAL: Payment ≠ Medication Dispatch

**This is one of the most common sources of patient confusion for TRT orders.**

Subscription payments do NOT automatically trigger medication dispatch. The relationship between payment and orders depends on the medication type:

**Auto-dispatch medications (payment = order):**
- **Tadalafil** — automatically renewed and dispatched monthly
- **Statin medication** — automatically renewed and dispatched monthly

**Manual-order medications (payment ≠ order):**
- **Testosterone Cypionate** — patient must manually place order via the app
- **Sustanon** — patient must manually place order via the app
- **HCG** — patient must manually place order via the app
- **Testosterone Cream** — patient must manually place order via the app
- **Clomifene** — patient must manually place order via the app
- **Anastrozole / Exemestane / Tamoxifen** — patient must manually place order via the app

### How to Detect This Issue
IF patient says "I've paid but haven't received my medication" AND they are on a manual-order medication:
  → confidence = 0.85 (SOLVE)
  → The patient likely paid their subscription but hasn't placed a medication order
  → "I can see your subscription payment has gone through. For [medication name], you'll need to place your order through the app to trigger the dispatch. Here's how:
    1. Open the Voy app and go to the **Plan** tab
    2. In the 'Running low on medication' section, tap **Order now**
    3. Select the medication and consumables you need
    4. Tap **Go to order recap**, confirm details, and tap **Place your order**"
  → "Once you've placed the order, it will go through clinical review and you'll receive a tracking link once it's dispatched."

IF patient asks when to reorder:
  → Use the expected duration data from get_order_history to estimate when they'll run low
  → This is guidance, not a strict timeline — remind them to reorder when running low
  → "Based on your current dosage and last order, you might want to reorder around [estimated date]. You can do this anytime through the app."

## Medication Delivery SLA Decision Tree

⚠️ REMINDER: This tree is for MEDICATION only. Blood test kits have their own SLA in blood-tests.md.

Always call get_order_history first.

IF days_since_shipped <= 3:
  → confidence = 0.90 (SOLVE)
  → Provide tracking link, confirm standard delivery window

IF days_since_shipped 4–5:
  → confidence = 0.85 (SOLVE)
  → Provide tracking, reassure: "Deliveries can occasionally take up to 5 working days"

IF days_since_shipped 6–14:
  → confidence = 0.55 (ESCALATE)
  → "This is taking longer than expected. I've flagged this with our team to investigate."
  → Internal note: include tracking number, days elapsed

IF days_since_shipped > 14:
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry about this delay. I've escalated this urgently for investigation."

IF no tracking available but order exists:
  → confidence = 0.55 (ESCALATE)
  → "Your order is in our system but I don't have tracking details yet. I've flagged this for our team to check."

## Tracking Shows Delivered but Patient Says Not Received

→ confidence = 0.55 (ESCALATE)
→ "I can see tracking shows this as delivered. I've escalated this for our team to investigate — they may need to arrange a reshipment."
→ Do NOT argue with the patient about tracking status

## Address Changes

IF order not yet shipped:
  → confidence = 0.55 (ESCALATE)
  → Include new address in internal_note
  → "I've passed your updated address to our team so they can update this before your order ships."

IF order already shipped:
  → confidence = 0.55 (ESCALATE)
  → "Unfortunately we can't redirect an order once it's been shipped. I've flagged this with our team to advise on next steps."

IF no pending order (general address update):
  → confidence = 0.55 (ESCALATE)
  → "I've passed your address update to our team."

## Self-Serve Order Tracking

IF patient asks how to check their order status and you don't have specific order data issues to address:
  → confidence = 0.90 (SOLVE)
  → Provide self-serve instructions:
    1. Open the Voy app and go to the **Plan** tab
    2. Select **Track Delivery** for the relevant order
    3. Possible statuses:
       - **Awaiting Approval** — clinical review in progress (usually 1–2 days)
       - **Approved** — cleared for dispatch, being prepared for shipping
       - **Dispatched** — with Royal Mail, in transit
       - **Delivered** — successfully delivered
    4. Click the tracking link in the app for real-time Royal Mail updates

## Patient Status Detection

**EXISTING patient signals:**
- Mentions dose changes, scheduled blood tests, ongoing treatment
- subscription_status = "active", has TRT medication orders

**NEW patient signals:**
- No order history, asks about starting treatment
- subscription_status = null or empty

## Product Recommendation Rules

**Enhanced Blood Test (£49–£119):** ONLY for NEW patients (pre-treatment)
**Monitoring Blood Test:** ONLY for EXISTING patients (FREE with subscription)

IF existing patient asks to buy a blood test:
  → Do NOT link the Enhanced test
  → Explain monitoring tests are free with their subscription
  → confidence = 0.85 (SOLVE with info), or escalate if they insist

## HCG-Specific Rules
- HCG may ship separately from testosterone
- Requires cold chain — if patient asks about HCG delivery timing, mention it may arrive in a separate package
- HCG storage: MUST be refrigerated (2–8°C) after mixing

## Medication Duration Reference

Use this to help patients understand when they might need to reorder:
- **Testosterone Cypionate**: 3–4 months per vial (depending on dosage)
- **Sustanon**: 3–4 months per supply (depending on dosage)
- **Testosterone Cream**: ~1 month per tube
- **HCG**: 4–6 weeks (depending on dosage)
- **Clomifene**: ~1 month (depending on dosage)
- **Tadalafil**: 1 month (auto-dispatched)
- **Anastrozole / Exemestane / Tamoxifen**: ~4 months (depending on dosage)

⚠️ These are estimates. Always refer to get_order_history for patient-specific reorder timing.
