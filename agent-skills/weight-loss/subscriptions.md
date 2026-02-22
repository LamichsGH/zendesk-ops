# Subscription Management

## Scope

This file covers:
- Cancellation requests (individual orders vs. entire subscription)
- Subscription pausing, rescheduling, and restarting
- Plan type changes (28-day ↔ 3-month/6-month)
- Billing cycle and payment timing
- Cancellation fee inquiries (REACTIVE ONLY)

This file does NOT cover:
- Order modifications after approval → see `orders.md`
- Dosage changes or titration → see `medication.md`
- Payment failures or refunds → see `billing.md`
- Clinical contraindications → see `safety-protocols.md`

Cross-references:
- `orders.md` for order approval timelines and shipping
- `medication.md` for dose restart requirements
- `billing.md` for payment troubleshooting
- `self-serve.md` for app navigation instructions

---

## Decision Trees

### Cancellation Request Classification

**Step 1: Clarify intent**
- IF patient says "cancel" without specifics:
  → confidence = 0.85 (SOLVE) → "Just to confirm, would you like to cancel your upcoming order only, or cancel your entire subscription?"

**Step 2: Check subscription type**
- ALWAYS call `get_subscription_status` to determine plan type (28-day, 3-month, 6-month)

**Step 3: Cancel upcoming order only**
- IF patient wants to cancel next order only:
  - Call `get_order_history`
  - IF order status = "approved" or "in progress":
    → confidence = 0.95 (SOLVE) → "Unfortunately, your next order has already been approved and cannot be cancelled. It will ship shortly."
  - IF order status = "pending" or "scheduled":
    → confidence = 0.80 (MONITOR) → "Cancelling a single order isn't possible, but you have two options: (1) delay your next order by up to 21 days, or (2) cancel your entire subscription. Would either work for you?"
  - IF patient wants to delay order:
    → confidence = 0.55 (ESCALATE) → Guide to self-serve OR escalate: "You can delay your order through the Voy app: Plan tab → profile icon → Manage Plans → Reschedule. If you prefer, I can connect you with our team to help."

**Step 4: Cancel entire subscription**
- IF subscription_type = "3-month" OR "6-month":
  → confidence = 0.95 (SOLVE) → "You're currently on a [X]-month plan, which requires completing the full commitment period. You won't be able to cancel until [end date]."
  - IF patient pushes back or asks "why not?":
    → confidence = 0.90 (SOLVE) → "Our longer plans are structured as commitment periods, so cancellation isn't available until the plan completes. Your plan ends on [date]."
    - ⚠️ CRITICAL: Do NOT mention cancellation fees unless patient explicitly mentions them first
    - IF patient repeatedly insists on cancelling:
      → confidence = 0.50 (ESCALATE) → "I understand this is frustrating. Let me connect you with our support team to review your options."

- IF subscription_type = "28-day":
  → confidence = 0.85 (SOLVE) → [Proceed to Step 5]

**Step 5: Understand cancellation reason (28-day plans only)**
- IF reason not already stated:
  → confidence = 0.80 (MONITOR) → "I'm sorry to hear you'd like to cancel. May I ask what's prompting this decision? It helps us improve."

**Step 6: Classify reason and route**

**UNCONTROLLABLE reasons** (guide to self-serve, NEVER escalate):
- Patient changed mind / doesn't want medication anymore
- Not aware of subscription model
- Achieved weight loss goal
- Already tried to engage but patient confirmed they want to cancel

→ confidence = 0.90 (SOLVE) → [Provide self-serve cancellation instructions - see Path B below]

