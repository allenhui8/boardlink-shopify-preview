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
