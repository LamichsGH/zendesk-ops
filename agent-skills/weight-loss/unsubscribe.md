# Unsubscribe & Marketing Communications

## Scope

**What this covers:**
- Unsubscribing from marketing emails and promotional communications
- Opting out of newsletters and offers
- Complaints about too many emails or "spam"

**What this does NOT cover:**
- Subscription cancellation (cancel plan/treatment) → see `subscriptions.md`
- Account deletion or data erasure requests → ESCALATE (GDPR SAR)
- Clinical or transactional email opt-outs → these cannot be stopped (explain why)

**Cross-references:**
- `subscriptions.md` — for patients who say "unsubscribe" but actually mean cancel their treatment plan

---

## Crucial Principles

1. **Marketing unsubscribe is a SOLVE.** The patient can do this themselves — always provide the solution directly.
2. **Never escalate simple unsubscribe requests**, even if the patient is frustrated.
3. **Distinguish unsubscribe from cancellation.** "Unsubscribe" often means marketing emails. If the patient also mentions their plan, medication, or subscription, the request may span both topics — handle unsubscribe and redirect to `subscriptions.md` for the rest.
4. **Clinical and account emails continue.** Patients cannot opt out of clinical communications, order updates, or safety notifications — these are required for patient safety.

---

## Decision Tree

### Step 1: Classify the request

Read the patient's actual message carefully. Classify into one of these categories:

**A) Pure marketing unsubscribe** — Patient wants to stop receiving marketing/promotional emails only
→ Go to Path A

**B) Frustrated marketing unsubscribe** — Patient is annoyed/frustrated about marketing emails ("spam", "stop emailing me", "too many emails")
→ Go to Path B

**C) Mixed intent** — Patient mentions unsubscribing AND cancelling/stopping their plan or subscription
→ Go to Path C

**D) Account deletion / data erasure** — Patient wants their account deleted, data removed, or mentions GDPR rights
→ Go to Path D

**E) Clinical email opt-out** — Patient wants to stop receiving clinical, order, or transactional emails
→ Go to Path E

---

### Path A: Pure Marketing Unsubscribe

→ confidence = 0.95 (SOLVE)

**KB Search:** Search Vector Store for unsubscribe/marketing communications articles. Include relevant article link.

"You can unsubscribe from our marketing emails by clicking the 'Unsubscribe' link at the bottom of any promotional email from us. This takes effect immediately.

Please note that you'll still receive important emails related to your treatment, orders, and account — these are necessary for your care and can't be turned off."

⚠️ Do NOT say "I've passed this to our team" — just provide the solution.
→ Close with: "I hope this helps!"

### Path B: Frustrated Marketing Unsubscribe

→ confidence = 0.90 (SOLVE)

**KB Search:** Search Vector Store for unsubscribe/marketing communications articles. Include relevant article link.

Lead with empathy, then solve directly:

"I'm sorry about the number of emails — I completely understand that can be frustrating. You can unsubscribe from our marketing emails straight away by clicking the 'Unsubscribe' link at the bottom of any promotional email from us. This takes effect immediately.

You'll still receive important emails about your treatment and orders, as these are necessary for your care, but you won't receive any further marketing communications."

⚠️ Do NOT escalate just because the patient is frustrated. Solve it directly.
→ Close with: "I hope this helps!"

### Path C: Mixed Intent (Unsubscribe + Cancellation)

→ confidence = 0.75 (MONITOR)
→ awaiting_customer_input = true

Handle the unsubscribe part, then ask about the cancellation:

"I can help with the marketing emails straight away — just click the 'Unsubscribe' link at the bottom of any promotional email from us, and you'll stop receiving them immediately.

Regarding cancelling your subscription — could you confirm if you'd like to cancel your entire treatment plan, or is it just the marketing emails you'd like to stop? If you'd like to cancel your plan, I can guide you through the options."

⚠️ Do NOT auto-escalate. MONITOR and wait for the patient to clarify.

