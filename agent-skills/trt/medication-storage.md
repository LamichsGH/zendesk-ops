# TRT Medication Storage & Travel

## Scope

**Covers:**
- Storage guidance for all TRT medications (testosterone, HCG, ancillaries)
- Temperature excursion questions
- Domestic and international travel with medication
- Travel letters and prescription copies
- Sharps disposal and sharps bin replacement

**Does NOT cover:**
- Injection technique or how to draw up medication (see `how-to-use-medication.md`)
- Delivery delays, missing orders, or shipping issues (see `orders-and-shipping.md`)
- Clinical decisions about medication safety after damage or contamination (escalate to clinical team)

**Cross-references:**
- `how-to-use-medication.md` — injection technique, reconstitution steps
- `orders-and-shipping.md` — delivery issues, order timing, address changes

---

## Storage Rules

### Testosterone (Cypionate / Sustanon / Cream)
- Store in a cool, dry place at room temperature (15-25C)
- Avoid heat, humidity, and direct sunlight
- Keep away from radiators, stoves, and windowsills
- Avoid humid environments like bathrooms
- Best location: cupboard or drawer
- Do NOT refrigerate testosterone
- ⚠️ Medication should NEVER be frozen

### HCG
- **Unmixed (powder vial):** cool, dry place at room temperature
- **After reconstitution (mixed):** MUST refrigerate at 2-8C; use within 3 months of mixing
- ⚠️ Medication should NEVER be frozen

### TorBAC Water (Bacteriostatic Water)
- **Unopened:** cool, dry place at room temperature
- **Opened / multi-dose vial:** refrigerate at 2-8C; use within 3 months of opening

### Clomid
- Cool, dry place at room temperature
- Avoid heat, moisture, and direct sunlight
- Best location: cupboard or drawer
- ⚠️ Medication should NEVER be frozen

### Tadalafil
- Cool, dry place at room temperature
- Avoid heat, humidity, and direct sunlight
- Best location: cupboard or bedside drawer
- ⚠️ Medication should NEVER be frozen

### Aromatase Inhibitors (AIs)
- Cool, dry place at room temperature
- Away from sunlight and heat
- Avoid moisture
- ⚠️ Medication should NEVER be frozen

### Statins
- Cool, dry place at room temperature
- ⚠️ Medication should NEVER be frozen

### Needles / Syringes
- Room temperature, clean dry place
- Keep in original packaging until use

---

## Storage Questions — Decision Tree

**"I left my testosterone out in the heat / cold":**
  → confidence = 0.85 (INFORM/REASSURE)
  → Search KB for temperature excursion guidance
  → General rule: brief exposure (a few hours) is usually fine
  → If exposed to extreme heat (>30C) or freezing for an extended period: confidence = 0.55, ESCALATE to clinical team: "Please don't use this until the clinical team has confirmed it's safe."
  → ⚠️ safety_flags: ["temperature_excursion"]

**"My medication looks different / discoloured / has particles":**
  → confidence = 0.55 (ESCALATE)
  → "Please do not use this medication. I've flagged this with our clinical team who will advise on a replacement."
  → ⚠️ safety_flags: ["medication_appearance_concern"]

**"Can I store testosterone in the fridge?":**
  → confidence = 0.90 (SOLVE)
  → "Testosterone should be stored at room temperature (15-25C) rather than in the fridge. A cupboard or drawer away from heat and sunlight is ideal."

**"My HCG was delivered warm / not cold":**
  → confidence = 0.55 (ESCALATE to clinical)
  → "I've flagged this with our clinical team to check whether this is safe to use."
  → ⚠️ safety_flags: ["cold_chain_concern"]

**"How long does HCG last after mixing?":**
  → confidence = 0.90 (SOLVE)
  → "Once mixed, HCG should be kept in the fridge at 2-8C and used within 3 months."

**"Can I freeze my medication?":**
  → confidence = 0.90 (SOLVE)
  → ⚠️ "No — medication should never be frozen. Please store at room temperature (or in the fridge for mixed HCG/opened bacteriostatic water)."

---

## Travel — Decision Tree

### General Travel Guidance
- Recommend ordering treatment at least 14 days before departure
- If patient needs a temporary address change for travel: remind them to update their address BEFORE their order is approved. Once approved, address changes are not possible.
- ⚠️ Voy cannot provide a cooling pouch

### Domestic Travel (UK)

**Patient travelling within the UK:**
  → confidence = 0.90 (SOLVE)
  → Carry medication in hand luggage
  → Keep at appropriate temperature
  → Prescription letter available from Patient Portal — search KB for how to access
  → "We'd recommend having your prescription letter with you when travelling, which you can download from your Patient Portal."

### International Travel

**Patient travelling internationally — by medication type:**
  → confidence = 0.85 (INFORM/REASSURE)
  → Always carry in hand luggage with prescription/travel letter

**Cypionate:**
  → Medication + injection supplies in hand luggage
  → confidence = 0.85 (INFORM/REASSURE)

**Sustanon:**
  → Take 1-2 ampoules depending on trip duration
  → Draw up once arrived at destination
  → ⚠️ CANNOT bring pre-drawn syringes on a plane
  → confidence = 0.85 (INFORM/REASSURE)

**HCG — Long Flight:**
  → Take powder vial + diluent vial unmixed
  → Mix at destination
  → Keep in fridge after mixing
  → confidence = 0.85 (INFORM/REASSURE)

