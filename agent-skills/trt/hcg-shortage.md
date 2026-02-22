# HCG Shortage

## Scope
This file covers patient queries about the global HCG supply shortage affecting the UK. Use when patient asks about HCG availability, why their HCG order is delayed or unavailable, whether they can get HCG, or about Clomid as an alternative.

**Covers:**
- HCG supply shortage status and explanation
- Why HCG is unavailable or delayed
- Clomid as an alternative to HCG
- Patient eligibility pathway (active vs non-active)
- Safety contraindications for Clomid
- Patients wanting to source HCG independently
- Patients trying to conceive while on HCG
- Restock timeline questions

**Does NOT cover:**
- General subscription management (see subscriptions.md)
- Dose changes unrelated to shortage (see clinical-routing.md)
- Side effects from current TRT (see side-effects.md)
- Billing or refund queries (see billing-and-payments.md, refunds.md)

**Cross-references:**
- subscriptions.md — subscription status, plan details
- clinical-routing.md — clinical questions, medication changes
- side-effects.md — side effect reporting and triage

---

## CRITICAL SAFETY — Clomid Contraindications
Any mention of these conditions when discussing Clomid → confidence = 0.35, send_response = false:
- History of retinal vein occlusion or visual disturbances
- History of blood clots (DVT, PE, thrombosis)
- Significant mental illness history (severe depression, psychosis)
- safety_flags: ["clomid_contraindication", "clinical_review_required"]

→ Internal note: "Patient has reported [condition] which is a contraindication for Clomid. Requires clinical review before any medication change."
→ Do NOT send a patient-facing response — clinical team must review first.

---

## Fertility Escalation
IF patient mentions they are trying to conceive or asks about HCG and fertility:
  → confidence = 0.55 (ESCALATE to CS)
  → "I understand fertility is an important concern. I've passed this to our patient care team so they can discuss your options with you in detail."
  → Do NOT provide fertility guidance
  → Include in internal_note: "Patient is trying to conceive / has fertility concerns related to HCG shortage."

---

## Decision Logic

### Step 1: Determine Patient Status

Call `get_subscription_status` to check if the patient has an active subscription.

- **Active subscription** → Path B (Active Patient)
- **No active subscription / expired / cancelled** → Path A (Non-Active Patient)

If subscription status is ambiguous, also call `get_order_history` to check for recent HCG orders.

---

### Step 2: Check for Safety Flags

Before proceeding with either path, check if the patient has mentioned any Clomid contraindications (see CRITICAL SAFETY section above). If yes → confidence = 0.35, send_response = false. Stop here.

Also check if the patient mentions trying to conceive → see Fertility Escalation above.

---

### Step 3: Follow the Appropriate Path

#### Path A — Non-Active Patient (No Active Subscription)

IF patient is asking about HCG availability or starting HCG treatment:
  → confidence = 0.85 (SOLVE)
  → Explain the shortage clearly and offer two options:
    1. Wait for supply to be restored (they will be notified by email automatically)
    2. Learn about Clomid as a potential alternative

  → Response should include:
    - "There is currently a global supply shortage of HCG affecting the UK. Unfortunately, we're unable to supply HCG at this time."
    - "You have two options: you can wait for supply to be restored — we'll notify you by email as soon as it's available again — or you may want to consider Clomid, which is the closest alternative available. I've included some information about Clomid below."
    - Always proactively include the Clomid information (see Clomid Information Block) so the patient has everything in one response

IF patient then asks about Clomid:
  → confidence = 0.85 (SOLVE with info)
  → Provide Clomid information (see Clomid Information Block below)
  → Always make clear: "Whether Clomid is suitable for you would need to be assessed by one of our clinicians."
  → Do NOT prescribe or confirm eligibility — always route to clinician for eligibility assessment

IF patient wants to proceed with Clomid after learning about it:
  → confidence = 0.55 (ESCALATE to clinical team)
  → "I've passed this to our clinical team so they can assess whether Clomid is suitable for you and discuss next steps."

---

#### Path B — Active Patient (Active Subscription Including HCG)

IF patient is asking about their HCG supply, order, or availability:
  → confidence = 0.55 (ESCALATE to clinical team)
  → Explain the shortage but do NOT suggest alternatives
  → "There is currently a global supply shortage of HCG affecting the UK. Unfortunately, we're unable to supply HCG at this time. I've passed this to your clinical team so they can review your treatment plan and discuss the best options with you."
  → Do NOT mention Clomid proactively — the clinician will decide what to recommend
  → Include in internal_note: "Active patient affected by HCG shortage. Clinical review needed to assess treatment alternatives."

