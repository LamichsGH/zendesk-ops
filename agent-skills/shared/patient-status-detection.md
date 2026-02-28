# Patient Status Detection

When backend tools (get_order_history, get_subscription_status) return empty results or fail, check whether the patient's message language suggests they are an active, current patient.

## Language Signals of an Active Patient

The patient is likely on active treatment if they:
- Reference specific medications by name (e.g. "my Cypionate", "my Clomifene", "my finasteride", "my Wegovy")
- Mention their dosage, dose changes, or injection schedule
- Describe side effects from treatment they are taking
- Reference past consultations with named clinicians (e.g. "Dr Reville updated my prescription")
- Say "my prescription", "my next order", "my subscription", "my account"
- Describe ongoing treatment experience ("since I started treatment", "my last blood test", "my last injection")
- Reference portal/account features only available to current patients

## When to Apply This Rule

Apply ONLY when BOTH conditions are true:
1. The patient's message contains one or more language signals above
2. Backend data tools returned empty, failed, or the patient is classified as non_customer/onboarding

If backend tools successfully return order/subscription data, this rule does not apply — use the actual data.

## What to Do When a Mismatch is Detected

### In your response to the patient:
- Address them as a current patient — do NOT treat them as new or prospective
- Acknowledge what they've told you about their treatment
- Ask them to confirm their account details so we can locate their records: "So I can pull up your account, could you confirm the full name and email address you registered with?"

### In the internal_note:
- Prepend this line before your normal summary: "⚠️ Patient language strongly suggests active treatment but backend data lookup returned no matching records. Possible data sync/migration issue — verify patient account manually."

### In confidence_reasoning:
- Explicitly note the mismatch (e.g. "Patient references specific medications and clinician but no backend data found — likely account/data issue")

### Routing decision:
- **MONITOR** if you can partially answer their question but need their account details to proceed. Set `awaiting_customer_input = true`.
- **ESCALATE** if their question requires backend data you cannot provide (e.g. confirming a personalised dose, checking a specific order) AND also requires team action (e.g. fixing a portal issue, verifying a prescription). Still ask for account details in your reply so the team has them ready.

### Do NOT:
- Tell the patient we can't find their account (this is alarming)
- Ask them to "create an account" or "sign up"
- Ignore the mismatch and respond as if they are a new customer
