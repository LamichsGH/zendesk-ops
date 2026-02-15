# Medication Storage

## Scope

This file covers medication storage requirements, travel guidance, sharps disposal, and prescription copies for weight loss medications.

**Covers:**
- Wegovy storage (fridge and room temperature rules)
- Mounjaro storage (fridge and room temperature rules)
- Travel with medication
- Sharps and pen disposal
- Requesting prescription copies

**Does NOT cover:**
- How to use medication / injection technique → see `how-to-use-medication.md`
- Cold chain concerns on delivery → see `orders-and-shipping.md`
- Side effects → see `side-effects.md`

**Cross-references:**
- `how-to-use-medication.md` — injection steps and pen usage
- `orders-and-shipping.md` — cold chain safety on arrival

---

## Standard Storage

IF patient asks about medication storage:
  → confidence = 0.90 (INFORM)
  → Call `get_order_history` to identify the patient's medication, then provide the relevant guidance below.

### Wegovy Storage

- **Before first use:** Always store in the refrigerator (2°C–8°C). Let warm to room temperature for 30 minutes before injecting.
- **Room temperature:** Can be stored at room temp (up to 30°C) for up to 6 weeks from when the pen was shipped.
- **42-day rule:** Once the pen has been at room temperature at any point, the total time limit is 42 days from when it first reached room temperature. This countdown does NOT reset if refrigerated again.
- **Refrigerated (unopened):** Up until the expiry date on the pen.

### Mounjaro Storage

- **Before first use:** Always store in the refrigerator (2°C–8°C). Let warm to room temperature for 30 minutes before injecting.
- **Room temperature (before first dose):** Can be stored at room temp (up to 30°C) for up to 30 days from when the pen was shipped.
- **After first dose:** The pen expires 30 days from the first dose, whether stored in the fridge (2–8°C) or at room temperature (up to 30°C).
- **Refrigerated (unopened):** Up until the expiry date on the pen.

### General Rules (Both Medications)

- ⚠️ NEVER freeze medication
- Protect from light — keep pen cap on
- Discard after room temp expiry period or label expiry date (whichever comes first)
- Always check the pen's colour and clarity before use — solution must be clear and colourless (Wegovy) or clear to slightly yellow (Mounjaro)
- If patient reports temperature-related concerns or damage → see `orders-and-shipping.md` cold chain section

---

## Travel with Medication

IF patient asks about travelling with their medication:
  → confidence = 0.88 (INFORM)

→ "Both Wegovy and Mounjaro can be safely transported at room temperature (up to 30°C). Here's what to keep in mind:

1. **Pack in hand luggage** — never in checked bags
2. **Prescription copy** — carry one if travelling internationally (available on request)
3. **Room temperature stability** — Wegovy is stable for up to 6 weeks, Mounjaro for up to 30 days

Would you like me to provide a copy of your prescription for travel?"

IF patient wants a cooling pouch or insulin travel bag:
  → confidence = 0.90 (INFORM)
  → "Unfortunately, we're unable to provide cooling pouches or insulin travel bags. These can be purchased from most pharmacies or online."

IF patient requests a prescription copy for travel:
  → Call `get_prescription_copy` tool
  → Provide the URL(s) to the patient
  → "Here's a downloadable link to your prescription: [URL]. This corresponds to your [medication name]."

⚠️ Prescription URLs always refer to the latest delivered order. If the patient needs an older prescription, escalate to customer service.

---

## Sharps and Pen Disposal

IF patient asks about disposing of needles:
  → confidence = 0.90 (INFORM)

→ "After each injection:
1. Carefully replace the outer needle shield
2. Unscrew the capped needle
3. Place used needles in your sharps disposal container immediately"

### Pen Disposal

→ "After removing the needle, you can discard your pen in household waste or in your sharps container."

### Pen Recycling

- **Mounjaro pens:** Currently NOT recyclable — cannot be refilled, and trace medication means they cannot be recycled
- **Wegovy pens:** Can be recycled by the manufacturer. Request a free dedicated recycling box via the Wegovy recycling programme

### Sharps Bin Disposal

→ "When your sharps bin is full, you have several disposal options:
1. **Council collection (free):** Arrange a free clinical waste collection through your local council at gov.uk/request-clinical-waste-collection
2. **Pharmacy drop-off:** Some local pharmacies accept sharps bins — contact yours in advance to confirm
3. **GP surgery:** Some GP surgeries accept sharps bins — check with yours beforehand"

### Ordering a New Sharps Bin

IF patient needs a new sharps bin:
  → confidence = 0.55 (ESCALATE)
  → "I can connect you with our team to arrange a new sharps bin for you."
  → ⚠️ Only mention it's free if the patient asks about cost — do not proactively state this

---

## Worked Examples

**Example 1: Wegovy storage question**
Patient: "Can I keep my Wegovy pen out of the fridge?"
→ confidence = 0.90 (INFORM)
→ "Yes, Wegovy can be stored at room temperature (up to 30°C) for up to 6 weeks from when it was shipped. However, once it's been at room temperature, the 42-day countdown begins and doesn't reset even if you put it back in the fridge. For longer storage, keep it refrigerated (2–8°C) until use."

**Example 2: Travel query**
Patient: "I'm going on holiday next week. Can I take my Mounjaro?"
→ confidence = 0.88 (INFORM)
→ "Yes, you can travel with your Mounjaro. Pack it in your hand luggage (not checked bags) and it will stay stable at room temperature for up to 30 days. If you're travelling internationally, I'd recommend carrying a copy of your prescription. Would you like me to provide one?"

**Example 3: Sharps disposal**
Patient: "My sharps bin is full. What do I do?"
→ confidence = 0.90 (INFORM)
→ "You have a few options for disposing of a full sharps bin: (1) Arrange a free council collection at gov.uk/request-clinical-waste-collection, (2) Drop it off at a local pharmacy that accepts sharps bins, or (3) Check if your GP surgery provides disposal. Would you also like a new sharps bin? I can connect you with our team to arrange one."
