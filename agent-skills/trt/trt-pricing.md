# TRT Pricing & Plans

## Scope
This file covers pricing questions, plan comparisons, cost breakdowns, discount code issues, and Refer-a-Friend (RAF) queries. Use when patient asks "how much does it cost", "what's included", "what are the plans", has discount/promo code problems, or asks about referral rewards.

Cross-references: `subscriptions.md` for plan management and cancellation details, `blood-tests.md` for blood test procedures and result queries.

---

## TRT Journey Costs (Full Breakdown)

### Step 1: Initial Blood Test (BT1 — Screening)
- **Home finger-prick kit**: £33.95
- Purpose: Screen for low testosterone
- If results show low T → proceed to enhanced test (BT2)

### Step 2: Enhanced Blood Test (BT2 — Comprehensive Panel)
- **Clinic visit (kit-less)**: £79.95
- **Kit only** (patient has someone to take the sample): £49.95
- **At-home nurse visit**: £119.95
- Tests 39 biomarkers — required before consultation
- If patient already paid for finger-prick and switches to venous: original test is refunded after venous purchase

### Step 3: Consultation
- **Specialist clinician consultation**: £79.95
- Reviews all blood test results
- Creates personalised treatment plan
- One-off fee (not recurring)

### Step 4: Monthly Subscription (Treatment)
- Pricing varies by treatment plan — see full plan table below
- All plans include: medication, injection supplies, ongoing blood monitoring, clinical support, repeat prescriptions
- Monitoring blood tests are FREE with active subscription (except Optimale-to-Voy migrations — see account-and-portal.md)

⚠️ Always search KB with file_search for the latest pricing — prices may have been updated since this file was written.

---

## Treatment Plan Pricing (Monthly)

| Plan | Monthly Cost |
|------|-------------|
| Testosterone Cream & HCG | £169 |
| Testosterone Cream & Clomid | £169 |
| Cypionate & HCG | £159 |
| HCG Monotherapy (High Dose) | £159 |
| HCG Monotherapy (Low Dose) | £119 |
| Sustanon & HCG | £139 |
| Cypionate & Clomid | £139 |
| Cypionate Only | £129 |
| Testosterone Cream Only | £129 |
| Sustanon Only | £99 |
| Clomid Only | £99 |
| Tadalafil Only | £30 |
| Tadalafil Add-On | £15 |

### What's Included in ALL Plans
- All medication listed in the plan
- Injection supplies (needles, bacteriostatic water, sharps bin, alcohol swabs)
- Regular blood tests (at 3 months, 6 months, then every 6 months thereafter)
- Doctor consultations after each blood test
- Ongoing support from the team within 48 hours
- Clinic visits included for blood monitoring, OR at-home nurse visits at £114.95 for non-AIOS patients

⚠️ Tadalafil Add-On (£15/month) is ONLY available as an add-on to an existing TRT plan — it cannot be purchased standalone at this price. Tadalafil Only (£30/month) is the standalone option.

⚠️ The clinician decides the treatment plan at consultation — patients do not choose their own plan. If a patient asks to switch plans, see `subscriptions.md`.

---

## Decision Trees

### General Pricing Queries

IF patient asks "how much does TRT cost in total":
  → confidence = 0.85 (SOLVE)
  → Provide the full journey breakdown (BT1 + BT2 + Consultation + Monthly Plan)
  → Be transparent: "The total cost depends on which blood test option you choose and the treatment plan your clinician recommends after consultation."
  → Give a realistic range: "Initial costs are typically between £193 and £234 depending on blood test options, plus your monthly plan which starts from £99/month."
  → Search KB for any current offers or updated pricing

IF patient asks about a specific step's cost:
  → confidence = 0.90 (SOLVE)
  → Provide the relevant pricing from the breakdown above
  → Search KB for any current offers or updated pricing

IF patient asks "what plan will I be on" or "which plan is best":
  → confidence = 0.85 (SOLVE)
  → "Your treatment plan is determined by your clinician based on your blood test results and medical history during your consultation. I can share the plan options and pricing so you know what to expect."
  → Provide the plan pricing table
  → Do NOT recommend a specific plan — that is a clinical decision

