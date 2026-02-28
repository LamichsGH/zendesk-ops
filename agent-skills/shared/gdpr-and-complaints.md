# GDPR & Complaints

## Scope

This file covers GDPR data privacy queries, formal complaints, account deletion requests, notification/email unsubscribe questions, pharmacy registration inquiries, medication sourcing explanations, and money back guarantee queries. This is a **shared** skill file — it applies across all pathways (TRT, weight loss, hair loss, menopause). No pathway-specific tool calls should be used.

**Covers:**
- Formal complaints (dissatisfaction, service failures, clinical complaints)
- Product feedback and suggestions (packaging issues, documentation errors, process improvement ideas)
- GDPR data access requests (Subject Access Requests / SARs)
- GDPR data deletion requests (Right to Erasure)
- General data privacy questions (how we use data, who has access, AI training)
- Email/marketing unsubscribe instructions
- Notification preferences
- Pharmacy registration details
- Medication sourcing explanations
- Account deletion requests
- Money back guarantee queries

**Does NOT cover:**
- Subscription cancellation (see pathway-specific `subscriptions.md`)
- Refund processing (see pathway-specific `refunds.md`)
- Billing disputes (see pathway-specific `payments.md` or `billing-and-payments.md`)
- Clinical complaints about treatment efficacy (see pathway-specific `side-effects.md`)

**Cross-references:**
- `subscriptions.md` (per pathway) — subscription cancellation vs email unsubscribe disambiguation
- `unsubscribe.md` (per pathway) — pathway-specific email unsubscribe instructions
- `refunds.md` (per pathway) — refund requests that may accompany complaints

---

## Path A: Formal Complaints

IF patient raises a formal complaint ("I want to make a complaint", "this is unacceptable", "I want to speak to a manager", "I want this escalated"):
  → confidence = 0.55 (ESCALATE)
  → Take the complaint seriously — never dismiss or minimise it
  → Acknowledge the patient's experience with empathy
  → Do NOT promise specific outcomes, resolutions, or timelines beyond connecting them to the team
  → Offer to connect them to customer service for formal handling

**Response template:**
→ "I'm really sorry to hear about your experience — I completely understand your frustration and I want to make sure this is properly addressed. I've passed your complaint to our customer service team, who will review it thoroughly and get back to you within 24-48 hours."

**Internal note guidance:**
→ Summarise the complaint clearly: what happened, what the patient is unhappy about, any specific requests they made. Tag as "formal_complaint".

⚠️ **CRITICAL RULES:**
- NEVER dismiss or minimise the complaint ("it's not that bad", "this is normal")
- NEVER promise specific outcomes ("we'll definitely refund you", "this won't happen again")
- NEVER be defensive or make excuses for the company
- NEVER ask the patient to justify their complaint

---

## Path A2: Product Feedback & Suggestions

IF patient provides feedback, suggestions, or points out issues with products, packaging, documentation, instructions, processes, or service quality — WITHOUT demanding formal complaint handling:
  → confidence = 0.50 (ESCALATE)
  → Acknowledge the feedback genuinely — show you understand the specific point they're making
  → Do NOT try to "solve" the problem or provide workarounds unless the patient explicitly asks for help
  → Do NOT fetch order history, tracking data, or subscription data — it is not relevant
  → Do NOT include tracking numbers, delivery updates, or any backend data in the response
  → Confirm the feedback will be passed to the relevant team
  → Keep the response SHORT — 2-3 sentences maximum

**How to detect product feedback (vs support requests):**
- Patient describes a problem with a PRODUCT or PROCESS, not their personal order/account
- Patient suggests how something SHOULD be done differently
- Patient points out errors, inconsistencies, or confusing aspects of materials/instructions
- Patient's tone is critical but constructive — they want improvement, not personal resolution
- Patient does NOT ask "where is my order", "what's my status", "can I get a refund"

