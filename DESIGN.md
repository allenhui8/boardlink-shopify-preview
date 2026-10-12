# BroadLink International — Online Store 2.0 Theme Design

Status: Draft v0.1 · Architecture: **Option A, custom Online Store 2.0 theme (Liquid sections and JSON templates)**
Reference: static HTML prototype (read-only). Interactive preview: `preview.html`.

## 1. Goal & Audience

**Goal.** Turn the static storefront prototype into a real, merchant-editable Shopify theme. Keep the visual language and replace the fake commerce UI (hardcoded cart badge, dead ATC button) with real Shopify cart patterns.

| Audience | Needs | Surfaces |
|---|---|---|
| B2C shoppers | Browse, compare, filter by ecosystem (Matter / Apple / Google / Alexa), buy quickly | Home, Collection, PDP, Cart drawer |
| B2B / integrators | Understand industry solutions and the software implementation offer (partner "implementation partner"), then enquire | Scenario pages, Software page, Contact form |
| Merchandisers | Edit content without a developer | Theme editor sections, metafields, metaobjects |

**Non-goals.** Headless (Hydrogen), app-first, custom checkout. The checkout stays Shopify-hosted.

**Visual tokens (ported from source CSS).**

| Token | Value |
|---|---|
| `--bg` | `#FFFFFF` |
| `--bg-soft` | `#F4F5F7` |
| `--surface` | `#EAEDF1` |
| `--text` | `#0A0F14` |
| `--accent` | `#006FFF` |
| `--accent-deep` | `#0058CC` |
| Fonts | Manrope (UI), Noto Sans TC (CJK fallback), JetBrains Mono (specs and annotations) |

These live in `config/settings_schema.json` (colors and typography) and are emitted as CSS variables in `snippets/css-variables.liquid`.

## 2. Option A Mapping: Static Pages → Shopify

| Prototype page | Shopify template | Main sections |
|---|---|---|
| Home | `templates/index.json` | `hero`, `featured-collection`, `category-tiles`, `scene-story`, `partner-strip` |
| Products overview | `templates/collection.list.json` | `collection-list`, `category-tiles` |
| Collection / category | `templates/collection.json` | `collection-banner`, `collection-filters`, `product-grid` |
| RM4 PRO PDP | `templates/product.json` | `product-main`, `product-details`, `delivery-policy`, `related-products`, `sticky-buy-bar` |
| Scenarios / B2B | `templates/page.scenarios.json` | `scenario-grid`, `cta-banner` |
| Technology | `templates/page.technology.json` | `rich-feature-rows`, `spec-table` |
| Software implementation | `templates/page.software.json` | `partner-intro`, `process-steps`, `scenario-grid`, `contact-form` |
| About | `templates/page.json` | `rich-text`, `image-with-text` |
| Contact | `templates/page.contact.json` | `contact-form`, `info-columns` |
| (new) Cart | `templates/cart.json` | `cart-items`, `cart-summary` (fallback when JS is off) |
| (new) Search, 404, Account | `search.json`, `404.json`, `customers/*` | Standard |

Reusable pieces become **snippets**: `product-card`, `price`, `swatch`, `icon`, `badge`, `breadcrumbs`, `cart-line`.

## 3. Proposed Theme Tree

```
theme/
├─ layout/
│  ├─ theme.liquid              # head, fonts, css vars, header/footer groups, cart drawer mount
│  └─ password.liquid
├─ templates/
│  ├─ index.json
│  ├─ collection.json
│  ├─ collection.list.json
│  ├─ product.json
│  ├─ cart.json
│  ├─ search.json
│  ├─ 404.json
│  ├─ page.json
│  ├─ page.scenarios.json
│  ├─ page.software.json
│  ├─ page.technology.json
│  ├─ page.contact.json
│  └─ customers/ (login, register, account, order, addresses)
├─ sections/
│  ├─ header.liquid             # sticky translucent nav + mega-menu (blocks = menu columns)
│  ├─ footer.liquid
│  ├─ announcement-bar.liquid
│  ├─ cart-drawer.liquid        # drawer shell, re-rendered via Section Rendering API
│  ├─ hero.liquid
│  ├─ featured-collection.liquid
│  ├─ category-tiles.liquid
│  ├─ scene-story.liquid
│  ├─ partner-strip.liquid
│  ├─ collection-banner.liquid
│  ├─ collection-filters.liquid # Storefront Filtering (chips from filter groups)
│  ├─ product-grid.liquid
│  ├─ product-main.liquid       # gallery, swatches, variant picker, qty, ATC
│  ├─ product-details.liquid    # tabs: specs, in the box, compatibility
│  ├─ delivery-policy.liquid
│  ├─ related-products.liquid
│  ├─ sticky-buy-bar.liquid
│  ├─ scenario-grid.liquid      # metaobject-driven (or manual blocks)
│  ├─ process-steps.liquid
│  ├─ cta-banner.liquid
│  ├─ contact-form.liquid
│  └─ rich-text.liquid / image-with-text.liquid
├─ snippets/
│  ├─ css-variables.liquid
│  ├─ product-card.liquid
│  ├─ price.liquid
│  ├─ swatch.liquid
│  ├─ badge.liquid
│  ├─ cart-line.liquid
│  ├─ icon.liquid
│  ├─ meta-tags.liquid
│  └─ section-annotation.liquid # dev-only `[section: name]` banner
├─ assets/
│  ├─ base.css
│  ├─ components.css
│  ├─ theme.js                  # bootstrap, event bus
│  ├─ cart.js                   # Ajax Cart API + drawer
│  ├─ product-form.js           # variant selection, price, ATC
│  ├─ facets.js                 # filter chips via section rendering
│  └─ mega-menu.js
├─ config/
│  ├─ settings_schema.json      # colors, type, cart type (drawer/page), free-shipping threshold
│  └─ settings_data.json
└─ locales/
   ├─ en.default.json
   ├─ en.default.schema.json
   └─ (additional locales added later)
```

