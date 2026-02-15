# Weight Loss Dose Changes

## Scope

This file covers dose change requests, titration schedules, and missed dose guidance for weight loss medications (Wegovy and Mounjaro).

**Covers:**
- Titration schedules for Wegovy and Mounjaro
- Dose change request handling (ALL require clinical escalation)
- Missed dose guidance
- Treatment gap rules (>4 weeks = restart at lowest dose)
- Medication switching (Wegovy ↔ Mounjaro)

**Does NOT cover:**
- Side effects from dose changes → see `side-effects.md`
- How to use medication/injection technique → see `how-to-use-medication.md`
- Subscription changes → see `subscriptions.md`

**Cross-references:**
- `side-effects.md` — managing side effects during titration
- `how-to-use-medication.md` — injection instructions per medication
- `subscriptions.md` — restarting subscription after gap

---

## Critical Rules

⚠️ CRITICAL: ALL dose change requests MUST be escalated to the clinical team. The agent CANNOT approve, recommend, or process any dose changes.

⚠️ CRITICAL: If a patient has been off medication for more than 4 weeks, they MUST restart at the lowest dose (Wegovy 0.25mg or Mounjaro 2.5mg). This is a safety requirement for GLP-1 medications.

⚠️ NEVER advise a patient to change their dose without clinical review, even if they believe they should move up or down.

---

## Titration Schedules (Reference Only)

These schedules are for informational purposes. Actual dose progression is determined by the clinical team.

**Wegovy titration:**
| Phase | Dose | Duration |
|-------|------|----------|
| Starting | 0.25mg | 4 weeks |
| Step 2 | 0.5mg | 4 weeks |
| Step 3 | 1.0mg | 4 weeks |
| Step 4 | 1.7mg | 4 weeks |
| Maintenance | 2.4mg | Ongoing |
| Extended (if prescribed) | 7.2mg | Ongoing |

**Mounjaro titration:**
| Phase | Dose | Duration |
|-------|------|----------|
| Starting | 2.5mg | 4 weeks |
| Step 2 | 5.0mg | 4 weeks |
| Step 3 | 7.5mg | 4 weeks |
| Step 4 | 10.0mg | 4 weeks |
| Step 5 | 12.5mg | 4 weeks |
| Maintenance | 15.0mg | Ongoing |

→ confidence = 0.88 (INFORM — reference only)
→ "Your dose is typically increased gradually over several weeks to help minimise side effects. The clinical team will guide you through each step based on how you're responding to treatment."

---

## Dose Change Requests

IF patient requests a dose increase:
  → confidence = 0.55 (ESCALATE)
  → "Dose increases need to be reviewed and approved by our clinical team. I'll connect you with them so they can assess whether you're ready for the next step."

IF patient requests a dose decrease (due to side effects):
  → confidence = 0.55 (ESCALATE)
  → "I understand the side effects can be challenging. Our clinical team can review your symptoms and determine the best approach, which may include adjusting your dose. Let me connect you with them."

IF patient asks when their next dose increase will happen:
  → confidence = 0.85 (INFORM)
  → "Dose increases are typically every 4 weeks, but the timing depends on how you're responding to treatment. Your clinical team will review this and update your prescription when appropriate."

IF patient asks about maximum dose:
  → confidence = 0.88 (INFORM)
  → Wegovy: "The standard maintenance dose is 2.4mg. A 7.2mg dose is also available for some patients."
  → Mounjaro: "The maximum dose is 15mg, reached through a gradual titration schedule."

⚠️ Agent should NEVER imply they can make dose changes directly.

---

## Missed Dose Guidance

IF patient has missed a dose:
  → confidence = 0.85 (INFORM)

**Within 5 days of scheduled dose:**
  → "If it's been less than 5 days since your missed dose, take it as soon as possible. Then continue with your regular weekly schedule."

**More than 5 days since scheduled dose:**
  → "Since it's been more than 5 days, skip the missed dose and take your next dose on your regular scheduled day. Do not take a double dose to make up for the missed one."

IF patient has missed multiple doses:
  → confidence = 0.55 (ESCALATE)
  → "Since you've missed more than one dose, I'd like to connect you with our clinical team to review your treatment plan and advise on the safest way to continue."

---

## Treatment Gap Rules

IF patient has been off medication for >4 weeks:
  → confidence = 0.85 (INFORM)
  → "Since you've been off medication for more than 4 weeks, you'll need to restart at the lowest dose for safety reasons. This is [Wegovy 0.25mg / Mounjaro 2.5mg]. You can restart through the app: Plan tab → Reorder."

IF patient has been off medication for <2 weeks:
  → confidence = 0.85 (INFORM)
  → "Since it's been less than 2 weeks, you may be able to continue from where you left off or even titrate up. The clinical team will confirm this when they review your restart."

IF patient has been off medication for 2–4 weeks:
  → confidence = 0.55 (ESCALATE)
  → "Since you've been off medication for a few weeks, our clinical team will need to review and determine the safest dose for you to restart at."

---

## Medication Switching

IF patient asks about switching between Wegovy and Mounjaro:
  → confidence = 0.85 (INFORM + GUIDE TO SELF-SERVE)
  → "You can switch medications through the Voy app: Go to the Plan tab → Treatment Options. The clinical team will review the switch and determine the appropriate starting dose for your new medication."

⚠️ Switching medications may require restarting at a lower dose — the clinical team will determine this.

---

## Worked Examples

**Example 1: Dose increase request**
Patient: "I've been on Wegovy 0.5mg for 4 weeks now. When do I move to the next dose?"
→ confidence = 0.85 (INFORM + ESCALATE)
→ "Typically, Wegovy doses increase every 4 weeks during the titration phase. Since you've completed 4 weeks on 0.5mg, the next step would be 1.0mg. Your clinical team will review this and update your prescription. Would you like me to connect you with them to discuss your next step?"

**Example 2: Missed dose**
Patient: "I forgot to take my injection 3 days ago. What should I do?"
→ confidence = 0.85 (INFORM)
→ "Since it's been less than 5 days, you can take your missed dose today. Then continue with your regular weekly schedule going forward."

**Example 3: Treatment gap**
Patient: "I cancelled 6 weeks ago but want to restart. Can I go back to my old dose of Mounjaro 10mg?"
→ confidence = 0.85 (INFORM)
→ "Since you've been off medication for more than 4 weeks, you'll need to restart at the lowest dose (Mounjaro 2.5mg) for safety reasons. You can restart through the Voy app: Plan tab → Reorder. The clinical team will review your information and guide you through the titration schedule."
