# Admin Feature Showcase + B2B Business Pitch: Design Spec

Date: 2026-07-25
Sites affected: `cliffindus.com`, `luckystop.cliffindus.com`
Type: Content and interaction redesign of existing single-file HTML sites. No new pages, no build tooling, no external dependencies added.

## Goals

1. `cliffindus.com` currently describes its admin/dashboard capabilities in generic agency language ("Web Dashboards: admin portals, management tools..."). Replace that with a section that shows the real depth of the Lucky Stop admin platform (inventory, employee management, rates engine, analytics, fraud protection, notices) so a visitor evaluating Cliff Industries as a technical partner sees concrete, accurate proof of capability.
2. `luckystop.cliffindus.com` currently pitches only the consumer app, with one small CTA block near the footer aimed vaguely at "Lucky Stop store owners." Replace that block with a proper section pitching **other business owners** (different chains, different brands) on getting their own version of this system built, using Lucky Stop as the live case study.
3. Both additions use a shared interaction pattern: cards that sit quiet at rest and reveal real detail on hover (tap on touch), with small custom-animated icons, so the sites feel more like working software demos than static marketing pages.

## Non-goals

- No new pages/routes, no framework, no build step. Everything stays a single `index.html` per site, matching the current architecture.
- No changes to `privacy.html`, `og.png`, or any content outside the two sections described here.
- No correction of the existing "5% cashback" simplification used elsewhere on `cliffindus.com` (e.g. the Lucky Stop work card) or the existing hero copy on `luckystop.cliffindus.com`. Those are out of scope; this spec's new copy uses accurate, non-oversimplified language for tiers/rates instead of a flat percentage.
- No Flaticon or other third-party icon service integration. Icons are hand-built inline SVG, animated with CSS.
- Style constraint that applies to all new copy in this spec: no em dashes (`—`) anywhere in card titles, taglines, or bullet copy. Use periods, colons, or separate lines instead.

## Part A: Shared interaction and icon mechanics

### Card structure

Each feature card has two states:
- **Rest**: icon + title + one-line tagline, same visual weight as the existing `.cap-cell` / `.f-card` treatment on each site.
- **Expanded**: an absolutely-positioned detail panel appears below the card (`position: absolute; top: calc(100% - 8px)`), containing 3 to 4 bullet points and, where relevant, a stat line. It overlaps content below rather than pushing the grid, so hovering never causes sibling cards or later sections to jump.

Trigger:
- **Desktop (hover-capable)**: `:hover` on the card triggers the expanded state. Sibling cards in the same grid dim to ~85% opacity while one card is hovered, implemented via the CSS `:has()` selector on the grid container (e.g. `.grid:has(.card:hover) .card:not(:hover) { opacity: .85 }`). This is a progressive enhancement: browsers without `:has()` support simply skip the dimming and the hover-expand still works.
- **Touch (no hover)**: a small script detects `(hover: none)` via matchMedia and switches cards to tap-to-toggle. Tapping a card expands it (adds an `.is-open` class driving the same CSS as `:hover`) and collapses any previously open card. No dimming on touch (dimming is a hover-only affordance).

Animation timing: expand/collapse uses the sites' existing easing (`cubic-bezier(.16,1,.3,1)` on cliffindus.com, standard `ease` on luckystop, matching each site's current reveal system) over roughly 250 to 300ms. Card lift on hover reuses the existing `translateY` + shadow pattern already present on `.work-card` / `.f-card`.

### Icon system

Six hand-built inline SVG icons, each with a small CSS keyframe animation that plays when its parent card is hovered or tapped-open. No external image requests, no Lottie/GIF assets.

1. **Box (Inventory)**: the lid, drawn as a separate SVG path, rotates open a few degrees on hover, like a flap lifting.
2. **Badge/Person (Employee & Access)**: a checkmark inside the badge draws in using a `stroke-dasharray` / `stroke-dashoffset` animation.
3. **Trophy/Star (Rates & Cashback)**: a small star or shine sweeps across the trophy on hover (a masked gradient sweep, same technique already used for `.red-gradient` text, adapted to a shape).
4. **Bar chart (Analytics & Reporting)**: three bars grow upward from baseline with a staggered delay (60ms between bars).
5. **Shield (Fraud Protection)**: the shield pulses once (scale 1 to 1.05 and back) and an internal checkmark draws in, same stroke technique as the badge icon.
6. **Bell (Notices & Alerts)**: the bell rotates a few degrees left and right (a short "ring" wiggle), with a small dot pinging at the top right, reusing the existing `ping` keyframe already defined on both sites.

`luckystop.cliffindus.com`'s four cards reuse the Box, Badge, Trophy, and Shield icons (Inventory, Employee, Rates, Fraud Protection), recolored to the site's red/navy palette instead of cliffindus.com's zinc/monochrome palette.

## Part B: cliffindus.com. "Inside the Admin Dashboard"

Replaces the current `#capabilities` section (the 3-column `.cap-grid` of six generic cells) in the same position in the page, keeping the `#capabilities` anchor so the nav link still works.

**Eyebrow**: Admin Platform
**Title**: What the people running the store actually see
**Subtitle**: Lucky Stop's admin isn't a settings page bolted onto an app. It's the operational core for 12+ stores. Hover to look inside.

