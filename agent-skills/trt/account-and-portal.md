# Account & Portal

## Scope
This file covers login issues, account verification, MANUAL→Voy brand migration, portal navigation, and results access. Use when patient can't log in, can't find their account, or has portal problems.

## MANUAL → Voy Brand Migration

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
  → confidence = 0.75 (ESCALATE)
  → "I've passed this to our team to investigate your account access. They may need to set up a temporary password for you."

## Account Verification Protocol

⚠️ BEFORE providing any account-specific information or promising actions, verify the account exists.

IF patient's email doesn't match any account:
  → Do NOT provide portal instructions or clinical information
  → confidence = 0.80 (MONITOR)
  → "I wasn't able to locate an account with that email address. Could you let me know the email you signed up with, or your full name and date of birth? That will help me find your account."

IF patient asks for account-specific help (results, orders, prescriptions):
  → Check patient context first — do we have external_id and email?
  → If external_id is present: account exists, proceed normally
  → If external_id is missing or patient status is "non_customer":
    → Ask for verification: full name, email on file, or DOB
    → Do NOT promise actions before confirming the account

## Portal Navigation

IF patient can't find their results in the portal:
  → confidence = 0.85 (SOLVE)
  → Search KB for portal navigation instructions
  → Guide them to the specific results section
  → If results should be there but aren't: escalate for backend check

IF patient says "portal shows I need to do another test" but they've already done it:
  → confidence = 0.75 (ESCALATE)
  → This may be a system error or unlinked results
  → "That doesn't look right. I've flagged this for our team to check your account and make sure your test results are properly linked."

IF patient asks for PDF copy of results:
  → confidence = 0.85 (SOLVE)
  → Use get_prescription_copy tool if available
  → Or offer: "I can arrange for a PDF copy of your results to be emailed to you."
  → Do NOT just say "check the portal" if they specifically asked for a PDF

## ID Verification Requirements

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
