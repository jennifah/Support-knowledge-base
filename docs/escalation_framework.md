# 🚨 Escalation Framework

**Purpose:** Ensure support agents exhaust all self-serve options before involving Engineering or senior staff.  
**Principle:** Every escalation should come with a complete diagnosis, not a question.

---

## The Core Rule

> Escalate findings, not problems.

When you escalate, you are not asking Engineering to investigate from scratch.
You are handing them a complete picture so they can act immediately.

---

## Escalation Tiers

### Tier 1 — Self-Resolve (You own this)
- Account questions, billing, password resets
- Known bugs with documented workarounds
- Configuration issues (wrong settings, permissions)
- Any issue covered in this knowledge base

**Target resolution time:** Same day

---

### Tier 2 — Internal Escalation (Team Lead / Senior Agent)
- Issue not covered in any runbook
- Customer is threatening churn or escalating emotionally
- Same issue reported by 3+ customers in the same week
- You have completed all runbook steps and are stuck

**What to bring:**
- Summary of the issue in 3 sentences
- Steps you already took
- What you ruled out
- Customer tier (SMB / Mid-market / Enterprise)

**Target response time:** 4 hours

---

### Tier 3 — Engineering Escalation
- Confirmed platform bug (reproducible, not config-related)
- Data loss or security concern
- API returning unexpected errors not explained by customer config
- Webhook infrastructure failure

**What to bring (mandatory):**
- Bug report using the standard template
- Affected customer ID and tier
- Steps to reproduce (exact, numbered)
- Expected vs actual behaviour
- Error logs, response codes, timestamps
- Whether other customers are affected

**Target response time:** Next business day (P1 bugs: immediate)

---

## Priority Levels

| Priority | Criteria | Response Target |
|----------|----------|-----------------|
| P1 — Critical | Data loss, security breach, full outage | Immediate |
| P2 — High | Core feature broken, enterprise customer blocked | 2 hours |
| P3 — Medium | Feature degraded, workaround exists | Same day |
| P4 — Low | Minor issue, cosmetic, single user affected | Next day |

---

## What a Good Escalation Looks Like

**❌ Bad escalation:**
> "Customer can't connect their webhook. Can you look into it?"

**✅ Good escalation:**
> "Customer (Enterprise, ID: 48291) reports webhook not firing since 14 Jan 14:00 UTC. I confirmed: webhook is Active, endpoint URL is correct (https), subscribed to `candidate.created` events. Test payload delivered successfully. Live events not arriving. Delivery logs show 3 failed attempts with `503` from their server — may be a capacity issue on their end. Attaching logs. No other customers reporting same issue. Recommending we advise customer to check their server load before we escalate further."

---

## After You Escalate

- Keep the customer updated every 24 hours — even if there is no update
- Own the ticket until it is resolved. Escalating does not mean handing off.
- When Engineering resolves it, document the fix in the knowledge base
