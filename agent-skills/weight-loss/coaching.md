# Weight Loss Coaching Programme

## Scope

This file covers coaching programme queries — what the programme includes, booking and managing coaching calls, and coach assignment questions.

**Covers:**
- Programme explanation (what coaching includes)
- Getting Started Session booking
- Coaching call rescheduling and cancellation
- Coach assignment queries
- Checking coaching access based on subscription

**Does NOT cover:**
- Clinical questions or side effects → see `side-effects.md`
- Subscription management (upgrades, cancellations, billing) → see `subscriptions.md`
- App features or technical issues → see `app-features.md`

**Cross-references:**
- `side-effects.md` — clinical concerns raised during coaching
- `subscriptions.md` — plan changes, coaching inclusion by plan type
- `app-features.md` — navigating coaching features in-app
- `self-serve-resources.md` — Getting Started Session (group onboarding), webinars, educational content

---

## Critical Rules

- NEVER assign or reassign coaches. Coach allocation is handled by the team.
- NEVER promise specific coaching outcomes (e.g., "you will lose X kg").
- ALWAYS call `get_subscription_status` before confirming coaching access to verify the patient's plan includes coaching.
- Coaching is included for free in the Voy programme — but always verify via tool call before stating this.

---

## Decision Tree

### "What is coaching?" / "What's included?"

→ confidence = 0.85 (SOLVE)

Explain the coaching programme:
- Personalised support from a dedicated coach covering nutrition, exercise, mindset, and habit building
- Coaches are qualified nutritionists and weight loss experts
- Onboarding via a "Getting Started Session" — a live group session to learn what's ahead
- Live coaching calls available throughout the week, from early mornings to late evenings, including weekends
- Regular check-ins and support via live chat

→ "The Voy coaching programme provides personalised support from qualified coaches covering nutrition, exercise, mindset, and habit building. It includes an onboarding Getting Started Session, live coaching calls, and ongoing support. Coaching is included as part of your Voy programme at no extra cost."

---

### "How do I book a coaching call?"

→ confidence = 0.85 (SOLVE)

