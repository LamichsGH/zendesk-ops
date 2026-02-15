# App Features

## Scope

This file covers Voy app navigation, features, bug reporting, and country/region changes for TRT patients.

**Covers:**
- App tab navigation (Home, Progress, Plan, Chat)
- Profile section features
- Download links
- Bug reporting
- Country/region change (iOS and Android)

**Does NOT cover:**
- Order tracking details --> see `orders-and-shipping.md`
- Login issues --> see `account-and-portal.md`
- Medication administration tutorials --> see `how-to-use-medication.md`

**Cross-references:**
- `account-and-portal.md` -- login issues, account access
- `how-to-use-medication.md` -- medication tutorials referenced in the Plan tab video
- `orders-and-shipping.md` -- Plan tab order tracking details
- `blood-tests.md` -- Progress tab blood test result details

---

## App Navigation

IF patient asks about app features or where to find something:
  --> confidence = 0.90 (INFORM)

⚠️ Only describe features that are listed below. If a patient asks about a feature not listed, let them know it is not currently available.

### Home Tab
- Next steps and reminders
- Learn & grow material
- Treatment status updates
- Daily tasks

### Progress Tab
- Steps tracking (iOS only -- connected via Apple Health; patients cannot log or add steps manually, must connect through Apple Health)
- Blood Test 1 and Blood Test 2 results

### Plan Tab
- **For patients on treatment:** shows treatment and order tracking
- **For patients not yet on treatment:** lets them know treatment info will appear here once available
- Informational video with instructions on how to administer medication (see `how-to-use-medication.md` for detailed guidance)
- Treatment guide
- Basic help with side effects

### Chat Tab
- Messaging with Coach Joy (AI) for customer service and clinical queries
- Ability to send messages and see infinite conversation history
- ⚠️ No ability to attach any file formats that are not images -- if other files need to be sent, they can be sent via email

### Profile Section
Accessible from Home, Progress, and Plan tabs via the icon in the top right:
- Personal information management
- Notification settings
- Account security options

---

## Download the App

IF patient needs to download the Voy app:
  --> confidence = 0.90 (INFORM)
  --> Ask if they have an iPhone or Android device
  --> Provide the appropriate app store link

> "You can download the free Voy app from your device's app store. Are you on an iPhone or Android? I'll share the direct link for you."

---

## Bug Reporting

IF patient reports an app bug or technical issue:

### Related to a specific feature
  --> confidence = 0.80 (INFORM/REASSURE)
  --> Guide the patient to the relevant section of the app

### Unable to complete an action
  --> confidence = 0.55 (ESCALATE after gathering info)
  1. Ask the patient to describe the challenge
  2. Request a screenshot via chat
  3. Escalate to customer service

> "I'm sorry you're having trouble with that. Could you describe what's happening and share a screenshot through the chat? I'll escalate this to our team for further investigation."

### Bug or technical problem
  --> confidence = 0.55 (ESCALATE after gathering info)
  1. Ask for a screenshot to report the issue
  2. Offer to escalate to customer service

> "I'm sorry you're experiencing this. Could you take a screenshot of the issue so we can report it? Let me connect you with our team to get this resolved."

---

## Country or Region Change

IF patient needs to change their app store country or region:
  --> confidence = 0.85 (INFORM)
  --> Ask if they are on iPhone or Android

### iPhone (Apple Account)
> "To change your Apple account country or region:
**Before starting:**
- Spend any remaining Apple Account balance
- Cancel subscriptions that block a country change (wait until the end of the subscription period)
- Have a valid payment method for your new country

**Steps:**
1. Open **Settings**
2. Tap your name --> **Media & Purchases**
3. Tap **View Account** (sign in if prompted)
4. Tap **Country/Region**
5. Select your new country and follow the confirmation flow
6. Enter your new payment information and billing address
7. Tap **Next**"

### Android (Google Play)
> "To change your Google Play country or region:
**Important:** You can only change once per year (12-month wait between changes). You must be in the new country and have a payment method from that country.

**Steps:**
1. Open the **Google Play Store** app
2. Tap the profile icon (top right)
3. Tap **Settings** --> **General** --> **Account and device preferences** --> **Country and profiles**
4. Tap the country you want to switch to
5. Follow the on-screen instructions to add a payment method

Note: The profile can take up to 48 hours to update."

---

## Worked Examples

**Example 1: Finding medication instructions**
Patient: "Where can I watch the video on how to take my medication?"
--> confidence = 0.90 (INFORM)
--> "You can find the instructional video in the Voy app: go to the Plan tab, and you'll see the informational video with instructions on how to administer your medication."

**Example 2: Steps not showing**
Patient: "I can't see my steps in the app."
--> confidence = 0.85 (INFORM/REASSURE)
--> "Step tracking is currently available on iOS only and is connected through Apple Health. Please make sure Apple Health is linked to the Voy app in your device settings. Note that steps cannot be logged or added manually -- they must sync through Apple Health. If you're on Android, step tracking is not yet available on that platform."

**Example 3: App crash on Plan tab**
Patient: "The app freezes every time I open the Plan tab."
--> confidence = 0.55 (ESCALATE)
--> "I'm sorry about that. Could you take a screenshot or describe exactly when the freeze happens? I'll connect you with our team to investigate and resolve this."

**Example 4: Patient asks about a feature not in the app**
Patient: "Can I log my meals in the app?"
--> confidence = 0.90 (INFORM)
--> "Meal logging is not currently a feature available in the Voy app for TRT patients. If this changes in the future, it will appear in your app. Is there anything else I can help you with?"

**Example 5: Requesting to send a PDF via chat**
Patient: "I need to send my doctor's letter through the chat."
--> confidence = 0.85 (INFORM/REASSURE)
--> "The chat currently only supports image attachments. For other file formats like PDFs, you can send them to us via email instead. Would you like me to provide the email address?"

**Example 6: Blood test results**
Patient: "Where can I see my blood test results?"
--> confidence = 0.90 (INFORM)
--> "You can view your Blood Test 1 and Blood Test 2 results in the Progress tab of the Voy app."
