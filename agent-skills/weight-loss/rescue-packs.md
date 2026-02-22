# Rescue Packs

## Scope

This skill handles patient queries about rescue pack care packages (Cyclizine and Loperamide) provided with first orders of Wegovy or Mounjaro.

**Covers:**
- What rescue packs contain and when they're provided
- Cyclizine (anti-nausea) usage, dosage, contraindications, and side effects
- Loperamide (anti-diarrhoea) usage, dosage, contraindications, and side effects
- Reorder requests for rescue pack medications

**Does NOT cover:**
- Other side effect management strategies (see `side-effects.md`)
- General medication questions (see `how-to-use-medication.md`)
- Prescription modifications (see `dose-changes.md`)

**Cross-references:**
- `side-effects.md` — for nausea/diarrhoea management beyond rescue packs
- `how-to-use-medication.md` — for general medication safety information

---

## Decision Trees

### General Rescue Pack Questions

**IF** patient asks what rescue packs are:
→ confidence = 0.95 (SOLVE)
→ "Your rescue pack is a one-time care package included with your FIRST order of Wegovy or Mounjaro. It contains Cyclizine (anti-nausea tablets) and Loperamide (anti-diarrhoea capsules) to help manage common side effects if they occur. You'll only receive this pack with your first order, not with subsequent refills."

**IF** patient asks why they didn't receive a rescue pack with their second/third/later order:
→ confidence = 0.93 (SOLVE)
→ "Rescue packs are only provided with your first medication order. If you've run out of Cyclizine or Loperamide and need additional supplies, I can connect you with a clinician who can assess your needs and arrange this for you."

---

## Cyclizine (Anti-Nausea)

### Usage and Dosage

**IF** patient asks how to use Cyclizine:
→ confidence = 0.94 (SOLVE)
→ "Cyclizine helps ease nausea by calming the signals in your brain that trigger sickness. Take one 50mg tablet up to three times daily as needed. Always leave at least 4 hours between doses and do not exceed three tablets in 24 hours. Please note that Cyclizine contains lactose."

**IF** patient asks about Cyclizine side effects:
→ confidence = 0.92 (SOLVE)
→ "Cyclizine may cause drowsiness or dizziness. Take extra care when driving or operating machinery. Avoid alcohol while taking Cyclizine as it can increase these effects."

### Cyclizine Contraindications

⚠️ CRITICAL: Always screen for contraindications before confirming Cyclizine safety.

**IF** patient mentions they have glaucoma, epilepsy, phaeochromocytoma, or urinary retention:
→ confidence = 0.45 (ESCALATE)
→ safety_flags: ["contraindication_check"]
→ "Based on your medical history, I need to connect you with a clinician to ensure Cyclizine is safe for you to use. They'll be able to provide personalized guidance."

**IF** patient mentions taking Gentamicin, Neomycin, or sedatives:
→ confidence = 0.42 (ESCALATE)
→ safety_flags: ["contraindication_check"]
→ "The medications you're taking may interact with Cyclizine. Let me connect you with a clinician who can review your medications and advise on the safest approach."

**IF** patient reports blurred vision, confusion, or other rare side effects from Cyclizine:
→ confidence = 0.38 (ESCALATE)
→ safety_flags: ["contraindication_check"]
→ "These symptoms need clinical assessment. I'm connecting you with a clinician right away to ensure you receive appropriate care."

---

## Loperamide (Anti-Diarrhoea)

### Usage and Dosage

**IF** patient asks how to use Loperamide:
→ confidence = 0.94 (SOLVE)
→ "Loperamide treats diarrhoea by slowing gut movement, which allows your body to absorb more water. Take two 2mg capsules after your first loose bowel movement, then one capsule after each further episode. Do not exceed 8 capsules in 24 hours. Stay well hydrated while using Loperamide. Please note it contains lactose."

**IF** patient asks how long they can use Loperamide:
→ confidence = 0.91 (SOLVE)
→ "Only use Loperamide for up to 48 hours unless advised otherwise by a clinician. If your diarrhoea persists beyond 2 days despite treatment, you'll need clinical assessment."

### Loperamide Contraindications

⚠️ CRITICAL: Screen for serious symptoms and medication interactions.

**IF** patient mentions fever, blood in stool, or severe stomach pain:
→ confidence = 0.35 (ESCALATE)
→ safety_flags: ["contraindication_check", "symptom_severity"]
→ "These symptoms require immediate clinical assessment. I'm connecting you with a clinician right away to ensure you receive appropriate care."

**IF** patient mentions taking Itraconazole, Ketoconazole, or Desmopressin:
→ confidence = 0.43 (ESCALATE)
→ safety_flags: ["contraindication_check"]
→ "The medications you're taking may interact with Loperamide. Let me connect you with a clinician who can review your medications and provide safe guidance."

**IF** patient reports diarrhoea lasting more than 2 days despite Loperamide treatment:
→ confidence = 0.40 (ESCALATE)
→ safety_flags: ["symptom_severity"]
→ "Since your diarrhoea has persisted for more than 2 days, this needs clinical review. I'm connecting you with a clinician to assess your symptoms and adjust your treatment plan if needed."

---

## Reorder Requests

⚠️ Agent cannot directly reorder rescue pack medications.

**IF** patient requests more Cyclizine or Loperamide (has run out):
→ confidence = 0.48 (ESCALATE to clinician, NOT customer service)
→ "I can connect you with a clinician who can assess whether you need additional supplies of Cyclizine or Loperamide and arrange this for you."

---

## Worked Examples

**Example 1: Basic Cyclizine usage**
Patient: "How do I take the anti-nausea tablets?"
Agent confidence: 0.94
Response: "Cyclizine helps ease nausea by calming the signals in your brain that trigger sickness. Take one 50mg tablet up to three times daily as needed. Always leave at least 4 hours between doses and do not exceed three tablets in 24 hours. Please note that Cyclizine contains lactose."

**Example 2: Contraindication escalation**
Patient: "I have glaucoma. Is it safe to take Cyclizine?"
Agent confidence: 0.45 (ESCALATE)
Safety flags: ["contraindication_check"]
Response: "Based on your medical history, I need to connect you with a clinician to ensure Cyclizine is safe for you to use. They'll be able to provide personalized guidance."

**Example 3: Reorder request**
Patient: "I've run out of the anti-diarrhoea capsules. Can I get more?"
Agent confidence: 0.48 (ESCALATE to clinician)
Response: "I can connect you with a clinician who can assess whether you need additional supplies of Cyclizine or Loperamide and arrange this for you."
