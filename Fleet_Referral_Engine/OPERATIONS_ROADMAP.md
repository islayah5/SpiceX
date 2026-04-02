# MASTER OPERATIONS ROADMAP
## SpiceX Fleet-to-Pipeline Referral Engine
## ClickUp-Ready Task Breakdown
## Date: April 2026

---

> **FORMAT**: Each task below is structured for direct import into ClickUp (or any PM tool). Priority tags: 🔴 P0 (Blocker) | 🟡 P1 (High) | 🟢 P2 (Standard)

---

## WEEK 1: FOUNDATION (Apr 7-11, 2026)

### Monday-Tuesday: Approvals & Setup
| # | Task | Assignee | Priority | Status | Dependency |
|---|---|---|---|---|---|
| 1.1 | Confirm referral payout amount ($500 locked, verify with Finance) | Mike Ryan / Finance | 🔴 P0 | Ready | None |
| 1.2 | Set up `spicex.com/go` redirect to hosted landing page | Dev Team | 🔴 P0 | Ready | None |
| 1.3 | Deploy landing page to hosting (update WEBHOOK_URL in JS) | Dev Team | 🔴 P0 | Ready | 1.2 |
| 1.4 | Send HARD_CARD_CREATIVE_BRIEF.md to graphic designer | Isaiah | 🔴 P0 | Ready | None |
| 1.5 | Send WEBHOOK_INTEGRATION_SPEC.md to dev team | Isaiah | 🔴 P0 | Ready | None |
| 1.6 | Order test print batch (50 cards) from print vendor | Ops | 🟡 P1 | Ready | 1.4 |

### Wednesday-Thursday: Technical Integration
| # | Task | Assignee | Priority | Status | Dependency |
|---|---|---|---|---|---|
| 1.7 | Configure CRM webhook endpoint (accept POST, create records) | Dev Team | 🔴 P0 | Blocked | 1.5 |
| 1.8 | Set up CRM pipeline: "Fleet Referral" with stages | Sales Ops | 🟡 P1 | Ready | None |
| 1.9 | Configure sales rep assignment rules (round-robin) | Sales Ops | 🟡 P1 | Ready | 1.8 |
| 1.10 | Install Meta Pixel on live landing page | Dev Team / Isaiah | 🟡 P1 | Blocked | 1.3 |
| 1.11 | Install GTM container on live landing page | Dev Team | 🟢 P2 | Blocked | 1.3 |

### Friday: QA & Testing
| # | Task | Assignee | Priority | Status | Dependency |
|---|---|---|---|---|---|
| 1.12 | End-to-end test: Scan QR → Submit direct lead → Verify CRM | Isaiah + Dev | 🔴 P0 | Blocked | 1.7 |
| 1.13 | End-to-end test: Submit referral → Verify both contacts in CRM | Isaiah + Dev | 🔴 P0 | Blocked | 1.7 |
| 1.14 | Test 2-hour SLA alert fires correctly | Sales Ops | 🟡 P1 | Blocked | 1.9 |
| 1.15 | Validate Meta Pixel events with Pixel Helper extension | Isaiah | 🟡 P1 | Blocked | 1.10 |
| 1.16 | Mobile QA: test on iPhone Safari + Android Chrome | Isaiah | 🟡 P1 | Blocked | 1.3 |

---

## WEEK 2: AUTOMATION & CREATIVE (Apr 14-18, 2026)

### Email Sequences
| # | Task | Assignee | Priority | Status | Dependency |
|---|---|---|---|---|---|
| 2.1 | Build Referrer Trust Drip (5 emails) in email platform | Marketing Ops | 🟡 P1 | Ready | None |
| 2.2 | Build Direct Lead Nurture Sequence (4 emails) | Marketing Ops | 🟡 P1 | Ready | None |
| 2.3 | Connect email triggers to CRM deal stage changes | Dev / Marketing Ops | 🟡 P1 | Blocked | 1.7, 2.1 |
| 2.4 | Test all email sequences with test data | Marketing Ops | 🟡 P1 | Blocked | 2.3 |

### Retargeting Prep
| # | Task | Assignee | Priority | Status | Dependency |
|---|---|---|---|---|---|
| 2.5 | Build 4 custom audiences in Meta Ads Manager | Isaiah | 🟡 P1 | Blocked | 1.10 |
| 2.6 | Design retargeting ad creatives (3 ad sets, 2 formats each) | Designer | 🟡 P1 | Ready | None |
| 2.7 | Write retargeting ad copy (provided in RETARGETING_BRIEF.md) | Isaiah | ✅ Done | — | — |

