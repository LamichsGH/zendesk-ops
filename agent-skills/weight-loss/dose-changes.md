# Weight Loss Dose Changes

## Scope

This file covers dose change requests, titration schedules, self-serve dose change paths, missed dose guidance, and medication switching for weight loss medications (Wegovy and Mounjaro).

**Covers:**
- Titration schedules for Wegovy and Mounjaro
- Self-serve dose changes (within titration schedule) via the app
- Dose change request handling and escalation paths
- Automatic titration default behaviour
- The 3-day order cutoff rule
- Appetite suppression assessment (day-6 rule)
- Missed dose guidance
- Treatment gap rules (>4 weeks = restart at lowest dose)
- Medication switching (Wegovy ↔ Mounjaro) including plan-length restrictions
- Dose safety rules (no skipping titration steps)

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

⚠️ CRITICAL: Dose changes that follow the standard titration schedule (one step up, maintain, or decrease) CAN be self-served by the patient in the app. The agent should guide them to the in-app flow. Only out-of-schedule changes, complex situations, or patients who cannot self-serve should be escalated.

⚠️ CRITICAL: If a patient has been off medication for more than 4 weeks, they MUST restart at the lowest dose (Wegovy 0.25mg or Mounjaro 2.5mg). This is a safety requirement for GLP-1 medications.

⚠️ CRITICAL: Skipping a titration step is NEVER allowed. If the requested dose is more than one step above the current active dose, it is unsafe. Inform the patient of the correct next possible dose.

⚠️ CRITICAL: The patient must change their dosage at least 3 days before their next order is due for it to take effect on that order.

⚠️ NEVER advise a patient to take half doses, split doses, microdose, or modify the standard dosing in any way. Patients must always take the full prescribed dose.

⚠️ Automatic dose increases are the DEFAULT. Unless a patient has explicitly opted out via the app, their dose WILL increase according to the titration schedule every 4 weeks. Never tell a patient their dose will "stay the same" unless they have actively opted out.

---

## Titration Schedules (Reference Only)

These schedules are for informational purposes. Actual dose progression is determined by the clinical team.

**Wegovy titration:**
| Step | Dose | Duration | Notes |
|------|------|----------|-------|
| 1 | 0.25mg | 4 weeks | Starting dose |
| 2 | 0.5mg | 4 weeks | |
| 3 | 1.0mg | 4 weeks | |
| 4 | 1.7mg | 4 weeks | |
| 5 | 2.4mg | Ongoing | Maintenance dose |
| 6 | 7.2mg | Ongoing | Patient must manually opt in via Plan tab — not an automatic increase |

**Mounjaro titration:**
| Step | Dose | Duration | Notes |
|------|------|----------|-------|
| 1 | 2.5mg | 4 weeks | Starting dose |
| 2 | 5.0mg | 4 weeks | |
| 3 | 7.5mg | 4 weeks | |
| 4 | 10.0mg | 4 weeks | First optional maintenance level |
| 5 | 12.5mg | 4 weeks | Patient can choose to increase |
| 6 | 15.0mg | Ongoing | Maximum dose |

→ confidence = 0.88 (SOLVE — reference only)
→ "Your dose is typically increased gradually over several weeks to help minimise side effects. The clinical team will guide you through each step based on how you're responding to treatment."

---

## Dose Change Decision Tree

Use `get_order_history` and `get_subscription_status` to establish the patient's current dose, next scheduled dose, and plan type before responding.

### Step 1: Classify the request

