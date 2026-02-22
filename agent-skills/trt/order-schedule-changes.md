# TRT Order Schedule Changes (Delay / Advance / Pause)

## Scope
This file covers patient requests to delay, advance, or pause their medication order schedule. Use when a patient asks to change when their next order ships, adjust their order timing, skip an order, or pause future orders.

**Covers:**
- Delay a future order (push back the schedule)
- Advance a future order (bring it forward)
- Pause orders (temporarily stop receiving medication)
- Questions about adjusting order dispatch timing
- Confusion between payment date changes and order schedule changes

**Does NOT cover:**
- Payment date changes (see [subscriptions.md](subscriptions.md) — Payment Date Changes section)
- Cancelling a subscription entirely (see [subscriptions.md](subscriptions.md) — Cancellation Requests section)
- Order tracking or delivery issues (see [orders-and-shipping.md](orders-and-shipping.md))
- Refund requests (see [refunds.md](refunds.md))
- Dose changes (see [subscriptions.md](subscriptions.md) — Dose Change Requests section)

**Cross-references:**
- [orders-and-shipping.md](orders-and-shipping.md) — Payment ≠ Order distinction, order statuses, manual vs auto-dispatch
- [subscriptions.md](subscriptions.md) — payment date changes, pause requests, billing cycle rules

---

## CRITICAL: Payment Date ≠ Order Schedule

This is the single most important distinction in this skill. Patients frequently conflate these two concepts.

**Payment date** — when the subscription payment is taken (~every 30 days). Changing this is a billing action handled via the Payment Date Changes section in [subscriptions.md](subscriptions.md).

**Order schedule** — when medication is dispatched. For most TRT medications, the patient must manually place orders via the app (see [orders-and-shipping.md](orders-and-shipping.md) — Payment ≠ Medication Dispatch). For auto-dispatch medications (Tadalafil, Statins), dispatch is tied to payment.

IF patient request is ambiguous (e.g., "can I move my next order"):
  → Clarify: "Just to make sure I help with the right thing — are you looking to change when your subscription payment is taken, or when your next medication order is dispatched?"
  → confidence = 0.75 (MONITOR)

---

## CRITICAL: The Agent CANNOT Modify Order Schedules

⚠️ **The email agent has NO ability to delay, advance, pause, or otherwise modify order schedules.** All operational changes require CS action. The agent's role is to:
1. Acknowledge the patient's request
2. Gather relevant context (current subscription status, order history)
3. Escalate with a clear internal note

---

## Decision Tree

Always call `get_subscription_status` and `get_order_history` first.

### 1. Order Already In Progress or Approved

IF the patient's next order has a status of **Awaiting Approval**, **Approved**, or **Dispatched**:
  → confidence = 0.55 (ESCALATE)
  → The order is already in the pipeline and cannot be changed by the agent
  → "I can see your next order is already [in progress / approved / dispatched], so unfortunately it can't be adjusted at this stage. I've flagged your request with our patient care team and they'll follow up with you."
  → Internal note: include order status, what the patient requested (delay/advance/pause), and any specific dates mentioned

### 2. Delay a Future Order

IF patient asks to delay / push back a future order AND no order is currently in progress:
  → confidence = 0.55 (ESCALATE)
  → "I've passed your request to delay your next order to our patient care team. They'll arrange this and confirm the updated schedule with you."
  → Internal note: include current next order date (if visible), requested delay period or target date, reason if given

### 3. Advance a Future Order

IF patient asks to advance / bring forward a future order AND no order is currently in progress:
  → confidence = 0.55 (ESCALATE)
  → "I've passed your request to bring your next order forward to our patient care team. They'll arrange this and confirm the updated schedule with you."
  → Internal note: include current next order date (if visible), requested new date, reason if given (e.g., running low on medication)

IF patient says they are running low on medication:
  → Check treatment type first

  IF auto-dispatch medication (Tadalafil, Statins):
    → confidence = 0.55 (ESCALATE)
    → "I can see you're running low. Since your medication ships automatically after payment, I've flagged this with our team to see if we can bring your next dispatch forward."

  IF manual-order medication (Cypionate, Sustanon, Cream, HCG, Clomifene, Anastrozole, etc.):
    → confidence = 0.85 (SOLVE with info) then ESCALATE if schedule change needed
    → First check: has the patient placed their most recent order via the app?
    → IF they haven't placed an order: "For your medication, you can place an order anytime through the app without waiting for your next payment date. Here's how:
      1. Open the Voy app and go to the **Plan** tab
      2. In the 'Running low on medication' section, tap **Order now**
      3. Select the medication and consumables you need
      4. Tap **Go to order recap**, confirm details, and tap **Place your order**"
    → IF they have placed an order and still need schedule change: escalate as above