IF patient asks about plan pricing or "what are the plans":
  → confidence = 0.90 (SOLVE)
  → Provide the full treatment plan pricing table
  → Clarify what is included in all plans
  → "Your clinician will recommend the most suitable plan for you based on your results."

IF patient complains costs weren't clear upfront:
  → confidence = 0.85 (SOLVE)
  → Acknowledge the feedback: "I understand that can be frustrating — let me break down all the costs so you know exactly what to expect."
  → Provide the full breakdown (BT1 → BT2 → Consultation → Monthly Plan)
  → Do NOT be defensive

IF patient asks about free webinar vs paid consultation:
  → confidence = 0.90 (SOLVE)
  → Webinar: Free educational session about TRT — no individual assessment
  → Consultation: Paid one-to-one with a clinician who reviews YOUR results and creates YOUR treatment plan
  → "The webinar is a great way to learn about TRT in general. The consultation is where our clinician reviews your specific blood results and creates a personalised plan."

### Pre-Sales Expectation Setting

IF non-customer asks about TRT:
  → confidence = 0.85 (SOLVE)
  → Explain: TRT is testosterone REPLACEMENT — it brings levels back to normal range, it does not boost above normal
  → It is a long-term commitment (not a short course)
  → Requires regular blood monitoring (free with subscription)
  → Must confirm low T with two blood tests before consultation

---

## Discount Codes

IF patient says discount code isn't working:
  → confidence = 0.55 (ESCALATE)
  → Collect: the code they're using, what they're trying to buy, and the error message
  → "I'm sorry the code isn't working. I've passed this to our team so they can apply the discount for you or provide an alternative."
  → Do NOT tell the patient to keep trying

IF patient forgot to apply a discount code at checkout:
  → confidence = 0.55 (ESCALATE)
  → "I've passed this to our team — they'll be able to apply the discount retrospectively or arrange a partial refund."

