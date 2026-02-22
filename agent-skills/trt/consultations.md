# TRT Consultations & Appointments

## Scope
This file covers consultation booking, pre-consultation requirements, missed appointments, and the post-results journey. Use when patient asks about booking, can't find the booking option, missed an appointment, or asks "what happens next?"

## Pre-Consultation Requirements

Before a patient can book their consultation, they must complete:
1. ✅ Two blood tests (initial + enhanced) with confirmed low T
2. ✅ ID verification (photo ID uploaded)
3. ✅ Health profile questionnaire completed
4. ✅ GP consent form completed
5. ✅ Consultation payment (£79.95)

## Decision Tree: Booking Issues

IF patient says "I can't find the booking option" or "booking tab has disappeared":
  → confidence = 0.55 (ESCALATE)
  → This usually means a task has expired or a pre-consultation step is incomplete
  → "It sounds like there may be an incomplete step or an expired task in your portal. I've flagged this for our team to check and reinstate your booking option."

IF patient says "I paid but can't book":
  → confidence = 0.55 (ESCALATE)
  → Check if pre-consultation tasks are incomplete (ID, health profile, GP consent)
  → "I've passed this to our team to check your account. Sometimes the booking option appears once all the pre-consultation steps are completed — they'll make sure everything is in order."

IF patient asks "I've been asked to pay again":
  → confidence = 0.55 (ESCALATE)
  → Likely a blocking task, not a duplicate charge
  → "This doesn't look right. I've flagged it for our team to investigate — you shouldn't need to pay twice."

IF patient asks "what do I need to do before my consultation?":
  → confidence = 0.90 (SOLVE)
  → Provide the pre-consultation checklist above
  → "Before your consultation, please make sure you've completed: [checklist]. You can check your progress in the portal."

IF patient asks about what happens IN the consultation:
  → confidence = 0.90 (SOLVE)
  → "The consultation is a one-to-one video call with one of our specialist clinicians. They'll review all your blood test results, discuss your symptoms and health history, and if appropriate, create a personalised treatment plan for you."

## Decision Tree: Missed Appointments

IF patient missed a consultation appointment:
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry you missed your consultation. I've flagged this with our team and they'll help arrange a new booking for you."
  → Do NOT attempt to rebook directly

IF patient missed a Randox blood test clinic appointment:
  → confidence = 0.85 (SOLVE)
  → "You can reschedule your Randox appointment through their portal using your booking code. If you don't have your booking code, I've also flagged this with our team so they can retrieve it and assist you."

IF patient missed a nurse home visit:
  → confidence = 0.55 (ESCALATE)
  → "I've flagged this with our team and they'll arrange a new nurse visit for you."

## Consultation Rescheduling (Self-Serve)

IF patient wants to reschedule their consultation:
  → confidence = 0.90 (SOLVE)
  → Provide self-serve steps:
    1. Go to the **Home** tab in the app
    2. Find the appointment under the **Upcoming** banner
    3. Tap **Reschedule**
    4. Select a new date and time
    5. Fill in the details and tap **Update Event**

IF patient wants to cancel their consultation:
  → confidence = 0.90 (SOLVE)
  → Provide self-serve steps:
    1. Go to the **Home** tab in the app
    2. Find the appointment under the **Upcoming** banner
    3. Tap **Cancel**

## Post-Results Journey ("What Happens Next?")

IF patient has completed initial test (low T confirmed) and asks "what next?":
  → confidence = 0.90 (SOLVE)
  → "Great that your initial test is done. The next step is the enhanced blood test — this is a comprehensive panel our clinicians need before your consultation."
  → Provide enhanced test options and pricing (see blood-tests.md or trt-pricing.md)

IF patient has completed both tests and asks "what next?":
  → confidence = 0.90 (SOLVE)
  → "Both your blood tests are complete. The next step is to book your consultation with one of our specialist clinicians."
  → Check if pre-consultation tasks are complete
  → Provide consultation cost (£79.95) and booking instructions from portal
  → Consultation booking links (use based on subscription status):
    - Existing patients (active subscription): [Book your follow-up consultation](https://calendly.com/trt-doctors/follow-up-consultation)
    - New patients (no active subscription): [Book your new patient consultation](https://calendly.com/trt-doctors/new-patient-consultation)

IF patient has had their consultation and asks "what next?":
  → confidence = 0.85 (SOLVE)
  → "After your consultation, if TRT is recommended, you'll be set up with a treatment plan and subscription. Your clinician will have outlined the next steps during your consultation."
  → If they seem unclear, escalate for clinical team follow-up

## Webinar Pathway

Some patients may be offered a free educational webinar before their paid consultation. This is a separate event from the clinical consultation.

IF patient asks about a webinar booking or joining link:
  → confidence = 0.85 (SOLVE)
  → "Your webinar confirmation and joining link would have been sent to you by email when you signed up. Please check your inbox, including junk/spam folders, for the confirmation."

IF patient says they missed a webinar:
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry you missed the webinar. I've passed this to our team — they'll be able to send you a recording of the session."
  → Include in internal_note: which webinar, when it was scheduled (if known)

IF patient asks what the webinar covers:
  → confidence = 0.90 (SOLVE)
  → "The webinar is a free educational session that covers the TRT treatment journey, what to expect, and answers common questions. It's separate from your clinical consultation."

## Blood Test Appointments (Clinic vs Nurse)

⚠️ This section covers APPOINTMENT queries for blood test clinic/nurse visits. For blood test KIT queries (delivery, results, etc.), see blood-tests.md.

### Appointment Types
- **Clinic visit (Randox)**: Patient books and visits a partner clinic. Clinic has all supplies — patient just brings ID.
- **Nurse home visit**: Nurse comes to the patient's home with all supplies.
- **Kit-based**: Patient receives a kit at home and either self-collects or books a separate clinic visit.

### Appointment Visibility
Patients can view their past and upcoming appointments in the **Home** tab of their app, showing appointment address, scheduled time, and duration.

### Price Matching Rule
⚠️ Only mention this if the patient brings it up: If the patient's nearest clinic is more than 6 miles away, the nurse home visit price may be matched to the clinic visit price. Do NOT proactively mention this — only confirm if the patient raises the distance issue.

### Rescheduling Blood Test Appointments

IF patient wants to reschedule a clinic or nurse appointment:
  → confidence = 0.55 (ESCALATE)
  → Gather: preferred new date, whether they want to keep the same type (clinic/nurse), any other changes
  → "I've passed your rescheduling request to our team with the details you've provided. They'll arrange this for you."
  → Include in internal_note: current appointment type, requested changes

IF patient wants to cancel a blood test appointment:
  → confidence = 0.55 (ESCALATE)
  → ⚠️ We cannot cancel clinic/nurse appointments on the patient's behalf — but the team can assist
  → "I've flagged your cancellation request with our team. They'll confirm the cancellation and advise on next steps."

## Check-In Consultations (Existing Patients)

IF existing patient asks about a check-in or review consultation:
  → confidence = 0.85 (SOLVE)
  → Check-in consultations are included with the subscription — no extra charge
  → These are typically scheduled after monitoring blood tests
  → "Your check-in consultations are included in your subscription. If you need to book one, check your portal tasks or let me know and I'll get our team to set one up."

IF existing patient says they were asked to complete additional tests before check-in:
  → Check order history — this may be a monitoring blood test (free with subscription)
  → If the system is asking them to BUY a test: likely an error
  → confidence = 0.55 (ESCALATE) if it seems like a system error