### Path D: Account Deletion / Data Erasure (GDPR SAR)

→ confidence = 0.50 (ESCALATE)
→ "I understand you'd like your account and data removed. This is a data protection request that our team needs to handle. I've passed this to our patient care team, and they'll be in touch within 24–48 hours to process this for you."

⚠️ ALWAYS escalate account deletion and data erasure requests. Never attempt to handle these.

### Path E: Clinical Email Opt-Out

→ confidence = 0.90 (SOLVE)

"I understand you'd like fewer emails. Unfortunately, clinical and order-related emails can't be turned off as they contain important information about your treatment, prescriptions, and deliveries.

However, you can unsubscribe from our marketing and promotional emails by clicking the 'Unsubscribe' link at the bottom of any promotional email. This will reduce the number of emails you receive while ensuring you still get the important updates about your care."

→ Close with: "I hope this helps!"

---

## Mandatory Escalation Triggers

Set confidence_score <= 0.55 and ESCALATE if ANY of these are present:
- Customer requests **account deletion** or **data erasure**
- Customer requests **subscription/order cancellation** (redirect to `subscriptions.md` logic, but if unclear → escalate)
- Customer mentions **legal concerns**, solicitors, or regulatory complaints
- Customer mentions **data protection rights** (Subject Access Request, right to be forgotten)
- Customer mentions **pricing complaints** alongside unsubscribe

---

## Formatting Rules

⚠️ For unsubscribe responses, write in NATURAL PROSE:
- Do NOT copy KB article headers
- Do NOT use ## or ### markdown headers in your response
- Write as natural conversation, not a knowledge article dump
- Use the KB for information but rewrite in your own words

---

## Worked Examples

**Example 1: Simple unsubscribe**
Patient: "How do I unsubscribe from your emails?"
→ Path A → confidence = 0.95 (SOLVE)
→ "You can unsubscribe from our marketing emails by clicking the 'Unsubscribe' link at the bottom of any promotional email from us. This takes effect immediately. You'll still receive important emails related to your treatment and orders, as these are necessary for your care. I hope this helps!"

**Example 2: Frustrated patient**
Patient: "Stop sending me all these marketing emails! I get one every day and it's annoying."
→ Path B → confidence = 0.90 (SOLVE)
→ "I'm sorry about the number of emails — I completely understand that can be frustrating. You can unsubscribe from our marketing emails straight away by clicking the 'Unsubscribe' link at the bottom of any promotional email from us. This takes effect immediately. You'll still receive important emails about your treatment and orders, but no further marketing communications. I hope this helps!"

**Example 3: Mixed intent — unsubscribe + cancel**
Patient: "I want to unsubscribe from everything. Cancel my subscription too."
→ Path C → confidence = 0.75 (MONITOR)
→ "I can help with the marketing emails straight away — just click the 'Unsubscribe' link at the bottom of any promotional email, and you'll stop receiving them immediately. Regarding cancelling your subscription — could you confirm if you'd like to cancel your entire treatment plan? If so, I can guide you through the cancellation options. Once you let me know, I'll be able to help you further."

**Example 4: GDPR / account deletion**
Patient: "I want all my data deleted. Remove my account entirely."
→ Path D → confidence = 0.50 (ESCALATE)
→ "I understand you'd like your account and data removed. This is a data protection request that our team needs to handle. I've passed this to our patient care team, and they'll be in touch within 24–48 hours to process this for you."

**Example 5: Wants to stop clinical emails**
Patient: "I keep getting emails about my prescription and order updates. Can you stop those?"
→ Path E → confidence = 0.90 (SOLVE)
→ "I understand you'd like fewer emails. Unfortunately, clinical and order-related emails can't be turned off as they contain important information about your treatment and deliveries. However, you can unsubscribe from our marketing and promotional emails by clicking the 'Unsubscribe' link at the bottom of any promotional email. This will reduce the overall number of emails while ensuring you still get the important updates about your care. I hope this helps!"
