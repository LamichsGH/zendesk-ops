# TRT Pricing & Plans

## Scope
This file covers pricing questions, plan comparisons, cost breakdowns, and discount code issues. Use when patient asks "how much does it cost", "what's included", "what are the plans", or has discount/promo code problems.

## TRT Journey Costs (Full Breakdown)

### Step 1: Initial Blood Test (Screening)
- **Home finger-prick kit**: From £49
- Purpose: Screen for low testosterone
- If results show low T → proceed to enhanced test

### Step 2: Enhanced Blood Test (Comprehensive Panel)
- **Home finger-prick kit**: From £49
- **Clinic visit (Randox)**: From £49
- **Nurse home visit**: From £119
- Tests 39 biomarkers — required before consultation
- If patient already paid for finger-prick and switches to venous: original test is refunded after venous purchase

### Step 3: Consultation
- **Specialist clinician consultation**: £79.95
- Reviews all blood test results
- Creates personalised treatment plan
- One-off fee (not recurring)

### Step 4: Monthly Subscription (Treatment)
- Pricing varies by treatment plan — search KB for current plan pricing
- Plans typically include: medication, ongoing blood monitoring, clinical support, repeat prescriptions
- Monitoring blood tests are FREE with active subscription

⚠️ Always search KB with file_search for the latest pricing — prices may have been updated since this file was written.

## Decision Tree

IF patient asks "how much does TRT cost in total":
  → confidence = 0.85 (SOLVE)
  → Provide the full journey breakdown above
  → Be transparent: "The total cost depends on which blood test option you choose and your treatment plan after consultation."
  → Search KB for current subscription plan pricing

IF patient asks about a specific step's cost:
  → confidence = 0.90 (SOLVE)
  → Provide the relevant pricing from above
  → Search KB for any current offers or updated pricing

IF patient complains costs weren't clear upfront:
  → confidence = 0.80 (SOLVE)
  → Acknowledge the feedback: "I understand that can be frustrating — let me break down all the costs so you know exactly what to expect."
  → Provide the full breakdown
  → Do NOT be defensive

IF patient asks about free webinar vs paid consultation:
  → confidence = 0.90 (SOLVE)
  → Webinar: Free educational session about TRT — no individual assessment
  → Consultation: Paid one-to-one with a clinician who reviews YOUR results and creates YOUR treatment plan
  → "The webinar is a great way to learn about TRT in general. The consultation is where our clinician reviews your specific blood results and creates a personalised plan."

## Pre-Sales Expectation Setting

IF non-customer asks about TRT:
  → confidence = 0.85 (SOLVE)
  → Explain: TRT is testosterone REPLACEMENT — it brings levels back to normal range, it does not boost above normal
  → It is a long-term commitment (not a short course)
  → Requires regular blood monitoring (free with subscription)
  → Must confirm low T with two blood tests before consultation

## Discount Codes

IF patient says discount code isn't working:
  → confidence = 0.75 (ESCALATE)
  → Collect: the code they're using, what they're trying to buy, and the error message
  → "I'm sorry the code isn't working. I've passed this to our team so they can apply the discount for you or provide an alternative."
  → Do NOT tell the patient to keep trying

IF patient forgot to apply a discount code at checkout:
  → confidence = 0.75 (ESCALATE)
  → "I've passed this to our team — they'll be able to apply the discount retrospectively or arrange a partial refund."

IF patient asks about which products a discount applies to:
  → Discount codes are usually product-specific (blood test discounts don't apply to consultations, etc.)
  → If unsure, escalate for team to verify

## Monitoring Tests vs Enhanced Tests (Existing Patients)

IF existing patient (active subscription) asks to BUY a blood test:
  → confidence = 0.85 (SOLVE)
  → "Monitoring blood tests are included free with your subscription — you don't need to purchase one separately."
  → Guide them to their portal tasks section
  → Do NOT link to the enhanced test purchase page

IF existing patient asks about "enhanced test":
  → Check context — they may have been asked by the system to complete one
  → If it's a system task: guide them through it
  → If they're browsing and confused: explain monitoring tests are free