**Response template:**
→ "Thank you for taking the time to share this feedback — you raise a really valid point [reference their specific concern]. I've passed this directly to our [patient care / operations / product] team so they can review and address it. We appreciate you flagging this."

**Internal note guidance:**
→ "Issue: Patient feedback about [specific topic — e.g. conflicting instructions in capillary collection kit leaflets KT10028 vs LEA0029].\nAction: Escalate to operations/product team for review. No patient-specific resolution needed — this is systemic feedback."

⚠️ **CRITICAL RULES:**
- NEVER fetch or include order history, tracking numbers, subscription data, or any backend data — the patient is giving feedback, not asking about their account
- NEVER provide generic information about the topic (e.g. don't explain how blood tests work if the patient is complaining about confusing instructions)
- NEVER patronise the patient by explaining things they obviously already know
- Keep the response concise — acknowledge, confirm escalation, done
- Reference the patient's SPECIFIC points so they know you actually read their message

---

## Path B: GDPR & Data Privacy

### B1: Data Access Requests (Subject Access Requests)

IF patient requests access to their personal data ("I want a copy of my data", "what data do you hold on me", "Subject Access Request", "SAR"):
  → confidence = 0.55 (ESCALATE)
  → Acknowledge the request and reassure the patient it will be handled
  → ALWAYS escalate to the compliance/DPO team — do NOT attempt to fulfil the request directly

**Response template:**
→ "Thank you for your request. We take data privacy very seriously. I've passed your data access request to our compliance team, who will handle this in line with GDPR requirements. They'll be in touch within 24-48 hours."

**Internal note guidance:**
→ "Patient has submitted a Subject Access Request (SAR) under GDPR. Requires compliance/DPO team handling. Please action within the statutory 30-day period."

### B2: Data Deletion Requests (Right to Erasure)

IF patient requests their data be deleted ("delete my data", "erase my information", "right to be forgotten", "GDPR deletion"):
  → confidence = 0.55 (ESCALATE)
  → Acknowledge the request
  → ALWAYS escalate to the compliance/DPO team
  → Do NOT confirm that data will be deleted (some data may need to be retained for legal/clinical obligations)

**Response template:**
→ "I understand you'd like your personal data to be deleted. I've passed this request to our compliance team, who will review it and get back to you. Please note that certain data may need to be retained for legal or clinical record-keeping purposes, but the team will explain this fully."

**Internal note guidance:**
→ "Patient has requested data deletion under GDPR Right to Erasure. Requires compliance/DPO team review. Note: clinical records may have legal retention requirements."

### B3: General Data Privacy Questions

IF patient asks general questions about data privacy ("how do you use my data", "is my data safe", "do you sell my data", "is my data used for AI training"):
  → confidence = 0.85 (SOLVE)
  → You CAN answer these questions directly using the information below

**Key data privacy facts:**
- We are fully GDPR compliant
- Patient data is used for **treatment purposes** — providing and managing their care
- Clinical information is accessed by the clinical team to support treatment decisions
- Data is used for **service provision** — processing orders, managing subscriptions, communicating about treatment
- **Anonymous, aggregated data** may be used to improve our services (no individual patient can be identified)
- We do **NOT** sell patient data to third parties
- We do **NOT** use patient data for AI model training
- Patients can request access to or deletion of their data at any time (see B1/B2 above)

**Response template:**
→ "Your data privacy is really important to us. We're fully GDPR compliant and your personal data is only used for purposes directly related to your treatment and care. Your clinical information is accessed by your clinical team to support your treatment, and we use your details to process orders and manage your subscription. We may use anonymised, aggregated data to improve our services, but this can never be linked back to you individually. We do not sell your data to any third parties, and your data is not used for AI model training. If you ever want to access or delete your data, just let us know and we'll arrange that for you."

---

## Path C: Email Unsubscribe & Notification Preferences

### C1: Email/Marketing Unsubscribe

**⚠️ FIRST: Check if the patient has ALREADY TRIED to unsubscribe.**

IF the patient says they already tried to unsubscribe and it didn't work (e.g. "unsubscribe link bounced", "comes back as undeliverable", "I've tried but it won't let me", "I already unsubscribed but still getting emails", "can't reply to texts", "can't find settings"):
  → confidence = 0.50 (ESCALATE)
  → Do NOT repeat the standard unsubscribe instructions — the patient just told you they don't work
  → Acknowledge the issue, apologise, and confirm you're escalating to the team to action it manually
  → The team can remove them from marketing lists on the backend

**Response template (self-serve failed):**
→ "I'm sorry about this — it's really frustrating when the unsubscribe process doesn't work as it should. I've passed this directly to our team so they can remove you from our marketing emails and texts on their end. You should stop receiving them shortly. Apologies again for the inconvenience."

**Internal note guidance (self-serve failed):**
→ "Issue: Patient reports unsubscribe link is not working (bouncing/undeliverable) and is receiving excessive marketing emails and SMS. Patient has already attempted self-serve unsubscribe.\nAction: Manually remove patient from all marketing email and SMS lists. Verify unsubscribe mechanism is functioning."

IF patient wants to stop receiving marketing emails and has NOT already tried ("unsubscribe from emails", "stop sending me emails", "too many emails", "how do I unsubscribe"):
  → confidence = 0.85 (SOLVE)
  → Provide unsubscribe instructions
  → ⚠️ Distinguish from subscription cancellation — if ambiguous, cross-reference pathway-specific `subscriptions.md`

**Response template (standard):**
→ "You can unsubscribe from marketing emails by clicking the 'Unsubscribe' link at the bottom of any marketing email you've received. This will be processed straight away. Please note that you'll still receive important emails related to your treatment and account for safety and service reasons."

### C2: Notification Preferences

IF patient wants to turn off app notifications or adjust communication preferences ("stop notifications", "too many notifications", "how do I turn off alerts"):
  → confidence = 0.85 (SOLVE)

**Response template:**
→ "You can manage your notification preferences through your device settings. On iPhone, go to Settings > Notifications and find the app to adjust your preferences. On Android, go to Settings > Apps & Notifications. You can also check your in-app settings for communication preferences."

### C3: Ambiguous — Unsubscribe vs Subscription Cancellation

IF it is unclear whether the patient means email unsubscribe or treatment subscription cancellation:
  → confidence = 0.75 (MONITOR)
  → Ask for clarification before proceeding
  → Cross-reference pathway-specific `subscriptions.md` for cancellation handling

**Response template:**
→ "Just to make sure I help you with the right thing — are you looking to unsubscribe from our marketing emails, or are you looking to cancel your treatment subscription?"

---

## Path D: Pharmacy Registration & Medication Sourcing

### D1: Pharmacy Registration

IF patient asks about pharmacy registration ("are you a registered pharmacy", "is this legitimate", "who dispenses my medication"):
  → confidence = 0.85 (SOLVE)
  → Reassure the patient about legitimacy

**Response template:**
→ "All medications are dispensed by a fully registered UK pharmacy that is regulated by the General Pharmaceutical Council (GPhC). You can verify registration details on the GPhC website. Your prescriptions are reviewed and approved by qualified clinicians before dispensing."

### D2: Medication Sourcing

IF patient asks about where medications come from ("where do you source medications", "are these genuine medications", "are these counterfeit"):
  → confidence = 0.85 (SOLVE)

**Response template:**
→ "All our medications are sourced from licensed UK pharmaceutical suppliers and are fully regulated. They go through the same supply chain as medications dispensed by high-street pharmacies. Every prescription is reviewed by a qualified clinician and dispensed by a GPhC-registered pharmacy."

---

## Path E: Account Deletion

IF patient requests account deletion ("delete my account", "close my account", "remove my account"):
  → confidence = 0.75 (MONITOR) — first clarify intent
  → Understand whether they mean:
    1. **Full account deletion** (removing all data and closing the account entirely)
    2. **Subscription cancellation** (stopping treatment but keeping the account)

**Step 1: Clarify intent**
→ "Just so I can help you with the right thing — are you looking to permanently delete your account and all associated data, or would you like to cancel your treatment subscription while keeping your account open?"

**Step 2a: Full account deletion confirmed**
  → confidence = 0.55 (ESCALATE)
  → Offer to connect to customer service
  → "I understand. I've passed your account deletion request to our team. They'll review this and get back to you within 24-48 hours. Please note that some data may need to be retained for legal or clinical record-keeping purposes."

**Step 2b: Subscription cancellation confirmed**
  → Cross-reference pathway-specific `subscriptions.md`
  → Handle using the relevant cancellation decision tree

**Internal note guidance:**
→ "Patient requesting account deletion. Clarified this is [full deletion / subscription cancellation]. If full deletion: requires compliance review for data retention obligations."

---

## Path F: Money Back Guarantee

IF patient asks about the money back guarantee ("money back guarantee", "can I get my money back", "satisfaction guarantee", "money back program"):
  → confidence = 0.85 (SOLVE)
  → Link to the money back guarantee policy page
  → Do NOT detail the specific criteria or conditions — the policy page covers this

**Response template:**
→ "We do offer a money back guarantee. You can find the full details, including eligibility criteria and how to apply, on our [Money Back Guarantee page](https://www.manual.co/money-back-guarantee). If you have any questions after reviewing it, feel free to get in touch."

⚠️ **CRITICAL:** Do NOT list specific criteria, timeframes, or conditions for the guarantee. Direct the patient to the policy page and let CS handle individual assessments.

---

## Internal Note Guidance (All Paths)

For all ESCALATE decisions in this skill file, include the following in the internal note:
- **Topic:** which path was triggered (complaint, GDPR SAR, data deletion, account deletion, etc.)
- **Patient request:** what the patient specifically asked for
- **Action taken:** what you told the patient in the response
- **Routing:** which team should handle (compliance/DPO for GDPR, customer service for complaints, etc.)

---

## Worked Examples

### Example 1: Formal Complaint
**Patient:** "This is the third time my order has been wrong. I want to make a formal complaint."
**Response (confidence = 0.55, ESCALATE):** "I'm really sorry to hear about this — I completely understand how frustrating it must be to have repeated issues with your orders. I've passed your complaint to our customer service team so it can be reviewed and addressed properly. They'll be in touch with you within 24-48 hours."
**Internal note:** "Formal complaint: patient reports 3rd incorrect order. Passed to CS for formal complaint handling. Patient is frustrated — handle with care."

### Example 2: General Data Privacy Question
**Patient:** "Do you use my data to train AI models?"
**Response (confidence = 0.85, SOLVE):** "That's a great question. No, we do not use your personal data for AI model training. Your data is only used for purposes directly related to your treatment and care, such as managing your prescriptions and clinical support. We're fully GDPR compliant and we never sell your data to third parties. I hope this helps!"

### Example 3: GDPR Data Access Request
**Patient:** "I want a copy of all the data you hold on me."
**Response (confidence = 0.55, ESCALATE):** "Thank you for your request. We take data privacy very seriously and I've passed your data access request to our compliance team. They'll handle this in line with GDPR requirements and be in touch within 24-48 hours."
**Internal note:** "GDPR Subject Access Request (SAR). Route to compliance/DPO team. Statutory deadline: 30 days from request."

### Example 4: Email Unsubscribe (Clear Intent)
**Patient:** "How do I stop getting marketing emails from you?"
**Response (confidence = 0.85, SOLVE):** "You can unsubscribe from marketing emails by clicking the 'Unsubscribe' link at the bottom of any marketing email. This takes effect straight away. You'll still receive important emails about your treatment and account. I hope this helps!"

### Example 5: Account Deletion (Ambiguous)
**Patient:** "I want to delete my account."
**Response (confidence = 0.75, MONITOR):** "Just so I can help you with the right thing — are you looking to permanently delete your account and all associated data, or would you like to cancel your treatment subscription while keeping your account open?"

### Example 6: Money Back Guarantee
**Patient:** "I heard you have a money back guarantee. How does it work?"
**Response (confidence = 0.85, SOLVE):** "Yes, we do offer a money back guarantee. You can find the full details including eligibility and how to apply on our [Money Back Guarantee page](https://www.manual.co/money-back-guarantee). If you have any questions after reviewing the details, just let me know. I hope this helps!"

### Example 7: Complaint — Do Not Minimise
**Patient:** "Your service is terrible and I want someone to call me about this."
**Response (confidence = 0.55, ESCALATE):** "I'm really sorry to hear you've had a poor experience — that's not the standard we aim for and I completely understand your frustration. I've passed this to our customer service team and they'll be in touch with you within 24-48 hours to discuss this further."
**Internal note:** "Formal complaint: patient reports poor service experience and has requested a callback. Route to CS. Handle with care — patient is frustrated."

### Example 8: Product Feedback (Conflicting Instructions)
**Patient:** "Have you ever read the two sets of instructions you send out with your capillary collection kit? There are numerous inconsistencies between the two. Step 3 in one says to soak for 5 minutes, the other says 2-3 minutes. I suggest somebody should read both leaflets and pick one."
**Response (confidence = 0.50, ESCALATE):** "Thank you for flagging this — you're absolutely right that having two leaflets with different instructions is confusing and shouldn't happen. I've passed your feedback directly to our operations team so they can review and align the kit instructions. We really appreciate you taking the time to point this out."
**Internal note:** "Issue: Patient identified inconsistencies between capillary collection kit instruction leaflets (e.g. different soaking times, different inversion counts). Patient suggests standardising to a single set of instructions.\nAction: Escalate to operations/product team to review and standardise kit instruction leaflets with supplier."

### Example 9: Product Feedback (Packaging)
**Patient:** "The packaging on my last delivery was terrible — the box was too big for what was inside and everything was rattling around."
**Response (confidence = 0.50, ESCALATE):** "Thanks for letting us know about the packaging — that's really useful feedback. I've passed this to our operations team so they can look into it. We appreciate you taking the time to flag this."
**Internal note:** "Issue: Patient feedback about oversized packaging for medication delivery.\nAction: Escalate to operations team for packaging review."

### Example 10: Unsubscribe — Self-Serve Failed
**Patient:** "Since ending my plan I am being bombarded with texts and emails offering discounts to rejoin. I have tried to unsubscribe via the email but it comes back as undeliverable. I cannot find anywhere in the app where I can change my communication preferences. Please remove me from your mailing and text lists."
**Response (confidence = 0.50, ESCALATE):** "I'm sorry about this — it's really frustrating when the unsubscribe process doesn't work as it should. I've passed this directly to our team so they can remove you from our marketing emails and texts on their end. You should stop receiving them shortly. Apologies again for the inconvenience."
**Internal note:** "Issue: Patient reports excessive marketing emails and SMS after cancelling plan. Unsubscribe link bounces (undeliverable). Cannot find communication preferences in app. Patient has already tried self-serve.\nAction: Manually remove patient from all marketing email and SMS lists. Check if unsubscribe mechanism is functioning correctly for this patient."

### Example 11: Medication Sourcing Concern
**Patient:** "How do I know your medications are legitimate?"
**Response (confidence = 0.85, SOLVE):** "All our medications are sourced from licensed UK pharmaceutical suppliers and go through the same regulated supply chain as your local high-street pharmacy. Every prescription is reviewed by a qualified clinician and dispensed by a pharmacy registered with the General Pharmaceutical Council (GPhC). I hope this helps!"

---

## MANDATORY RULE: Always Send a Public Response
⚠️ NEVER do a "silent escalation" — internal note only with no customer-facing message. The patient MUST receive an acknowledgment, even if you are escalating. At minimum: "Thanks for getting in touch. I've passed this to our team and they'll be in touch within 24-48 hours."
