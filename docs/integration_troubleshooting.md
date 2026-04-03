# 🔌 Integration Troubleshooting Runbook

**Last Updated:** 2024  
**Owner:** Support Team  
**Applies To:** All B2B customers using API or webhook integrations

---

## How To Use This Runbook

Start at Step 1. Do not skip steps. Only escalate to Engineering after completing all steps and documenting findings.

---

## Issue Type 1: Webhook Not Firing

### Symptoms
- Customer reports their system is not receiving events
- No payload appearing in their endpoint logs
- Integration was working previously

### Step-by-Step Diagnosis

**Step 1 — Confirm the webhook is active**
- Navigate to Settings → Integrations → Webhooks
- Check status is set to `Active` (not `Paused` or `Disabled`)
- If paused: ask customer when it was paused and why

**Step 2 — Verify the endpoint URL**
- Ask the customer to share their endpoint URL
- Check for common mistakes:
  - Trailing slash missing or added incorrectly
  - `http://` instead of `https://`
  - Typo in the subdomain

**Step 3 — Check event subscriptions**
- Confirm which events the webhook is subscribed to
- Ask the customer which specific event they expect to receive
- Cross-reference — if the event type is not subscribed, it will never fire

**Step 4 — Test with a manual trigger**
- Use the "Send Test Payload" button in the integration settings
- Ask the customer to confirm receipt on their end
- If test payload arrives but live events do not → issue is likely timing/trigger related

**Step 5 — Review recent delivery logs**
- Check the Webhook Delivery Logs for failed attempts
- Look for HTTP response codes:
  - `200` = delivered successfully
  - `4xx` = customer endpoint rejected it (their side)
  - `5xx` = their server errored (their side)
  - `timeout` = their server took too long to respond

**Step 6 — Document and escalate if unresolved**
- Complete the escalation template before filing to Engineering
- Include: webhook ID, endpoint URL, event type, last successful delivery timestamp, error codes seen

---

## Issue Type 2: API Authentication Failures

### Symptoms
- Customer receiving `401 Unauthorized` or `403 Forbidden` errors
- API was working, now returning auth errors
- New developer on customer team cannot authenticate

### Step-by-Step Diagnosis

**Step 1 — Confirm API key is valid**
- Ask customer to navigate to Settings → API Keys
- Confirm the key they are using matches what is displayed (last 4 characters)
- Check if the key has an expiry date set

**Step 2 — Check key permissions**
- Each API key has permission scopes (read, write, admin)
- Ask the customer what action they are attempting
- Verify the key has the required scope for that action

**Step 3 — Check for recent key rotation**
- Ask: "Has anyone on your team regenerated or deleted API keys recently?"
- If yes: old key is now invalid — they need to update their integration with the new key

**Step 4 — Test with a simple API call**
Ask the customer to run this in their terminal to isolate the issue:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" https://api.example.com/v1/ping
```

Expected response: `{"status": "ok"}`  
If they receive `401`: key is wrong or expired  
If they receive `403`: key is valid but lacks permission

**Step 5 — Escalate if unresolved**
- If all steps pass but auth still fails → potential platform-side issue
- Escalate with: API key ID (not the key itself), permission scope, exact error response body

---

## Issue Type 3: Data Not Syncing

### Symptoms
- Records created in one system not appearing in the other
- Sync delay longer than expected
- Partial sync — some records appear, others do not

### Step-by-Step Diagnosis

**Step 1 — Check sync status**
- Navigate to Settings → Integrations → Sync Status
- Note the last successful sync timestamp
- If sync is showing errors, note the error message exactly

**Step 2 — Check field mapping**
- Many sync failures are caused by mismatched field types
- Ask: "Have any custom fields been added or renamed recently?"
- A renamed field breaks the mapping silently

**Step 3 — Test with a single record**
- Ask the customer to create one test record and watch if it syncs
- This isolates whether the issue is bulk or real-time

**Step 4 — Check for filter rules**
- Some integrations have filter conditions (e.g. "only sync records with Status = Active")
- Ask the customer if any filter rules are configured
- Records that do not match filters will never sync — this is expected behaviour

**Step 5 — Escalate with full context**
- Include: integration name, last successful sync time, sample record IDs that failed, any filter rules active

---

## Escalation Checklist

Before escalating any integration issue to Engineering, confirm you have:

- [ ] Completed all relevant steps above
- [ ] Documented what you tried and the outcome of each step
- [ ] Collected error codes, timestamps, and affected record IDs
- [ ] Checked if the issue affects one customer or multiple (if multiple = higher priority)
- [ ] Filled out the bug report template

> ⚠️ Do not escalate without this information. Engineering will return the ticket.
