# Weight Loss Self-Serve Resources

## Scope

This file covers webinars, live talks, educational content in the app, and the Getting Started Session (group onboarding).

**Covers:**
- Getting Started Session (group onboarding) — booking, joining, what it is
- Webinar and live talk access (nutrition, exercise, mindset, maintenance)
- Educational content in the Voy app and web
- Webinar technical issues (joining problems)

**Does NOT cover:**
- 1-to-1 coaching sessions or coach assignment → see `coaching.md`
- Clinical consultations or side effects → see `side-effects.md`
- App bugs or technical issues unrelated to webinars → see `app-features.md`

**Cross-references:**
- `coaching.md` — 1-to-1 coaching calls, coach assignment
- `side-effects.md` — clinical concerns
- `app-features.md` — general app navigation and technical issues

---

## Critical Rules

- NEVER promise specific webinar schedules — they change frequently. Always direct patients to the app or email for the latest information.
- NEVER attempt to book or reschedule webinars on behalf of the patient — provide self-serve links only.
- NEVER confirm whether a specific piece of educational content is available — direct the patient to the relevant section in the app.
- Do NOT provide details about the content of educational resources beyond what is listed in this file.

---

## Decision Tree

### "How do I join the Getting Started Session?"

→ confidence = 0.85 (SOLVE)

The Getting Started Session is a 30-minute live group session delivered by a member of the Voy coaching team. It covers:
- Starting treatment and what to expect
- Managing side effects
- Building healthy habits in nutrition, movement, and mindset
- Live Q&A

Joining instructions:
1. Check email or calendar for the invite
2. Click the Google Meet link (or "Join Meeting")
   - On mobile: download the Google Meet app, then click the link again
   - On desktop: Google Meet opens automatically in browser
3. Camera and microphone access is optional — no requirement to use them
4. If having trouble, look for the Google Meet code in the email (format: xxx-xxx-xxxx) and join at meet.google.com

→ "The Getting Started Session is a free 30-minute live group session covering how to use the app, what to expect from your treatment, and lifestyle tips. You can join using the Google Meet link in your confirmation email or calendar invite. Camera and microphone are optional."

---

### "How do I book a Getting Started Session?"

→ confidence = 0.85 (SOLVE)

