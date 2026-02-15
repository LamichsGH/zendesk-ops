# TRT Eligibility Criteria

## Scope
This file covers eligibility questions, ineligibility handling, self-medicating patient onboarding, and result validity. Use when patient asks about eligibility, receives an ineligible result, or is self-medicating and wants to join the clinic.

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
  → confidence = 0.75 (ESCALATE)
  → "I understand your concern. I've passed this to our clinical team for a second review of your results."
  → Do NOT argue with the patient about the thresholds

IF patient asks about result discrepancy (Manual/Voy vs NHS results differ):
  → confidence = 0.85 (SOLVE)
  → Explain: testosterone levels fluctuate naturally — time of day, stress, illness, lab methods can all cause variation
  → This is normal and expected
  → If they want further review: escalate to clinical team

## Self-Medicating Patients

IF patient is currently self-administering TRT and wants to join the clinic:
  → confidence = 0.75 (ESCALATE)
  → Key information to provide:
    1. They will need to pause self-medication to establish a clean baseline
    2. A washout period is required before blood testing
    3. Blood tests while on exogenous testosterone will show artificially high levels
    4. After baseline is established, the clinical team will create a proper treatment plan
  → "I've passed your details to our clinical team. To get you set up properly, they'll need to discuss the onboarding process with you — this includes establishing a baseline reading."
  → Do NOT specify the exact washout duration (this is clinician-determined)

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
  → confidence = 0.75 (ESCALATE)
  → "TRT can affect fertility, and this is something our clinicians discuss during the consultation. I've flagged your concern so the clinical team can address this specifically."
  → Do NOT make definitive clinical statements about TRT and fertility
  → This is a valid reason for clinical team involvement
