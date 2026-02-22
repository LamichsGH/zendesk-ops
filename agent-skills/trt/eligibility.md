# TRT Eligibility Criteria

## Scope
This file covers eligibility questions, ineligibility handling, self-medicating patient onboarding, and result validity. Use when patient asks about eligibility, receives an ineligible result, or is self-medicating and wants to join the clinic.

## Pre-Screening Red Flags (CHECK FIRST)

⚠️ These are HARD STOPS. If ANY of these apply, the patient is NOT eligible for TRT. These override all other eligibility criteria.

IF patient is under 18 years old:
  → confidence = 0.35 (ESCALATE, send_response = false)
  → safety_flags: ["underage_patient"]
  → "Unfortunately, our TRT service is only available to patients aged 18 and over."

IF patient resides outside mainland UK:
  → confidence = 0.35 (ESCALATE, send_response = false)
  → safety_flags: ["location_ineligible"]
  → "Our TRT service is currently only available to patients based in mainland UK."

IF patient has active prostate, breast, or liver cancer:
  → confidence = 0.35 (ESCALATE, send_response = false)
  → safety_flags: ["cancer_contraindication"]
  → Do NOT provide any clinical reasoning. Simply: "Based on what you've mentioned, I've passed your query to our clinical team who will be best placed to advise you."

IF patient has severely high or uncontrolled blood pressure:
  → confidence = 0.35 (ESCALATE, send_response = false)
  → safety_flags: ["uncontrolled_bp"]
  → Do NOT list specific BP thresholds. Simply: "Because you've mentioned some existing conditions, I've passed this to our clinical team to review and advise on the best options for you."

⚠️ For cancer and blood pressure flags: follow the Clinical Tone Rule from tone-and-formatting.md — keep responses reassuring and general. Do NOT list specific contraindications or explain WHY these conditions are exclusions.

### Detecting Red Flags in Email Context
Since the patient won't explicitly answer screening questions in a single email, look for these signals:
- Age mentions: "I'm 17", "my son wants to...", school/college context
- Location: Non-UK addresses, mentions of other countries, international phone formats
- Medical history mentions: "I have prostate cancer", "being treated for...", "currently undergoing chemo"
- BP mentions: "my blood pressure is very high", "uncontrolled hypertension"

IF red flag signals are ambiguous (e.g., unclear age, might be UK-based):
  → Do NOT assume ineligibility
  → confidence = 0.70 (MONITOR)
  → Ask ONE clarifying question: "Just to confirm, are you based in the UK and over 18?"
  → awaiting_customer_input = true

## Hard Eligibility Rules

### Total Testosterone Threshold
- **Total T > 18 nmol/L = INELIGIBLE** — this is a hard threshold, regardless of symptoms
- **Total T ≤ 18 nmol/L = potentially eligible** — requires further assessment
- Free T alone does NOT determine eligibility if Total T is above threshold

### Biological Sex Requirement
- TRT service is for **biological males only**
- IF patient is female, on HRT, or gender context suggests this isn't appropriate:
  → confidence = 0.55 (ESCALATE)
  → "Our TRT service is designed for biological males. I've passed your query to our clinical team who can advise on the best options for you."

### Age Requirement
- Must be 18+ to use the service

## Decision Tree

IF patient asks "am I eligible for TRT?":
  → confidence = 0.85 (SOLVE)
  → "Eligibility is determined by your blood test results. You'll need two blood tests showing consistently low testosterone (Total T ≤ 18 nmol/L), followed by a consultation with one of our specialist clinicians."

IF patient received an "ineligible" result:
  → confidence = 0.85 (SOLVE)
  → Acknowledge their frustration empathetically
  → Explain: "Your blood test showed your testosterone levels are within the normal range, which means TRT wouldn't be appropriate at this time."
  → Suggest: GP referral for symptom investigation, lifestyle factors that affect T levels
  → Do NOT make clinical claims about what their levels "should" be

IF patient has low Free T but normal Total T (>18):
  → confidence = 0.85 (SOLVE)
  → "While your Free Testosterone is on the lower side, your Total Testosterone is within the normal range (above 18 nmol/L). Our clinicians use Total T as the primary eligibility marker, so TRT wouldn't be recommended at this stage."
  → Suggest GP referral for further investigation of symptoms

IF patient disputes their results or wants a second opinion:
  → confidence = 0.55 (ESCALATE)
  → "I understand your concern. I've passed this to our clinical team for a second review of your results."
  → Do NOT argue with the patient about the thresholds

IF patient asks about result discrepancy (Manual/Voy vs NHS results differ):
  → confidence = 0.85 (SOLVE)
  → Explain: testosterone levels fluctuate naturally — time of day, stress, illness, lab methods can all cause variation
  → This is normal and expected
  → If they want further review: escalate to clinical team

## Self-Medicating Patients

IF patient is currently self-administering TRT and wants to join the clinic:
  → confidence = 0.55 (ESCALATE)
  → Key information to provide:
    1. They will need to pause self-medication to establish a clean baseline
    2. A washout period is required before blood testing
    3. Blood tests while on exogenous testosterone will show artificially high levels
    4. After baseline is established, the clinical team will create a proper treatment plan
  → "I've passed your details to our clinical team. To get you set up properly, they'll need to discuss the onboarding process with you — this includes establishing a baseline reading."
  → Do NOT specify the exact washout duration (this is clinician-determined)

### Switching from Another Provider

IF patient is currently with another TRT provider (NHS, private GP, or international clinic) and wants to switch:
  → confidence = 0.55 (ESCALATE)
  → Key information to provide:
    1. If NOT currently enrolled with their provider: must provide blood test results from previous provider
    2. If currently enrolled: must provide a treatment letter containing:
       - Exact name of TRT medication
       - Prescribed doses (with prescribing doctor's details and registration number)
       - PDF copies of blood test results
       - Start and end dates of treatment
    3. All evidence must be shared in PDF format via email
  → "I've passed your details to our clinical team. They'll review your previous treatment history and advise on the onboarding process."

IF patient asks about discontinuation protocol (how to safely stop self-medication):
  → confidence = 0.55 (ESCALATE)
  → This is a clinical question — ALWAYS escalate
  → "Stopping TRT needs to be done carefully under medical guidance. I've passed this to our clinical team who can advise you."

## Result Validity

- Blood test results are valid for **6 months** from the date of the test
- IF patient has results older than 6 months: they will need to retest
  → confidence = 0.85 (SOLVE)
  → "Blood test results are valid for 6 months. As yours are older than that, you'd need a new blood test for an accurate assessment."

IF patient has recent external results (within 6 months):
  → See blood-tests.md "External Results Acceptance" section

## Fertility Concerns

IF patient mentions fertility concerns about TRT:
  → confidence = 0.55 (ESCALATE)
  → "TRT can affect fertility, and this is something our clinicians discuss during the consultation. I've flagged your concern so the clinical team can address this specifically."
  → Do NOT make definitive clinical statements about TRT and fertility
  → This is a valid reason for clinical team involvement
