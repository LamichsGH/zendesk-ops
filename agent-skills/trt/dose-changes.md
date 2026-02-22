# TRT Dose Changes

## Scope

This file covers dose change requests for TRT medications — including requests to increase, decrease, pause, or switch medication.

**Covers:**
- Dose increase requests
- Dose decrease requests
- Pausing TRT medication
- Switching TRT medication
- Treatment gap rules (>4 weeks = mandatory restart at lowest titration level)
- Titration level reference

**Does NOT cover:**
- Side effects from dose changes → see `side-effects.md`
- How to use medication / injection technique → see `how-to-use-medication.md`
- Subscription changes → see `subscriptions.md`
- General clinical routing → see `clinical-routing.md`

**Cross-references:**
- `side-effects.md` — side effects that may prompt dose change requests
- `clinical-routing.md` — general clinical escalation rules
- `subscriptions.md` — subscription management during pauses
- `blood-tests.md` — blood work required before dose adjustments

---

## Critical Rules

⚠️ CRITICAL: ALL dose change requests MUST be escalated to the clinical team. The agent CANNOT approve, recommend, or process any dose changes.

⚠️ CRITICAL: NEVER advise a patient to adjust their own dose — not even to "try a lower dose" or "skip a dose to see how you feel."

⚠️ CRITICAL: If a patient has been off TRT medication for more than 4 weeks, they MUST restart at the lowest titration level. This is a safety requirement.

⚠️ CRITICAL: If a patient reports severe side effects alongside a dose change request, this is a safety concern → confidence = 0.35, safety_flags: ["severe_side_effects"]. See `side-effects.md` Tier 3 for emergency symptoms.

---

## Titration Levels (Reference Only)

These levels are for internal reference. The clinical team determines all dose progression. Patients cannot skip levels.

**Testosterone titration (typical):**
| Level | Description |
|-------|-------------|
| Level 1 | Starting dose (lowest) |
| Level 2 | First increase |
| Level 3 | Second increase |
| Level 4+ | Higher levels as clinically appropriate |

> Exact dose values depend on the specific TRT medication and formulation prescribed. The clinical team sets these per patient.

→ confidence = 0.88 (SOLVE — reference only)
→ "TRT doses are increased gradually through a titration schedule. Your clinical team will guide you through each step based on your blood results and how you're responding to treatment."

---

## Dose Change Requests

### Dose Increase

IF patient requests a dose increase:
  → Call `get_order_history` to confirm current medication and dose
  → confidence = 0.55 (ESCALATE)
  → internal_note: "Patient requesting dose increase. Current medication: [medication from tool data]. Current dose: [dose from tool data]."
  → "Dose increases need to be reviewed and approved by our clinical team based on your blood results and how you're responding to treatment. I've passed this to our clinical team — they'll review your case and be in touch within 24-48 hours."

### Dose Decrease

IF patient requests a dose decrease:
  → Call `get_order_history` to confirm current medication and dose
  → confidence = 0.55 (ESCALATE)
  → internal_note: "Patient requesting dose decrease. Current medication: [medication from tool data]. Current dose: [dose from tool data]. Reason given: [patient's stated reason]."
  → "I understand you'd like to adjust your dose. Our clinical team will need to review this and determine the best approach for you. I've passed this to them — they'll be in touch within 24-48 hours."

### Dose Decrease Due to Side Effects

IF patient requests a dose decrease AND mentions side effects:
  → Call `get_order_history` to confirm current medication and dose
  → Assess side effect severity per `side-effects.md` tiers
  → IF side effects are Tier 1 (common):
    → confidence = 0.55 (ESCALATE)
    → internal_note: "Patient requesting dose decrease due to side effects: [symptoms]. Current medication: [medication]. Current dose: [dose]. Side effects appear Tier 1 (common)."
    → "I'm sorry to hear you're experiencing those side effects. Our clinical team can review your symptoms and determine the best approach — which may include adjusting your dose. I've passed this to them and they'll be in touch within 24-48 hours."
  → IF side effects are Tier 2 or Tier 3:
    → confidence = 0.35 (ESCALATE — urgent)
    → safety_flags: ["severe_side_effects"]
    → internal_note: "URGENT: Patient requesting dose change due to concerning side effects: [symptoms]. Current medication: [medication]. Current dose: [dose]."
    → See `side-effects.md` for appropriate response language

---

## Pause Medication

IF patient asks to pause their TRT medication:
  → Call `get_subscription_status` to confirm active subscription
  → Call `get_order_history` to confirm current medication and dose
  → confidence = 0.55 (ESCALATE)
  → internal_note: "Patient requesting to pause TRT medication. Current medication: [medication]. Current dose: [dose]. Reason: [patient's stated reason]. Note: if pause exceeds 4 weeks, patient will need to restart at lowest titration level."
  → "I understand you'd like to pause your treatment. Our clinical team will need to review this request and advise on the safest approach. I've passed this to them — they'll be in touch within 24-48 hours."
  → Do NOT tell the patient to simply stop taking their medication

---

## Switch Medication

