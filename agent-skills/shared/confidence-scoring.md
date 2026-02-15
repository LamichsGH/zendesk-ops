# Confidence Scoring & Routing

**CRITICAL RULE: You cannot both ASK A QUESTION and ESCALATE. Pick one.**

## Route Decision Tree

**SOLVE (0.90+):** Query fully answered from KB articles OR skill file decision trees
- You have all the info needed
- Customer doesn't need to do anything
- Close with: "I hope this helps!"

**MONITOR (0.60-0.89):** You need info from the CUSTOMER before you can help
- Missing information that only THEY can provide
- Need them to choose between options YOU can handle
- Set awaiting_customer_input = true
- **CRITICAL:** Do NOT say "I've passed this to our team" - YOU are waiting for their answer
- Close with: "Once you let me know [X], I'll be able to help you further."
- Example: "Which option would you prefer? Once you let me know, I can arrange this for you."

**ESCALATE (<0.60):** The TEAM needs to handle this
- Refunds, billing issues, clinical concerns
- Team needs to take action (not just answer a question)
- **CRITICAL:** Do NOT ask customer to make decisions - the team will handle it
- Close with: "I've passed this to our patient care team. They'll be in touch within 24-48 hours."
- Example: Refund requests - Just escalate, don't ask "do you want a refund or replacement?"

## Specific Score Triggers
- **<=0.35 (URGENT)**: Clinical emergencies, legal threats - send_response = false
- **<=0.55**: Refunds, billing disputes, clinical concerns, medication changes - ESCALATE to team
- **<=0.75**: Complex issues where skill file does not specify a SOLVE path - ESCALATE to team

## Skill File Override Rule (CRITICAL)
When a pathway skill file (e.g., blood-tests.md, orders-and-shipping.md) specifies a confidence score for a scenario, USE THAT SCORE — even if the general rules above would suggest a different route.

Example: If blood-tests.md says "4-7 days since shipped → confidence = 0.85 (SOLVE)", then you SOLVE at 0.85. Do NOT override this to escalate just because the general rules mention "missing data" or "replacement requests."

Skill file decision trees are MORE SPECIFIC than these general rules. Specific always wins over general.

## Key Rules
- awaiting_customer_input: TRUE only if you asked a specific question needing their answer
- Multi-question: reduce confidence by 0.1 per unanswered question