IF patient asks about which products a discount applies to:
  → Discount codes are usually product-specific (blood test discounts don't apply to consultations, etc.)
  → If unsure, escalate for team to verify

---

## Refer-a-Friend (RAF)

### How It Works — For the Referrer
- Unique referral link is in the app: tap the **gift icon** (top right) → referral link → share with friend
- Rewards:
  - **1 friend referred** → 25% discount on next order
  - **2+ friends referred in the same month** → 50% discount on next order
  - **£250 one-time reward** when the referred friend starts treatment AND stays for 3 months

### How It Works — For the Person Being Referred
- Receives **50% off their first blood test (BT1)**

### RAF Decision Trees

IF patient asks "how do I refer a friend":
  → confidence = 0.90 (SOLVE)
  → "You can find your unique referral link in the app — tap the gift icon in the top right corner, then share your referral link with your friend."
  → Explain the reward tiers (25% for 1 referral, 50% for 2+ in same month, £250 after friend stays 3 months)

IF patient asks "where is my referral reward" or "my RAF discount didn't apply":
  → confidence = 0.55 (ESCALATE)
  → Rewards should apply instantly when the qualifying event occurs
  → Do NOT ask the patient to confirm — escalate directly and let the team investigate
  → "I've flagged this for our team to look into — they'll check the referral status and make sure your reward is applied. They'll be in touch within 24-48 hours."

IF patient asks "what do I get for referring a friend":
  → confidence = 0.90 (SOLVE)
  → Provide full reward breakdown:
    - 25% off next order for 1 referral
    - 50% off next order for 2+ referrals in the same month
    - £250 one-time reward when referred friend starts treatment and stays for 3 months

IF referred person asks "where is my discount":
  → confidence = 0.55 (ESCALATE)
  → The referred person should receive 50% off BT1 automatically
  → If it didn't apply, escalate for team to investigate and apply manually

⚠️ The £250 reward is given before the 3-month mark is reached. If a pattern of abuse is detected (e.g., referring many people who cancel within 1 month), this is flagged internally. Do NOT mention abuse detection to the patient.

⚠️ Do NOT promise specific reward timelines beyond "rewards should apply when the qualifying event occurs." If there is a delay, escalate.

---

## Monitoring Tests vs Enhanced Tests (Existing Patients)

IF existing patient (active subscription) asks to BUY a blood test:
  → confidence = 0.85 (SOLVE)
  → "Monitoring blood tests are included free with your subscription — you don't need to purchase one separately."
  → Guide them to their portal tasks section
  → Do NOT link to the enhanced test purchase page
  → ⚠️ EXCEPTION: If the patient migrated from Optimale, blood tests may NOT be included — see account-and-portal.md. Do NOT tell Optimale patients their tests are free without verifying their plan.

IF existing patient asks about "enhanced test":
  → Check context — they may have been asked by the system to complete one
  → If it's a system task: guide them through it
  → If they're browsing and confused: explain monitoring tests are free

IF existing patient asks about blood test schedule:
  → confidence = 0.90 (SOLVE)
  → "Your monitoring blood tests are at 3 months, 6 months, and then every 6 months after that — all included free with your subscription."
  → ⚠️ EXCEPTION: Optimale migration patients — omit the "free" claim and check their plan. See account-and-portal.md.

---

## Worked Examples

### Example 1: Pre-sale patient asking about total cost
**Patient**: "How much is TRT going to cost me in total?"
**Response**: "Great question — let me break it all down for you. The journey starts with an initial screening blood test (£33.95), followed by a comprehensive enhanced blood test (from £49.95 depending on the option you choose), and then a one-off consultation with our specialist clinician (£79.95). After that, your monthly treatment plan starts from £99/month depending on what your clinician recommends. All plans include your medication, injection supplies, regular monitoring blood tests, and doctor consultations — so there are no hidden extras. The total initial cost before your subscription begins is typically between £163 and £234."

### Example 2: Patient asking about plan pricing
**Patient**: "What are the different plans and prices?"
**Response**: "Here are our current treatment plans: [provide the plan table]. Every plan includes all your medication, injection supplies, regular blood tests at 3, 6, and then every 6 months, plus doctor consultations after each test and ongoing support. Your clinician will recommend the most suitable plan for you based on your blood test results and consultation."

### Example 3: Referrer asking about missing reward
**Patient**: "I referred my mate 2 weeks ago and he signed up but I haven't got my discount yet."
**Response**: "Thanks for referring your friend! The referral rewards should apply automatically when the qualifying event occurs. I've flagged this for our team to investigate and make sure your reward is applied. They'll get back to you within 24-48 hours."

### Example 4: Referred friend asking about BT1 discount
**Patient**: "My friend referred me but I didn't get the 50% off my blood test."
**Response**: "I'm sorry the discount didn't apply automatically. Let me flag this for our team — they'll be able to investigate and apply the 50% off your first blood test for you."

### Example 5: Patient confused about monitoring vs purchased blood tests
**Patient**: "I need to do a blood test — which one do I buy?"
**Response** (check if existing patient first): "As you're on an active subscription, your monitoring blood tests are included free — you don't need to purchase one separately. You can find your upcoming blood test in your portal under the tasks section. If you need any help ordering your kit, just let me know!"

---

## Referral Credit / Wallet System

IF patient asks about referral credits or their wallet balance:
  → confidence = 0.55 (ESCALATE)
  → The email agent cannot look up wallet balances — escalate for CS to check
  → "I've passed this to our team who can check your credit balance and provide details."
  → General info to share:
    - Referral credits are applied to upcoming orders automatically
    - Credits don't expire while the subscription is active

## Money Back Promise

IF patient asks about the Money Back Promise in a pricing context:
  → confidence = 0.85 (SOLVE with link)
  → "You can find the details of our Money Back Promise, including eligibility and how to apply, on [this page](https://manual.co/money-back-promise)."
  → Do NOT list or guess the eligibility criteria

## Financial Hardship

IF patient mentions they're struggling to afford treatment:
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry to hear you're finding the cost difficult. I've passed this to our team — they can review what options might be available to help."
  → Do NOT promise discounts or specific assistance
  → Include in internal_note: "Patient reports financial hardship. Please review available options."
