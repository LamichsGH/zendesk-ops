# Payments

## Scope

This document covers payment failures, billing schedules, payment method updates, and retry processes for weight loss treatment subscriptions.

**Covered:**
- Payment failure handling and auto-retry schedules
- Payment method updates (cards accepted, how to update)
- Billing cycles and timing
- First order billing process
- Self-service payment retry via web interface

**Not Covered:**
- Refunds or cancellations → see `billing-and-refunds.md`
- Pricing questions → see `pricing-and-coverage.md`
- Order status (non-payment) → see `order-management.md`

**Tool Required:** `get_order_history`

---

## Payment Failure Decision Tree

**Step 1: Identify the affected order**

```
CALL get_order_history
  IF order status = "In Progress" → confidence = 0.95 (REFER TO THAT ORDER)
  IF no "In Progress" order found:
    IF previous order payment_status = "AWAITING_PAYMENT" → confidence = 0.90 (REFER TO THAT ORDER)
    IF neither condition met → confidence = 0.45 (ASK FOR ORDER NUMBER) → ESCALATE if not provided
```

**Step 2: Confirm payment failure and provide resolution**

```
IF payment_status = "AWAITING_PAYMENT" → confidence = 0.92:
  → Inform: "Your order could not be processed due to a payment failure."
  → Explain auto-retry schedule: days 1, 2, 3, 5, 10, 15, 20, 25, and 28
  → Clarify: "Auto-retries happen automatically regardless of any action you take."
  → Check: "Please ensure you have sufficient funds on your card."

  IF insufficient funds suspected → confidence = 0.85:
    → "Update your payment method: Open Voy app → tap your initials (top-right) → Account Details → Billing Details → Add or update payment method"

  IF sufficient funds confirmed → confidence = 0.88:
    → "You can retry payment immediately via our web interface: [Billing & Payments link]"
    → Offer to guide through self-service retry process

  → Remind: "Your order will remain in 'Pending Payment' status until payment is successful."
```

⚠️ CRITICAL: NEVER offer to connect to customer service for payment failures. Patients can self-serve using the web interface or wait for auto-retries.

---

## Updating Payment Method

**Accepted payment methods:**
- Visa, Mastercard, American Express
- UK debit cards

**Update steps → confidence = 0.93:**

```
"To update your payment method:
1. Open the Voy app
2. Tap your initials in the top-right corner
3. Select 'Account Details'
4. Go to 'Billing Details'
5. Add or update your payment method

Please update before your next billing date to avoid payment failures."
```

---

## Billing Schedule & Timing

**Subscription cycle → confidence = 0.95:**
- 28-day recurring subscription cycle
- Payment is taken ON the order date, not before
- Order approval typically happens 3 days before the order is due
- Pricing may change with dose strength adjustments (visible in account)

**Treatment gap risk → confidence = 0.88:**

```
IF payment delay >21 days → confidence = 0.82:
  ⚠️ "A delay of more than 21 days may result in a gap in your treatment. If the gap exceeds 4 weeks, you may need to restart at the lowest dose (Wegovy 0.25mg or Mounjaro 2.5mg)."
```

---

## First Order Billing

**First order payment process → confidence = 0.94:**

```
"For your first order:
- The payment amount is reserved on your card before your order is approved
- Your card is only charged after approval
- If your order is not approved, the reserved amount will be released within 7 business days"
```

---

## Payment Retry Self-Serve

⚠️ Patients can ONLY retry payments via the web interface, NOT through the app.

**Self-serve retry guidance → confidence = 0.90:**

```
"You can retry your payment immediately using our web interface:
1. Visit [Billing & Payments web link]
2. Log in with your Voy account credentials
3. Select the pending order
4. Click 'Retry Payment'
5. Confirm your payment method and submit

Would you like me to guide you through this process?"
```

---

## Worked Examples

### Example 1: Payment Failure with Insufficient Funds

**Patient:** "My payment failed. What do I do?"

**Agent Response:**
```
[CALL get_order_history]
[IF order found with payment_status = "AWAITING_PAYMENT"]

"I can see your order could not be processed due to a payment failure. Here's what happens next:

- Voy will automatically retry your payment on days 1, 2, 3, 5, 10, 15, 20, 25, and 28
- These retries happen automatically, regardless of any action you take
- Please ensure you have sufficient funds on your card

If you need to update your payment method:
1. Open the Voy app
2. Tap your initials (top-right)
3. Go to Account Details → Billing Details
4. Add or update your payment method

You can also retry payment immediately via our web interface: [Billing & Payments link]. Would you like me to guide you through that process?"

[confidence = 0.92]
```

### Example 2: First Order Payment Question

**Patient:** "When will I be charged for my first order?"

**Agent Response:**
```
"For your first order:
- We reserve the payment amount on your card before your order is approved
- Your card is only charged after your order has been approved
- If your order is not approved, the reserved amount will be released within 7 business days

Your order approval typically happens about 3 days before it's due to be dispatched."

[confidence = 0.94]
```

### Example 3: Update Payment Method Before Next Billing

**Patient:** "How do I update my card details before my next payment?"

**Agent Response:**
```
"To update your payment method:
1. Open the Voy app
2. Tap your initials in the top-right corner
3. Select 'Account Details'
4. Go to 'Billing Details'
5. Add or update your payment method

We accept Visa, Mastercard, American Express, and UK debit cards. Please make sure to update before your next billing date to avoid any payment issues."

[confidence = 0.93]
```

---

## Cross-References

- **Refunds & Cancellations:** `billing-and-refunds.md`
- **Pricing Questions:** `pricing-and-coverage.md`
- **Order Status:** `order-management.md`
- **Account Access Issues:** `account-management.md`