IF active patient specifically asks about Clomid or alternatives:
  → confidence = 0.55 (ESCALATE to clinical team)
  → You may briefly explain what Clomid is (see Clomid Information Block below) but emphasise clinical review is required
  → "Clomid is one option that your clinical team may consider. I've flagged this with them so they can review your treatment plan and advise on the best approach for you."
  → Do NOT tell active patients that Clomid is their replacement — the clinician decides

---

## Clomid Information Block

Use this when explaining Clomid to patients (both paths, where appropriate):

> Clomid (clomiphene citrate) is an oral medication that is well-established and evidence-based. It works by signalling the brain to stimulate the body's own production of testosterone and sperm. While it is not a direct replacement for HCG, it is the closest equivalent available for supporting testicular function and sperm production.
>
> It is worth noting that Clomid is used off-label for this purpose — this means it is not specifically licensed for this use, but doctors may prescribe it based on clinical evidence. Thousands of patients are currently taking Clomid with no to minor side effects, and most patients report little to no difference in symptom control compared to HCG.
>
> For full transparency, the evidence does suggest that Clomid may be less effective than HCG for testicular function over the long term, which is why it is not typically used as a first-line option. However, given the current supply situation, it is a well-supported alternative.

---

## Patient Wants to Source HCG Independently

IF patient says they want to buy HCG from another supplier or source it themselves:
  → confidence = 0.85 (SOLVE)
  → "HCG is a regulated prescription medicine. Any new supplier or brand requires regulatory approval before it can be prescribed. We're unable to prescribe HCG from unverified sources. We'll notify you by email as soon as our approved supply is restored."
  → Do NOT recommend any external suppliers
  → Do NOT tell the patient how to obtain HCG outside the service

---

## Restock / Timeline Questions

IF patient asks when HCG will be back in stock or asks for a date:
  → confidence = 0.85 (SOLVE)
  → "Unfortunately, we don't have a confirmed date for when HCG supply will be restored. This is a global shortage and we're monitoring the situation closely. You'll be notified by email as soon as it becomes available again."
  → NEVER promise a specific restock date
  → NEVER offer to set a reminder or personally notify them — the email notification is automatic

---

## Mandatory Rules

1. **NEVER promise a restock date** — the shortage is global and unpredictable.
2. **NEVER tell the patient you will remind or notify them** — they will receive an automatic email when supply is restored.
3. **NEVER suggest Clomid proactively to active patients** — their clinician must review and decide.
4. **NEVER prescribe or confirm Clomid eligibility** — always route to clinician.
5. **NEVER recommend external HCG suppliers** — HCG is a regulated prescription medicine.
6. **Always check for Clomid contraindications** before discussing Clomid (retinal vein occlusion, blood clots, significant mental illness history).
7. **Always send a public response** — never do a silent escalation with only an internal note.

---

## Worked Examples

### Example 1: Non-Active Patient Asking About HCG
**Patient:** "Hi, I was looking into starting HCG treatment. Is it available?"
**Action:** Call `get_subscription_status` — no active subscription found.
**Response (confidence = 0.85, SOLVE):**
"Hi [First Name],

Thank you for getting in touch. Unfortunately, there is currently a global supply shortage of HCG affecting the UK, and we're unable to supply it at this time.

You have two options: you can wait for supply to be restored — we'll notify you by email as soon as it's available again — or you may want to consider Clomid, which is the closest alternative available.

Clomid is an oral medication that works by indirectly signalling your brain to produce more testosterone and sperm. It's a well-established, evidence-based treatment used by thousands of patients in our clinic. While it's not a direct replacement for HCG, it offers the closest equivalent outcome available in the UK for both testicular function and sperm production. Whether Clomid is suitable for you would need to be assessed by one of our clinicians.

If you'd like to explore Clomid as an option, or if you have any other questions, just reply to this email and we'll be happy to help.

Best,
[Brand Name]"

### Example 2: Active Patient Asking About Their HCG Order
**Patient:** "My HCG hasn't arrived and I can't see it in the order form. What's going on?"
**Action:** Call `get_subscription_status` — active subscription. Call `get_order_history` — confirms HCG was part of treatment.
**Response (confidence = 0.55, ESCALATE):**
"Hi [First Name],

Thank you for reaching out. There is currently a global supply shortage of HCG affecting the UK, which is why it's not available to order at the moment. I understand this is frustrating.

I've passed this to your clinical team so they can review your treatment plan and discuss the best options with you. We're currently experiencing high demand, so please allow 24-48 hours for a response.

