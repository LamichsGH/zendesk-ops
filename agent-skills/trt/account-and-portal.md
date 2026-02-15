# Account & Portal

## Scope

**Covers:** Login issues, account verification, MANUAL-to-Voy brand migration, portal navigation, results access, ID verification (primary and secondary), health questionnaire, GP details updates.

**Does NOT cover:** Subscription changes or cancellations (see `subscriptions.md`), clinical treatment questions (see `treatment.md`), blood test booking or kit issues (see `blood-tests.md`).

**Cross-references:**
- `required-tasks.md` — for task status checks (ID verification task, health questionnaire task)
- `subscriptions.md` — for plan or billing queries raised alongside account issues

---

## MANUAL to Voy Brand Migration

The company rebranded from "MANUAL" to "Voy". Many customers still have accounts under the MANUAL brand.

IF patient can't log in to Voy:
  → confidence = 0.85 (SOLVE)
  → "If you had an account with MANUAL, your account is still active — it's the same service under a new name. Try logging in with the credentials you used for MANUAL."
  → If they used manual.co: their account still works, the MANUAL portal remains valid during migration

IF patient says links redirect to the wrong site:
  → confidence = 0.85 (SOLVE)
  → Explain the rebrand and that both sites are valid during migration

IF patient says "phone number already linked to an account":
  → confidence = 0.85 (SOLVE)
  → "That's your existing account from when the service was called MANUAL. You can log in with your original credentials."

---

## Login Troubleshooting

IF patient says "password reset not working" or "no reset email received":
  → confidence = 0.85 (SOLVE)
  → Troubleshooting steps:
    1. Check spam/junk folder
    2. Check they're using the correct email address
    3. Try both MANUAL and Voy portals
    4. Common email typos: ivloud→icloud, gmial→gmail, hotmial→hotmail
  → If none of these work: escalate for backend check

IF patient says "reset button not visible" or "can't find reset option":
  → confidence = 0.85 (SOLVE)
  → Provide direct password reset link from KB
  → Search KB for "password reset" or "forgot password"

IF patient has reCAPTCHA failure during signup/login:
  → confidence = 0.85 (SOLVE)
  → "This is usually a browser compatibility issue. Try switching to a different browser (Chrome works well) or clearing your browser cache."

IF all troubleshooting fails:
  → confidence = 0.55 (ESCALATE)
  → "I've passed this to our team to investigate your account access. They may need to set up a temporary password for you."

---

## Account Verification Protocol

⚠️ BEFORE providing any account-specific information or promising actions, verify the account exists.

IF patient's email doesn't match any account:
  → Do NOT provide portal instructions or clinical information
  → confidence = 0.80 (INFORM/REASSURE)
  → "I wasn't able to locate an account with that email address. Could you let me know the email you signed up with, or your full name and date of birth? That will help me find your account."

IF patient asks for account-specific help (results, orders, prescriptions):
  → Check patient context first — do we have external_id and email?
  → If external_id is present: account exists, proceed normally
  → If external_id is missing or patient status is "non_customer":
    → Ask for verification: full name, email on file, or DOB
    → Do NOT promise actions before confirming the account

---

## Portal Navigation

IF patient can't find their results in the portal:
  → confidence = 0.85 (SOLVE)
  → Search KB for portal navigation instructions
  → Guide them to the specific results section
  → If results should be there but aren't: escalate for backend check

IF patient says "portal shows I need to do another test" but they've already done it:
  → confidence = 0.55 (ESCALATE)
  → This may be a system error or unlinked results
  → "That doesn't look right. I've flagged this for our team to check your account and make sure your test results are properly linked."

IF patient asks for PDF copy of results:
  → confidence = 0.85 (SOLVE)
  → Use get_prescription_copy tool if available
  → Or offer: "I can arrange for a PDF copy of your results to be emailed to you."
  → Do NOT just say "check the portal" if they specifically asked for a PDF

---

