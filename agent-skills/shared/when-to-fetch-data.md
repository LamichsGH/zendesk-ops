# When to Fetch Backend Data

This guide tells you when to use each backend data tool. Only fetch data when the customer's query requires it.

## Get Order History
**Use when customer asks about:**
- Orders, deliveries, shipping, tracking
- Medication they've received or are waiting for
- Blood test kits (TRT pathway)
- Refund requests (need order details)
- "Where is my order/kit?"

**Do NOT fetch when:**
- Customer asks a general question (e.g., "how do I unsubscribe?")
- Question is about policies, processes, or general info
- Question is purely about their subscription (use Get Subscription instead)

## Get Subscription Status
**Use when customer asks about:**
- Subscription status, billing, payments
- Cancellation requests
- Next payment date
- Subscription changes or pausing

**Do NOT fetch when:**
- Customer asks about a specific order/delivery
- Question is general info that doesn't need account data

## Get Customer Tasks
**Use when customer asks about:**
- Pending tasks or outstanding actions
- Blood test status (TRT - may have pending task)
- "What do I need to do?"

**Do NOT fetch when:**
- Customer's question doesn't relate to account actions
- Simple informational queries

## Get Answered Questionnaire
**Use when customer asks about:**
- Order approval status (clinical review stage)
- Why their order hasn't been approved yet
- "Where is my order?" when order is still pending approval
- Tasks blocking their order

**Do NOT fetch when:**
- Order is already shipped or delivered (use Get Order History instead)
- Customer is asking about subscription, billing, or cancellation
- Question is about general info, policies, or processes
- Patient is NOT on the weight loss pathway

**How to use with Get Order History:**
- Always call Get Order History FIRST to identify the order in progress
- Then call Get Answered Questionnaire to get the clinical approval status
- Match the AQ to the order by order number
- The AQ status is the source of truth for approval stage (NOT the order status from Get Order History)

## General Rules
- If in doubt, fetch the data - it's better to have context than to miss something
- For refund/complaint tickets, fetch Order History to understand the situation
- For TRT blood test queries, ALWAYS fetch Order History
- For subscription queries, ALWAYS fetch Subscription Status
- You can fetch multiple data sources if the query spans topics
