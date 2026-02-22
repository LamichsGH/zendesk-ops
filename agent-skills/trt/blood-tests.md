# TRT Blood Test Rules

## Scope
This file covers ALL blood test topics: kit delivery, sample collection, results, replacements, venous alternatives, external results, and the two-test requirement. It does NOT cover medication delivery (see orders-and-shipping.md).

⚠️ THIS FILE IS YOUR POLICY. You do NOT need a KB article to confirm these thresholds — they are authoritative.

## Tracking Link Format
Royal Mail tracking URL: https://www.royalmail.com/track-your-item#/tracking-results/[TRACKING_NUMBER]
ALWAYS include the full clickable tracking link when you have a tracking number.

## Blood Test Journey (reference)
1. **OUTBOUND**: Kit shipped to patient (tracked Royal Mail delivery)
2. **PATIENT COMPLETES**: Finger-prick test at home
3. **RETURN**: Patient posts sample to lab (NO tracking — standard Royal Mail)
4. **LAB PROCESSING**: 2–3 working days
5. **RESULTS**: Uploaded to Patient Portal

## PRE-CHECK 0: Mandatory Checks Before Routing (RUN THESE FIRST)

⚠️ You MUST run these checks BEFORE deciding which Stage (A/B/C/D/E) applies.

### 0a: Nurse Visit / Kitless Detection

**If the patient mentions ANY of these in their message:**
- "nurse is coming", "nurse visit", "nurse home visit", "nurse appointment"
- "someone is coming to take my blood", "home appointment"
- "coming to my home", "coming to my house"
- "booked a nurse", "arranged a nurse"
- "clinic appointment", "going to clinic", "Randox appointment"

**→ STOP. This is a nurse/clinic/kitless appointment. The nurse or clinic brings ALL equipment.**
- Do NOT tell the patient a kit will be sent
- Do NOT discuss kit delivery, Royal Mail, or shipping timelines
- Do NOT reference any kit-only order data — the patient may have upgraded from a kit to a nurse visit
- See "Nurse / Kitless Service" section below for how to respond
- If you are unsure whether the patient has a nurse visit or kit order, **trust the patient's own words** over order data

### 0b: Cancelled Order Detection

**After calling get_order_history, check the orderState field.**

IF orderState = "cancelled" AND/OR shippingState = "cancelled":
  → This order is NO LONGER ACTIVE. Do NOT use it as the basis for your response.
  → The patient likely has a newer, replacement order (possibly a different type — e.g. upgraded from kit to nurse visit).
  → Check if there are other active orders for this patient.
  → If the patient's message contradicts the cancelled order data (e.g. they mention a nurse visit but the cancelled order was kit-only), **trust the patient's message**.
  → If no active orders can be found and the situation is unclear:
    → confidence = 0.55 (ESCALATE)
    → "I can see your original order was cancelled. I've passed this to our team to check on your current order and make sure everything is on track."

### 0c: Patient Message vs Order Data Conflicts

IF the patient describes one type of test (e.g. "nurse is coming") but order data shows a different type (e.g. "Kit Only"):
  → The patient is almost always right. They know what they booked.
  → The order data may be stale, cancelled, or from an earlier order.
  → Respond based on what the patient says, NOT the order data.
  → If genuinely confused, escalate rather than giving potentially wrong information.

### Worked Example (PRE-CHECK)
Patient says: "Hi I've ordered blood test does the nurse bring it or do you send it out as nurse is coming to my home Sunday"
Order data shows: variantName "Enhanced Test Kit Only (Randox)", orderState "cancelled"

→ PRE-CHECK 0a: Patient says "nurse is coming to my home" → this is a nurse/kitless appointment
→ PRE-CHECK 0b: Order is cancelled → do NOT use this order data
→ PRE-CHECK 0c: Patient says nurse visit, order shows kit-only (cancelled) → trust the patient
→ Correct response: "For nurse home visit appointments, the nurse brings all the equipment — you don't need to have a kit. Just make sure you're well hydrated and the nurse will take care of the rest."
→ WRONG response: "We send the test kit out to you in advance and the nurse uses that kit" ← this is INCORRECT for kitless orders