Provide the booking link: [Getting Started Session with Voy](https://cal.com/team/clinical-team-test-account/voy-getting-started-session)

→ "You can book your Getting Started Session here: [Getting Started Session with Voy](https://cal.com/team/clinical-team-test-account/voy-getting-started-session). It's a free 30-minute live group session where you'll learn what to expect from your treatment and pick up lifestyle tips."

---

### "How do I access educational content?"

→ confidence = 0.85 (SOLVE)

**In the app:**
- Open the **Progress tab** and scroll to the bottom for weekly educational content
- Click "All weeks" to access the full programme and choose topics
- Topics include: nutrition essentials, weight loss strategies, movement tips, and mindset for lasting change
- For treatment-specific resources: open the **Plan tab** and scroll to the bottom for "Treatment advice" and "Tips from our doctors"

**On the web:**
- Go to the **Resources tab** and click "Guides & Tips"
- Articles cover medication information, diet plans, side effects, and weight loss journey guidance

→ "You can find educational content in the Voy app under the Progress tab — scroll down to see your weekly content, and tap 'All weeks' to browse the full programme. For treatment-specific resources, check the Plan tab. If you're on the web, head to the Resources tab and click 'Guides & Tips'."

---

### "I missed a webinar / live talk"

→ confidence = 0.85 (SOLVE)

Recordings are typically available after the session. Guide the patient to check:
- Their email for a follow-up with a recording link
- The app for any available recordings

→ "No worries — recordings of our webinars and live talks are typically made available afterward. Check your email for a follow-up message with the recording link, or look in the app for any available recordings."

---

### "I can't join the webinar / technical issues"

→ confidence = 0.85 (SOLVE)

Troubleshooting steps:
1. Check the joining link in the confirmation email
2. Try opening the link in an incognito/private browser window
3. Ensure Google Meet is not blocked by a firewall or corporate network
4. If a joining code was provided (format: xxx-xxx-xxxx), join directly at meet.google.com using the code
5. On mobile: ensure the Google Meet app is installed

→ "Here are a few things to try:\n1. Check the joining link in your confirmation email\n2. Try opening it in an incognito/private browser window\n3. Make sure Google Meet isn't blocked by your network or firewall\n4. If you have a joining code (format: xxx-xxx-xxxx), you can join directly at meet.google.com\n5. On mobile, make sure the Google Meet app is installed\n\nIf none of these work, reply to this email with the error you're seeing and we'll investigate further."

---

### "What webinars are available?"

→ confidence = 0.85 (SOLVE)

Available live talks and webinars include:
- **Getting Started Session** — onboarding for new patients
- **Maintenance Webinar** — a 30-minute group session with a Voy Health Coach and Clinician on lasting results
- **Nutrition Live Talk** — a 30-minute group session on nutrition
- **Exercise Live Talk** — a 30-minute group session on exercise and movement
- **Mindset Live Talk** — a 30-minute group session on motivation and long-term mindset change

Use `file_search` to check KB for current booking links and schedules.

→ "We offer several live sessions including a Getting Started Session for new patients, plus live talks on Nutrition, Exercise, and Mindset, and a Maintenance Webinar. All are 30-minute group sessions led by our coaching team. You can find upcoming sessions and booking links in the Voy app."

---

### "When is the next webinar?"

IF schedule information is found in KB:
→ confidence = 0.85 (SOLVE)
→ Provide the schedule details found.

IF schedule information is NOT found in KB:
→ confidence = 0.75 (MONITOR)
→ Suggest checking the app for upcoming sessions.
→ "I don't have the exact schedule to hand, but you can check the Voy app for upcoming sessions. Could you let me know which type of session you're interested in so I can look into it?"

---

## Tool Calls

| Tool | When to Use |
|------|-------------|
| `get_subscription_status` | When checking whether webinars/resources are included in the patient's plan |
| `file_search` | To search KB for current webinar schedules or booking links |

---

## Worked Examples

**Example 1: Booking a Getting Started Session**

Patient: "I just started Wegovy and I'd like to attend one of those intro sessions. How do I sign up?"

→ confidence = 0.85 (SOLVE)

```json
{
  "patient_reply": "Hi [First Name],\n\nWelcome to Voy! The Getting Started Session is a great place to begin — it's a free 30-minute live group session where you'll learn what to expect from your treatment and pick up tips on nutrition, movement, and mindset.\n\nYou can book your session here: [Getting Started Session with Voy](https://cal.com/team/clinical-team-test-account/voy-getting-started-session)\n\nSessions are available throughout the week, so you should be able to find a time that suits you.\n\nI hope this helps! If you have any other questions, just reply to this email.\n\nBest,\nVoy\n\n---\n*This response was generated by the AI Assistant.*",
  "internal_note": "Issue: New patient wants to book Getting Started Session.\nAction: Provided booking link.",
  "confidence_score": 0.85,
  "confidence_reasoning": "Skill file decision tree: booking Getting Started Session → SOLVE at 0.85. Link provided.",
  "awaiting_customer_input": false
}
```

---

**Example 2: Accessing educational content in the app**

Patient: "Where can I find the nutrition guides in the app? I can't seem to find them."

→ confidence = 0.85 (SOLVE)

```json
{
  "patient_reply": "Hi [First Name],\n\nYou can find educational content including nutrition guides in the Voy app:\n\n- Open the **Progress tab** and scroll to the bottom — you'll see your weekly educational content there\n- Tap **\"All weeks\"** to browse the full programme and choose the topics you're interested in\n- For treatment-specific resources, check the **Plan tab** — scroll to the bottom for \"Treatment advice\" and \"Tips from our doctors\"\n\nI hope this helps! If you have any other questions, just reply to this email.\n\nBest,\nVoy\n\n---\n*This response was generated by the AI Assistant.*",
  "internal_note": "Issue: Patient looking for nutrition guides in the app.\nAction: Directed to Progress tab and Plan tab for educational content.",
  "confidence_score": 0.85,
  "confidence_reasoning": "Skill file decision tree: educational content access → SOLVE at 0.85. Clear directions provided.",
  "awaiting_customer_input": false
}
```

---

**Example 3: Webinar technical issues**

Patient: "I'm trying to join the live talk but the link isn't working. It just shows a blank screen."

→ confidence = 0.85 (SOLVE)

```json
{
  "patient_reply": "Hi [First Name],\n\nSorry to hear you're having trouble joining! Here are a few things to try:\n\n1. **Check the joining link** in your confirmation email — make sure you're using the most recent one\n2. **Try an incognito/private browser window** — this can resolve issues caused by browser extensions or cached data\n3. **Check your network** — make sure Google Meet isn't blocked by a firewall or corporate network\n4. If you have a **joining code** (format: xxx-xxx-xxxx), you can join directly at meet.google.com\n5. **On mobile**, make sure the Google Meet app is installed and up to date\n\nIf none of these work, let me know what error you're seeing and I'll do my best to help.\n\nBest,\nVoy\n\n---\n*This response was generated by the AI Assistant.*",
  "internal_note": "Issue: Patient unable to join live talk — blank screen.\nAction: Provided troubleshooting steps for Google Meet.",
  "confidence_score": 0.85,
  "confidence_reasoning": "Skill file decision tree: webinar technical issues → SOLVE at 0.85. Troubleshooting steps provided.",
  "awaiting_customer_input": false
}
```

---

## Internal Note Guidance

For SOLVE decisions, include:
- **Issue**: Brief summary of what the patient asked
- **Action**: What was done (e.g., "Provided booking link", "Directed to app section", "Provided troubleshooting steps")

For MONITOR decisions, include:
- **Issue**: Summary of the query
- **Action**: What information is needed from the patient (e.g., "Waiting for patient to confirm which session type they're interested in")

---

## Mandatory Rules

1. **Never promise specific webinar schedules** — schedules change. Always direct to the app or email for the latest.
2. **Never book or reschedule webinars on behalf of the patient** — provide self-serve booking links only.
3. **Never confirm specific content availability** — direct patients to the relevant app section to browse themselves.
4. **Always direct to app/email for the latest information** — do not rely on potentially outdated schedule data.
5. **Always Send a Public Response** — every ticket must receive a patient-facing reply. Never leave a ticket with only an internal note.
