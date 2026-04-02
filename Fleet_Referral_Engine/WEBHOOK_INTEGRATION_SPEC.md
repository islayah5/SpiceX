# WEBHOOK & CRM INTEGRATION SPEC
## SpiceX Fleet-to-Pipeline Referral Engine
## Handoff: Development Team
## Date: April 2026

---

> **OBJECTIVE**: Define the technical integration between the Fleet Referral landing page and SpiceX's CRM/backend system. This spec is CRM-agnostic — designed to work with any system that accepts REST webhook POSTs (HubSpot, Salesforce, GoHighLevel, custom, etc.).

---

## ARCHITECTURE OVERVIEW

```
[QR Scan] → [Landing Page] → [Form Submit]
                                    ↓
                          [POST JSON to Webhook Endpoint]
                                    ↓
                    [CRM / API Gateway / Middleware]
                          ↙              ↘
              [Create Contact]    [Create Deal/Opp]
                          ↓              ↓
              [Tag: Source]    [Assign Sales Rep]
                                    ↓
                          [Alert: 2-Hour SLA]
                                    ↓
                    [Deal Stage Changes → Trigger Emails]
```

---

## WEBHOOK ENDPOINT REQUIREMENTS

The landing page will POST JSON data to a single configurable endpoint.

| Attribute | Requirement |
|---|---|
| **Method** | POST |
| **URL** | Configurable in landing page JS (line ~150 of index.html) |
| **Content-Type** | `application/json` |
| **Auth** | Optional bearer token header (add to JS if needed) |
| **CORS** | Must accept requests from deployment domain |
| **Response** | 200 OK on success, any 4xx/5xx logged as failure |
| **Timeout** | Landing page will wait 5 seconds max, then show success anyway |

**Current placeholder in code:**
```javascript
const WEBHOOK_URL = 'https://YOUR-CRM-ENDPOINT.com/api/webhook/fleet-referral';
```

---

## PAYLOAD SCHEMAS

### Event 1: Direct Lead Submission (`fleet_lead_direct`)

Triggered when a user selects "I Need This" and submits their contact info.

```json
{
  "event": "fleet_lead_direct",
  "timestamp": "2026-04-02T14:38:29-04:00",
  "source": "truck_hardcard",
  "lead": {
    "first_name": "Jane",
    "last_name": "Doe",
    "email": "jane@enterprise.com",
    "company": "Acme Corp",
    "title": "CIO"
  },
  "metadata": {
    "card_batch_id": "FLEET_Q2_2026",
    "utm_source": "fleet",
    "utm_medium": "qr_hardcard",
    "utm_campaign": "fleet_referral_q2"
  }
}
```

**Required Fields:** `event`, `lead.first_name`, `lead.last_name`, `lead.email`
**Optional Fields:** `lead.company`, `lead.title`

---

### Event 2: Referral Submission (`fleet_lead_referral`)

Triggered when a user selects "I Know Someone" and submits both their info and their contact's info.

```json
{
  "event": "fleet_lead_referral",
  "timestamp": "2026-04-02T14:38:29-04:00",
  "source": "truck_hardcard",
  "referrer": {
    "name": "Mike Smith",
    "email": "mike@gmail.com"
  },
  "referred_lead": {
    "name": "Jane Doe",
    "email": "jane@enterprise.com",
    "company": "Acme Corp"
  },
  "metadata": {
    "card_batch_id": "FLEET_Q2_2026",
    "utm_source": "fleet",
    "utm_medium": "qr_hardcard",
    "utm_campaign": "fleet_referral_q2"
  }
}
```

**Required Fields:** `event`, `referrer.name`, `referrer.email`, `referred_lead.name`, `referred_lead.email`
**Optional Fields:** `referred_lead.company`

---

## CRM FIELD MAPPING

These are the recommended CRM fields. The dev team should map these to whatever system SpiceX uses.

### Contact Record (Lead)
| Webhook Field | CRM Field | Type | Notes |
|---|---|---|---|
| `lead.first_name` / `referred_lead.name` | First Name | Text | Split name on space if needed |
| `lead.last_name` | Last Name | Text | |
| `lead.email` / `referred_lead.email` | Email | Email | Primary identifier |
| `lead.company` / `referred_lead.company` | Company | Text | Optional |
| `lead.title` | Job Title | Text | Optional |
| `source` | Lead Source | Dropdown | Value: `Truck_Handout` |
| `metadata.card_batch_id` | Custom: Card Batch | Text | For tracking print runs |
| `metadata.utm_campaign` | Campaign | Text | `fleet_referral_q2` |