### Hard Card Production
| # | Task | Assignee | Priority | Status | Dependency |
|---|---|---|---|---|---|
| 2.8 | Review designer's first proof of Hard Card | Isaiah + Mike | 🟡 P1 | Blocked | 1.4 |
| 2.9 | Test-scan QR code on printed proof at 6" and 12" distances | Isaiah | 🔴 P0 | Blocked | 2.8, 1.6 |
| 2.10 | Approve final Hard Card design | Mike Ryan | 🔴 P0 | Blocked | 2.9 |
| 2.11 | Order full production run (500 cards) | Ops | 🟡 P1 | Blocked | 2.10 |

---

## WEEK 3: LAUNCH (Apr 21-25, 2026)

| # | Task | Assignee | Priority | Status | Dependency |
|---|---|---|---|---|---|
| 3.1 | Launch retargeting campaigns (3 ad sets) | Isaiah | 🟡 P1 | Blocked | 2.5, 2.6 |
| 3.2 | Distribute cards to fleet vehicles (truck + Tesla) | Ops / Field | 🟡 P1 | Blocked | 2.11 |
| 3.3 | Brief field team on 10-second pitch script | Sales / Mike | 🟡 P1 | Ready | None |
| 3.4 | Set up weekly KPI dashboard (see KPIs below) | Analytics | 🟢 P2 | Ready | None |
| 3.5 | Day 1 monitoring: watch for webhook errors, form issues | Dev + Isaiah | 🔴 P0 | Blocked | 3.2 |

---

## WEEK 4+: OPTIMIZE (Ongoing)

| # | Task | Assignee | Priority | Cadence |
|---|---|---|---|---|
| 4.1 | Review weekly KPI dashboard | Isaiah + Mike | 🟡 P1 | Weekly (Mondays) |
| 4.2 | A/B test landing page headline variants | Dev Team | 🟢 P2 | Bi-weekly |
| 4.3 | Optimize retargeting ad spend based on ROAS | Isaiah | 🟡 P1 | Weekly |
| 4.4 | Review 2-hour SLA compliance | Sales Ops | 🟡 P1 | Weekly |
| 4.5 | Reorder cards if inventory drops below 100 | Ops | 🟢 P2 | As needed |
| 4.6 | Monthly ROI report to leadership | Isaiah | 🟡 P1 | Monthly |

---

## 10-SECOND FIELD PITCH SCRIPT

**For anyone handing out cards from the truck/Tesla:**

> "Hey — we're SpiceX. We build the software layer that makes enterprise systems actually talk to each other. If you're in tech or know someone who is, scan that code. If your referral becomes a customer, we'll pay you $500. Takes 60 seconds."

**Key rules:**
- Don't oversell. Hand the card, deliver the pitch, move on.
- Let the card and QR code do the heavy lifting.
- If they ask questions, point them to the "Just Curious" path on the landing page.

---

## KPI TRACKING (Weekly Dashboard)

| KPI | Source | Target |
|---|---|---|
| Cards distributed | Field team manual log | Track cumulative |
| QR scans (unique) | GA4 / Meta Pixel | 500+/month |
| Scan-to-fork rate | Analytics | >90% |
| Fork-to-submission rate | Form completions | >25% |
| Speed-to-lead (avg hours) | CRM timestamps | <2 hours |
| Lead-to-opportunity rate | CRM | >30% |
| Pipeline value generated | CRM | Track monthly ($) |
| Referral payouts triggered | Finance | Track cumulative ($) |
| Retargeting ROAS | Meta Ads | >4:1 |
| Cost per acquired lead | Ops calc | <$15 |

---

## FILE STRUCTURE

```
Fleet_Referral_Engine/
├── landing-page/
│   └── index.html                    ← Production-ready landing page
├── HARD_CARD_CREATIVE_BRIEF.md       ← Designer handoff
├── WEBHOOK_INTEGRATION_SPEC.md       ← Dev team handoff
├── RETARGETING_CAMPAIGN_BRIEF.md     ← Ad ops handoff
└── OPERATIONS_ROADMAP.md             ← THIS DOCUMENT (ClickUp import)
```

---

*Roadmap prepared for the SpiceX Operations Team — April 2026*