**HCG — Short Flight:**
  → Use cool packs to keep cool during transit
  → ⚠️ Voy cannot provide a cooling pouch
  → confidence = 0.85 (INFORM/REASSURE)

**Clomid / Cream / Tadalafil / AIs / Statins:**
  → Hand luggage is fine; no special temperature requirements
  → confidence = 0.90 (SOLVE)

### Travel Letters

**Patient asks for a travel letter:**
  → confidence = 0.55 (ESCALATE to CS)
  → Collect travel dates + destination from the patient
  → Escalate to customer service team to arrange the travel letter
  → ⚠️ Agent cannot provide travel letters directly — must escalate to CS
  → "I'll pass your travel dates and destination to our customer service team so they can arrange a travel letter for you."

### Prescription Copies

**Patient asks for a prescription copy:**
  → confidence = 0.90 (SOLVE)
  → Call `get_prescription_copy` tool
  → If order not yet approved: advise the patient to reach out again once the order has been approved
  → "I've requested a copy of your prescription for you."

---

## Sharps Disposal — Decision Tree

**"How do I dispose of needles / sharps bin?":**
  → confidence = 0.85 (INFORM/REASSURE)
  → All TRT patients receive a sharps bin as part of their treatment for safe disposal of needles, syringes, glass ampoules, or anything broken
  → When the sharps bin is full, patients can dispose via three options:

  **Option 1 — Local Pharmacy Drop-Off:**
    → Some pharmacies accept sharps bins
    → Advise patient to confirm availability with their local pharmacy in advance

  **Option 2 — Council Collection (FREE):**
    → Arrange via: https://www.gov.uk/request-clinical-waste-collection
    → Patient needs a "Patient Letter" to provide to the council
    → ⚠️ Customer service CANNOT contact the patient's council on their behalf — the patient must arrange this directly and provide the council with the necessary documentation

  **Option 3 — GP Surgery Disposal:**
    → Patient can check with their GP surgery
    → Availability varies by surgery

**"I need a new sharps bin":**
  → confidence = 0.85 (INFORM/REASSURE)
  → Voy provides replacement sharps bins — escalate to CS to order one
  → ⚠️ Only mention that the sharps bin is free if the patient specifically asks about cost
  → "I've flagged this with our team and they'll arrange a replacement sharps bin for you."

**"Do I need to pay for a sharps bin?":**
  → confidence = 0.90 (SOLVE)
  → "No, Voy provides sharps bins free of charge. I've flagged this with our team and they'll get one sent out to you."
  → Escalate to CS to order

**"Where do I put broken glass ampoules?":**
  → confidence = 0.90 (SOLVE)
  → "Broken glass ampoules should go in your sharps bin along with used needles and syringes. Never put them in your normal household waste."

---

## Worked Examples

### Example 1: Patient asking about HCG storage after mixing
> **Patient:** "I mixed my HCG yesterday, where should I keep it?"
>
> **Agent action:** confidence = 0.90 (SOLVE)
> **Response:** "Once mixed, HCG needs to be kept in the fridge at 2-8C. It should be used within 3 months of mixing. A good spot is the main body of the fridge rather than the door, to keep the temperature more consistent."

### Example 2: Patient travelling internationally with Sustanon
> **Patient:** "I'm flying to Spain next month and I'm on Sustanon. How do I take it with me?"
>
> **Agent action:** confidence = 0.85 (INFORM/REASSURE). Collect travel dates + destination for travel letter.
> **Response:** "For Sustanon, take 1-2 ampoules in your hand luggage depending on how long your trip is, and draw up once you've arrived. Please note you can't bring pre-drawn syringes on the plane. I'd also recommend ordering your treatment at least 14 days before departure. I can arrange a travel letter for you — could you confirm your travel dates and destination?"
> **Follow-up:** Escalate to CS with travel dates and destination to arrange travel letter.

### Example 3: Patient needs to dispose of a full sharps bin
> **Patient:** "My sharps bin is full, what do I do with it?"
>
> **Agent action:** confidence = 0.85 (INFORM/REASSURE)
> **Response:** "There are a few options for disposing of a full sharps bin: you can drop it off at your local pharmacy (worth calling ahead to check they accept them), arrange a free council collection at https://www.gov.uk/request-clinical-waste-collection, or check with your GP surgery. For the council collection, you'll need to provide them with a Patient Letter. Would you also like me to request a replacement sharps bin from our team?"

### Example 4: Medication left in a hot car
> **Patient:** "I left my testosterone in the car and it was really hot today — is it still safe?"
>
> **Agent action:** Check severity. Brief exposure = confidence 0.85 (INFORM/REASSURE). Extended extreme heat = confidence 0.55 (ESCALATE).
> **Response (brief exposure):** "If it was just for a few hours, it should generally be fine. Testosterone is quite stable at room temperature. Going forward, try to keep it in a cool, dry place at 15-25C."
> **Response (extended/extreme):** "Given the extended exposure to high temperatures, I'd recommend not using this until our clinical team has confirmed it's safe. I've flagged this for them and they'll be in touch."

### Example 5: Patient asks for a travel letter
> **Patient:** "I need a letter for my medication for when I fly."
>
> **Agent action:** confidence = 0.55 (ESCALATE to CS). Collect travel dates + destination.
> **Response:** "I can help with that. Could you let me know your travel dates and destination? I'll pass those details to our customer service team and they'll arrange a travel letter for you."
