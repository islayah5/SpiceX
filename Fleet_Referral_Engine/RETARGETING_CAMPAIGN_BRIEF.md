# RETARGETING CAMPAIGN BRIEF
## SpiceX Fleet-to-Pipeline Referral Engine — Phase 2
## Handoff: Ad Ops / Digital Marketing Team
## Date: April 2026

---

## PIXEL ARCHITECTURE

### Meta Pixel Setup
1. Install Meta Pixel base code in `<head>` of `spicex.com/go` (already stubbed in landing page)
2. Replace `YOUR_PIXEL_ID` with actual pixel ID
3. Events fire automatically based on user behavior:

| Event | Fires When | Parameters |
|---|---|---|
| `PageView` | Page loads | `content_category: fleet_referral` |
| `ViewContent` | User selects a path | `content_name: path_direct/path_referral/path_curious` |
| `InitiateCheckout` | User clicks first form field | `content_name: form_started` |
| `Lead` | Form successfully submitted | `content_name: direct_lead/referral_lead` |

### Google Tag Manager Setup
1. Create new GTM container for `spicex.com/go`
2. Map the same events as custom GA4 events:
   - `fleet_page_view`, `fleet_path_select`, `fleet_form_start`, `fleet_form_complete`
3. Configure GA4 conversions for `fleet_form_complete`

---

## AUDIENCE SEGMENTS

Build these in Meta Ads Manager → Audiences → Custom Audiences → Website.

| # | Segment Name | Rule | Lookback | Est. Size | Priority |
|---|---|---|---|---|---|
| 1 | **Fleet - Scanned, Bounced** | PageView on `/go` AND NOT Lead event | 30 days | Growing | 🔴 High |
| 2 | **Fleet - Curious Path** | ViewContent where path=`curious` AND NOT Lead | 30 days | Growing | 🔴 High |
| 3 | **Fleet - Form Abandoners** | InitiateCheckout AND NOT Lead | 14 days | Small | 🟡 Medium |
| 4 | **Fleet - Converted** (EXCLUDE) | Lead event fired | 90 days | — | Exclude from all |

**Lookalike Audiences (Phase 3):**
- 1% lookalike of Segment 4 (Fleet - Converted) → Target for prospecting
- 1% lookalike of existing SpiceX website visitors → Broader awareness

---

## AD CREATIVE SPECS

### Ad Set 1: "The Reminder" (Targeting: Segment 1)
**Goal:** Re-engage users who scanned the QR code but bounced without choosing a path.

| Element | Spec |
|---|---|
| **Headline** | You scanned. We noticed. |
| **Primary Text** | SpiceX is the AI-native automation layer that makes your enterprise systems finally work together. No rip-and-replace. No 18-month migration. You were curious enough to scan — now see it in action. |
| **CTA Button** | "Book a 15-Min Demo" |
| **Destination** | `spicex.com/go?utm_source=meta&utm_medium=retargeting&utm_campaign=fleet_reminder` |
| **Format** | 1080×1080 (Feed) + 1080×1920 (Stories/Reels) |
| **Budget** | $15-25/day |
| **Bid Strategy** | Lowest cost per lead |

**Visual Brief for Designer:**
```
FEED (1080×1080):
- Dark background (#050505)
- SpiceX logo top-center, subtle red glow behind it
- Architecture Layer Cake diagram as hero graphic (center)
  - Use simplified version from Generated_Graphics/02
- Bottom: "YOUR SYSTEMS. FINALLY UNIFIED." in Montserrat ExtraBold
- Red gradient accent bar at very bottom

STORIES (1080×1920):
- Same dark background
- Top third: SpiceX logo + "You scanned. We noticed." in Oswald
- Middle: Architecture Layer Cake, larger
- Bottom third: "BOOK A DEMO" CTA button styled like the landing page
```

---

### Ad Set 2: "The Proof" (Targeting: Segment 2)
**Goal:** Educate users who chose "Just Curious" but didn't convert, using social proof.

