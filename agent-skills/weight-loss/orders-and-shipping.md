# Orders and Shipping

## Scope

**What this covers:**
- Order status interpretation and communication
- Shipping timelines and tracking
- Required tasks blocking order approval
- Delivery issues (stuck, missing, failed)
- Cold chain integrity and medication safety
- Address changes and delivery preferences
- Subscription cycle and auto-order creation

**What this does NOT cover:**
- Payment failures → see `payments.md`
- Subscription cancellation or plan changes → see `subscriptions.md`
- Medication usage instructions → see `how-to-use-medication.md`
- Medication storage → see `medication-storage.md`

**Cross-references:**
- `payments.md` — for AWAITING_PAYMENT status
- `subscriptions.md` — for subscription schedule changes
- `medication-storage.md` — for cold chain storage rules

---

## Order Status Decision Tree

**ALWAYS call `get_order_history` first before responding.**

IF Order status = 'NEW' AND Shipping status = 'READY':
  → Call `get_customer_tasks`
  → IF pending tasks exist:
      → confidence = 0.90 (INFORM)
      → "Your order is pending clinical review. Before it can be approved, you'll need to complete the following: [list tasks]. You can do this in the Voy app under the Home tab. Once completed, our clinical team typically reviews within 1–2 working days."
  → IF recently completed tasks exist (within last 2 working days):
      → confidence = 0.88 (REASSURE)
      → "Thank you for completing your tasks. Our clinical team is currently reviewing — this typically takes up to 2 working days. Your order will be approved and dispatched once the review is complete."
      → ⚠️ NEVER offer escalation for recently completed tasks
  → IF no pending or recent tasks:
      → confidence = 0.85 (REASSURE)
      → "Your order is currently awaiting clinical approval, which typically takes 1–2 working days. Once approved, it will be dispatched within 1–3 working days via Royal Mail."

IF Order status = 'NEW' AND Payment State = 'Paid':
  → confidence = 0.92 (REASSURE)
  → "Your order has been approved and payment confirmed. It will be dispatched within 1–3 working days via Royal Mail tracked delivery."

IF Order status = 'FULFILLED' OR Shipping status = 'SHIPPED':
  → confidence = 0.95 (PROVIDE TRACKING)
  → "Your order has been dispatched. You can track your delivery in the Voy app: Plan tab → Track Delivery. [Include tracking link if available]. Royal Mail typically delivers within 1–3 working days, Monday–Saturday."

IF Payment State = 'AWAITING_PAYMENT':
  → confidence = 0.70 (REDIRECT)
  → "Your order is awaiting payment. Would you like help with retrying your payment? You can do this via our web interface."
  → See `payments.md` for full payment failure handling

IF no order data found:
  → confidence = 0.50 (ESCALATE)

---

## Self-Serve Tracking Instructions

→ confidence = 0.90 (INFORM)

"You can check your order status anytime in the Voy app:
1. Open the Voy app
2. Go to the **Plan** tab
3. Select **Track Delivery** for your order

Order statuses explained:
- **Awaiting Approval** — Your order is under clinical review (usually 1–2 days)
- **Approved** — Cleared for dispatch, being prepared for shipping
- **Dispatched** — With Royal Mail, in transit to you
- **Delivered** — Successfully delivered"

---

## Stuck Packages

IF order dispatched AND >3 working days since dispatch:
  → confidence = 0.75 (INVESTIGATE)
  → Provide Royal Mail tracking link from `get_order_history`
  → IF no tracking movement for 3+ days:
      → confidence = 0.55 (ESCALATE)
      → "Your order was dispatched on [date] but tracking hasn't updated. I'm escalating this to our team to investigate with Royal Mail."

IF order dispatched AND ≤3 working days since dispatch:
  → confidence = 0.88 (REASSURE)
  → "Your order was dispatched on [date]. Standard delivery takes 1–3 working days (Monday–Saturday). You can track progress here: [tracking link]."

⚠️ Only escalate stuck packages if >3 working days since dispatch AND no tracking updates.

---

## Missing or Failed Deliveries

IF patient reports package not received:
  → confidence = 0.80 (TROUBLESHOOT)
  → "I'm sorry to hear your delivery hasn't arrived. Could you please check:
    1. Confirm the delivery address on your account is correct
    2. Check with household members or neighbours
    3. Look for any Royal Mail delivery notices (they may have left a card)
    4. Check the tracking link for the latest status: [tracking link]"

IF patient confirms all checked and still not found:
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry about this. Let me escalate to our team to investigate and arrange next steps."

---

## Cold Chain and Medication Safety

