# Billing & Payments

## Scope
This file covers payment issues, failed transactions, discount codes, and billing confusion. Use when patient reports payment problems, asks about charges, or has discount code issues. For refund requests, see refunds.md. For subscription billing, see subscriptions.md.

## ⚠️ Key Principle: Payment ≠ Medication Dispatch

Subscription payments do NOT automatically trigger medication dispatch for most TRT treatments.

**Auto-dispatch (payment = order):** Tadalafil, Statins only
**Manual order required (payment ≠ order):** All other TRT medications (Cypionate, Sustanon, Cream, HCG, Clomid, etc.)

IF patient reports "I paid but no medication arrived":
  → First check if they're on a manual-order medication
  → If yes: this is likely the issue — see orders-and-shipping.md for the full decision tree
  → confidence = 0.85 (SOLVE) — guide them to place their order via the app

## Payment Status Confusion

IF patient says "payment was taken but system shows failed":
  → confidence = 0.55 (ESCALATE)
  → "I can see how that would be concerning. I've passed this to our team to verify the payment status. Sometimes transactions take a moment to process fully in our system."
  → Mark as urgent if patient has an appointment today

IF patient says "Apple Pay showed an error but I was charged":
  → confidence = 0.55 (ESCALATE)
  → "Apple Pay transactions occasionally show an error even when the payment goes through. I've flagged this for our team to check — if the payment was processed, you won't need to pay again."

IF patient says "payment failed but funds left my account":
  → confidence = 0.55 (ESCALATE)
  → "If the funds have left your account, the payment may have been processed on our end too. I've flagged this for urgent investigation."
  → IF patient has appointment today: add "URGENT" to internal_note

IF patient says "I'm ready to proceed" after consultation:
  → confidence = 0.85 (SOLVE)
  → Direct them to the payment section in their portal
  → Search KB for payment/subscription setup instructions

## Discount Code Issues

IF patient says discount code doesn't work at checkout:
  → confidence = 0.55 (ESCALATE)
  → Collect: the exact code, what product they're buying, and the error message
  → "I'm sorry the code isn't working. I've passed this to our team so they can either apply the discount manually or provide a working alternative."

IF patient forgot to apply a discount code:
  → confidence = 0.55 (ESCALATE)
  → "No worries — I've flagged this for our team. They can apply the discount retrospectively or arrange a partial refund for the difference."

IF patient asks which products a code applies to:
  → confidence = 0.55 (ESCALATE)
  → Discount codes are usually product-specific
  → "Let me check that for you — I've passed the code to our team to confirm exactly what it covers."

## Double Charges / Billing Errors

IF patient reports being charged twice:
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry about that. I've escalated this for urgent investigation. If there is a duplicate charge, our team will arrange a refund."
  → Do NOT ask for screenshots unless the team specifically needs them

IF patient disputes a charge they don't recognise:
  → confidence = 0.55 (ESCALATE)
  → "I've flagged this for our team to investigate the charge. They'll get back to you within 24–48 hours."

## Checkout / Technical Issues

IF patient can't complete checkout (errors, page not loading):
  → confidence = 0.85 (SOLVE)
  → Troubleshooting steps:
    1. Try a different browser (Chrome recommended)
    2. Clear browser cache
    3. Disable any ad blockers or VPNs
    4. Try on a different device
  → If none work: escalate for team to check

IF patient has reCAPTCHA failure during checkout:
  → confidence = 0.85 (SOLVE)
  → "This is usually a browser compatibility issue. Try switching to Chrome or clearing your cache."
