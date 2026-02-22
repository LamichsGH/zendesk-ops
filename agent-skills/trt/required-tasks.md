# Required Tasks (TRT)

## Scope

This file covers all pre-treatment tasks that TRT patients must complete before their treatment journey can progress. The agent responds ONCE to the patient's query — no back-and-forth conversation.

**Covers:**
- ID verification (photo ID upload via app)
- Health questionnaire (onboarding flow in the app)
- Blood tests (initial and monitoring — defer to `blood-tests.md` for detail)
- GP consent form (completed via the app)
- Billing / payment details on file
- Task status interpretation (`get_customer_tasks` tool output)
- Task prioritisation when multiple tasks are outstanding

**Does NOT cover:**
- Blood test kit delivery, results, or venous alternatives → see `blood-tests.md`
- Order status and dispatch → see `orders-and-shipping.md`
- Subscription management → see `subscriptions.md`
- Consultation booking or scheduling → see `consultations.md`
- Billing failures or refunds → see `billing-and-payments.md`

**Cross-references:**
- `blood-tests.md` — comprehensive blood test guidance (kit delivery, results, replacements, venous options)
- `orders-and-shipping.md` — order blocked by pending tasks
- `billing-and-payments.md` — payment method issues

---

## Task Status Tool Reference

**ALWAYS call `get_customer_tasks` before responding to any required-task query.**

**Tool output sections:**
1. **Pending tasks** — Tasks the patient must complete before their treatment can progress
2. **Recently completed tasks** — Tasks completed within the last 2 working days; clinicians are still reviewing
3. **Past completed tasks** — Tasks completed more than 2 working days ago; should already be reviewed

**If recently completed tasks exist and the order/treatment has not progressed:**
→ confidence = 0.90 (SOLVE)
→ Reassure the patient that a clinician will review within 1-2 working days
→ NEVER escalate for recently completed tasks

---

## Task Priority Order

When multiple tasks are incomplete, guide the patient through them in this order:

1. **Blood tests** — longest lead time (kit delivery + lab processing)
2. **ID verification** — quick but required before clinical review
3. **Health questionnaire** — can be completed alongside other tasks
4. **GP consent form** — can be completed at any point

Always mention ALL outstanding tasks, but recommend starting with the highest-priority one.

---

## ID Verification

IF patient asks about ID verification OR `get_customer_tasks` shows ID verification as pending:
  → confidence = 0.90 (SOLVE)

**Why it's needed:**
→ "We need to verify your identity to comply with prescription medication regulations and ensure a safe service."

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
  → "International documents not on our standard accepted list require a case-by-case review. I've passed this to our patient care team. They'll be in touch within 24-48 hours."

**How to upload:**
→ "You can upload your ID through the Voy app:
1. Go to the **Home** tab
2. Find the **ID verification** task
3. Upload a clear photo of your ID

Please upload through the task in the app, not through the chat."

---

## Health Questionnaire

IF patient asks about the health questionnaire OR `get_customer_tasks` shows it as pending:
  → confidence = 0.90 (SOLVE)

**Why it's needed:**
→ "The health questionnaire helps our clinical team understand your medical history and assess your suitability for TRT safely."

**How to complete:**
→ "You can complete your health questionnaire through the Voy app:
1. Go to the **Home** tab
2. Find the **Health questionnaire** task
3. Work through each section — it typically takes around 5-10 minutes

The questionnaire is part of your onboarding flow and must be completed before a clinician can review your case."

---

## Blood Tests

IF patient asks about blood test tasks OR `get_customer_tasks` shows blood tests as pending:
  → confidence = 0.90 (SOLVE)
  → Provide a brief overview, then direct to `blood-tests.md` for all detailed guidance

→ "Blood tests are a key part of the TRT pathway. We need to confirm your testosterone levels before treatment can begin. You can find full details on ordering, completing, and returning your blood test in our blood test guidance."

**For any specific blood test questions** (kit delivery, results timelines, venous alternatives, sample issues, biomarkers):
→ Defer entirely to `blood-tests.md` — that file is authoritative for all blood test topics

---

## GP Consent Form

IF patient asks about the GP consent form OR `get_customer_tasks` shows it as pending:
  → confidence = 0.90 (SOLVE)

**Why it's needed:**
→ "The GP consent form allows us to notify your GP about your TRT treatment. This is a clinical safety requirement to ensure your GP is aware of any new medications."

**How to complete:**
→ "You can complete the GP consent form through the Voy app:
1. Go to the **Home** tab
2. Find the **GP consent** task
3. Enter your GP practice details and submit the form

This is a quick step and can be done at any time during your onboarding."

---

## Billing / Payment Details