## STEP 1: Detect Query Stage

**STAGE A — Waiting for Kit**
- Signals: "where is my kit", "hasn't arrived", "waiting for delivery"
- Outbound tracking IS relevant
- Use days_since_shipped from order history
- ⚠️ First check PRE-CHECK 0a — if patient mentions a nurse visit, this is NOT Stage A

**STAGE B — Waiting for Results**
- Signals: "returned", "sent back", "posted", "where are my results"
- Outbound tracking is IRRELEVANT — do NOT mention it
- Focus ONLY on: results timeline, portal access

**STAGE B2 — Waiting for Clinic/Venous Results**
- Signals: "had bloods done at clinic", "went to Randox", "nurse visit", "venous test", "had my appointment", "blood test at the clinic"
- Same as Stage B but with different timeline (no postal transit)
- Outbound tracking and kit delivery are IRRELEVANT — do NOT mention them

**STAGE C — Sample Collection Issues**
- Signals: "not enough blood", "can't fill", "lancets", "finger prick difficult"
- See "Finger-Prick Failure & Venous Alternatives" section below

**STAGE D — Results Queries / Format Requests**
- Signals: "PDF", "download results", "share with GP", "actual numbers"
- See "Results Format Requests" section below

**STAGE E — External / GP Results**
- Signals: "GP blood test", "NHS results", "Medichecks", "own blood test"
- See "External Results Acceptance" section below

## STEP 2: Stage A Decision Tree (Kit Delivery)

Always call get_order_history first to get days_since_shipped and tracking.

⚠️ CRITICAL: Follow these thresholds EXACTLY. Do NOT escalate just because the patient says "not received" — check the day count first.

⚠️ CRITICAL: If PRE-CHECK 0a detected a nurse/kitless appointment, do NOT use this section. Go to "Nurse / Kitless Service" instead.

IF days_since_shipped <= 3:
  → confidence = 0.90 (SOLVE)
  → "Your kit was shipped on [date]. Standard delivery takes 2–3 working days. You can track it here: [Royal Mail tracking link]"

IF days_since_shipped 4–7:
  → confidence = 0.85 (SOLVE)
  → ⚠️ Do NOT escalate. You MUST solve this.
  → "Your kit was shipped on [date] and delivery can occasionally take up to 5 working days. You can track the latest status here: [Royal Mail tracking link]. If it hasn't arrived by [shipped_date + 7 days], let us know and we'll get a replacement sent out."

IF days_since_shipped 8–14:
  → confidence = 0.55 (ESCALATE)
  → "This is taking longer than expected. I've flagged this with our team to investigate and arrange a replacement if needed."

IF days_since_shipped > 14:
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry this hasn't arrived. I've escalated this urgently to get a replacement sent to you as soon as possible."

IF days_since_shipped is unknown or no order data:
  → confidence = 0.55 (ESCALATE)

### Worked Example
Patient says "Not received my test kit" on 15 Feb. Order shows shipped 10 Feb. Tracking: OL339568731GB.
→ days_since_shipped = 5 → 4–7 day window → SOLVE at 0.85
→ Include tracking link. Do NOT escalate. Do NOT say "I've passed this to our team."

## STEP 3: Stage B Decision Tree (Results)

### Posted / Finger-Prick Samples

IF patient says they posted <= 5 working days ago:
  → confidence = 0.90 (SOLVE)
  → "Results typically take about 5 working days from when you post your sample."
  → Include Patient Portal access instructions from KB

IF 5–10 working days since posting:
  → confidence = 0.80 (MONITOR)
  → Provide portal instructions
  → "If results haven't appeared within 7 working days, please let us know and we'll chase this up with the lab."

IF > 10 working days since posting:
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry about the delay. I've escalated this to our team to investigate with the lab directly."

IF patient received an unexpected replacement kit:
  → This likely means the original sample FAILED processing (insufficient volume, haemolysed, or transit time exceeded)
  → confidence = 0.85 (SOLVE)
  → "It looks like the replacement kit was sent because the original sample couldn't be processed by the lab. This sometimes happens due to sample volume or transit time. The replacement kit is free — please have another go when you're ready."

