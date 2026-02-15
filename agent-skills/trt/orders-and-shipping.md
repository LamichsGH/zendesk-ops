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
  → confidence = 0.70 (ESCALATE)
  → "This is taking longer than expected. I've flagged this with our team to investigate."
  → Internal note: include tracking number, days elapsed

IF days_since_shipped > 14:
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry about this delay. I've escalated this urgently for investigation."

IF no tracking available but order exists:
  → confidence = 0.70 (ESCALATE)
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
