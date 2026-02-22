# Orders and Shipping

## Scope

**What this covers:**
- Order status interpretation and communication (WISMO — "Where Is My Order?")
- Clinical approval status via answered questionnaire (AQ)
- Required tasks blocking order approval
- Shipping timelines and tracking
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

**Tools used:**
- `get_order_history` — order status, shipping status, payment state, tracking URLs
- `get_answered_questionnaire` — clinical approval status (AQ), last status change date
- `get_customer_tasks` — pending patient tasks blocking approval

---

## Crucial Principles

1. **AQ status is the source of truth** for clinical approval. Do NOT use order status from `get_order_history` to determine approval stage — only use it for shipping, payment, and tracking info.
2. **Match AQ to order by order number.** Both `get_order_history` and `get_answered_questionnaire` return order numbers (O-...) — match them to link the correct AQ to the correct order.
3. **Focus on weight loss products.** The AQ output may contain multiple products. Always focus on the one containing "Wegovy", "Mounjaro", "Saxenda", "Ozempic", "Orlos (Orlistat)", or "Glucomannan Complex" unless the patient mentions a different product.
4. **Always check payment status** before finalising — an approved order can still be blocked by AWAITING_PAYMENT.
5. **Always close with self-serve tracking instructions** — every WISMO response should end with how to check status in the Voy app.
6. **Single reply** — you cannot ask follow-up questions. Provide a complete answer using all available data in one response.

---

## Order Status Decision Tree

**Step 1: ALWAYS call `get_order_history` first.**

**Step 2: Determine which path to follow based on order data.**

### Path A: Order Currently In Progress

IF an order is currently in progress:
  → Call `get_answered_questionnaire` to get the clinical approval status
  → Match the AQ to the order by order number (O-...)

  **IF no AQ match found → go to Path K (No AQ Match)**

  **IF AQ match found, route by AQ status:**

#### Path B: AQ Status = "New"

→ confidence = 0.88 (REASSURE)
→ Patient has nothing to complete. Clinician will review within 1–2 working days.
→ Check payment status → go to Path H
→ "Your order is currently being reviewed by our clinical team. This typically takes 1–2 working days, and you'll receive a notification once it's been approved. Once approved, your order will be dispatched within 1–3 working days via Royal Mail."

#### Path C: AQ Status = "Awaiting patient task"

→ Call `get_customer_tasks` to see what tasks are outstanding
→ confidence = 0.90 (INFORM)
→ List ALL pending tasks and explain how to complete them
→ "Your order is pending review, but before it can be approved you'll need to complete the following:

[List each pending task]

You can complete these in the Voy app under the Home tab, or on our website. Once completed, our clinical team typically reviews within 1–2 working days."

**Possible tasks to explain:**
- Uploading a photo: full-body photo in form-fitting clothing with face visible (for BMI assessment)
- Upload ID for verification: copy of photo ID
- Proof of previous prescription: proof of most recent prescription (for starting on higher dose)
- Weight reading: recent weight reading to continue treatment

#### Path D: AQ Status = "Awaiting patient response"

→ confidence = 0.90 (INFORM)
→ A clinician has asked for more information via email
→ "Your order is on hold because our clinical team has sent you a message requesting further information. Please check your inbox (including spam/junk folders) for an email from our clinicians and respond to their questions. Once you've replied, they'll continue reviewing your order."

⚠️ Do NOT offer to connect to the clinician directly. Patient must respond via email.

#### Path E: AQ Status = "Patient response available"

→ Check "Last Status Change" of the relevant product:
  → IF Last Status Change < 2 working days ago:
      → confidence = 0.88 (REASSURE)
      → "Our clinical team has received your response and is reviewing it. This typically takes up to 2 working days. You should receive an update soon."
      → ⚠️ NEVER offer escalation when last change < 2 days
  → IF Last Status Change > 2 working days ago:
      → confidence = 0.55 (ESCALATE)
      → "Your response was received more than 2 days ago and is still under review, which is longer than usual. I'm passing this to our patient care team to follow up."