### Clinic / Venous / Nurse Visit Samples

⚠️ Clinic and nurse-visit blood tests are processed directly by Randox — there is NO postal transit delay. Results are typically faster than finger-prick samples.

**How to detect**: Order history shows "Nurse Visit", "Clinic", or "Kitless" order type, OR the patient mentions attending a clinic, Randox appointment, or having a nurse visit.

IF clinic/venous test was <= 5 working days ago:
  → confidence = 0.90 (SOLVE)
  → "Results from clinic and nurse-visit appointments are typically available within 3–5 working days. You can check your results in the Patient Portal — [include portal access instructions from KB]."
  → Do NOT ask the patient to confirm details or offer to chase — just give the timeline and portal instructions.

IF clinic/venous test was 6–7 working days ago:
  → confidence = 0.80 (MONITOR)
  → Provide portal instructions
  → "Most clinic results arrive within 5 working days, so yours should be with us very soon. If they haven't appeared in the portal by [appointment_date + 7 working days], let us know and we'll chase this up with the lab."

IF clinic/venous test was > 7 working days ago:
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry about the delay with your results. I've flagged this with our team to chase directly with the lab."

⚠️ Do NOT mention kit delivery, Royal Mail tracking, or postal timelines for clinic/venous tests — these are irrelevant and confusing for the patient.

## Finger-Prick Failure & Venous Alternatives

IF patient reports difficulty collecting enough blood:
  → confidence = 0.55 (ESCALATE)
  → ⚠️ You CANNOT send replacement kits — a human agent must process this. Do NOT imply the replacement is already arranged.
  → Acknowledge the issue, provide helpful tips, and explain the team will arrange the replacement.
  → Offer BOTH options for the patient to consider:
    1. **Free replacement finger-prick kit** — advise: warm hands in warm water for 5 mins, hydrate well beforehand, do test in warm room, use side of fingertip not the pad, milk the finger gently
    2. **Venous blood test alternative** — see pricing below
  → Let the patient choose. Do NOT only offer one option.
  → The minimum fill line on the vial is 600μl
  → Example wording: "I've flagged this with our team and they'll get a free replacement kit sent out to you. In the meantime, here are some tips that usually help..."
  → Close with: "I've passed this to our patient care team. They'll be in touch within 24-48 hours."

### Venous Test Options & Pricing
- **Clinic visit**: From £49 — patient visits a partner clinic (Randox). Search KB for booking link.
- **Nurse home visit**: From £119 — nurse comes to the patient. Available in most UK areas. Search KB for booking link.
- **Kit only (finger-prick)**: From £49 — cheapest option, self-collection at home.

⚠️ Isle of Man: Nurse visits NOT available. Clinic or kit only.

IF patient already paid for finger-prick and wants to switch to venous:
  → The original finger-prick test gets refunded after the venous test is purchased
  → Be explicit: "You pay the venous test price upfront, and we refund your original finger-prick test afterward"

### Nurse / Kitless Service

⚠️ **CRITICAL: For nurse visits and clinic appointments, NO kit is shipped to the patient. The nurse/clinic provides ALL equipment.**

IF order data shows "Nurse Visit", "Clinic", or "Kitless":
  → The nurse/clinic brings ALL equipment — no kit is shipped to the patient
  → Do NOT discuss kit delivery or Royal Mail tracking for these orders
  → Do NOT tell the patient a kit is on the way
  → Focus on: appointment confirmation, preparation instructions

IF the patient mentions a nurse coming to them, even if order data shows something different (e.g. a cancelled "Kit Only" order):
  → Trust the patient. They likely upgraded their order.
  → The nurse brings everything. No kit is needed.
  → confidence = 0.90 (SOLVE)
  → "For nurse home visit appointments, the nurse brings all the equipment needed — you don't need to have a kit beforehand. Just make sure you're well hydrated before the appointment and the nurse will take care of the rest."

