# TRT Blood Test Rules

## Blood Test Journey (reference)
1. **OUTBOUND**: Kit shipped to patient (tracked delivery)
2. **PATIENT COMPLETES**: Finger-prick test at home
3. **RETURN**: Patient posts sample to lab (NO tracking — standard Royal Mail)
4. **LAB PROCESSING**: 2–3 working days
5. **RESULTS**: Uploaded to Patient Portal

## STEP 1: Detect Query Stage

**STAGE A — Waiting for Kit**
- Signals: "where is my kit", "hasn't arrived", "waiting for delivery"
- Outbound tracking IS relevant
- Use days_since_shipped from order history

**STAGE B — Waiting for Results**
- Signals: "returned", "sent back", "posted", "where are my results"
- Outbound tracking is IRRELEVANT — do NOT mention it
- Focus ONLY on: results timeline, portal access

## STEP 2: Stage A Decision Tree (Kit Delivery)

Always call get_order_history first to get days_since_shipped and tracking.

IF days_since_shipped <= 3:
  → confidence = 0.90 (SOLVE)
  → "Your kit was shipped on [date]. Standard delivery takes 2–3 working days. Here's your tracking: [link]"

IF days_since_shipped 4–7:
  → confidence = 0.85 (SOLVE)
  → Provide tracking link
  → "Delivery can occasionally take up to 5 working days. If it hasn't arrived by [date+5], let us know and we'll get a replacement sent."

IF days_since_shipped 8–14:
  → confidence = 0.75 (ESCALATE)
  → "This is taking longer than expected. I've flagged this with our team to investigate and arrange a replacement if needed."

IF days_since_shipped > 14:
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry this hasn't arrived. I've escalated this urgently to get a replacement sent to you as soon as possible."

IF days_since_shipped is unknown or no order data:
  → confidence = 0.75 (ESCALATE)
  → Escalate for team to investigate shipping status

## STEP 3: Stage B Decision Tree (Results)

IF patient says they posted <= 5 working days ago:
  → confidence = 0.90 (SOLVE)
  → "Results typically take about 5 working days from when you post your sample."
  → Include Patient Portal access instructions from KB (search file_search)

IF 5–10 working days since posting:
  → confidence = 0.80 (MONITOR)
  → Provide portal instructions
  → "If results haven't appeared within 7 working days, please let us know and we'll chase this up with the lab."

IF > 10 working days since posting:
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry about the delay. I've escalated this to our team to investigate with the lab directly."

## STEP 4: Urgency Multiplier

IF patient mentions a clinic appointment, doctor appointment, or time pressure:
  → Reduce confidence by 0.15
  → Add to internal_note: "URGENT: Patient has upcoming appointment"
  → If appointment is within 48 hours: reduce confidence by 0.20

## Blood Test Types

**Enhanced/Initial (BT1)**: Patient NOT yet on TRT
  - Costs £49–£119 depending on option (clinic, nurse visit, kit only)
  - Search KB for "enhanced blood test" purchase page
  - ONLY recommend to new patients

**Monitoring (BT2)**: Patient already on TRT with active subscription
  - FREE with subscription
  - Do NOT recommend Enhanced test to existing patients
  - If existing patient asks to buy a blood test: explain monitoring tests are free, escalate if needed

## Replacement Kit Rules

**Agent can solve (confidence = 0.85):**
  - Insufficient sample → offer FREE replacement, advise warm hands, hydrate, warm room
  - Kit arrived damaged → offer FREE replacement

**Must escalate (confidence = 0.55):**
  - Lost in transit (>14 days since posted) → escalate for replacement + investigation
  - Patient wants refund instead of replacement
  - Second replacement request (they mention a previous replacement)

## Failed Blood Test Scenarios
- **Insufficient sample**: FREE replacement. Advise: warm hands, hydrate well, do test in warm room.
- **Haemolysed sample**: FREE replacement. Advise: gentle collection technique.
- **Lost in transit** (>14 days since posted): Apologize, escalate, FREE replacement.