IF patient reports damaged or warm packaging:
  → confidence = 0.95 (SAFETY CHECK)
  → "For your safety, please check: (1) Is the packaging intact? (2) Is the medication warm to touch? (3) Is the liquid clear with no particles or cloudiness?"

  → IF any concerns (damaged, warm, cloudy, particles):
      → confidence = 0.50 (ESCALATE)
      → safety_flags: ["medication_safety"]
      → "Please do NOT use this medication. I'm escalating this immediately to arrange next steps."

  → IF packaging intact and medication looks normal:
      → confidence = 0.90 (REASSURE)
      → "Your medication should be safe to use. All deliveries use temperature-controlled packaging. A broken cold chain doesn't automatically mean the medication is unsafe."

⚠️ CRITICAL: If any indication of compromised medication → advise DO NOT USE and ESCALATE immediately.

**Room temperature stability:**
- Wegovy: stable at room temp (≤30°C) for up to 6 weeks from ship date. Once at room temp, the 42-day countdown begins and does NOT reset if refrigerated again.
- Mounjaro: stable at room temp (≤30°C) for up to 30 days from ship date.
- Ice packs maintain cold chain for ~48 hours during transit. Disposable in household waste.

---

## Address Changes

IF patient requests address change AND order NOT yet dispatched:
  → confidence = 0.55 (ESCALATE)
  → "Your order hasn't shipped yet. Let me check if we can update the delivery address."

IF patient requests address change AND order already dispatched:
  → confidence = 0.88 (INFORM)
  → "Your order has already been dispatched and we cannot change the delivery address. However, Royal Mail offers these options:
    - Divert to a safe place or neighbour
    - Collect from your local Post Office
    You can manage these via your Royal Mail tracking link."

⚠️ CRITICAL: Never tell patients we can change address for approved or dispatched orders.

IF patient asks about letterbox delivery:
  → confidence = 0.90 (INFORM)
  → "Due to package size, Royal Mail cannot deliver through your letterbox. Your driver may leave with a neighbour, in a safe place, or leave a collection notice for your local Post Office."

---

## General Information & Hard Rules

⚠️ CRITICAL: We CANNOT provide signed deliveries with Royal Mail. Never tell a patient this is an option.

⚠️ No guaranteed time-specific deliveries. Delivery depends on Royal Mail.

- **Subscription cycle:** Orders auto-created every 28 days — patients do NOT need to place orders manually
- **Delivery window:** 1–3 working days after dispatch, Monday–Saturday
- **Carrier:** Royal Mail only — no other courier available
- **Packaging:** Discreet, no external branding or medication labels
- **One pen** covers 4 weeks of treatment (one injection per week)
- **Ice packs** disposable in normal household waste
- If patient wants delivery before a specific date → recommend moving order date 2–3 days earlier

⚠️ Patients often refer to "future orders" as "current" when their last order was >10 days ago. Always clarify which order they mean.

---

## Worked Examples

**Example 1: Order blocked by pending tasks**
Patient: "Why hasn't my order been approved yet?"
Agent calls `get_order_history` → status = 'NEW', shipping = 'READY'
Agent calls `get_customer_tasks` → pending: "Upload ID for verification"
→ confidence = 0.90 (INFORM)
→ "Your order is pending clinical review. Before it can be approved, you'll need to complete your ID verification. You can do this in the Voy app: Home tab → ID Verification task. Once completed, our clinical team typically reviews within 1–2 working days."

**Example 2: Stuck package after 5 days**
Patient: "My order was dispatched 5 days ago but tracking hasn't updated."
Agent calls `get_order_history` → dispatched 5 days ago, tracking shows no movement
→ 5 days > 3 working day threshold → confidence = 0.55 (ESCALATE)
→ "Your order was dispatched on [date] and the tracking hasn't updated in 5 days, which is unusual. I'm escalating this to our team to investigate with Royal Mail."

**Example 3: Cold chain concern**
Patient: "My package arrived warm and the ice packs were melted."
→ confidence = 0.95 (SAFETY CHECK)
→ "Please check: (1) Is the outer packaging intact? (2) Is the medication warm? (3) Is the liquid clear with no particles? If the packaging is intact and the medication looks clear, it's likely safe — Wegovy stays stable at room temperature for up to 6 weeks, and Mounjaro for up to 30 days. However, if you notice any damage, warmth, or changes to the liquid, please do NOT use it and let me know so I can arrange next steps."

**Example 4: Address change after dispatch**
Patient: "I need to change my delivery address."
Agent calls `get_order_history` → status = 'SHIPPED'
→ confidence = 0.88 (INFORM)
→ "Your order has already been dispatched and unfortunately we can't change the delivery address at this stage. Royal Mail offers options to divert to a safe place, deliver to a neighbour, or collect from your local Post Office — you can manage these via your tracking link. For future orders, you can update your address in the Voy app under Account Details."