Six cards, 3-column grid (2 rows), same rhythm as today's Capabilities section:

1. **Box icon. Inventory Management**
   Tagline: Every SKU tracked from request to restock.
   Detail bullets:
   - Scanned-product catalog with a manual "Add Product" fallback for anything without a barcode
   - Per-store Order Lists with a Quick Add panel for fast reordering
   - Auto-reopens on close, plus a standing per-store instructions field so nothing falls through the cracks

2. **Badge icon. Employee & Access Management**
   Tagline: Role-based access, enforced at the API. Not just hidden UI.
   Detail bullets:
   - 5-level role hierarchy: DevAdmin, SuperAdmin, Store Manager, Employee, Customer
   - Every permission checked server-side, not just hidden in the interface
   - Weekly shift scheduling with employee-submitted shift-change requests

3. **Trophy icon. Rates & Cashback Engine**
   Tagline: 5 loyalty tiers, per-category bonuses, live-configurable.
   Detail bullets:
   - Bronze through Diamond tiers, each with its own base cashback rate
   - Category bonus rates stack on top: groceries, hot foods, tobacco, and more
   - Optional flat cents-per-gallon mode for gas and diesel instead of a percentage

4. **Bar chart icon. Analytics & Reporting**
   Tagline: Daily numbers, broken down by store.
   Detail bullets:
   - Daily transaction and revenue charts, including platform-fee breakdown
   - Inventory Intelligence: most-ordered items, by category, by store
   - Auto-generated monthly invoices per store

5. **Shield icon. Fraud Protection & Audit Trail**
   Tagline: Every grant, edit, and override is logged.
   Detail bullets:
   - Receipt photo mandatory before any points grant
   - Full activity log across point grants, offer edits, staff and schedule changes
   - Customer-initiated disputes resolved against the receipt on file

6. **Bell icon. Notices & Alerts**
   Tagline: One pinned message, seen by every store instantly.
   Detail bullets:
   - Admin-authored notices pinned to every employee's chat
   - Push notifications (Firebase Cloud Messaging) across iOS, Android, and web
   - Built-in support chat between store staff and HQ

## Part C: luckystop.cliffindus.com. "Bring This To Your Store"

Replaces the current `.store-cta` block (the single "Are you a Lucky Stop store owner?" box near the footer) with a full section in the same position, before the footer, keeping the navy/red visual language of the rest of the page.

**Eyebrow**: For Business Owners
**Title**: Your store. Your brand. This system.
**Subtitle**: Lucky Stop Rewards is a full operations platform running live across 12+ locations, built and maintained by Cliff Industries. We can build the same caliber of system for your business, under your own name.

Four cards (reusing Box, Badge, Trophy, Shield icons), lighter and more benefit-framed than the cliffindus.com set. Each still follows the same rest/expand structure from Part A: a short tagline at rest, 2 bullet points on hover (fewer than cliffindus.com's 3 to 4, since these are benefit statements rather than technical feature lists).

1. **Box icon. Inventory, handled.**
   Tagline: Track stock requests and restocks per store.
   Detail bullets:
   - Your staff always know what to reorder, no guessing
   - Per-store restock lists, updated in real time

2. **Badge icon. Your team, organized.**
   Tagline: Role-based logins for managers and cashiers.
   Detail bullets:
   - Shift scheduling built in, no spreadsheets
   - Each role sees only what it needs to

3. **Trophy icon. Loyalty rules you control.**
   Tagline: Set your own tiers and cashback rates.
   Detail bullets:
   - Configure rates by product category, change them anytime
   - Run bonus events whenever you want, no code required

4. **Shield icon. Fraud protection built in.**
   Tagline: Every point grant requires a receipt photo.
   Detail bullets:
   - Every action logged and tied to the employee who did it
   - Customer disputes resolved against the receipt on file

Below the cards, a proof strip reusing real, already-established stats: **12+ locations live**, **iOS, Android, and Web**, **receipt-verified on every transaction**.

Closing CTA, replacing today's single button: "Get a system built for your business" (mailto, same address as the current CTA), plus a smaller text link: "Want the full technical breakdown? See what's inside the dashboard on cliffindus.com" linking to `https://cliffindus.com/#capabilities`.

## Testing plan

Since these are static marketing pages with no test framework, verification is manual:
- Open both files directly in a browser (or a simple local static server) and check:
  - Hover-expand and dimming behave correctly on desktop (mouse)
  - Tap-to-toggle behaves correctly on a touch-emulated viewport (browser devtools device mode)
  - Icon animations play once per hover/tap and don't get stuck mid-animation on rapid mouse movement
  - No layout shift/jump in sections below when a card expands
  - No `—` (em dash) anywhere in the new copy (grep the diff before committing)
  - Existing nav anchors (`#capabilities` on cliffindus.com) still scroll to the right place
- Confirm responsive behavior at the existing breakpoints (860px/560px on cliffindus.com, 900px on luckystop.cliffindus.com): cards should stack to fewer columns, consistent with how `.cap-grid` and `.bento` already respond.

## Rollout

Changes are committed to `main` in the `S:\WEBSITES` repo (matching this repo's existing history of direct-to-main commits). Push to `origin/main` triggers the existing Cloudflare Workers auto-deploy for each site. Push happens only after explicit user confirmation, since it deploys straight to production.