## 4. Key UX Flows

### 4.1 Home
`hero` → `featured-collection` (product-card, compare-at price badge) → `category-tiles` (collection links) → `scene-story` (alternating image/text) → `partner-strip`. Each section has a schema with blocks, so the order is editable in the theme editor.

### 4.2 Collection filters
- Use **Storefront Filtering** (Search & Discovery app or native filters), rendered as chips.
- Filter groups: **Category** (product type or collection), **Platform** (metafield `platforms`: Matter / Apple / Google / Alexa), **Status** (tags `new`, `hot`, plus availability and on-sale).
- Logic: OR within a group, AND across groups. This matches the preview behaviour.
- Chip clicks update URL params (`?filter.p.m.custom.platforms=Matter`) and re-render `product-grid` with the Section Rendering API (`?sections=product-grid`). The URLs stay shareable and crawlable.

### 4.3 PDP: variants, Ajax ATC, cart drawer
- Variant model: **Color × Spec** (two options, up to 3 × 3 = 9 variants). Color uses swatches (`snippet swatch`), Spec uses pill buttons.
- Selecting an option finds the variant, then updates the URL (`?variant=ID`), price, compare-at, SKU, availability and the gallery image. Use the Section Rendering API (`?option_values=`) or inline variant JSON.
- Qty stepper shows a live line total (`price × qty`).
- **Add to cart**: `POST /cart/add.js` with `{id, quantity, sections: "cart-drawer,cart-icon-bubble"}`. The response carries re-rendered section HTML, so the drawer opens and the badge updates with no second request.
- Drawer: line items (`cart-line`), qty adjust and remove via `POST /cart/change.js`, subtotal, a free-shipping progress bar (threshold from theme settings, e.g. "Free shipping over $500"), and a **Checkout** button (`<a href="/checkout">`).
- Fallbacks: without JS, the form posts to `/cart/add` and lands on `templates/cart.json`. A setting `cart_type = drawer | page` is exposed.
- `sticky-buy-bar` appears after the main ATC scrolls out of view and mirrors the selected variant.

### 4.4 B2B as Shopify pages with reusable sections
- B2B content stays in **Pages** using alternate templates (`page.scenarios`, `page.software`).
- `scenario-grid` reads a **metaobject list** (industry cards) or manual blocks.
- `contact-form` uses the native Shopify `{% form 'contact' %}`, with hidden fields for `industry` and `source_page`.
- Optional later: Shopify B2B on Plus (company accounts, price lists) is orthogonal to these pages.

## 5. Data Model

**Products.** Title, handle, media, vendor, type, tags (`new`, `hot`).
**Variants.** Option 1 `Color` (Black, White, Slate), Option 2 `Spec` (Standard, Pro, Kit). SKU follows `MODEL-COLOR-SPEC`. Compare-at price drives sale badges.
**Collections.** Smart collections by product type: Remotes, Plugs, Lights, Switches, Sensors, Cameras, Hubs. Feature collections: "Popular", "New".

| Metafield / metaobject | Type | Purpose |
|---|---|---|
| `custom.platforms` | list.single_line_text (Matter, Apple, Google, Alexa) | Compatibility chips and filter |
| `custom.model_sku` | single_line_text | Display model, e.g. "RM4 PRO" |
| `custom.warranty` | single_line_text | e.g. "Local warranty, 24 months" |
| `custom.shipping_note` | multi_line_text | e.g. "Ships within 24 hours" |
| `custom.in_the_box` | list.single_line_text | Box contents |
| `custom.spec_table` | json | Rows for the spec tab |
| `custom.related` | list.product_reference | Curated related products |
| Metaobject `partner_icon` | name, svg/file, url | Partner strip |
| Metaobject `scenario_card` | title, industry, summary, image, link, order | Scenario grid |
| Metaobject `faq_item` | question, answer | PDP and B2B FAQ |

Shop-level policy copy (placeholders): "Local shipping", "Free shipping over $500", "Authorized service", "Regional support".

## 6. Cart & Checkout (theme-native)

