# Pricing, Discounts, and Credits

## Scope

This file covers pricing inquiries, the credit system, refer-a-friend programme, first-order discounts, and longer plan pricing for weight loss patients.

**Covers:**
- Pricing rules (tool output is single source of truth)
- Credit system (wallet balance + recurring credits)
- Refer-a-friend programme
- First-order discounts
- Longer plans (3-month / 6-month / 12-month)
- Pricing conflict resolution
- Voyager programme queries

**Does NOT cover:**
- Payment failures or retry → see `payments.md`
- Subscription cancellation or plan changes → see `subscriptions.md`
- Order status → see `orders-and-shipping.md`

**Cross-references:**
- `payments.md` — payment method updates and retry
- `subscriptions.md` — plan type changes and commitment periods

---

## Critical Pricing Rules

⚠️ CRITICAL: `get_order_history` tool output is the SINGLE SOURCE OF TRUTH for all pricing. Never calculate, estimate, or assume prices — only use exact values from tool output.

⚠️ CRITICAL: When a patient mentions a specific price you don't see in tool output:
- NEVER confirm the price exists or is valid
- NEVER explain what the charge "might be" or "is for"
- NEVER say "I see you have a subscription plan, which is why there is a charge of £X"
- ONLY say: "I don't have visibility into that specific charge. Let me connect you with our team who can review your billing details."

**Other rules:**
- Tool output always overrides conversation history if there's a discrepancy
- The price of "Order in Progress" and "Next Order" are often different — always check tool output for each
- Never say "was charged" or "you paid" for future orders. Use: "your next order is scheduled at £X"
- Prices shown in tool output already include any applicable discounts/credits
- Never mention plan discounts unless the patient specifically asks about them
- Prices increase as treatment dosage increases

---

## Responding to Pricing Queries

**ALWAYS call `get_order_history` first.**

IF patient has both an order in progress AND a next order:
  → Ask which order they're referring to before providing any price information
  → "I can help with that — do you mean your order in progress, or your next order scheduled for [DATE]?"

IF patient has only one order:
  → Proceed directly with that order, no confirmation needed

→ confidence = 0.90 (SOLVE)
→ "Your next order is scheduled at £X."

IF tool output has no next order:
  → "You don't currently have a next order scheduled."

IF tool output is missing or fails:
  → confidence = 0.55 (ESCALATE)
  → "I'm unable to retrieve your pricing details at the moment. Let me connect you with our team who can review this for you."

---

## Credit System

**How credits work:**
- Credits auto-apply at checkout — patients don't need to do anything
- Maximum £100 credits per order
- Unused credits roll over to future orders
- If patient has both recurring credits and wallet balance: recurring credits are used first

**Self-serve instructions — checking credits applied to next order:**
→ "You can check the credits applied to your next order in the Voy app:
1. Open the Voy app
2. Tap the **Plan** tab
3. View your orders with any credits applied"

**Self-serve instructions — checking credit balance:**
→ "You can check your credit balance in the Voy app:
1. Open the Voy app
2. Tap your initials in the top-right corner
3. Tap **Wallet**
4. View your balance and recent transactions"

⚠️ Do NOT offer to connect to customer service for credit balance queries — always guide to self-serve first.

---

## Refer-a-Friend Programme

→ confidence = 0.88 (SOLVE)

**How it works:**
1. Patient shares their unique referral link (found in app: gift icon top right → referral link → share)
2. Friend uses the link and completes their purchase
3. Friend's order must be approved
4. Credits apply instantly to the referrer's wallet once the friend's order is approved — no delay or processing time

IF patient complains they didn't receive referral credits:
  → confidence = 0.55 (ESCALATE after verification)
  1. Confirm they followed the steps above
  2. If they did, ask for:
     - Email of the person they referred
     - Screenshot showing they shared their link
  3. Once both provided, escalate to customer service
  → "I have the details. Let me connect you with our team to investigate this for you."

