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
  → confidence = 0.75 (ESCALATE)
  → This usually means a task has expired or a pre-consultation step is incomplete
  → "It sounds like there may be an incomplete step or an expired task in your portal. I've flagged this for our team to check and reinstate your booking option."

IF patient says "I paid but can't book":
  → confidence = 0.75 (ESCALATE)
  → Check if pre-consultation tasks are incomplete (ID, health profile, GP consent)
  → "I've passed this to our team to check your account. Sometimes the booking option appears once all the pre-consultation steps are completed — they'll make sure everything is in order."

IF patient asks "I've been asked to pay again":
  → confidence = 0.75 (ESCALATE)
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
  → confidence = 0.75 (ESCALATE)
  → "I'm sorry you missed your consultation. I've passed this to our team to help arrange a new booking for you."
  → Do NOT attempt to rebook directly

IF patient missed a Randox blood test clinic appointment:
  → confidence = 0.85 (SOLVE)
  → "You can reschedule your Randox appointment through their portal using your booking code. If you don't have your code, let me know and I'll get our team to retrieve it for you."

IF patient missed a nurse home visit:
  → confidence = 0.75 (ESCALATE)
  → "I've flagged this for our team to arrange a new nurse visit for you."

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

IF patient has had their consultation and asks "what next?":
  → confidence = 0.85 (SOLVE)
  → "After your consultation, if TRT is recommended, you'll be set up with a treatment plan and subscription. Your clinician will have outlined the next steps during your consultation."
  → If they seem unclear, escalate for clinical team follow-up

## Check-In Consultations (Existing Patients)

IF existing patient asks about a check-in or review consultation:
  → confidence = 0.85 (SOLVE)
  → Check-in consultations are included with the subscription — no extra charge
  → These are typically scheduled after monitoring blood tests
  → "Your check-in consultations are included in your subscription. If you need to book one, check your portal tasks or let me know and I'll get our team to set one up."

IF existing patient says they were asked to complete additional tests before check-in:
  → Check order history — this may be a monitoring blood test (free with subscription)
  → If the system is asking them to BUY a test: likely an error
  → confidence = 0.75 (ESCALATE) if it seems like a system error