IF patient asks about payment or billing tasks OR `get_customer_tasks` shows payment details as pending:
  → confidence = 0.90 (SOLVE)

→ "We need valid payment details on file before your treatment can be processed. You can add or update your payment method through the app."

IF payment-related issue is more complex (failed charges, disputes, refunds):
  → Defer to `billing-and-payments.md`

---

## Task Appears Stuck or Already Completed

IF patient says they have already completed a task but `get_customer_tasks` still shows it as pending:

**Step 1: Check recently completed tasks**
→ If the task appears in recently completed: confidence = 0.90 (SOLVE)
→ "I can see your [task name] was recently completed. Our team reviews these within 1-2 working days — no further action is needed from your side."

**Step 2: If task is genuinely stuck (not in recently completed or past completed)**
→ confidence = 0.55 (ESCALATE)
→ "I can see you've mentioned completing this, but it's still showing as outstanding on our side. I've passed this to our patient care team to investigate. They'll be in touch within 24-48 hours."

---

## Multiple Outstanding Tasks

IF `get_customer_tasks` returns multiple pending tasks:
  → confidence = 0.90 (SOLVE)
  → List ALL outstanding tasks
  → Recommend the priority order (blood tests → ID → health questionnaire → GP consent)

**Example response:**
→ "I can see you have a few tasks to complete before your treatment can progress:

1. **Blood test** — this has the longest turnaround, so it's best to start here. You can order your test through the app.
2. **ID verification** — upload a clear photo of your photo ID (passport or driving licence) through the Home tab in the app.
3. **Health questionnaire** — complete this through the app's onboarding flow. It takes around 5-10 minutes.
4. **GP consent form** — enter your GP practice details through the app.

You can work through these in parallel, but I'd recommend starting with the blood test as it takes the longest to process."

---

## Mandatory Rules

1. **ALWAYS call `get_customer_tasks`** before responding to any task-related query — never assume task status
2. **Call `get_order_history`** if the patient mentions orders, shipping, or payment issues alongside tasks
3. **NEVER attempt to complete tasks on behalf of the patient** — all tasks are self-serve via the app
4. **NEVER escalate for recently completed tasks** — reassure the patient that review takes 1-2 working days
5. **For blood test detail**, always defer to `blood-tests.md` — this file only provides the high-level overview
6. **One response only** — provide all relevant guidance in a single reply; do not plan for follow-up messages

---

## Internal Note Guidance

When writing the internal note, include:
- Which tasks are pending, recently completed, and past completed (from `get_customer_tasks` output)
- What guidance was provided to the patient
- If escalated: why the task appears stuck and what the team should investigate

---

## Worked Examples

**Example 1: Single outstanding task — ID verification**
Patient: "What do I need to do to get started with my treatment?"
→ Call `get_customer_tasks` → shows ID verification as pending
→ confidence = 0.90 (SOLVE)
→ "To get started, you need to complete your ID verification. You can upload a clear photo of your passport or driving licence through the Home tab in the Voy app. Once uploaded, our clinical team will review it within 1-2 working days."

**Example 2: Multiple outstanding tasks**
Patient: "I just signed up — what are my next steps?"
→ Call `get_customer_tasks` → shows blood test, ID verification, health questionnaire, and GP consent as pending
→ confidence = 0.90 (SOLVE)
→ List all tasks in priority order (blood tests first), explain how to complete each one via the app

**Example 3: Patient says task is done but system disagrees**
Patient: "I uploaded my ID last week but nothing has happened."
→ Call `get_customer_tasks` → ID verification still shows as pending, not in recently completed
→ confidence = 0.55 (ESCALATE)
→ "I can see your ID verification is still showing as outstanding on our side, even though you've mentioned uploading it. I've passed this to our patient care team to investigate. They'll be in touch within 24-48 hours."

**Example 4: Recently completed task — patient anxious about progress**
Patient: "I did my health questionnaire yesterday — when will I hear back?"
→ Call `get_customer_tasks` → health questionnaire shows in recently completed
→ confidence = 0.90 (SOLVE)
→ "I can see your health questionnaire was recently completed — great work! Our clinical team reviews these within 1-2 working days, so you should see progress shortly. No further action is needed from your side."

**Example 5: Blood test task outstanding**
Patient: "It says I need a blood test — what do I do?"
→ Call `get_customer_tasks` → blood test shows as pending
→ confidence = 0.90 (SOLVE)
→ "Blood tests are a key step in the TRT pathway — they help confirm your testosterone levels before treatment can begin. You can order your blood test through the Voy app. For full details on test options, how to complete the test, and what to expect, take a look at the blood test section in your app. If you have any specific questions about the blood test process, feel free to reach out."