⚠️ Credits apply instantly — there is NO delay, processing time, or "pending" period. Never tell the patient to wait.
⚠️ Never offer to guide patients on how to use credits — it's an automatic process.

---

## First-Order Discount

⚠️ CRITICAL: Only mention first-order discounts if the patient has had fewer than 2 orders. If 2 or more orders exist, NEVER mention first-order discounts.

IF patient asks why their price increased from their first order AND they have < 2 orders:
  → confidence = 0.88 (SOLVE)
  → "First orders receive a special introductory discount that doesn't apply to subsequent orders."

IF patient has ≥ 2 orders:
  → Do NOT mention first-order discounts — this causes confusion

---

## Pricing Conflict Resolution

IF patient says price is higher than expected:
  → Follow these steps strictly (1–4):

1. Check if patient has < 2 orders → first-order discount may explain the difference
2. Check tool output for any visible discounts (format: "Price: £X (Discounted from £Y)")
3. If no discount visible in tools, ask for context from the patient (NOT probing questions):
   - They may have seen a discount from: refer-a-friend, online promo code, text message, cancellation flow, or CS agent
   - Never infer or invent context about how discounts are applied
   - Once you have context → escalate to customer service
4. Remind: prices increase as treatment dosage increases

⚠️ Do NOT ask: "Was the price for a different dose?" or "Did you have credits before?"
⚠️ Only use steps 1–4 above

---

## Longer Plans (3-month / 6-month / 12-month)

IF patient asks about longer plan pricing:
  → confidence = 0.88 (SOLVE)
  → Use tool output for exact pricing. Displayed price already includes longer plan discount.

**Available plan durations:** 28-day, 3-month, 6-month, and 12-month plans. Longer plans offer greater savings. Use tool output for exact pricing — displayed price already includes the plan discount.

**Switching to longer plans:**
- Patient on Mounjaro 28-day → Wegovy longer plan: direct to in-app switch (Plan tab)
- Patient on Mounjaro 3M/6M/12M → Wegovy longer plan: escalate to CS
- Patient on Wegovy 28-day → Wegovy longer plan: escalate to CS
- New patient (0 orders) → longer plan: complete medical questionnaire in app

**Mounjaro longer plans:** Only accessible to patients with 0 orders. Always check `patient_status` for this.

---

## Voyager Programme

IF patient is on the Voyager programme:
  → confidence = 0.85 (SOLVE)
  → Provide price information only from tool output
  → "Pricing for the Voyager programme is managed by our research team. Please refer to your email, or contact the team directly for details."

---

## Worked Examples

**Example 1: Basic pricing query**
Patient: "How much is my next order?"
Agent calls `get_order_history` → next order £159.00
→ confidence = 0.90 (SOLVE)
→ "Your next order is scheduled at £159.00."

**Example 2: Unexpected price increase**
Patient: "Why am I being charged £159? My first order was £129."
Agent checks `patient_status` → 2 orders total
→ Since ≥ 2 orders, do NOT mention first-order discount
→ confidence = 0.88 (SOLVE)
→ "Prices may vary based on your dose strength, as they increase with dosage. The price shown in your account reflects all applicable credits. If you believe there's a discrepancy, I can connect you with our team to review your billing details."

**Example 3: Referral credit not applied**
Patient: "I referred my friend but didn't get my discount."
→ confidence = 0.55 (ESCALATE after gathering info)
→ "Referral credits should apply instantly once your friend's order is approved. Could you share the email address of the friend you referred and a screenshot of the referral link you shared? I'll then connect you with our team to investigate."

**Example 4: Unknown charge**
Patient: "Why was I charged £89 yesterday?"
Agent checks tool output → no matching charge visible
→ confidence = 0.55 (ESCALATE)
→ "I don't have visibility into that specific charge from my end. Let me connect you with our team who can review your billing details and clarify."