- Ajax Cart API: `/cart/add.js`, `/cart/change.js`, `/cart.js`, plus the **Section Rendering API** to refresh the drawer and badge.
- One `cart` JS module owns state events (`cart:updated`), used by the header badge, drawer, sticky bar and the collection quick-add.
- Checkout is Shopify-hosted (`/checkout`). The theme only styles the checkout through the Checkout Branding settings (colors, font, button radius = 0 to match the rectangular blue button).
- Edge states: sold-out variants disable ATC, `quantity_rule` and inventory limits show inline errors, and the empty cart shows a CTA to the collection.
- Accessibility: the drawer is a focus-trapped `<dialog>` or `role="dialog"` with `aria-live` for cart updates.

## 7. Phased Build Plan

| Phase | Scope | Exit criteria |
|---|---|---|
| **1. MVP theme** | Layout, header/mega-menu, footer, home, collection (native filters), PDP with variants, Ajax ATC, cart drawer, cart page, tokens | Full purchase path on a dev store; Lighthouse mobile ≥ 85 |
| **2. Metafields & content** | Platforms, warranty, shipping, spec table, related, scenario and partner metaobjects, B2B page templates, contact form | All prototype content editable in the theme editor, with no hardcoded copy |
| **3. Polish** | Quick-add on cards, predictive search, sticky buy bar, compare tray, locales, SEO structured data | Accessibility audit passed |
| **4. Optional app extension** | Theme app extension blocks (reviews, B2B quote request, ERP sync, a configurator for integrators) | Only if a need is proven; the theme remains app-agnostic |

## 8. Annotation Convention (preview only)

Each Shopify section in `preview.html` is flagged with a small mono banner: `[section: name]` (e.g. `[section: hero]`, `[section: product-main]`, `[section: cart-drawer]`). Snippets use `[snippet: name]`. Names match the filenames in `sections/` and `snippets/`. Annotations can be toggled with the "Annotations" switch. In the theme, the dev-only `snippets/section-annotation.liquid` can render the same banner when `settings.show_annotations` is on. It must be off in production.

## 9. Open Questions
1. Use native filtering or Search & Discovery?
2. Is Shopify Plus (B2B catalogs) needed?
3. Is a comparison feature in scope for MVP?
4. What are the languages and markets for launch?

---

# DESIGN ADDENDUM — B2B-first Online Store 2.0 preview

## Scope
The storefront leads with commercial solutions, industry scenarios, integration capability and inquiry. The device catalog is a light proof-of-stack. Distribution, wholesale pricing, reseller tiers, partner-program ladders and channel enrollment are **out of scope** and must not appear as storefront features. The "Beyond Distribution. Built for Integration." line is a tagline only.

Scenarios are illustrative proposed use cases, not verified deployments. API/SDK availability depends on compatibility and licensing.

## Tab → template mapping

| Preview tab | Shopify template | Key sections |
|---|---|---|
| Solutions (home) | `templates/index.json` | `hero-solutions`, `capability-grid`, `architecture-strip`, `featured-scenarios`, `deliverables-table`, `inquiry-cta` |
| Scenarios | `templates/page.scenarios.json` | `scenario-cards` (six blocks: workflow / pilot / measure), `delivery-flow` |
| Devices | `templates/collection.json` (light) + `templates/product.json` (light) | `main-collection` with category chips, `main-product` (drawer in the preview), `cart-drawer` (secondary) |
| Inquire | `templates/page.contact.json` | `contact-form`, `project-brief-checklist` |

Global: `header` (tabs → main menu), `footer`, announcement bar for neutral trust copy.

## Metafields

| Key | Used on | Notes |
|---|---|---|
| `custom.scenario` | scenario cards, featured scenarios | Industry key plus workflow / pilot / measure text |
| `custom.integration_layer` | capability and architecture sections, products | Business apps / integration / platform and edge / assets |
| `custom.platforms` | product | Apple Home, Google Home, Alexa, SmartThings |
| `custom.protocols` | product | Matter (where tagged), IR, RF, Wi-Fi, FastCon |
| `custom.warranty` | product, trust row | Neutral copy only |

## Implementation notes
- Scenario cards: a metaobject `scenario` (title, industry, workflow, pilot_shape, success_measure, note) rendered by a section with blocks.
- Inquire form: use Shopify's contact form; the fields industry, existing platforms, device needs, timeline and first decision map to hidden/visible `contact[...]` inputs. The preview uses a toast only, and no contact destination is configured.
- Cart drawer and Quick add stay as a light catalog demo; the cart CTA routes to the contact page.
- Tokens: accent `#006FFF`, deep `#0058CC`, bg `#FFFFFF`, soft `#F4F5F7`, surface `#EAEDF1`, text `#0A0F14`. Fonts: Manrope, Noto Sans TC, JetBrains Mono. Currency "$". Times in America/Toronto or UTC.
- Annotations are preview-only, toggled by the header switch, and are not shipped in theme output.
- Trust copy: "Local shipping", "Local warranty", "Authorized service", "Regional support".
- Copy refers to an "implementation partner" without naming one. No real phones, emails or customer names appear anywhere.