| Element | Spec |
|---|---|
| **Headline** | 75% less. 10x faster. Zero headaches. |
| **Primary Text** | 46% of enterprise CIOs are ditching legacy automation for AI-native platforms like SpiceX. See how companies like Aetna cut professional services costs by 75% while deploying in weeks instead of months. Your current stack is holding you back. |
| **CTA Button** | "See the Case Study" |
| **Destination** | `spicex.com/go?utm_source=meta&utm_medium=retargeting&utm_campaign=fleet_proof` |
| **Format** | 1080×1080 (Feed) + 1080×1920 (Stories/Reels) |
| **Budget** | $20-30/day |

**Visual Brief for Designer:**
```
FEED (1080×1080):
- Dark background
- SOC 2 / HIPAA Cyber Badge floating center (reference: Generated_Graphics/08)
- Stat callouts around badge: "75% LOWER COSTS" / "10x FASTER" / "SOC 2 TYPE II"
  - Use Speed Block style (red rectangles, white text, Montserrat ExtraBold)
- Bottom: Small SpiceX logo + "CASE STUDY: AETNA" in Oswald

STORIES (1080×1920):
- Speed Block sequence (like the B-roll animation):
  - Frame 1: "75% LESS" (red block)  
  - Frame 2: "10x FASTER" (red block)
  - Frame 3: "ZERO HEADACHES" (red block)
  - Frame 4: SpiceX logo + CTA
- If possible, export as 15-second video with slam-cut transitions
```

---

### Ad Set 3: "The Nudge" (Targeting: Segment 3)
**Goal:** Recover users who started the form but abandoned before submitting.

| Element | Spec |
|---|---|
| **Headline** | You were this close. |
| **Primary Text** | Your referral bonus doesn't expire, but your lead's attention might. Finish your submission in 30 seconds and earn $500 when the deal closes. No cap. No limit. |
| **CTA Button** | "Complete My Referral" |
| **Destination** | `spicex.com/go?utm_source=meta&utm_medium=retargeting&utm_campaign=fleet_nudge&path=referral` |
| **Format** | 1080×1080 (Feed only — lower funnel, precision targeting) |
| **Budget** | $10-15/day |

**Visual Brief for Designer:**
```
FEED (1080×1080):
- Dark background
- Centered: Stylized rendering of the Hard Card (Side B, angled 3D perspective)
- Overlay: Glowing progress bar at 70% (red fill, grey remaining)
- Text: "YOU WERE THIS CLOSE." in Montserrat ExtraBold, White
- Below: "$500 PER CLOSED DEAL" in Oswald, SpiceX Red
- SpiceX logo bottom-right, small
```

---

## CAMPAIGN SETTINGS

| Setting | Value |
|---|---|
| **Campaign Objective** | Leads (optimizing for form completions) |
| **Placements** | Meta Feed, Instagram Feed, Instagram Stories, Reels |
| **Scheduling** | Run continuously, review weekly |
| **Frequency Cap** | Max 3 impressions per user per 7 days |
| **Exclusions** | Segment 4 (Fleet - Converted), SpiceX employees, existing customers |
| **Attribution Window** | 7-day click, 1-day view |
| **Target ROAS** | >4:1 (measure against pipeline value, not just leads) |

---

## KPI TARGETS

| Metric | Target |
|---|---|
| CTR (Feed) | >1.5% |
| CTR (Stories) | >0.8% |
| Cost per Click | <$3.00 |
| Cost per Lead (retargeted) | <$25 |
| Bounce Pool → Lead Conversion | >8% |
| Weekly spend | $315-490 total across 3 ad sets |

---

## REFERENCE ASSETS FOR DESIGNER

| Asset | Location | Use |
|---|---|---|
| Architecture Layer Cake | `Visual_Assets/Generated_Graphics/02_Architecture_Layer_Cake.png` | Ad Set 1 hero |
| SOC 2 Shield | `Visual_Assets/Generated_Graphics/08_SOC2_HIPAA_Shield.png` | Ad Set 2 badge |
| Speed Blocks animation | `Visual_Assets/Animated_BRoll/03_speed_blocks.html` | Ad Set 2 video reference |
| Cyber Badge | `Visual_Assets/Animated_BRoll/05_cyber_badge.html` | Ad Set 2 visual reference |
| SpiceX Logo | `https://islayah5.github.io/image-assets/SpiceX_logo/SpiceX%20Logo.png` | All ads |

---

*Brief prepared for the SpiceX Digital Marketing Team — April 2026*
