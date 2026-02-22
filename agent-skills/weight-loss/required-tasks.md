# Required Tasks

## Scope

This file covers all pre-order tasks that patients must complete before their medication order can be approved and dispatched.

**Covers:**
- ID verification (accepted documents and upload process)
- BMI picture verification (requirements and guidelines)
- Video BMI verification call (alternative to photo)
- Proof of previous prescription (for patients starting at higher doses)
- Weight reading requirement
- First order billing (reservation vs charge)
- Task status interpretation (`get_customer_tasks` tool output)

**Does NOT cover:**
- Order status and dispatch → see `orders-and-shipping.md`
- Payment failures → see `payments.md`
- Subscription management → see `subscriptions.md`

**Cross-references:**
- `orders-and-shipping.md` — order blocked by pending tasks (Path A)
- `payments.md` — first order billing reservation

---

## Task Status Tool Reference

**ALWAYS call `get_customer_tasks` to check task status before responding.**

**Tool output sections:**
1. **Pending tasks** — Tasks the patient must complete before their order can be reviewed and approved by a clinician
2. **Recently completed tasks** — Tasks completed within the last 2 working days; clinicians are still reviewing
3. **Past completed tasks** — Tasks completed more than 2 working days ago; should already be reviewed

⚠️ If there are recently completed tasks and the order has not yet been approved, reassure the patient that a clinician will review within 1–2 working days. NEVER escalate for recently completed tasks.

---

## ID Verification

IF patient asks about ID verification:
  → confidence = 0.90 (SOLVE)

**Why it's needed:**
→ "We need to verify your identity to comply with prescription medication regulations and to provide a safe service."

**Accepted ID documents:**
- Passport (UK, Channel Islands, Isle of Man, British Overseas Territory, EEA state, or Commonwealth country — including Irish Passport Cards)
- Driving licence (UK, Channel Islands, Isle of Man, or EEA state — including provisional licences)
- Blue Badge
- ID card with a Proof of Age Standards Scheme (PASS) hologram
- Biometric immigration document
- Ministry of Defence Form 90 (Defence Identity Card)
- National identity card from an EEA state

Any valid form of ID containing full name, date of birth, and a photo is accepted.

IF patient has only international documents not listed above:
  → confidence = 0.55 (ESCALATE)
  → "International documents not on our standard accepted list require a case-by-case review. Let me connect you with our team to assist."

**How to upload:**
→ "You can upload your ID through the Voy app:
1. Go to the **Home** tab
2. Find the ID verification task
3. Upload a clear photo of your ID

⚠️ Please upload through the task in the app, not through the chat."

---

## BMI Picture Verification

IF patient asks about BMI picture requirements:
  → confidence = 0.90 (SOLVE)

**Why it's needed:**
→ "We need to verify your BMI alongside your ID to comply with prescription medication regulations."

**Photo requirements:**
- Face must be visible (to match with ID)
- Full body must be shown
- Wear tight/fitted clothing so BMI can be estimated
- Must be a recent photo taken specifically for this verification — holiday or old photos are not permitted

**How to upload:**
→ "Upload your BMI photo through the Voy app:
1. Go to the **Home** tab
2. Find the BMI verification task
3. Upload your photo

⚠️ Please upload through the task in the app, not through the chat."

IF patient is uncomfortable submitting a photo:
  → confidence = 0.85 (SOLVE — offer alternative)
  → "I understand this can feel uncomfortable. We offer an alternative: a short video BMI verification call that takes less than 5 minutes. Would you like me to provide the link for that instead?"
  → Proceed to Video BMI Verification section

---

## Video BMI Verification

IF patient prefers video verification or cannot submit a photo:
  → confidence = 0.85 (SOLVE)

→ "You can complete your BMI verification via a short video call instead of submitting a photo. The call typically takes less than 5 minutes."

1. Provide the Open Room video verification call link
2. Ask if they have any questions about the process

⚠️ Agent CANNOT confirm or deny a patient's attendance to a BMI verification call. Always ask the patient if they've attended.

IF patient says they already attended:
  → confidence = 0.88 (SOLVE)
  → "Great, no further action is needed from your side. The results will be reviewed by our clinical team."

IF patient says they have NOT attended:
  → confidence = 0.85 (SOLVE)
  → Provide the Open Room video verification link
  → "The call should take less than 5 minutes."

⚠️ NEVER confirm or suggest that you know the patient's video BMI verification date.

---

## Proof of Previous Prescription

IF patient has a "proof of previous prescription" task:
  → confidence = 0.88 (SOLVE)

**Why it's needed:**
→ "Patients who wish to start treatment at a higher dose need to provide proof of a recent prescription from a previous provider. This confirms treatment continuity."

**Requirements — ALL are mandatory:**
- Prescription must be from within the previous 8 weeks
- Must show the patient's full legal name
- Must show the prescription approval date
- Must show the name of the medication
- Must show the strength/dose of the medication

⚠️ Make it very clear that ALL requirements are mandatory for order approval.

**How to upload:**
→ "You should have a task notification in the Home tab of your app. Upload a clear photo of your previous prescription there."

IF no task is visible in the app:
  → "If you don't see the task in your app, you can upload a photo of your prescription through the in-app chat."

⚠️ CRITICAL: Do NOT comment on whether the prescription photo meets requirements. Once a photo is shared, respond ONLY with:
→ "Thank you for sharing your prescription. Would you like me to connect you with our team to have this reviewed?"

---

## Weight Reading

IF patient has a "weight reading" task:
  → confidence = 0.90 (SOLVE)
  → "You're required to provide a recent weight reading to continue your treatment. You can submit this through the task in your app (Home tab)."

---

## First Order Billing

IF patient asks about first order charges or billing:
  → confidence = 0.90 (SOLVE)

→ "For first orders, the payment amount is reserved on your card when you place the order. Your card is only charged after the order is approved by our clinical team."

IF patient asks about what happens if the order is not approved:
  → confidence = 0.90 (SOLVE)
  → "If your order is not approved, the reserved amount will be released back to your account within 7 business days."

---

## Worked Examples

**Example 1: ID verification question**
Patient: "What ID do I need to upload?"
→ confidence = 0.90 (SOLVE)
→ "You can upload any valid photo ID that shows your full name, date of birth, and a photo. This includes a passport, driving licence (including provisional), Blue Badge, PASS card, biometric immigration document, MoD Form 90, or an EEA national identity card. Please upload it through the task in your Home tab, not through the chat."

**Example 2: BMI photo concerns**
Patient: "I don't feel comfortable sending a full body photo."
→ confidence = 0.85 (SOLVE — offer alternative)
→ "I completely understand. We offer a video BMI verification call as an alternative — it takes less than 5 minutes and you'll speak with a member of our team. Would you like me to provide the link?"

**Example 3: Proof of prescription requirements**
Patient: "I was on Wegovy 1.7mg with another provider. What do I need to provide?"
→ confidence = 0.88 (SOLVE)
→ "To start at a higher dose, you'll need to upload proof of your previous prescription. The prescription must be from within the last 8 weeks and clearly show your full name, the approval date, medication name (Wegovy), and the dose strength (1.7mg). All of these details are required. You can upload this through the task in your Home tab."

**Example 4: First order billing concern**
Patient: "I was charged but my order hasn't been approved yet."
→ confidence = 0.90 (SOLVE)
→ "For first orders, the payment amount is reserved on your card when you place the order, but you're only fully charged once the clinical team approves it. If the order isn't approved, the reserved amount will be released within 7 business days."
