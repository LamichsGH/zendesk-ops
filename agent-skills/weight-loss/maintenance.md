# Weight Loss Maintenance

## Scope

This file covers maintenance phase guidance for patients who have reached or are approaching their goal weight, including stepping down or stopping treatment.

**Covers:**
- Maintenance phase explanation (what it is, why it matters)
- How to start maintenance (booking a maintenance consultation)
- Down-titration (gradual dose reduction process)
- Stopping treatment / treatment breaks
- Mounjaro to Wegovy switch for affordability during maintenance
- Lifestyle maintenance tips (general guidance)

**Does NOT cover:**
- Dose changes during active treatment (still losing weight) → see `dose-changes.md`
- Subscription cancellation, pausing, or plan changes → see `subscriptions.md`
- Side effects from medication → see `side-effects.md`
- General lifestyle/nutrition principles → see `lifestyle-guidance.md`

**Cross-references:**
- `dose-changes.md` — titration schedules, treatment gap rules
- `subscriptions.md` — cancellation flow, restart process
- `side-effects.md` — managing side effects during down-titration
- `lifestyle-guidance.md` — general nutrition, exercise, and mindset guidance

---

## Tool Calls

Before responding, gather context:

1. **`get_subscription_status`** — Confirm the patient has an active subscription and identify plan type
2. **`get_order_history`** — Identify current medication (Wegovy/Mounjaro) and dose level

---

## Critical Rules

- NEVER advise a patient to stop medication abruptly. Stopping GLP-1 medication suddenly can lead to rebound weight gain and other effects. Always recommend gradual dose reduction under clinical supervision.
- NEVER promise specific weight maintenance outcomes. Weight regain risk varies by individual.
- ALWAYS recommend clinical review before any changes to maintenance approach. Down-titration and medication switches are clinician-led decisions.
- NEVER provide specific pricing information for medications or plans.
- NEVER create a personalised maintenance plan — that is the clinician's role.

---

## Decision Trees

### "What is maintenance?" / Patient has reached goal weight

IF patient asks what the maintenance phase is OR mentions reaching their goal weight:
  → confidence = 0.85 (SOLVE)

"Congratulations on reaching this milestone — that's a real achievement! The maintenance phase is the next stage of your journey, and it's all about protecting the progress you've made.

Rather than stopping medication suddenly, maintenance involves working with your clinician to gradually reduce your dose to find your 'steady dose' — the lowest effective dose where:
- Food noise and cravings feel manageable
- You have moderate, healthy hunger (enough to eat regularly and enjoy food)
- You feel comfortable with no significant side effects

Most patients continue on a low maintenance dose long-term. GLP-1 medications are safe for ongoing use and are well-studied — think of it like a long-term treatment for blood pressure or cholesterol.