**CONTROLLABLE reasons** (ESCALATE):
- Price concerns / affordability
- Side effects (nausea, vomiting, fatigue, etc.)
- GLP-1 contraindications (Crohn's disease, heart failure, cancer history)

→ confidence = 0.50 (ESCALATE) → "I'd like to connect you with our clinical team who can discuss [price options / side effect management / medical concerns] with you before you cancel. Would that be helpful?"

---

## Path B: Self-Serve Cancellation Instructions

→ confidence = 0.90 (SOLVE)

**Email template:**
"You can cancel your subscription directly through the Voy app:

1. Open the Voy app
2. Go to the **Plan** tab
3. Tap the profile icon (top right corner)
4. Select **Manage Plans**
5. Choose **Cancel Plan**

**Important timing:**
- You must cancel before your next order is approved (typically 3 days before your order date)
- Once an order is approved, no partial refunds are available
- You'll receive a confirmation email once cancellation is successful
- Your app access will continue until your current treatment ends

Your next order is scheduled for [DATE]. Please cancel before [APPROVAL DATE] if you'd like to avoid being charged."

---

## Path C: Cancellation Fee Inquiries

⚠️ CRITICAL: ONLY respond to fees if patient explicitly mentions "cancellation fee", "fee to cancel", "charge for cancelling", etc.

- IF patient is on 28-day plan:
  → confidence = 0.95 (SOLVE) → "There are no cancellation fees for 28-day plans. You can cancel anytime before your next order is approved."

- IF patient is on 3-month or 6-month plan AND mentions fee:
  → confidence = 0.55 (ESCALATE) → "For longer commitment plans, cancellation terms vary. Let me connect you with our team who can review the specifics of your plan and any applicable fees."

⚠️ Do NOT:
- Mention fees first
- Invent fee amounts or terms
- Say "there may be a fee" proactively
- Bring up fees when explaining why they cannot cancel early

---

## Restart Subscription

**Self-serve restart (preferred):**
→ confidence = 0.85 (SOLVE)

"You can restart your subscription through the Voy app:
1. Log in to the app
2. Go to the **Plan** tab
3. Select **Reorder**
4. Answer the medical questionnaire
5. Confirm your shipping and payment details

**Important notes:**
- If you've been off medication >4 weeks: You must restart at the lowest dose (Wegovy 0.25mg or Mounjaro 2.5mg)
- If you've been off <2 weeks: You may be able to titrate up from where you left off
- Our clinical team will review your information again
- Approval typically takes 1-2 working days

Please try restarting through the app first. If you encounter any issues, let me know and I can escalate to our team."

⚠️ NEVER escalate restart requests until patient has attempted self-serve

---

## Plan Type Changes

### Switching to Longer Plans (3-month / 6-month)

**New patients (0 orders):**
- Available at signup only
- Cannot be changed via agent or app after first order

**Existing patients (on 28-day plan):**
- Switching to longer plan REQUIRES escalation
- Cannot be done via app or agent tools

→ confidence = 0.55 (ESCALATE)

"Switching to a [3-month / 6-month] plan requires our team's assistance. To help expedite this:
- Which medication are you currently on? [Wegovy / Mounjaro]
- Which plan length would you prefer? [3-month / 6-month]

Let me connect you with our team who can process this change for you."

⚠️ Do NOT attempt to process plan changes directly

---

## Pause Requests

→ confidence = 0.95 (SOLVE)

"Unfortunately, pausing subscriptions isn't currently supported. However, you can delay your next order by up to 21 days, which might give you the flexibility you need.

Would you like to reschedule your next order instead? You can do this through the app (Plan tab → Manage Plans → Reschedule) or I can help connect you with our team."

⚠️ Pausing is NOT possible under any circumstances

---

## Change Order Schedule

**Valid rescheduling range:**
- Up to 7 days earlier
- Up to 21 days later

→ confidence = 0.80 (MONITOR)

"You can reschedule your order through the Voy app: Plan tab → profile icon → Manage Plans → Reschedule.

Your next order is currently scheduled for [DATE]. You can move it between [EARLIEST DATE] and [LATEST DATE]."

**IF patient wants to delay >21 days:**
→ confidence = 0.85 (SOLVE)

"If you delay more than 4 weeks total, you'll need to restart at the lowest dose when you resume. This is a safety requirement for GLP-1 medications.

Would you still like to proceed with rescheduling, or would you prefer to cancel your subscription instead?"

⚠️ Agent should NEVER imply they can make schedule changes directly
⚠️ Always guide to self-serve or escalate

---

## Billing Cycle

**Standard 28-day cycle:**
- Recurring every 28 days
- Payment charged on order date
- Order approval typically occurs 3 days before scheduled date
- Pricing varies by dose strength

→ confidence = 0.90 (SOLVE)

"Your subscription runs on a 28-day cycle. Payment is taken when your order is placed, and orders are typically approved 3 days before your scheduled date. Your next billing date is [DATE]."

---

## Worked Examples

**Example 1: Vague cancellation request**
- Patient: "I want to cancel"
- Agent calls `get_subscription_status` → returns "28-day plan"
- Agent: "Just to confirm, would you like to cancel your upcoming order only, or cancel your entire subscription?"
- Patient: "The whole thing, I can't afford it anymore"
- Agent recognizes CONTROLLABLE reason (price)
- → confidence = 0.50 (ESCALATE)
- Agent: "I'd like to connect you with our team who can discuss pricing options with you before you cancel. Would that be helpful?"

**Example 2: Cancel next order only**
- Patient: "Can I skip my next shipment?"
- Agent calls `get_order_history` → order status = "approved"
- → confidence = 0.95 (SOLVE)
- Agent: "Unfortunately, your next order has already been approved and cannot be cancelled. It will ship shortly. If you'd like to cancel future orders, I can guide you through cancelling your subscription."

**Example 3: 6-month plan early cancellation**
- Patient: "I need to cancel, this isn't working for me"
- Agent calls `get_subscription_status` → returns "6-month plan, ends 2026-05-15"
- → confidence = 0.95 (SOLVE)
- Agent: "You're currently on a 6-month plan, which requires completing the full commitment period. You won't be able to cancel until May 15, 2026."
- Patient: "That's ridiculous, I want to cancel now"
- → confidence = 0.90 (SOLVE, do NOT mention fees)
- Agent: "I understand this is frustrating. Our longer plans are structured as commitment periods, so cancellation isn't available until the plan completes."
- Patient continues to insist
- → confidence = 0.50 (ESCALATE)
- Agent: "Let me connect you with our support team to review your options."

**Example 4: Patient mentions fee first**
- Patient: "If I cancel my 3-month plan early, is there a cancellation fee?"
- → confidence = 0.55 (ESCALATE)
- Agent: "For longer commitment plans, cancellation terms vary. Let me connect you with our team who can review the specifics of your plan and any applicable fees."

**Example 5: Restart after 6 weeks off**
- Patient: "I cancelled 6 weeks ago, can I restart where I left off?"
- → confidence = 0.85 (SOLVE)
- Agent: "Since you've been off medication for more than 4 weeks, you'll need to restart at the lowest dose (Wegovy 0.25mg or Mounjaro 2.5mg) for safety. You can restart through the app: Plan tab → Reorder. The clinical team will review your information, and approval takes 1-2 working days."