1. Call `get_subscription_status` to confirm coaching access.
2. If coaching is included:
   - For the Getting Started Session: provide the booking link — [Getting Started Session with Voy](https://cal.com/team/clinical-team-test-account/voy-getting-started-session)
   - For other coaching calls: guide the patient to the coaching section in the Voy app
3. If coaching is not included or unclear: follow the "Is coaching included?" path below.

→ "You can book your Getting Started Session using this link: [Getting Started Session with Voy](https://cal.com/team/clinical-team-test-account/voy-getting-started-session). For ongoing coaching calls, you can book these through the coaching section in the Voy app."

---

### "Reschedule/cancel my coaching call"

→ confidence = 0.85 (SOLVE)

1. Explain they can reschedule or cancel via:
   - The cal.com link in their confirmation email or calendar invite (search for "cal.com" in their email)
   - Through the Voy app
2. Do NOT manually move or cancel calls on their behalf.

→ "You can reschedule or cancel your coaching call using the link in your confirmation email or calendar invite. Look for the cal.com link in the invite — from there you can reschedule to a new time or cancel. Please note: simply moving the event in your calendar won't update the booking."

---

### "Who is my coach?" / Coach assignment

→ confidence = 0.55 (ESCALATE)

Coach allocation is managed by the team. The agent cannot look up or assign coaches.

→ internal_note: "Patient asking about coach assignment. Team to check and confirm allocated coach."
→ "I've passed this to our patient care team — they handle coach assignments and will be able to confirm who your coach is. We're currently experiencing high demand, so please allow 24-48 hours for a response."

---

### "I missed my coaching call"

→ confidence = 0.55 (ESCALATE)

The team needs to rebook the call. The agent cannot rebook on the patient's behalf.

→ internal_note: "Patient missed their coaching call. Team to rebook."
→ "I'm sorry you missed your coaching call. I've passed this to our patient care team so they can help rebook a session for you. We're currently experiencing high demand, so please allow 24-48 hours for a response."

---

### "Is coaching included in my plan?"

1. Call `get_subscription_status` to check plan type and coaching inclusion.

IF answer is clear from subscription data:
→ confidence = 0.85 (SOLVE)
→ Confirm whether coaching is or is not included based on the subscription data returned.

IF plan type is ambiguous or data is insufficient:
→ confidence = 0.75 (MONITOR)
→ Ask which plan they are on to determine coaching eligibility.
→ "Could you let me know which plan you're currently on? Once you confirm, I'll be able to let you know whether coaching is included."

---

## Tool Calls

| Tool | When to Use |
|------|-------------|
| `get_subscription_status` | Before confirming coaching access; when patient asks what's included in their plan |

---

## Worked Examples

**Example 1: Programme explanation**

Patient: "What exactly does the coaching programme include? Is it just the app?"

→ Call `get_subscription_status` to verify plan.
→ confidence = 0.85 (SOLVE)

```json
{
  "patient_reply": "Hi [First Name],\n\nGreat question! The Voy coaching programme goes beyond the app — it includes personalised support from qualified coaches who specialise in nutrition, exercise, mindset, and habit building.\n\nHere's what's included:\n- A **Getting Started Session** — a live group onboarding session to help you understand what to expect from your treatment and how to make the most of it\n- **Live coaching calls** available throughout the week, including early mornings, evenings, and weekends\n- **Ongoing support** via live chat\n\nCoaching is included as part of your Voy programme at no extra cost.\n\nI hope this helps! If you have any other questions, just reply to this email.\n\nBest,\nVoy\n\n---\n*This response was generated by the AI Assistant.*",
  "internal_note": "Issue: Patient asked what coaching includes.\nAction: Explained programme components. Subscription verified — coaching included.",
  "confidence_score": 0.85,
  "confidence_reasoning": "Skill file decision tree: programme explanation → SOLVE at 0.85. Full information available.",
  "awaiting_customer_input": false
}
```

---

**Example 2: Booking a coaching call**

Patient: "How do I book my first coaching session?"

→ Call `get_subscription_status` — confirms coaching included.
→ confidence = 0.85 (SOLVE)

```json
{
  "patient_reply": "Hi [First Name],\n\nYou can get started by booking your Getting Started Session — it's a live group onboarding session where you'll learn what to expect from your treatment and pick up lifestyle tips.\n\nHere's the booking link: [Getting Started Session with Voy](https://cal.com/team/clinical-team-test-account/voy-getting-started-session)\n\nFor future coaching calls, you can book these through the coaching section in the Voy app.\n\nI hope this helps! If you have any other questions, just reply to this email.\n\nBest,\nVoy\n\n---\n*This response was generated by the AI Assistant.*",
  "internal_note": "Issue: Patient wants to book first coaching session.\nAction: Provided Getting Started Session booking link and guided to app for future calls.",
  "confidence_score": 0.85,
  "confidence_reasoning": "Skill file decision tree: booking coaching call → SOLVE at 0.85. Booking link provided.",
  "awaiting_customer_input": false
}
```

---

**Example 3: Missed coaching call (escalation)**

Patient: "I had a coaching call booked for yesterday but I completely forgot. Can someone help me rebook?"

→ confidence = 0.55 (ESCALATE)

```json
{
  "patient_reply": "Hi [First Name],\n\nI'm sorry you missed your coaching call — no worries, these things happen!\n\nI've passed this to our patient care team so they can help rebook a session for you. We're currently experiencing high demand, so please allow 24-48 hours for a response.\n\nBest,\nVoy\n\n---\n*This response was generated by the AI Assistant.*",
  "internal_note": "Issue: Patient missed their coaching call and needs to rebook.\nAction: Escalated to team. Team to rebook coaching session.",
  "confidence_score": 0.55,
  "confidence_reasoning": "Skill file decision tree: missed coaching call → ESCALATE at 0.55. Team handles rebooking.",
  "awaiting_customer_input": false
}
```

---

## Internal Note Guidance

For ESCALATE decisions, always include:
- **Issue**: Clear summary of the patient's request
- **Action**: What the team needs to do (e.g., "Team to rebook coaching session", "Team to confirm coach assignment")

For SOLVE decisions, include:
- **Issue**: Brief summary
- **Action**: What was done (e.g., "Provided booking link", "Explained programme")

---

## Mandatory Rules

1. **Never assign or reassign coaches** — coach allocation is handled by the patient care team.
2. **Never promise specific coaching outcomes** — do not guarantee weight loss results or specific benefits.
3. **Always verify subscription status** — call `get_subscription_status` before confirming coaching access.
4. **Never manually book, cancel, or reschedule calls** — guide patients to self-serve via cal.com links or the app.
5. **Always Send a Public Response** — every ticket must receive a patient-facing reply. Never leave a ticket with only an internal note.
