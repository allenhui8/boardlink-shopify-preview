# MASKED FEATURES BRIEF FOR CLAUDE

## Design context (Option A — Online Store 2.0)
- Existing preview path will be replaced: tabbed Shopify-oriented IA with section annotations.
- Visual tokens: accent #006FFF / deep #0058CC, bg #FFFFFF, soft #F4F5F7, surface #EAEDF1, text #0A0F14; fonts Manrope + Noto Sans TC + JetBrains Mono.
- Cart drawer / ATC may remain as a light catalog demo only — not the primary journey.
- Currency as "$". Times: America/Toronto or UTC.

## Audience reframing (critical)
BroadLink International smart-home offering **mainly targets B2B environments** (property, hospitality, offices, education, facilities, energy). Shopify preview must lead with **commercial solutions, industry scenarios, integration capability, and inquiry/CTA**. Keep only a **light product catalog** (featured devices as proof of stack). **Do not** present distribution, wholesale pricing, reseller tiers, SI partner program ladders, or channel enrollment as storefront features.

## Masking rules (absolute — follow without restating banned strings)
- Omit any specific implementation-partner company/legal name; say **"implementation partner"** or omit.
- No city/region names, regional dialing codes, regional TLDs in emails, courier brand names tied to a market, or timezone labels other than America/Toronto / UTC.
- Neutral trust copy only: "Local shipping", "Local warranty", "Authorized service", "Regional support".
- No real phones, emails, or customer names. Contact = toast or placeholder form.

---

# BroadLink product & platform features (light catalog — from storefront prototype)

## Catalog themes (supporting evidence, not the homepage hero)
- Universal remotes: RM4 PRO (IR + RF), RM Mini 3 (IR), RM4 Mini (IR + Wi-Fi)
- Smart plugs / power: SP3S Wi-Fi plug, SP4L 4-outlet strip
- Lighting: LB27 RGBCW bulb, LB1 entry bulb, LED Strip Pro (5 m)
- Switches: MCB1; Sensors: PIR motion; Cameras: CAM-G3 indoor 2K; Hubs: HUB scene control panel

## RM4 PRO highlights (optional PDP depth)
- Dual-mode IR + RF (433 MHz); 10,000+ brand remote database; ~60s learning
- Platforms: Apple Home, Google Home, Alexa, SmartThings; Matter where tagged
- Scenes: home / cinema / sleep
- Variants: Color × Spec; model example RM4P-IRF01
- Trust: Local shipping within 24h; free shipping over $500; local warranty; authorized service

## Protocols / metafields
- Matter, Apple Home, Google Home, Alexa, SmartThings
- FastCon (chip-to-cloud / local AI + cloud NLP / OEM module narrative)
- Annotate: `metafield: custom.platforms`, `custom.protocols`, `custom.scenario`, `custom.integration_layer`, `custom.warranty`

## Category taxonomy (light filters if Collection tab kept)
Remotes · Plugs · Lights · Switches · Sensors · Cameras · Hubs · FastCon modules

---

# B2B platform & scenario features (from partner corporate deck — masked)

Deck is an **implementation-partner** corporate presentation. Scenarios are **illustrative proposed use cases**, not verified deployments. API/SDK availability depends on compatibility and licensing.

## Positioning (B2B hero language)
- Beyond Distribution. Built for Integration. *(marketing tagline — do **not** build wholesale/distribution UX)*
- Technology value-add: product selection + software development + middleware integration for commercial projects
- Collaboration: system integrator leads customer relationship; implementation partner supports discovery and agreed integration scope

## Four capabilities (solutions framing, not a store catalog)
1. Technology supply & lifecycle for project BOMs
2. Software — portals, dashboards, mobile app integration
3. API / SDK & middleware — adapters, event bus, cross-system workflows
4. SI enablement — architecture review, PoC, technical handover

## Engineering deliverables
| Area | Scope | Deliverable |
| Applications | Resident/ops apps, portals, dashboards | UX flows, scope, acceptance tests |
| Integration | API clients, SDK adapters, middleware | Interface spec, mappings, error handling |
| Automation | Event routing, notifications, work orders | Rules, escalation, audit trail |
| Operations | Device health, monitoring, maintenance | Runbook, support ownership, release plan |

## Reference architecture layers
Business apps (resident / property ops / PMS / campus / BMS) → Integration & automation (gateway, adapters, event bus, rules) → Platform & edge (device registry, space mapping, gateways, local logic) → Connected assets (sensors, meters, room controls, equipment) + Security/ops (identity, audit, retention, network boundaries). Events rise; permitted commands return. Cloud / edge / hybrid by project.

## Pre-implementation checks
Access & compatibility · Security & privacy (RBAC, retention) · Event integrity (dedupe, retries, space mapping) · Operational continuity (offline, override, rollback, escalation)

## Six industry scenarios (PRIMARY preview content)
1. **Residential property** — leak/smoke → gateway → middleware → resident app + property queue; unit mapping, ack, escalation. Life-safety remains independent.
2. **Offices / meeting rooms** — booking + occupancy → lighting/HVAC scenes; unused reservation flags; manual override.
3. **Education / campus** — timetable + sensors → facilities dashboard; classroom readiness; plant-room leak alerts; privacy-aware aggregated occupancy.
4. **Hospitality** — PMS ↔ room mapping ↔ GRMS; check-in welcome scene; check-out setback + housekeeping; DND; offline local room operation.
5. **Car parks & energy** — metering + rules → energy dashboard; ERG assessment (verify with supplier); baseline kWh/runtime; no savings guarantee.
6. **Buildings & facilities** — IoT + BMS → normalized alarms → maintenance tasks; start read-only; BACnet/Modbus/MQTT/HTTPS conditional.

Each scenario card should show: workflow · pilot shape · success measure.

## Pilot success metrics (targets, not claimed ROI)
Alert delivery / response time · Acknowledgement / task closure · Integration defects / rework · kWh / runtime under comparable conditions

## Delivery flow (for Solutions / how-we-work section)
Discover → Assess → Prove (PoC) → Deliver → Operate — with explicit gates. Skip partner-program tier ladders and wholesale enrollment.

## Primary CTA
- "Request a project consultation" / "Start with one pilot use case"
- Form fields (placeholder): industry, existing platforms, device needs, timeline, first decision (interface assessment / solution scope / SI discussion)
- No real contact destinations — toast only

---

# Update task

Rewrite **preview.html** as a complete self-contained interactive HTML document.

## IA (B2B-first tabs)
1. **Solutions** (default/home) — commercial hero, four capabilities, architecture strip, featured scenario highlights, inquiry CTA
2. **Scenarios** — six industry cards (workflow / pilot / measure) + delivery flow
3. **Devices** (light catalog) — compact collection grid + optional RM4-style PDP drawer or subview; Quick-add/cart is secondary
4. **Inquire** — consultation form (client-side toast), project brief checklist

Keep Shopify section annotations: `[section: …]` and metafield hints. Toggleable Annotations switch.

Optional short **DESIGN-ADDENDUM.md**: map B2B sections → Liquid templates (`index.json` solutions-first, `page.scenarios.json`, light `collection`/`product`, `page.contact.json`), note that distribution/wholesale is out of scope.

## Output format exactly
---preview.html---
```html
...full document...
```
---DESIGN-ADDENDUM.md---
```markdown
...addendum...
```