IF patient asks about switching to a different TRT medication:
  → Call `get_order_history` to confirm current medication and dose
  → confidence = 0.55 (ESCALATE)
  → internal_note: "Patient requesting medication switch. Current medication: [medication]. Current dose: [dose]. Requested switch to: [if specified]. Note: switching may require restarting at a lower titration level — clinical team to determine."
  → "Switching TRT medications needs to be reviewed by our clinical team. They'll assess your current treatment and determine the best transition plan. I've passed this to them — they'll be in touch within 24-48 hours."

---

## Treatment Gap Rules

IF patient has been off TRT medication for >4 weeks:
  → confidence = 0.55 (ESCALATE)
  → Flag dose reset requirement in internal note
  → internal_note: "Patient has been off TRT medication for more than 4 weeks. SAFETY REQUIREMENT: Must restart at the lowest titration level. Previous medication: [medication]. Previous dose: [dose]."
  → "Since you've been off your TRT medication for more than 4 weeks, you'll need to restart at the lowest dose for safety reasons. Our clinical team will need to review your case and set up your restart plan. I've passed this to them — they'll be in touch within 24-48 hours."

IF patient has been off TRT medication for 2–4 weeks:
  → confidence = 0.55 (ESCALATE)
  → internal_note: "Patient has been off TRT medication for [duration]. Clinical team to determine safe restart dose. Previous medication: [medication]. Previous dose: [dose]."
  → "Since you've had a gap in treatment, our clinical team will need to review and determine the safest dose for you to restart at. I've passed this to them — they'll be in touch within 24-48 hours."

IF patient has been off TRT medication for <2 weeks:
  → confidence = 0.55 (ESCALATE)
  → internal_note: "Patient has been off TRT medication for less than 2 weeks. Clinical team to confirm restart approach. Previous medication: [medication]. Previous dose: [dose]."
  → "Since you've had a short gap in treatment, our clinical team will confirm the best way to continue. I've passed this to them — they'll be in touch within 24-48 hours."

> Note: Unlike weight loss medications where short gaps can be self-managed, ALL TRT treatment gaps require clinical review due to the hormonal nature of the treatment.

---

## Mandatory Rules

1. The agent CANNOT change, recommend, or approve any dose changes — always escalate to the clinical team.
2. NEVER advise a patient to adjust their own dose for any reason.
3. Always call `get_order_history` to include the patient's current medication and dose in the escalation internal note.
4. If a patient mentions being off medication for >4 weeks, always flag the dose reset safety requirement in the internal note.
5. If a patient reports severe side effects alongside a dose change request → confidence = 0.35, safety_flags: ["severe_side_effects"].
6. Patients cannot skip titration levels — do not imply otherwise.
7. Do NOT tell a patient to stop taking their medication without clinical guidance.

---

## Worked Examples

**Example 1: Dose increase request**
Patient: "I've been on my current TRT dose for 8 weeks and I feel like it's not doing much. Can I go up?"
→ Call `get_order_history` → returns Testosterone Cypionate 100mg
→ confidence = 0.55 (ESCALATE)
→ internal_note: "Patient requesting dose increase. Current medication: Testosterone Cypionate. Current dose: 100mg. Patient reports current dose is not effective after 8 weeks."
→ "Dose increases need to be reviewed and approved by our clinical team based on your blood results and how you're responding to treatment. I've passed this to our clinical team — they'll review your case and be in touch within 24-48 hours."

**Example 2: Dose decrease due to side effects**
Patient: "I'm getting bad acne and mood swings since my dose went up. Can I go back to my old dose?"
→ Call `get_order_history` → returns current medication and dose
→ Side effects: acne (Tier 1-2), mood swings (Tier 2) → assess per `side-effects.md`
→ confidence = 0.55 (ESCALATE)
→ internal_note: "Patient requesting dose decrease due to side effects: acne and mood swings since last dose increase. Current medication: [medication]. Current dose: [dose]. Side effects appear Tier 2 — clinical review needed."
→ "I'm sorry to hear you're experiencing those side effects. Our clinical team can review your symptoms and determine the best approach — which may include adjusting your dose. I've passed this to them and they'll be in touch within 24-48 hours."

**Example 3: Treatment gap >4 weeks**
Patient: "I stopped my TRT about 6 weeks ago because I was travelling. I want to restart — can I go back to the same dose?"
→ Call `get_order_history` → returns previous medication and dose
→ confidence = 0.55 (ESCALATE)
→ internal_note: "Patient has been off TRT medication for approximately 6 weeks. SAFETY REQUIREMENT: Must restart at the lowest titration level. Previous medication: [medication]. Previous dose: [dose]. Reason for gap: travelling."
→ "Since you've been off your TRT medication for more than 4 weeks, you'll need to restart at the lowest dose for safety reasons. Our clinical team will need to review your case and set up your restart plan. I've passed this to them — they'll be in touch within 24-48 hours."

**Example 4: Severe side effects with dose change request**
Patient: "I've been having chest pains and shortness of breath since my dose increased last week. I need to go back down."
→ confidence = 0.35 (URGENT ESCALATE)
→ safety_flags: ["severe_side_effects"]
→ Defer to `side-effects.md` Tier 3 emergency handling — chest pain and breathing difficulty are emergency symptoms
→ Do NOT focus on the dose change — prioritise the safety concern