- Patient wants to change their dose (increase, decrease, or maintain) → Step 2
- Patient wants a dose recommendation → go to [Appetite Suppression Assessment](#appetite-suppression-assessment-day-6-rule)
- Patient wants to modify dosing (half dose, splitting, timing) → go to [Dose Modification Requests](#dose-modification-requests)
- Patient wants to switch medication (Wegovy ↔ Mounjaro) → go to [Medication Switching](#medication-switching)

### Step 2: Check safety

Compare the patient's current active dose with their requested dose using the titration schedules above.

IF requested dose is MORE than one titration step above the current dose:
  → confidence = 0.55 (ESCALATE)
  → "For safety, dose increases follow a step-by-step titration schedule. From your current dose of [current], the next available step would be [next step]. Skipping steps is not permitted. I've passed this to our patient care team for review. They'll be in touch within 24-48 hours."

IF requested dose is the SAME as the next scheduled dose (i.e., already planned):
  → confidence = 0.85 (SOLVE)
  → "Good news — your next order is already set to [next scheduled dose], so no changes are needed. Is there anything else I can help with?"

IF requested dose is a valid one-step increase, a decrease, or maintaining the current dose → Step 3.

### Step 3: Check the 3-day order cutoff

IF the patient's next order is due within 3 days:
  → confidence = 0.55 (ESCALATE)
  → "Dose changes need to be made at least 3 days before your next order for them to take effect. Since your next order is due very soon, I've passed this to our patient care team to see what options are available. They'll be in touch within 24-48 hours."

IF the next order is more than 3 days away → Step 4.

### Step 4: Determine self-serve eligibility

IF the dose change is within the standard titration schedule (one step up, maintain, or decrease) AND the patient has no order currently in progress:
  → confidence = 0.85 (SOLVE — guide to self-serve)
  → Go to [Self-Serve Dose Change Instructions](#self-serve-dose-change-instructions)

IF the patient has an order currently in progress, or has no next order, or is blocked from self-serve:
  → confidence = 0.55 (ESCALATE)
  → Gather the required information (requested dose, order date context, reason for change), then:
  → "I've prepared your dose change request with all the details and passed it to our patient care team. They'll review and apply the change as soon as possible — you'll hear from them within 24-48 hours."

---

## Self-Serve Dose Change Instructions

These instructions apply to all plan types (28-day, 3-month, 6-month, and 12-month).

→ confidence = 0.85 (SOLVE)

**App instructions:**
→ "You can change your dose directly in the Voy app. Here's how:
1. Go to the **Plan** tab
2. Select **Change Dose**
3. Choose the dose you'd like
4. Submit for review

The clinical team will review your request — this usually takes up to 48 hours. Once approved, the change will apply to your next order. You'll receive a notification when it's confirmed."

**Additional notes to include when relevant:**
- IF patient is staying on their current dose (opting out of automatic increase): "Since doses increase automatically every 4 weeks, you'll need to use the Change Dose option in the app to stay on your current dose."
- IF patient asks about Wegovy 7.2mg: "The 7.2mg dose will appear as an option in the app. You'll need to manually opt in to this dose via the Plan tab — it's not an automatic increase."

---

## Appetite Suppression Assessment (Day-6 Rule)

IF patient asks whether they should change their dose (wants a recommendation):

**Step 1:** Ask how they are feeling on their current dose and whether they are experiencing any side effects.

IF they have side effects:
  → confidence = 0.55 (ESCALATE)
  → "Since you're experiencing side effects, I'd like to connect you with our clinical team for personalised guidance on whether a dose adjustment is right for you. I've passed this to our patient care team. They'll be in touch within 24-48 hours."

IF they feel well (no side effects) → Step 2.

**Step 2:** Ask when during their weekly cycle they notice their appetite suppression starting to wear off.

IF appetite suppression wears off BEFORE day 6 of the 7-day cycle:
  → confidence = 0.85 (SOLVE)
  → "Since your appetite suppression is tapering off before day 6 of your weekly cycle, it may be worth considering moving to the next dose. The decision is ultimately yours, but titrating up could help maintain more consistent appetite control throughout the week. If you'd like to go ahead, you can change your dose in the app: Plan tab → Change Dose."

IF appetite suppression lasts until day 6 or beyond:
  → confidence = 0.85 (SOLVE)
  → "Since your appetite suppression is lasting well through your weekly cycle, it may make sense to stay on your current dose for now. The right dose is the one that works best for you — increasing doesn't always mean better results. If you'd like to stay on this dose, you can confirm it in the app: Plan tab → Change Dose, and select your current dose to opt out of the automatic increase."

---

## Dose Modification Requests

IF patient asks about half doses, dose splitting, slower increases, or any non-standard dosing:
  → confidence = 0.55 (ESCALATE)
  → "For safety, the medication must be taken as a single full dose once per week. Modifications such as half doses or dose splitting are not supported outside of a documented clinical need. I've passed this to our clinical team for review. They'll be in touch within 24-48 hours."

---

## Dose Change Requests (Quick Reference)

IF patient requests a dose increase (within one titration step):
  → confidence = 0.85 (SOLVE — guide to self-serve)
  → Check 3-day cutoff rule, then guide to [Self-Serve Dose Change Instructions](#self-serve-dose-change-instructions).

IF patient requests a dose decrease:
  → confidence = 0.85 (SOLVE — guide to self-serve)
  → Ask reason. If due to side effects, acknowledge and still guide to self-serve: "I understand the side effects can be challenging. You can decrease your dose through the app: Plan tab → Change Dose. The clinical team will review your request within 48 hours."

IF patient requests to stay on their current dose (opt out of automatic increase):
  → confidence = 0.85 (SOLVE — guide to self-serve)
  → "Since doses increase automatically every 4 weeks, you'll need to actively opt out. You can do this in the app: Plan tab → Change Dose, then select your current dose. Once submitted, you'll stay on this dose until you decide to change it."

IF patient asks when their next dose increase will happen:
  → confidence = 0.85 (SOLVE)
  → "Dose increases happen automatically every 4 weeks following the titration schedule, unless you've opted out. You can check your upcoming dose in the Plan tab of the app."

IF patient asks about maximum dose:
  → confidence = 0.88 (SOLVE)
  → Wegovy: "The standard maintenance dose is 2.4mg. A 7.2mg dose is also available — patients must manually opt in via the Plan tab."
  → Mounjaro: "The maximum dose is 15mg, reached through a gradual titration schedule."

---

## Missed Dose Guidance

IF patient has missed a dose:
  → confidence = 0.85 (SOLVE)

**Within 5 days of scheduled dose:**
  → "If it's been less than 5 days since your missed dose, take it as soon as possible. Then continue with your regular weekly schedule."

**More than 5 days since scheduled dose:**
  → "Since it's been more than 5 days, skip the missed dose and take your next dose on your regular scheduled day. Do not take a double dose to make up for the missed one."

IF patient has missed multiple doses:
  → confidence = 0.55 (ESCALATE)
  → "Since you've missed more than one dose, I've passed this to our clinical team to review your treatment plan and advise on the safest way to continue. They'll be in touch within 24-48 hours."

---

## Treatment Gap Rules

IF patient has been off medication for >4 weeks:
  → confidence = 0.85 (SOLVE)
  → "Since you've been off medication for more than 4 weeks, you'll need to restart at the lowest dose for safety reasons. This is [Wegovy 0.25mg / Mounjaro 2.5mg]. You can restart through the app: Plan tab → Reorder."

IF patient has been off medication for <2 weeks:
  → confidence = 0.85 (SOLVE)
  → "Since it's been less than 2 weeks, you may be able to continue from where you left off or even titrate up. The clinical team will confirm this when they review your restart."

IF patient has been off medication for 2-4 weeks:
  → confidence = 0.55 (ESCALATE)
  → "Since you've been off medication for a few weeks, our clinical team will need to review and determine the safest dose for you to restart at. I've passed this to our patient care team. They'll be in touch within 24-48 hours."

---

## Medication Switching

Use `get_order_history` and `get_subscription_status` to determine the patient's current medication and plan type.

### Patient has no active subscription

IF patient is placing their first order:
  → confidence = 0.85 (SOLVE)
  → "To get started, go to the Home tab in the Voy app and complete the medical questionnaire. You'll be shown a suggested treatment plan and can select your preferred treatment length. An additional first-order discount is applied at checkout."

IF patient is reordering:
  → confidence = 0.85 (SOLVE)
  → "You can reorder through the app: Plan tab → Reorder. You'll answer a few questions to make sure the best treatment is offered, then choose your preferred treatment length."

### Patient on a 28-day plan

IF patient wants to switch between Wegovy and Mounjaro:
  → confidence = 0.85 (SOLVE — guide to self-serve)
  → "You can switch medications through the Voy app: Go to the Plan tab → [Treatment Options](https://app.vfrst.com/plan). Choose which medication you'd like to switch to and your preferred treatment length. The clinical team will review and approve your new treatment within 48 hours — you'll receive a notification once confirmed."

### Patient on a longer plan (3, 6, or 12 months)

IF patient wants to switch medication or change dose AND has NOT completed all paid orders in their commitment period:
  → confidence = 0.85 (SOLVE)
  → "Since you're on a [3/6/12]-month plan, changes to your medication or dose can only be made once you've received all the orders in your current plan. After your commitment period ends, you'll receive an email to confirm whether you'd like to continue, change plans, or cancel."

IF patient wants to switch medication AND HAS completed all paid orders:
  → confidence = 0.85 (SOLVE — guide to self-serve)
  → "Since you've completed your current plan, you can now make changes. Go to the Plan tab in the app and select Treatment Options to switch medication, or Manage or Cancel Plan to update your plan length."

### Switching safety notes

- Patient should leave a minimum of 7 days from their last dose before starting a different GLP-1 medication.
- If previously on Mounjaro and switching to Wegovy, the clinical team may recommend starting at a higher Wegovy dose to prevent regression. The clinicians will determine the ideal starting dose.
- Switching medications may require restarting at a lower dose — the clinical team will determine this during their review.

---

## Important Dose Change Rules

- A new order is generated every 28 days after the last order.
- Dose changes must be submitted at least **3 days before the next order** to take effect.
- Automatic dose increases are the default every 4 weeks. To prevent an increase, the patient must actively opt out via the app.
- All dose increases require clinical review (even self-serve changes go through clinical approval within 48 hours).
- Dose decreases and maintaining current dose are always permitted.
- GLP-1 injections are taken as a once-weekly injection — no dose splitting.
- No doses exist outside the Wegovy and Mounjaro titration schedules listed above.
- Impact of dose changes patients should be aware of:
  - Increasing dose may lead to more pronounced side effects.
  - Decreasing dose may reduce side effects but also impact weight loss progress.
  - Stopping medication may lead to weight regain and loss of appetite control.
  - Increasing dose does not always mean faster weight loss — GLP-1 medications work differently for everyone.

---

## Worked Examples

**Example 1: Self-serve dose increase (within schedule)**
Patient: "I've been on Wegovy 0.5mg for 4 weeks now. I'd like to move up to 1.0mg."
→ Use `get_order_history` to confirm current dose is 0.5mg and next order is >3 days away.
→ 1.0mg is one step above 0.5mg — valid and safe.
→ confidence = 0.85 (SOLVE)
→ "Great — moving from 0.5mg to 1.0mg is the next step in the titration schedule. You can make this change in the Voy app: Plan tab → Change Dose → select 1.0mg → submit for review. The clinical team will review within 48 hours and you'll receive a notification once confirmed."

**Example 2: Patient wants to stay on current dose**
Patient: "I don't want my dose to go up next month. I want to stay on Mounjaro 5mg."
→ Use `get_order_history` to confirm current dose and next scheduled dose.
→ confidence = 0.85 (SOLVE)
→ "Since doses increase automatically every 4 weeks, you'll need to opt out of the automatic increase. You can do this in the Voy app: Plan tab → Change Dose → select 5mg (your current dose) → submit. Once confirmed, you'll stay on 5mg until you decide to change it."

**Example 3: Missed dose**
Patient: "I forgot to take my injection 3 days ago. What should I do?"
→ confidence = 0.85 (SOLVE)
→ "Since it's been less than 5 days, you can take your missed dose today. Then continue with your regular weekly schedule going forward."

**Example 4: Treatment gap**
Patient: "I cancelled 6 weeks ago but want to restart. Can I go back to my old dose of Mounjaro 10mg?"
→ confidence = 0.85 (SOLVE)
→ "Since you've been off medication for more than 4 weeks, you'll need to restart at the lowest dose (Mounjaro 2.5mg) for safety reasons. You can restart through the Voy app: Plan tab → Reorder. The clinical team will review your information and guide you through the titration schedule."

**Example 5: Dose change too close to order date**
Patient: "My next order is tomorrow and I want to decrease my dose."
→ Use `get_order_history` to confirm order timing.
→ confidence = 0.55 (ESCALATE)
→ "Dose changes need to be made at least 3 days before your next order to take effect. Since your next order is due tomorrow, I've passed this to our patient care team to see what can be done. They'll be in touch within 24-48 hours."

**Example 6: Unsafe dose skip**
Patient: "I'm on Wegovy 0.25mg but want to jump to 1.0mg."
→ 1.0mg is two steps above 0.25mg — this skips 0.5mg.
→ confidence = 0.55 (ESCALATE)
→ "For safety, dose increases follow a step-by-step schedule. From 0.25mg, the next available step is 0.5mg — skipping steps isn't permitted. I've passed this to our patient care team for review. They'll be in touch within 24-48 hours."

**Example 7: Appetite suppression wearing off early**
Patient: "I'm on Mounjaro 5mg and I feel like my appetite comes back after about 4-5 days. Should I increase?"
→ Appetite suppression wears off before day 6.
→ confidence = 0.85 (SOLVE)
→ "Since your appetite suppression is wearing off before day 6 of your weekly cycle, it may be worth considering moving to the next dose (7.5mg). The decision is yours, but titrating up could help maintain more consistent appetite control. If you'd like to go ahead, you can change your dose in the app: Plan tab → Change Dose."

**Example 8: Medication switch on a 28-day plan**
Patient: "I'm on Wegovy but want to try Mounjaro instead."
→ Use `get_subscription_status` to confirm 28-day plan.
→ confidence = 0.85 (SOLVE)
→ "You can switch from Wegovy to Mounjaro through the app: Plan tab → Treatment Options. Choose Mounjaro and your preferred treatment length. The clinical team will review within 48 hours and determine the right starting dose. Just note — you should leave at least 7 days from your last Wegovy injection before starting Mounjaro."

**Example 9: Medication switch blocked by longer plan**
Patient: "I'm on a 6-month Wegovy plan and want to switch to Mounjaro. I've only had 2 orders."
→ Use `get_subscription_status` to confirm 6-month plan, not yet completed.
→ confidence = 0.85 (SOLVE)
→ "Since you're on a 6-month plan, changes to your medication can only be made once you've received all the orders in your current plan. After your commitment period ends, you'll receive an email to confirm whether you'd like to continue, switch medications, or cancel."
