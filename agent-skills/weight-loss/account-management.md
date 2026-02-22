# Account Management

## Scope

This file covers account detail updates, address management, GP details, and personal information changes for weight loss patients.

**Covers:**
- Updating shipping and billing address (self-serve)
- Updating phone number (self-serve)
- Name and email changes (cannot self-serve)
- Date of birth and height (not viewable/editable)
- GP details update (requires escalation)
- Billing history and receipts

**Does NOT cover:**
- Login issues and account verification → see `account-and-portal.md` (shared)
- Subscription changes → see `subscriptions.md`
- Payment method updates → see `payments.md`
- Order address changes after dispatch → see `orders-and-shipping.md`

**Cross-references:**
- `orders-and-shipping.md` — address changes for dispatched orders (Royal Mail options)
- `payments.md` — updating billing/payment method
- `subscriptions.md` — plan changes and cancellation

---

## Update Shipping Address, Billing Address, or Phone Number

IF patient wants to update their shipping address, billing address, or phone number:
  → confidence = 0.90 (SOLVE — guide to self-serve)

⚠️ Patients must update their address BEFORE their order is placed.

**App flow:**
→ "You can update your details through the Voy app:
1. Open the Voy app
2. Tap the profile icon in the top right corner
3. Select **Account Details**
4. Edit the relevant fields and save"

**Web flow:**
→ "You can also update via the website:
1. Open the Voy website
2. Tap **Account** in the top right corner
3. Select **Account** in the middle left section, then tap **Account Details**
4. Edit the relevant fields and save"

⚠️ Phone numbers must start with `+` (e.g., +44 for UK)

---

## Address Changes and Order Timing

IF patient requests address change AND order is NOT yet approved:
  → confidence = 0.90 (SOLVE)
  → Guide to self-serve update (app or web flow above)
  → "Please make sure to update your address before your next order is approved."

IF patient requests address change AND order is approved but NOT dispatched:
  → confidence = 0.55 (ESCALATE)
  → "Your order has already been approved. Let me connect you with our team to see if the delivery address can be updated before dispatch."
  → ⚠️ NEVER promise they will be able to change it

IF patient requests address change AND order is already dispatched:
  → confidence = 0.88 (SOLVE)
  → "Your order has already been dispatched and we cannot change the delivery address. However, Royal Mail offers these options:
    - Divert to a safe place or neighbour
    - Collect from your local Post Office
    You can manage these via your Royal Mail tracking link."

⚠️ CRITICAL: Agent CANNOT create a new address — only UPDATE existing address fields. If patient reports they cannot update, escalate.

---

## Name, Email, Date of Birth, and Height

IF patient wants to update their first name, last name, or email:
  → confidence = 0.55 (ESCALATE)
  → "You can view these details in your account, but they cannot be updated through the app or website. Let me connect you with our team to make this change."

IF patient wants to view or update their date of birth:
  → confidence = 0.90 (SOLVE)
  → "Date of birth cannot be viewed or edited within the app or website."
  → IF they need to change it: → confidence = 0.55 (ESCALATE)

IF patient asks about their height:
  → confidence = 0.90 (SOLVE)
  → "Height information is not viewable or editable in the app."

---

## GP Details Update

IF patient wants to add or update their GP details:
  → confidence = 0.55 (ESCALATE)

1. Inform the patient that updating GP details must be done by customer service
2. Collect the following information:
   - GP name
   - GP address
   - GP phone number
   - GP email (if available)
3. Once collected, escalate to customer service

→ "Updating GP details requires our team's assistance. Could you please provide your GP's name, address, and phone number? Once I have these details, I'll connect you with our team to process the update."

---

## Billing History and Receipts

IF patient asks for payment history or receipts:
  → confidence = 0.90 (SOLVE — guide to self-serve)

→ "You can view your payment history and receipts in the Voy app:
1. Open the Voy app
2. Tap your initials in the top-right corner
3. Select **Your Orders**
4. Browse the list to view payment details and delivery status

Receipts are provided automatically and can be downloaded from your account."

IF patient needs a custom or official receipt format:
  → confidence = 0.55 (ESCALATE)
  → "Custom or official format receipts require our team's assistance. This typically takes 1–2 working days. Let me connect you with them."

---

## Worked Examples

**Example 1: Address update before order**
Patient: "I need to change my delivery address."
Agent calls `get_order_history` → no order in progress
→ confidence = 0.90 (SOLVE)
→ "You can update your delivery address in the Voy app: tap the profile icon (top right) → Account Details → edit your address and save. Please make sure to do this before your next order is approved."

**Example 2: GP details update**
Patient: "I've changed my GP. How do I update this?"
→ confidence = 0.55 (ESCALATE)
→ "Updating GP details requires our team's assistance. Could you please share your new GP's name, address, phone number, and email (if available)? I'll then connect you with our team to get this updated."

**Example 3: Name change**
Patient: "I got married and need to change my last name."
→ confidence = 0.55 (ESCALATE)
→ "Congratulations! Name changes can't be done through the app or website, but our team can help with this. Let me connect you with them."

**Example 4: Address change after dispatch**
Patient: "My order shipped but I need it sent somewhere else."
→ confidence = 0.88 (SOLVE)
→ "Unfortunately, we can't change the delivery address once an order has been dispatched. Royal Mail does offer options to divert to a safe place, a neighbour, or collect from your local Post Office — you can manage these via your tracking link. For future orders, you can update your address in the Voy app under Account Details."