#### Path F: AQ Status = "Approved"

→ Call `get_order_history` to check payment and shipping status:
  → IF Payment State = 'AWAITING_PAYMENT':
      → go to Path H (Payment Status)
  → IF Payment State = 'Paid' AND NOT yet shipped:
      → confidence = 0.92 (REASSURE)
      → "Your order has been approved and payment confirmed. It should be dispatched today or tomorrow at the latest."
  → IF Shipping status = 'SHIPPED':
      → confidence = 0.95 (PROVIDE TRACKING)
      → Provide tracking link if available
      → "Your order has been approved and dispatched. You can track your delivery here: [tracking link]. Royal Mail typically delivers within 1–3 working days, Monday–Saturday."

### Path G: No Order In Progress

IF no order currently in progress:
  → Check if a next order exists in `get_order_history`:
  → IF next order scheduled:
      → confidence = 0.90 (INFORM)
      → "You don't have an order currently being processed. Your next order is scheduled for [date] and will be created automatically — you don't need to do anything. Once it's created, our clinical team will review it within 1–2 working days."
  → IF no next order found:
      → confidence = 0.80 (TROUBLESHOOT)
      → "I can't find any current or upcoming orders on your account. Please make sure you're logged in with the correct email address. You can check your subscription status in the Voy app: Plan tab → profile icon → Manage Plans."

### Path H: Payment Status

⚠️ **Only flag payment issues if status is AWAITING_PAYMENT.** Other payment states are not blocking.

IF Payment State = 'AWAITING_PAYMENT':
  → confidence = 0.85 (INFORM)
  → "Your order is currently awaiting payment. It will only be dispatched once payment goes through. You can update your billing details in the Voy app:

1. Tap the profile icon (top right) on the Home tab
2. Select Account Details
3. Tap Billing Details and update your payment method

Once updated, payment will be retried automatically on your next order date. You can also retry payment manually if it's already been attempted once."

IF Payment State = 'Authorised':
  → Funds are held but card is only charged after approval. If order is rejected/cancelled, funds release in 3–5 working days.

IF Payment State = 'Cancelled':
  → No payment taken. Funds released in 3–5 working days.

### Path K: No AQ Match (Fallback)

IF `get_answered_questionnaire` returns no match for the order in progress:
  → Use `get_order_history` as the source of truth for this path only
  → IF Order status = 'NEW' AND Shipping status = 'READY':
      → Call `get_customer_tasks` to check for pending tasks
      → IF no tasks: reassure clinician will review in 1–2 days (confidence = 0.85)
  → IF Order status = 'NEW' AND Payment State = 'Paid':
      → Order approved, will dispatch today or tomorrow (confidence = 0.92)
  → IF Payment State = 'AWAITING_PAYMENT':
      → go to Path H

---

## Self-Serve Tracking Instructions

⚠️ **Include this at the end of EVERY WISMO response.**

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
  → "Your order hasn't shipped yet. Let me connect you with our team to see if the delivery address can be updated before dispatch."

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
- Royal Mail cannot deliver medication through letterboxes. Advise patients to update delivery to neighbour, safe place, or choose another available option.

⚠️ Patients often refer to "future orders" as "current" when their last order was >10 days ago. Assume patients with their last order more than 10 days ago are referring to an upcoming order, not one from history.

---

## Mandatory Fallback (Tool Failure)

IF `get_order_history` or `get_answered_questionnaire` fails:
  → confidence = 0.80 (INFORM)
  → Provide self-serve tracking instructions (see Self-Serve Tracking Instructions section above)
  → "I'm currently unable to retrieve your live order details. In the meantime, you can check your order status in the Voy app: Plan tab → Track Delivery."

IF patient confirms self-serve didn't resolve their issue:
  → confidence = 0.55 (ESCALATE)

---

## Worked Examples