## ID Verification Requirements

### When ID Verification Is Needed

IF patient asks when ID verification is needed:
  → confidence = 0.90 (SOLVE)
  → ID verification is required for:
    - TRT consultation booking
    - Prescription-related requests
    - Data access/deletion requests (GDPR)
  → ID is NOT required for:
    - Blood test ordering only
    - General product/pricing questions
    - Portal navigation help

### Accepted Forms of ID

Any valid form of ID containing **all three** of: full name, date of birth, and a photo.

Specific accepted documents:
- **Passport** — UK, Channel Islands, Isle of Man, British Overseas Territory, EEA state, or Commonwealth country (including Irish Passport Cards)
- **Driving licence** — UK, Channel Islands, Isle of Man, or EEA state (including provisional licences)
- **Blue Badge**
- **ID card** with a Proof of Age Standards Scheme (PASS) hologram
- **Biometric immigration document**
- **Ministry of Defence Form 90** (Defence Identity Card)
- **National identity card** from an EEA state

IF patient only has international documents NOT on the above list:
  → confidence = 0.55 (ESCALATE)
  → "I'll need to check with our team whether we can accept that form of ID. Let me pass this on and they'll confirm for you."
  → This is assessed case-by-case

### How to Upload ID

IF patient asks how to upload their ID:
  → confidence = 0.90 (SOLVE)
  → Steps:
    1. Go to the **Home** tab in the app
    2. Under the **Actions** section, find the **"ID Verification"** task
    3. Follow the on-screen prompts to upload a photo of their ID
  → Always inform the patient: "After uploading your ID, there will also be a secondary identity check — either during your video consultation with the doctor, or our team will reach out to you afterwards."
  → Cross-reference: see `required-tasks.md` for task status checks

### Secondary ID Check

⚠️ The secondary ID check is a separate step from the initial ID upload. It is handled in one of two ways:

1. **During the initial video consultation** — the doctor cross-checks the patient's identity live on the call
2. **After consultation** — the customer service team will reach out to the patient requesting a selfie or photo of the patient holding their ID next to their face

⚠️ NEVER offer to connect the patient to customer service for the secondary ID check. This is initiated by the team via a call, or a human agent will reach out to them directly.

IF patient asks about the secondary ID check:
  → confidence = 0.85 (INFORM/REASSURE)
  → "The secondary identity check is either done during your video consultation with the doctor, or our team will reach out to you afterwards to arrange it. You don't need to do anything extra right now."

IF patient says they haven't had a secondary ID check and is worried:
  → confidence = 0.85 (INFORM/REASSURE)
  → "If the doctor didn't do the identity check during your consultation, our team will be in touch to arrange it. It's a standard part of the process and nothing to worry about."

IF patient asks to be connected to someone for the secondary check:
  → confidence = 0.85 (INFORM/REASSURE)
  → Do NOT escalate or offer to connect them
  → "Our team will reach out to you directly to arrange this — you don't need to contact anyone separately."

---

## Health Questionnaire

The health questionnaire is required to determine eligibility for TRT treatment and to identify any additional health checks that may be needed.

IF patient asks about the health questionnaire or how to complete it:
  → confidence = 0.90 (SOLVE)
  → Steps:
    1. Go to the **Home** tab in the app
    2. Find the **"Complete your health profile"** task
    3. Follow the on-screen questions to complete the questionnaire
  → Cross-reference: see `required-tasks.md` for task status checks

IF patient cannot see the health questionnaire task even though it is listed as a "task to complete":
  → confidence = 0.55 (ESCALATE)
  → "It sounds like there may be a display issue with that task. I'll flag this for our team to look into so they can get it sorted for you."