### 4. Pause Orders

IF patient asks to pause their orders / temporarily stop receiving medication:
  → confidence = 0.55 (ESCALATE)
  → ⚠️ Pausing requires CS action — the agent cannot process pauses
  → "I've passed your pause request to our patient care team. They'll arrange this and let you know the details."
  → Internal note: include subscription status, reason for pause if given, whether the patient wants to pause orders only or the full subscription

⚠️ **Pause orders vs pause subscription:** A patient may want to pause just their medication orders while keeping the subscription active, or they may want to pause everything. If ambiguous, note both possibilities in the internal note and let CS clarify with the patient. Do NOT ask the patient to clarify the difference — this creates unnecessary friction.

### 5. Payment Date Change (Redirect)

IF patient is actually asking to change their payment date (not order timing):
  → Use the Payment Date Changes section in [subscriptions.md](subscriptions.md)
  → confidence = 0.55 (ESCALATE)
  → "I've passed your request to change your payment date to our patient care team. They'll review this and get back to you."
  → Internal note: requested new date, current payment date

---

## Internal Note Guidance

Every escalation MUST include an internal note with the following:

1. **Request type:** Delay / Advance / Pause / Payment date change
2. **Current subscription status:** Active, plan type (from `get_subscription_status`)
3. **Current order status:** Next order date and status if visible (from `get_order_history`)
4. **Patient's request:** Specific dates or timeframes mentioned
5. **Reason:** Why the patient wants to change (if provided)
6. **Treatment type:** Medication name (relevant for auto vs manual dispatch)

Example internal note:
> Patient requests to delay next order by 2 weeks. Current subscription: Active, AIOS plan. Treatment: Testosterone Cypionate (manual order). Last order placed: 10 Feb, delivered 14 Feb. Patient states they still have medication remaining and doesn't need the next order until early March. No order currently in progress.

---

## Worked Examples

### Example 1: Delay Request — No Order In Progress
**Patient:** "Can I push my next order back by a couple of weeks? I still have medication left."
**Action:** Call `get_subscription_status` and `get_order_history`. Confirm no order is currently in progress.
**Response (confidence = 0.55, ESCALATE):** "Of course — I've passed your request to delay your next order to our patient care team. They'll arrange this and confirm the updated schedule with you."
**Internal note:** "Patient requests 2-week delay on next order. Reason: still has medication remaining. Subscription: Active, AIOS. Treatment: Testosterone Cypionate (manual order). No order currently in progress."

### Example 2: Order Already In Progress
**Patient:** "I'd like to delay my next order."
**Action:** Call `get_order_history`. Find next order status is "Approved".
**Response (confidence = 0.55, ESCALATE):** "I can see your next order has already been approved for dispatch, so unfortunately it can't be adjusted at this stage. I've flagged your request with our patient care team and they'll follow up with you."
**Internal note:** "Patient wants to delay order but current order status is Approved. Escalating as order is already in pipeline."

### Example 3: Running Low — Manual Order Medication
**Patient:** "I'm running out of my Sustanon, can I get my next order sooner?"
**Action:** Call `get_order_history`. Check if patient has placed an order via the app.
**Response (confidence = 0.85, SOLVE with info):** "For Sustanon, you can place an order anytime through the app without waiting for your next payment date. Here's how:
1. Open the Voy app and go to the **Plan** tab
2. In the 'Running low on medication' section, tap **Order now**
3. Select the medication and consumables you need
4. Tap **Go to order recap**, confirm details, and tap **Place your order**

Once placed, your order will go through clinical review and you'll receive a tracking link once it's dispatched."

### Example 4: Pause Request
**Patient:** "I'm going travelling for a month, can I pause my orders?"
**Action:** Call `get_subscription_status`.
**Response (confidence = 0.55, ESCALATE):** "I've passed your pause request to our patient care team. They'll arrange this and let you know the details."
**Internal note:** "Patient requests order pause for ~1 month due to travel. Subscription: Active, Non-AIOS. Treatment: Testosterone Cypionate. Unclear if patient wants full subscription pause or just order pause — CS to clarify."

### Example 5: Ambiguous — Payment Date vs Order Schedule
**Patient:** "Can I move my next order to after the 15th?"
**Response (confidence = 0.75, MONITOR):** "Just to make sure I help with the right thing — are you looking to change when your subscription payment is taken, or when your next medication order is dispatched?"

---

## MANDATORY RULE: Always Send a Public Response
⚠️ NEVER do a "silent escalation" — internal note only with no customer-facing message. The patient MUST receive an acknowledgment, even if you are escalating. At minimum: "Thanks for getting in touch. I've passed this to our team and they'll be in touch within 24-48 hours."
