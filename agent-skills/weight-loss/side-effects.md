# Weight Loss Side Effects

## Scope

This file covers side effect triage, severity classification, and concurrent medication guidance for GLP-1 weight loss medications (Wegovy and Mounjaro).

**Covers:**
- 3-tier side effect severity classification
- Common GLP-1 side effects (Tier 1)
- Concerning side effects requiring clinical review (Tier 2)
- Emergency symptoms (Tier 3)
- Concurrent medication rules
- Injection site reactions

**Does NOT cover:**
- Rescue pack medications (Cyclizine/Loperamide) → see `rescue-packs.md`
- Dose changes due to side effects → see `dose-changes.md`
- Medication usage instructions → see `how-to-use-medication.md`

**Cross-references:**
- `rescue-packs.md` — Cyclizine (anti-nausea) and Loperamide (anti-diarrhoea) usage
- `dose-changes.md` — titration and dose adjustment requests
- `how-to-use-medication.md` — injection technique and pen usage

---

## CRITICAL SAFETY — EMERGENCY (Tier 3)

Any mention of these → confidence = 0.35, send_response = false:
- Symptoms of thyroid tumours (lump in neck, difficulty swallowing, persistent hoarseness)
- Severe allergic reaction (swelling of face/lips/tongue, difficulty breathing, rapid heartbeat)
- Signs of anaphylaxis
- Pancreatitis symptoms (severe persistent abdominal pain radiating to the back, with vomiting)
- Suicidal thoughts or severe mood disturbance
- Kidney problems (changes in urination, swelling in legs/feet)
- Gallbladder issues (severe upper stomach pain, fever, yellowing of skin/eyes)
- safety_flags: ["clinical_emergency"]

⚠️ CRITICAL: NEVER tell patient to "just stop taking medication" — all medication changes must go through the clinical team.

---

## Side Effect Severity Triage

### Tier 1 — Common/Expected (SOLVE)

These are well-documented GLP-1 side effects. Provide reassurance and management tips.

- Nausea (especially in first weeks or after dose increase)
- Constipation
- Diarrhoea
- Injection site reactions (redness, mild pain, bruising)
- Headache
- Fatigue or tiredness
- Bloating or gas
- Mild dizziness

→ confidence = 0.85 (SOLVE)
→ "This is a commonly reported side effect when starting GLP-1 medication or after a dose increase. It usually improves within the first few weeks as your body adjusts."
→ Always add: "If this persists or worsens, our clinical team can review your treatment — just reply to this email and we'll arrange it."

### Tier 2 — Needs Clinical Review (ESCALATE)

- Persistent or severe nausea/vomiting lasting more than a few days
- Inability to eat or drink due to nausea
- Persistent diarrhoea beyond 48 hours
- Significant fatigue affecting daily activities
- Hair thinning or hair loss
- Persistent heartburn or acid reflux
- Rapid heart rate
- Changes in vision
- Depression or significant mood changes

→ confidence = 0.55 (ESCALATE to clinical team)
→ Provide general reassurance but emphasise clinical review is needed
→ "I'd like to connect you with our clinical team so they can review your symptoms and adjust your treatment if needed."
→ ⚠️ Do NOT tell patient to stop medication

### Tier 3 — Emergency

→ See CRITICAL SAFETY section above

---

## Decision Logic

IF patient describes a side effect:
  1. Is it Tier 3 (emergency)? → immediate escalate, no response
  2. Is it Tier 2 (concerning)? → escalate to clinical with reassurance
  3. Is it Tier 1 (common)? → solve with management tips
  4. If unclear which tier → default to Tier 2 (escalate to be safe)

IF patient asks "is [X] normal?":
  → Tier 1: "Yes, [X] is commonly reported with GLP-1 medications..." (SOLVE)
  → Tier 2: "Some patients do experience this, but it's worth having our clinical team review." (ESCALATE)
  → Not listed: "I want to make sure you get accurate advice, so I'd like to connect you with our clinical team." (ESCALATE)

IF patient asks about stopping or changing medication due to side effects:
  → confidence = 0.55 (ESCALATE)
  → "Please don't stop your medication without speaking to your clinical team first. I've flagged this so they can review and advise on the best approach."
  → ⚠️ NEVER advise stopping or changing medication

---

## Concurrent Medication Rules

⚠️ Agent CANNOT provide definitive clinical advice on drug interactions. Always offer to connect to clinical team for specific guidance.

### Medication Mechanism Reference

Understanding how Voy's medications work helps contextualise interaction risks:
- **Mounjaro (tirzepatide):** Dual GIP/GLP-1 receptor agonist. Weekly subcutaneous injection. Regulates blood glucose by enhancing insulin secretion, suppressing glucagon release, and slowing gastric emptying. Reduces food intake by acting on appetite centres in the brain.
- **Wegovy (semaglutide):** GLP-1 receptor agonist. Weekly subcutaneous injection. Mimics the incretin hormone GLP-1 — increases insulin production, decreases glucagon secretion, delays gastric emptying, and promotes satiety through central nervous system pathways.
- **Orlistat (Orlos):** Lipase inhibitor. Taken orally three times daily with meals. Works locally in the GI tract by binding to gastric and pancreatic lipases, preventing hydrolysis of approximately 30% of ingested triglycerides. Blocks fat absorption, leading to fat excretion and reduced caloric absorption.