IF patient is having difficulties completing the health questionnaire (errors, won't submit, stuck on a question):
  → confidence = 0.55 (ESCALATE)
  → "I'm sorry you're having trouble with the questionnaire. I've raised this with our team and they'll help get it resolved."

IF patient asks what the health questionnaire is for:
  → confidence = 0.85 (INFORM/REASSURE)
  → "The health questionnaire helps our clinical team assess your eligibility for TRT treatment and identify if any additional health checks are needed before you can proceed."

---

## GP Details Update

⚠️ Updating GP details must be done by the customer service team — patients cannot do this themselves in the app or on the website.

IF patient wants to update their GP details:
  → confidence = 0.55 (ESCALATE)
  → Collect the following before escalating:
    - **GP name**
    - **GP address**
    - **GP phone number**
    - **GP email** (if available)
  → Once all details are collected, escalate with the information
  → "I'll pass your updated GP details to our team and they'll get this updated for you."

IF patient wants to update their first name, last name, or email:
  → These are visible in the account but cannot be updated via the app or website
  → Only offer to escalate if the patient explicitly asks for a change
  → IF patient explicitly asks:
    → confidence = 0.55 (ESCALATE)
    → "I can't update that directly, but I'll pass this to our team to get it changed for you."

IF patient asks to view or update their date of birth:
  → confidence = 0.85 (INFORM/REASSURE)
  → Date of birth cannot be viewed or edited in the app or website
  → IF patient needs it changed:
    → confidence = 0.55 (ESCALATE)
    → "Date of birth isn't something that can be updated through the app. I'll raise this with our team to assist."

---

## Worked Examples

### Example 1: Patient can't log in after rebrand

> **Patient:** "I'm trying to log in but it says my account doesn't exist. I used to use MANUAL."
>
> **Agent response (confidence = 0.85, SOLVE):**
> "Your account is still active — MANUAL has rebranded to Voy, but your login credentials remain the same. Please try logging in with the email and password you used for your MANUAL account. Both the MANUAL and Voy portals are working during the migration period, so you can use either one."

### Example 2: Patient asks what ID they need

> **Patient:** "What ID do I need to upload for my TRT consultation?"
>
> **Agent response (confidence = 0.90, SOLVE):**
> "You'll need a valid form of photo ID that shows your full name and date of birth. We accept passports (UK or international from EEA/Commonwealth countries), driving licences (including provisional), Blue Badges, biometric immigration documents, national identity cards from EEA states, and PASS-hologram ID cards. To upload it, head to the Home tab in your app and look for the 'ID Verification' task under Actions. Just so you're aware, there'll also be a secondary identity check — either during your consultation or our team will reach out afterwards to arrange it."

### Example 3: Patient wants to update GP details

> **Patient:** "I've changed GPs and need to update my records."
>
> **Agent response (confidence = 0.55, ESCALATE):**
> "I can arrange for your GP details to be updated. Could you provide me with your new GP's name, address, phone number, and email address if you have it? Once I have those details, I'll pass them to our team to get your records updated."

### Example 4: Patient asks about secondary ID check

> **Patient:** "I've uploaded my ID but nobody has checked it yet. Can you put me through to someone?"
>
> **Agent response (confidence = 0.85, INFORM/REASSURE):**
> "The secondary identity check is either completed during your video consultation with the doctor, or our team will reach out to you directly afterwards to arrange it. You don't need to contact anyone separately — they'll be in touch with you."

### Example 5: Patient can't find health questionnaire

> **Patient:** "The app says I need to complete my health profile but I can't see the task anywhere."
>
> **Agent response (confidence = 0.55, ESCALATE):**
> "I'm sorry about that — it sounds like there may be a display issue. I've flagged this for our team to investigate so they can get the health questionnaire accessible for you."

### Example 6: Patient has an international ID not on the accepted list

> **Patient:** "I only have a US state driver's licence. Will that work?"
>
> **Agent response (confidence = 0.55, ESCALATE):**
> "Our standard accepted IDs include UK and EEA-issued documents. Since your ID is from outside those regions, I'll need to check with our team whether it can be accepted. Let me pass this on and they'll confirm for you."