To get started, you can book a maintenance consultation with your clinician: [Book a Maintenance Consultation](https://booking.vfrhealth.com/s/maintenance)"

---

### "How do I start maintenance?"

IF patient asks how to begin the maintenance phase:
  → confidence = 0.85 (SOLVE)

"To get started with maintenance, you'll need a consultation with your clinician, who will create a personalised maintenance plan based on your progress and goals. You have two options:

1. **1:1 Clinician Call** — A dedicated call to walk through your maintenance options and build a plan that works for you. You can book here: [Book a Maintenance Consultation](https://booking.vfrhealth.com/s/maintenance)
2. **Group Webinar** — A 30-minute group session with a Health Coach and Clinician to learn what works for lasting results. You can book here: [Book a Maintenance Webinar](https://booking.vfrhealth.com/s/maintenance-webinar)

During the consultation, your clinician will discuss options like adjusting your dose, and — if you're on Mounjaro — potentially switching to a more affordable medication for the maintenance phase."

---

### "Can I stop taking medication?"

IF patient asks about stopping medication entirely or taking a treatment break:
  → confidence = 0.85 (SOLVE)

"It's understandable to wonder about stopping medication once you've reached your goal. Here's what's important to know:

- **Treatment breaks are possible** if your BMI is above 22. Your clinician can assess whether this is appropriate for you.
- **Weight regain is common** — research shows that patients can regain up to two-thirds of the weight they lost after stopping GLP-1 medication. The maintenance phase is specifically designed to reduce this risk.
- **Never stop suddenly** — if you do decide to step back from medication, it should be done gradually under clinical guidance (down-titration), not by stopping abruptly.
- **You can always return** — if you take a break and find you need medication again, you can restart (though you may need to begin at the lowest dose if more than 4 weeks have passed).

I'd recommend discussing this with your clinician so they can advise on the best approach. You can book a maintenance consultation here: [Book a Maintenance Consultation](https://booking.vfrhealth.com/s/maintenance)"

---

### Down-titration questions

IF patient asks about reducing their dose during maintenance:
  → confidence = 0.85 (SOLVE)

"Down-titration is the process of gradually reducing your medication dose to find the lowest dose that still keeps your weight stable and cravings manageable. Here's how it works:

- Your clinician will guide each step — this is not something to do on your own.
- Dose reductions happen gradually over several weeks, with monitoring at each step.
- The goal is to find your 'steady dose' where you feel in control, have healthy hunger, and experience no significant side effects.
- Some patients maintain on a very low dose, while others may need a moderate dose — it varies.

You're always in control of your dose — your clinician won't change it unless you're ready. If you'd like to start this process, I'd recommend booking a maintenance consultation."

---

### Mounjaro to Wegovy switch for affordability

IF patient on Mounjaro asks about switching to Wegovy for maintenance / cost reasons:
  → confidence = 0.55 (ESCALATE)

**internal_note:** "Issue: Patient on Mounjaro is interested in switching to Wegovy for maintenance/affordability reasons.\nAction: Clinical team to review current dose, treatment history, and approve the medication switch. Patient has been informed this requires clinical review."

"Switching from Mounjaro to Wegovy can be a great option for maintenance — Wegovy works in a similar way by reducing food noise and cravings, and can be more affordable long-term. Your clinician would move you to an equivalent dose, so you wouldn't be starting from scratch.

However, this switch does need to be reviewed and approved by our clinical team to ensure it's right for you. I've passed this to our patient care team, and they'll be in touch within 24-48 hours to discuss your options."

---

### "What should I do to maintain my weight?" / Lifestyle maintenance tips

IF patient asks for general tips on maintaining weight after reaching their goal:
  → confidence = 0.85 (SOLVE)

"Here are some general principles that can support long-term weight maintenance:

- **Weigh regularly** — Step on the scales 1-2 times a week at the same time of day. Catching small shifts early helps you stay on track.
- **Stay active** — Aim for 40-60 minutes of activity on most days. Brisk walking, cycling, swimming, or dancing all count.
- **Prioritise protein and fibre** — Include a palm-sized portion of protein (eggs, chicken, fish, tofu) and plenty of vegetables at each meal. These keep you full and support muscle health.
- **Strength training** — 2-3 short sessions per week (squats, push-ups, resistance bands) help preserve muscle, which supports your metabolism.
- **Prioritise sleep** — Aim for 7-9 hours most nights. Good sleep reduces cravings and supports healthy choices.

*This is general guidance — your clinician can advise on what's right for you.*

You can also book a maintenance consultation to get a personalised plan from your clinician: [Book a Maintenance Consultation](https://booking.vfrhealth.com/s/maintenance)"

---

## Worked Examples

**Example 1: Patient reached goal weight and wants to understand maintenance**

Patient: "I've hit my target weight! Do I just stop taking Mounjaro now?"

- Agent calls `get_subscription_status` → active, 28-day plan
- Agent calls `get_order_history` → Mounjaro 10mg

→ confidence = 0.85 (SOLVE)

"Congratulations on reaching your target weight — that's a fantastic achievement!

Rather than stopping Mounjaro suddenly, the recommended approach is to enter the maintenance phase. This involves working with your clinician to gradually reduce your dose to find the lowest level that keeps your weight stable and cravings manageable. Stopping GLP-1 medication abruptly can lead to weight regain — research shows patients can regain up to two-thirds of the weight they lost.

Most patients continue on a low maintenance dose long-term, and GLP-1 medications are safe for ongoing use.

To get started, I'd recommend booking a maintenance consultation with your clinician. You have two options:
1. **1:1 Clinician Call**: [Book a Maintenance Consultation](https://booking.vfrhealth.com/s/maintenance)
2. **Group Webinar**: [Book a Maintenance Webinar](https://booking.vfrhealth.com/s/maintenance-webinar)

I hope this helps! If you have any other questions, just reply to this email."

---

**Example 2: Patient wants to switch from Mounjaro to Wegovy for cost**

Patient: "I'm in maintenance now and Mounjaro is expensive. Can I switch to something cheaper?"

- Agent calls `get_subscription_status` → active
- Agent calls `get_order_history` → Mounjaro 5mg

→ confidence = 0.55 (ESCALATE)

internal_note: "Issue: Patient on Mounjaro 5mg is in maintenance phase and wants to switch to a more affordable medication.\nAction: Clinical team to review and approve Mounjaro → Wegovy switch."

"Switching from Mounjaro to Wegovy can be a great option during maintenance — it works in a similar way and can be more affordable long-term. Your clinician would move you to an equivalent dose, so you wouldn't need to start from scratch.

This does need to be reviewed and approved by our clinical team to make sure it's the right move for you. I've passed this to our patient care team. They'll be in touch within 24-48 hours to discuss your options.

We're currently experiencing high demand, so please allow 24-48 hours for a response."

---

**Example 3: Patient asks about stopping medication entirely**

Patient: "I've been on Wegovy for 8 months and I'm at my goal weight. I'd really like to come off the medication completely. Is that possible?"

- Agent calls `get_subscription_status` → active
- Agent calls `get_order_history` → Wegovy 1.0mg

→ confidence = 0.85 (SOLVE)

"It's great that you've been thinking about this, and it's absolutely something you can discuss with your clinician. Here's what to keep in mind:

- Treatment breaks are possible if your BMI is above 22, which your clinician can assess.
- It's important not to stop suddenly — your clinician would guide you through a gradual dose reduction (down-titration) to give your body time to adjust.
- Weight regain is common after stopping GLP-1 medication — up to two-thirds of weight lost can return. The maintenance phase and down-titration process are designed to reduce this risk.
- If you do take a break and decide to restart later, you can — though if more than 4 weeks pass, you'd need to restart at the lowest dose (Wegovy 0.25mg).

I'd recommend booking a maintenance consultation so your clinician can create a plan tailored to you: [Book a Maintenance Consultation](https://booking.vfrhealth.com/s/maintenance)

I hope this helps! If you have any other questions, just reply to this email."

---

**Example 4: Patient asks for lifestyle tips to maintain weight**

Patient: "I'm starting maintenance soon. What should I be doing day-to-day to keep the weight off?"

→ confidence = 0.85 (SOLVE)

"Great question — building strong daily habits is one of the best things you can do to support long-term weight maintenance. Here are some evidence-based principles:

- **Weigh regularly** — 1-2 times a week, same time of day. Catching small changes early makes them easier to address.
- **Stay active** — Aim for 40-60 minutes of activity on most days. Walking, cycling, swimming, or anything you enjoy all count.
- **Prioritise protein and fibre** — A palm of protein and plenty of vegetables at each meal will keep you full and support muscle health.
- **Strength training** — 2-3 sessions per week helps preserve muscle, which keeps your metabolism strong.
- **Prioritise sleep** — 7-9 hours most nights. Quality sleep reduces cravings and supports better choices.

*This is general guidance — your clinician can advise on what's right for you.*

When you book your maintenance consultation, your clinician will also create a personalised plan that takes your specific situation into account.

I hope this helps! If you have any other questions, just reply to this email."