### "Same Medications Previously Approved" Rule

IF patient confirms they are taking the same medications that were previously approved while on Mounjaro, and they have had no other changes to their medical history:
  → confidence = 0.88 (SOLVE)
  → They can be reassured it is safe to continue taking those medications with Wegovy
  → You do not need to ask for more details

→ "If these are the same medications that were previously reviewed and approved alongside your Mounjaro treatment, and there have been no other changes to your medical history, it's safe to continue taking them with Wegovy."

⚠️ This rule ONLY applies when:
1. The patient explicitly confirms they are taking the **same medications** as before
2. They were previously approved while on Mounjaro
3. They confirm **no other changes** to their medical history

IF any of these conditions are not met → treat as a new medication query (ESCALATE at 0.55).

### New or Unconfirmed Medication Queries

IF patient asks about a new medication or supplement, OR the "same meds" rule does not apply:
  → confidence = 0.55 (ESCALATE)
  → Ask which specific medication or supplement they are referring to
  → Ask them to disclose all medications they are currently taking (unless already clear)
  → Provide general guidance (see below), then offer clinical review

### Orlistat-Specific Interactions

IF patient is on Orlistat and asks about interactions:
  → confidence = 0.55 (ESCALATE)
  → Orlistat can reduce absorption of fat-soluble vitamins (A, D, E, K) and some oral medications
  → Always escalate Orlistat interaction queries to the clinical team

### General Advice (never make patient-specific)
- GLP-1 medications slow stomach emptying, which may affect oral medication absorption
- **Timing rule:** Take time-sensitive oral medications (e.g., thyroid medications, contraceptives) at least 2 hours before GLP-1 injection
- Common supplements (multivitamins, vitamin D, calcium, omega-3s) are generally considered safe, but always best checked with a clinician
- Over-the-counter medications (e.g., paracetamol) are typically well tolerated but best checked with a clinician

### Hard Rules
- ⚠️ NEVER take multiple weight loss medications simultaneously
- ⚠️ Do NOT combine with insulin or sulfonylureas without clinical supervision
- ⚠️ Avoid starting new prescription medications without informing Voy's clinical team
- ⚠️ Always offer to connect to the clinical team for specific guidance on individual medications

---

## Worked Examples

**Example 1: Common nausea**
Patient: "I feel really nauseous after my Wegovy injection."
→ confidence = 0.85 (SOLVE — Tier 1)
→ "Nausea is one of the most commonly reported side effects with Wegovy, especially during the first few weeks or after a dose increase. It usually improves as your body adjusts. Eating smaller, lighter meals and staying hydrated can help. Your rescue pack also includes Cyclizine (anti-nausea tablets) if needed. If the nausea persists or worsens, our clinical team can review and advise."

**Example 2: Persistent vomiting**
Patient: "I've been vomiting every day for the past week since my dose increase."
→ confidence = 0.55 (ESCALATE — Tier 2)
→ "I'm sorry you're experiencing this. Persistent vomiting lasting more than a few days needs clinical review, especially after a dose change. I'd like to connect you with our clinical team so they can assess your symptoms and adjust your treatment if needed."

**Example 3: Concurrent medication — new medication**
Patient: "I just started taking a new blood pressure medication. Is it safe with Mounjaro?"
→ confidence = 0.55 (ESCALATE)
→ "GLP-1 medications like Mounjaro can slow stomach emptying, which may affect how some oral medications are absorbed. For new prescription medications, I'd recommend connecting you with our clinical team who can review the specific interaction and provide personalised guidance. I've passed this to our patient care team. They'll be in touch within 24-48 hours."

**Example 4: Same medications — Mounjaro to Wegovy switch**
Patient: "I've switched from Mounjaro to Wegovy. I'm still taking the same blood pressure and thyroid medication that was approved when I started Mounjaro. Is that okay?"
→ confidence = 0.88 (SOLVE)
→ "If these are the same medications that were previously reviewed and approved alongside your Mounjaro treatment, and there have been no other changes to your medical history, it's safe to continue taking them with Wegovy. Just remember to take your thyroid medication at least 2 hours before your Wegovy injection, as GLP-1 medications can slow stomach emptying and may affect oral medication absorption."

**Example 5: Same medications but with changes**
Patient: "I was on Mounjaro and now I'm on Wegovy. I'm on the same meds as before, but my GP also added a new antibiotic last week."
→ confidence = 0.55 (ESCALATE)
→ "Since there's been a change to your medications with the new antibiotic, I'd like our clinical team to review the full picture and confirm everything is safe to take together. I've passed this to our patient care team. They'll be in touch within 24-48 hours."

**Example 6: Orlistat interaction query**
Patient: "I'm taking Orlistat and my GP wants to put me on a new vitamin supplement. Will they interact?"
→ confidence = 0.55 (ESCALATE)
→ "Orlistat can affect the absorption of some vitamins and oral medications, so it's important to have this reviewed. I've passed this to our clinical team so they can provide specific guidance. They'll be in touch within 24-48 hours."

**Example 7: Multiple weight loss medications**
Patient: "Can I take Orlistat alongside my Wegovy injection to speed things up?"
→ confidence = 0.55 (ESCALATE)
→ "It's important not to take multiple weight loss medications at the same time without clinical supervision. I've flagged this with our clinical team so they can advise on the best approach for your treatment. They'll be in touch within 24-48 hours."