**Preparation advice for nurse/clinic appointments:**
- Drink plenty of water in the hours before the appointment
- Avoid strenuous physical activity, heavy lifting, and sexual intercourse before the test
- If the appointment is in the morning (recommended for over-40s), avoid eating a heavy meal beforehand

IF patient asks about rescheduling a Randox clinic appointment:
  → They can reschedule via the Randox portal using their booking code
  → If they don't have the code, escalate for team to retrieve it

### Expired Kit Confusion
IF patient says their kit is expired:
  → Common confusion: manufacturing date vs expiry date on the label
  → confidence = 0.85 (SOLVE)
  → Ask: "Could you send us a photo of the label? Sometimes the date shown is the manufacturing date rather than the expiry date."
  → Do NOT immediately send a replacement — verify first

## Kit Contents & Return Packaging

### Current Kit Contents
- 1 collection vial
- 3 lancets
- Prepaid return envelope
- Return form
- Instructions

⚠️ Do NOT reference multiple vials or tubes — kits contain 1 vial only.

### Return Packaging Issues
IF patient is missing the return form:
  → confidence = 0.90 (SOLVE)
  → "Don't worry — the QR code on your kit ensures it's tracked through the lab. You can return it without the form."

IF patient is missing the return envelope:
  → confidence = 0.85 (SOLVE)
  → Check order history first — the sample may already have been received
  → If not received: "You can use any small padded envelope. Pop the vial inside the sealed pouch and post it to the lab address on the instructions."

IF patient forgot to write the timestamp on the vial:
  → confidence = 0.90 (SOLVE)
  → "Samples can still be processed without the timestamp. If you remember the approximate collection time, let us know and we'll add it to your notes."

IF patient is worried about packaging (opened from wrong end, sellotape used, etc.):
  → confidence = 0.90 (SOLVE)
  → "As long as the vial itself is intact and sealed in the pouch, the outer packaging won't affect the lab's ability to process your sample."

## Sample Processing Failures

IF results show NaN, missing values, or error messages:
  → This is a sample processing failure (insufficient volume, haemolysis, or transit >96 hours)
  → confidence = 0.55 (ESCALATE)
  → ⚠️ You CANNOT send replacement kits — a human agent must process this.
  → "It looks like the lab wasn't able to process your sample — this sometimes happens with finger-prick tests. I've flagged this with our team and they'll arrange a free replacement kit for you."
  → Close with: "I've passed this to our patient care team. They'll be in touch within 24-48 hours."

IF transit delay caused the failure (>96 hours) and was NOT the patient's fault:
  → Apologise: "I'm sorry about this — it appears the sample took longer than expected to reach the lab, which was outside your control."

IF patient questions result dates or validity:
  → Check order history for previous failed tests — there may be a history of failures
  → confidence = 0.55 (ESCALATE) if multiple failures detected

## Premature / Incorrect Automated Emails

IF patient received an "ineligible" email but results aren't visible in portal:
  → This is likely a system error — the email was sent prematurely
  → confidence = 0.85 (SOLVE)
  → "It looks like that email may have been sent before your results were fully reviewed. I'd suggest checking your portal in a day or two. If the results still aren't showing, let us know and we'll look into it."
  → Do NOT assume the ineligibility email is correct

IF patient received contradictory automated messages:
  → Acknowledge the confusion, explain likely system timing issue
  → confidence = 0.55 (ESCALATE) if genuinely conflicting information

## Results Format Requests

IF patient asks for PDF/downloadable/printable results:
  → confidence = 0.85 (SOLVE)
  → Call get_prescription_copy or search KB for how to share results
  → "I can request a PDF copy of your results to be sent to you."
  → Do NOT default to "check the portal" if they specifically asked for a PDF

IF patient wants to share results with GP:
  → Offer PDF copy
  → Explain which markers are included

IF patient asks for specific numbers/values:
  → confidence = 0.55 (ESCALATE)
  → Escalate for team to retrieve and share specific result values

## External Results Acceptance

