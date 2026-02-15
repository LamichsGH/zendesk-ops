# TRT Medication Storage & Travel

## Storage Rules

**Testosterone (all forms):**
- Room temperature: 15–25°C
- Away from direct light and heat
- Do NOT refrigerate testosterone

**HCG (after reconstitution/mixing):**
- MUST refrigerate: 2–8°C
- Use within 30 days of mixing (verify via KB)
- Before mixing: can store at room temperature

**Needles/syringes:**
- Room temperature, clean dry place
- Keep in original packaging until use

## Common Questions — Decision Tree

**"I left my testosterone out in the heat / cold":**
  → confidence = 0.85 (SOLVE)
  → Search KB for temperature excursion guidance
  → General rule: brief exposure (a few hours) is usually fine
  → If exposed to extreme heat (>30°C) or freezing for an extended period: confidence = 0.55, escalate to clinical team: "Please don't use this until the clinical team has confirmed it's safe."

**"My medication looks different / discoloured / has particles":**
  → confidence = 0.55 (ESCALATE)
  → "Please do not use this medication. I've flagged this with our clinical team who will advise on a replacement."
  → safety_flags: ["medication_appearance_concern"]

**"Can I store testosterone in the fridge?":**
  → confidence = 0.90 (SOLVE)
  → "Testosterone should be stored at room temperature (15–25°C) rather than in the fridge."

**"My HCG was delivered warm / not cold":**
  → confidence = 0.55 (ESCALATE to clinical)
  → "I've flagged this with our clinical team to check whether this is safe to use."
  → safety_flags: ["cold_chain_concern"]

## Travel Decision Tree

**Domestic travel (UK):**
  → confidence = 0.90 (SOLVE)
  → Carry in hand luggage
  → Keep at appropriate temperature
  → Prescription letter available from Patient Portal — search KB for how to access

**International travel:**
  → confidence = 0.85 (SOLVE)
  → Carry in hand luggage with prescription letter
  → Search KB for travel letter / prescription copy guidance
  → For HCG: use cool bag with ice pack
  → "We'd recommend downloading your prescription letter from your Patient Portal before you travel."

**Patient asks for travel letter / prescription letter:**
  → Use get_prescription_copy tool if patient explicitly asks for a copy
  → Otherwise: direct to Patient Portal with KB instructions
  → confidence = 0.90 (SOLVE)

## Needle Disposal
- "How do I dispose of needles?"
  → confidence = 0.85 (SOLVE)
  → Search KB for sharps disposal guidance
  → General advice: "Your local pharmacy can provide a sharps bin for safe disposal of needles."