### Contact Record (Referrer) — Only for `fleet_lead_referral` events
| Webhook Field | CRM Field | Type | Notes |
|---|---|---|---|
| `referrer.name` | Full Name | Text | |
| `referrer.email` | Email | Email | Primary identifier |
| — | Contact Type | Tag/Dropdown | Value: `Referrer` |
| — | Referred Lead ID | Lookup | Link to the lead contact |

### Deal/Opportunity Record
| Field | Value | Notes |
|---|---|---|
| Deal Name | `Fleet Referral: [Lead Company/Name]` | Auto-generated |
| Pipeline | `Fleet Referral` | Create this pipeline if it doesn't exist |
| Stage | `New Lead` | First stage |
| Lead Source | `Truck_Handout_Direct` or `Truck_Handout_Referral` | Based on event type |
| Assigned To | Round-robin or specific rep | Configure in CRM |
| Referrer ID | Link to referrer contact | Only for referral events |
| Created Date | `timestamp` from payload | |

---

## AUTOMATION TRIGGERS

These automations should be configured in the CRM or middleware (Zapier/Make.com).

### Trigger 1: New Lead Alert (2-Hour SLA)
```
WHEN: New deal created in "Fleet Referral" pipeline
DO:
  1. Assign to sales rep (round-robin or territory-based)
  2. Send Slack notification: "🔥 New Fleet Lead: [Name] at [Company] — 2hr SLA"
  3. Send email alert to assigned rep with lead details
  4. Start SLA timer (2 hours)
```

### Trigger 2: Referrer Confirmation Email
```
WHEN: fleet_lead_referral event received
DO:
  1. Send email to referrer.email:
     Subject: "Your referral is in good hands."
     Body: Confirmation that SpiceX received the referral + next steps
```

### Trigger 3: Lead Contacted Update
```
WHEN: Deal stage changes from "New Lead" to "Contacted"
DO:
  1. IF deal has referrer → Send email to referrer:
     Subject: "We've reached out to [Lead First Name]."
     Body: Status update + "we'll keep you posted"
```

### Trigger 4: Deal Closed/Won Payout
```
WHEN: Deal stage changes to "Closed/Won"
DO:
  1. IF deal has referrer:
     a. Send email to referrer: "🎉 You just earned $500."
     b. Create payout task for Finance team
     c. Tag referrer contact as "Payout_Pending"
  2. Log closed revenue against Fleet Referral campaign
```

### Trigger 5: Deal Lost Notification
```
WHEN: Deal stage changes to "Closed/Lost"
DO:
  1. IF deal has referrer → Send email to referrer:
     Subject: "Update on your referral."
     Body: Graceful close + "Have another referral? Submit here →"
```

---

## MIDDLEWARE OPTIONS (If CRM Lacks Native Webhooks)

If SpiceX's CRM cannot directly receive webhook POSTs, use middleware:

| Option | Complexity | Cost | Best For |
|---|---|---|---|
| **Zapier** | Low | $20-50/mo | Simple routing, limited volume |
| **Make.com** | Medium | $10-30/mo | Complex logic, higher volume |
| **n8n** (self-hosted) | Medium | Free | Full control, technical team |
| **Custom API Gateway** | High | Varies | Maximum flexibility |

**Zapier Example Flow:**
1. Trigger: Webhook (Catch Hook)
2. Filter: Route by `event` field
3. Action: Create Contact in CRM
4. Action: Create Deal in CRM
5. Action: Send Slack notification

---

## TESTING CHECKLIST

- [ ] Webhook endpoint is live and accepting POST requests
- [ ] CORS headers allow requests from `spicex.com` domain
- [ ] Direct lead submission creates contact + deal in CRM
- [ ] Referral submission creates lead contact + referrer contact + deal
- [ ] Lead source tags are applied correctly (`Truck_Handout_Direct` vs `Truck_Handout_Referral`)
- [ ] Sales rep receives Slack/email alert within 30 seconds of submission
- [ ] Referrer confirmation email sends within 60 seconds
- [ ] Deal stage change triggers downstream automations
- [ ] Duplicate detection: Same email submitted twice doesn't create duplicate contacts
- [ ] Error handling: Landing page shows success even if webhook fails (graceful degradation)

---

## DEPLOYMENT NOTES

1. **Update the `WEBHOOK_URL` constant** in `landing-page/index.html` (line ~150)
2. **Uncomment the Meta Pixel** `fbq('init', 'YOUR_PIXEL_ID')` line and insert the real pixel ID
3. **Host the landing page** at the `spicex.com/go` path (or configure redirect)
4. **Set up SSL** — the form submits sensitive data; HTTPS is mandatory

---

*Spec prepared for the SpiceX Development Team — April 2026*