**Example 1: WISMO — order awaiting patient task**
Patient: "Where is my order? I've been waiting for days."
Agent calls `get_order_history` → order in progress, order number O-12345
Agent calls `get_answered_questionnaire` → AQ matched by O-12345, status = "Awaiting patient task"
Agent calls `get_customer_tasks` → pending: "Upload ID for verification", "Upload photo"
→ confidence = 0.90 (INFORM)
→ "I can see your order O-12345 is currently pending clinical review. Before it can be approved, you'll need to complete two outstanding tasks: your ID verification and a full-body photo upload. You can complete both in the Voy app under the Home tab. Once they're done, our clinical team typically reviews within 1–2 working days.

You can also check your order status anytime in the app: Plan tab → Track Delivery."

**Example 2: WISMO — order approved and shipped**
Patient: "Has my order been dispatched?"
Agent calls `get_order_history` → status = 'FULFILLED', shipping = 'SHIPPED', tracking link available
Agent calls `get_answered_questionnaire` → AQ status = "Approved"
→ confidence = 0.95 (PROVIDE TRACKING)
→ "Yes, your order has been approved and dispatched! You can track it here: [tracking link]. Royal Mail typically delivers within 1–3 working days, Monday–Saturday.

You can also check delivery updates anytime in the Voy app: Plan tab → Track Delivery."

**Example 3: WISMO — clinician waiting for patient response**
Patient: "Why hasn't my order been approved?"
Agent calls `get_order_history` → order in progress
Agent calls `get_answered_questionnaire` → AQ status = "Awaiting patient response"
→ confidence = 0.90 (INFORM)
→ "Your order is currently on hold because our clinical team has sent you a message requesting further information. Please check your inbox (including spam/junk folders) for an email from our clinicians and respond to their questions. Once you've replied, they'll continue reviewing your order.

You can check your order status in the Voy app: Plan tab → Track Delivery."

**Example 4: Stuck package after 5 days**
Patient: "My order was dispatched 5 days ago but tracking hasn't updated."
Agent calls `get_order_history` → dispatched 5 days ago, tracking shows no movement
→ 5 days > 3 working day threshold → confidence = 0.55 (ESCALATE)
→ "Your order was dispatched on [date] and the tracking hasn't updated in 5 days, which is unusual. I'm passing this to our patient care team to investigate with Royal Mail. They'll be in touch within 24–48 hours."

**Example 5: WISMO — payment blocking approved order**
Patient: "My order was approved but nothing's happened."
Agent calls `get_order_history` → order in progress, Payment State = 'AWAITING_PAYMENT'
Agent calls `get_answered_questionnaire` → AQ status = "Approved"
→ confidence = 0.85 (INFORM)
→ "Your order has been approved by our clinical team, but it's currently waiting for payment to go through before it can be dispatched. You can update your billing details in the Voy app: tap the profile icon (top right) on the Home tab → Account Details → Billing Details. Once updated, payment will be retried automatically.

You can check your order status in the app: Plan tab → Track Delivery."

**Example 6: Cold chain concern**
Patient: "My package arrived warm and the ice packs were melted."
→ confidence = 0.95 (SAFETY CHECK)
→ "Please check: (1) Is the outer packaging intact? (2) Is the medication warm? (3) Is the liquid clear with no particles? If the packaging is intact and the medication looks clear, it's likely safe — Wegovy stays stable at room temperature for up to 6 weeks, and Mounjaro for up to 30 days. However, if you notice any damage, warmth, or changes to the liquid, please do NOT use it and let me know so I can arrange next steps."

**Example 7: No AQ match — fallback to order history**
Patient: "What's happening with my order?"
Agent calls `get_order_history` → order in progress, status = 'NEW', shipping = 'READY'
Agent calls `get_answered_questionnaire` → no matching AQ for this order number
→ Path K fallback
Agent calls `get_customer_tasks` → no pending tasks
→ confidence = 0.85 (REASSURE)
→ "Your order is currently awaiting clinical approval, which typically takes 1–2 working days. Once approved, it will be dispatched within 1–3 working days via Royal Mail.

You can check your order status anytime in the Voy app: Plan tab → Track Delivery."
