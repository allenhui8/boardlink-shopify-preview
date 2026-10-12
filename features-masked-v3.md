# MASKED BRIEF — Visual, short-copy refresh of Shopify B2B preview

## Task
Rewrite `/workspace/boardlink-shopify-preview/preview.html` as a complete self-contained HTML file (CSS+JS inline). Sync goal: less text, more imagery. Keep interactive tabs and Shopify `[section: …]` annotations (toggleable).

## Audience (unchanged)
B2B-first commercial smart-home / smart-building solutions. Light device catalog. Inquiry CTA primary. Do **not** build distribution, wholesale, or reseller-enrollment UX.

## Masking (absolute — do not write banned geographic or partner legal names)
- Refer to collaborators only as **"implementation partner"** (never a company/legal name).
- No city/region names, regional dialing codes, regional email TLDs, courier brands, or timezones other than America/Toronto or UTC.
- Trust copy: "Local shipping", "Local warranty", "Authorized service", "Regional support". Currency "$". No real phones/emails.

## Copy rules (critical)
- Headlines + **1–2 short sentences** max per block.
- Capability cards: title + one line each (no paragraphs).
- Scenario cards: title + one-line workflow; hide long pilot/metric text behind a compact "Details" expand or omit — prefer photo-led cards.
- Remove or collapse long engineering tables into 4 compact chips / icon cards (one line each).
- Architecture: **visual diagram** (CSS/SVG boxes + arrows), not a wall of text.
- Inquire: short form labels; checklist as 4 compact chips, not a prose list.

## Visuals (required — use these relative paths; files already on disk)
Hero / solutions imagery:
- `assets/business/software.jpg` (hero preferred)
- Partner strip icons: `assets/partners/matter_icon.png`, `applehome_icon.png`, `googlehome_icon.png`, `alexa_icon.png`, `smartthings_icon.png`

Scenario card covers (map exactly):
1. Residential → `assets/business/property.jpg`
2. Offices → `assets/business/office.jpg`
3. Education → `assets/business/education.jpg`
4. Hospitality → `assets/business/hospitality.jpg`
5. Car parks & energy → `assets/business/carpark.jpg` (secondary energy cue: `esg.jpg` optional)
6. Buildings & facilities → `assets/business/facilities.jpg`

Device cards: product-style **SVG illustrations** (gradient tile + simple device glyph) — no external product photos required. Optional: small Matter/platform icons on cards.

Use `<img src="…" alt="…">` with `object-fit: cover`. Large hero ~420–520px tall desktop; scenario covers ~160–200px; device tiles square.

## IA tabs (keep)
1. **Solutions** (default) — hero with photo, short pitch, 4 capability cards, visual architecture diagram, 3 scenario teaser cards with photos, CTA
2. **Scenarios** — 6 photo cards + short delivery flow (5 steps as compact stepper, not essays)
3. **Devices** — compact grid with SVG art; detail modal stays short (bullets ≤5)
4. **Inquire** — photo strip or side image (`software.jpg` or `facilities.jpg`) + short form + toast on submit

## Design tokens
Accent `#006FFF` / `#0058CC`; bg `#FFFFFF`; soft `#F4F5F7`; surface `#EAEDF1`; text `#0A0F14`; fonts Manrope, Noto Sans TC, JetBrains Mono (Google Fonts CDN OK).

## Content themes to keep (short form)
- Tagline vibe: Beyond Distribution. Built for Integration. (marketing only — no wholesale UI)
- 4 capabilities: supply & lifecycle · software · API/SDK middleware · SI enablement
- 6 industries as above with one-line workflows
- Delivery: Discover → Assess → Prove → Deliver → Operate
- Light catalog: RM4 PRO, plugs, lights, sensors, hubs; platforms Matter / Apple / Google / Alexa / SmartThings; FastCon as a one-line protocol note
- Primary CTA: Request a project consultation

## Output format exactly
---preview.html---
```html
...full document...
```