Best,
[Brand Name]"
**Internal note:** "Active patient affected by HCG shortage. HCG confirmed in treatment history. Clinical review needed to assess treatment alternatives."

### Example 3: Non-Active Patient Interested in Clomid
**Patient:** "You mentioned Clomid — can you tell me more about it? And can I start it?"
**Action:** Check for safety contraindications in patient message — none found.
**Response (confidence = 0.85, SOLVE with info):**
"Hi [First Name],

Of course — happy to share some more detail.

Clomid (clomiphene citrate) is an oral medication that is well-established and evidence-based. It works by signalling the brain to stimulate the body's own production of testosterone and sperm. While it is not a direct replacement for HCG, it is the closest equivalent available for supporting testicular function and sperm production.

It is used off-label for this purpose, which means it isn't specifically licensed for this use but doctors may prescribe it based on clinical evidence. Thousands of patients are currently taking Clomid with no to minor side effects, and most report little to no difference in symptom control compared to HCG. For full transparency, the evidence does suggest Clomid may be less effective than HCG for testicular function over the long term, which is why it isn't typically a first-line option — but given the current supply situation, it is a well-supported alternative.

Whether Clomid is suitable for you would need to be assessed by one of our clinicians. If you'd like to explore this option, just let me know and I'll pass it to our clinical team.

Best,
[Brand Name]"

### Example 4: Patient With Contraindication Mentions Clomid
**Patient:** "I have a history of blood clots. Can I switch to Clomid?"
**Action:** Safety flag triggered — blood clot history is a Clomid contraindication.
**Response: send_response = false (confidence = 0.35)**
**Internal note:** "Patient has reported a history of blood clots, which is a contraindication for Clomid. Requires clinical review before any medication change. Do not suggest Clomid to this patient without clinician clearance."
**safety_flags:** ["clomid_contraindication", "clinical_review_required"]

### Example 5: Active Patient Asks Specifically About Clomid
**Patient:** "I've heard Clomid can replace HCG. Can I switch?"
**Action:** Call `get_subscription_status` — active subscription confirmed.
**Response (confidence = 0.55, ESCALATE):**
"Hi [First Name],

Thanks for reaching out. Clomid is one option that your clinical team may consider — it's an oral medication that works by stimulating the body's own testosterone and sperm production. However, whether it's the right option for you needs to be assessed by your clinician.

I've flagged this with your clinical team so they can review your treatment plan and advise on the best approach. We're currently experiencing high demand, so please allow 24-48 hours for a response.

Best,
[Brand Name]"
**Internal note:** "Active patient requesting switch from HCG to Clomid due to shortage. Clinical review needed."

### Example 6: Patient Wants to Source HCG Elsewhere
**Patient:** "Can I just buy HCG from another pharmacy and you prescribe it?"
**Response (confidence = 0.85, SOLVE):**
"Hi [First Name],

I understand the frustration with the current shortage. However, HCG is a regulated prescription medicine, and any new supplier or brand requires regulatory approval before it can be prescribed. We're unable to prescribe HCG from unverified sources.

We're monitoring the supply situation closely and you'll be notified by email as soon as our approved supply is restored.

Best,
[Brand Name]"

### Example 7: Patient Trying to Conceive
**Patient:** "We're trying for a baby and I was relying on HCG for fertility. What do I do now?"
**Response (confidence = 0.55, ESCALATE):**
"Hi [First Name],

I completely understand how important this is. Fertility is something our clinical team will want to discuss with you directly to make sure you're supported with the right plan.

I've passed this to our patient care team so they can discuss your options with you in detail. We're currently experiencing high demand, so please allow 24-48 hours for a response.

Best,
[Brand Name]"
**Internal note:** "Patient is trying to conceive and has fertility concerns related to HCG shortage. Needs clinical review for appropriate fertility support."

### Example 8: Patient Asks for Restock Date
**Patient:** "When will HCG be available again?"
**Response (confidence = 0.85, SOLVE):**
"Hi [First Name],

Unfortunately, we don't have a confirmed date for when HCG supply will be restored — this is a global shortage affecting suppliers across the UK. We're monitoring the situation closely, and you'll be notified by email as soon as it becomes available again.

Best,
[Brand Name]"

---

## MANDATORY RULE: Always Send a Public Response
NEVER do a "silent escalation" — internal note only with no customer-facing message. The patient MUST receive an acknowledgment, even if you are escalating. The only exception is when send_response = false due to safety flags (confidence = 0.35), in which case the clinical team handles the response.