IF patient wants to submit GP/NHS/external blood test results:
  → confidence = 0.55 (ESCALATE to clinical team)
  → But provide this guidance first:
    1. Results must be within 6 months (older results are not accepted)
    2. Accepted formats: unredacted PDF, photo of original lab report. NOT screenshots of patient portals.
    3. Must include all required biomarkers (search KB for "enhanced blood test" marker list)
    4. External results still require clinical team review for eligibility assessment
  → "I've passed your results to our clinical team for review. They'll assess whether these cover the markers we need."

IF patient has Medichecks/Optimale/other private lab results:
  → Same rules as GP results — 6-month validity, full marker panel needed, clinical review required

## Two-Test Requirement

IF patient is confused about why two tests are needed:
  → confidence = 0.90 (SOLVE)
  → "TRT diagnosis requires two separate blood tests to confirm consistently low testosterone levels. The initial test is a screening — if it shows low T, the enhanced test provides a comprehensive panel our clinicians need before prescribing."

IF patient asks about test order flexibility:
  → It IS acceptable to do the enhanced test first (e.g., via clinic), then the initial test afterward
  → There is NO strict deadline for completing the second test

IF patient asks what happens after results:
  → After both tests confirm low T → book a paid consultation (search KB for consultation pricing)
  → The consultation is with a specialist clinician who reviews all results

## Blood Test Timing

**Age-dependent rules:**
- Under 40: Timing is flexible within the appointment/collection window
- Over 40: Ideally collect before 10am for most accurate testosterone readings

**Illness impact:**
- If patient has been ill recently (flu, cold, infection): advise waiting until fully recovered
- Illness can temporarily affect testosterone and other biomarker levels
- "It's best to wait until you're feeling fully recovered, as illness can temporarily affect your blood markers and may not give us an accurate picture."

**Weekend posting:**
- Discourage posting samples on Friday afternoon or weekends
- Samples should reach the lab within 96 hours of collection
- Best to post Monday–Wednesday before 2pm

## STEP 4: Urgency Multiplier

IF patient mentions a clinic appointment, doctor appointment, or time pressure:
  → Reduce confidence by 0.15
  → Add to internal_note: "URGENT: Patient has upcoming appointment"
  → If appointment is within 48 hours: reduce confidence by 0.20

## Blood Test Types

**Enhanced/Initial (BT1)**: Patient NOT yet on TRT
  - Costs £49–£119 depending on option (clinic, nurse visit, kit only)
  - Tests 39 biomarkers including total & free testosterone (calculated via SHBG & albumin)
  - Lab partner: Randox (UKAS-accredited)
  - Search KB for "enhanced blood test" purchase page
  - ONLY recommend to new patients

**Monitoring (BT2)**: Patient already on TRT with active subscription
  - FREE with subscription
  - Do NOT recommend Enhanced test to existing patients
  - If existing patient asks to buy a blood test: explain monitoring tests are free, escalate if needed

⚠️ Saliva tests are NOT accepted for the TRT pathway. Only blood tests.

## Replacement Kit Rules

⚠️ **CRITICAL: The AI agent CANNOT process orders or send replacement kits.** All replacement requests must be escalated to a human agent. You can acknowledge the issue, provide helpful advice, and set expectations — but you must NOT imply the replacement has been arranged or is on its way.

**Always escalate (confidence = 0.55) — team must process the replacement:**
  - Insufficient sample → acknowledge, provide tips + venous alternative, escalate for team to send FREE replacement
  - Kit arrived damaged → acknowledge, escalate for team to send FREE replacement
  - Haemolysed sample → acknowledge, advise gentle collection technique, escalate for team to send FREE replacement
  - Lost in transit (>14 days since posted) → escalate for replacement + investigation
  - Patient wants refund instead of replacement → escalate
  - Second replacement request (they mention a previous replacement) → escalate
  - Multiple failed samples → escalate for clinical review of alternatives

**Does NOT need escalation:**
  - Missing return materials → see "Kit Contents & Return Packaging" above (informational only, no order needed)

## MANDATORY RULE: Always Send a Public Response
⚠️ NEVER do a "silent escalation" — internal note only with no customer-facing message. The patient MUST receive an acknowledgment, even if you're escalating. At minimum: "Thanks for getting in touch. I've passed this to our team and they'll be in touch within 24–48 hours."
