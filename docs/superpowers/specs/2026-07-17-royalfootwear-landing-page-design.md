# Royal Footwear landing page — design

**Date:** 2026-07-17
**Status:** Approved

## Context

Royal Footwear (wholesale/retail footwear business, Tenali, Andhra Pradesh) has a backend + admin dashboard (separate repo, `Royal_foot_wear`) but no public-facing web presence. Following the existing pattern in this repo — `cliffindus.com` (dev portfolio) and `luckystop.cliffindus.com` (that project's own front-facing site) — Royal Footwear gets its own dedicated subdomain site: `royalfootwear.cliffindus.com`.

This is a real business site, not a portfolio showcase entry. No card is being added to `cliffindus.com`'s project list as part of this work.

## Audience & goal

Primary audience: retailers/wholesale buyers evaluating Royal Footwear as a supplier. Goal: communicate what the business sells and why a retailer should reach out, with contact (phone/WhatsApp/email) as the call to action. There is no live self-serve retailer signup (the mobile app doesn't exist yet), so the CTA is "contact us," not "create an account."

## Real business facts (use verbatim, do not embellish)

- Business name: **Royal Footwear**
- Categories: general, sports, school, health, indoor — footwear "of all kinds"
- Phone / WhatsApp: **+91 95025 45219** (WhatsApp link: `https://wa.me/919502545219`)
- Email: **Saaju1017@gmail.com**
- Address: **Shamshekhan Street, Islampet, Tenali**

No numeric stats (retailer counts, years in business, SKU counts) are available and none should be invented or displayed.

## Page structure

1. **Nav** — "Royal Footwear" wordmark + shoe-icon mark. Anchor links: Categories, Why Partner, Contact. "Contact Us" button (links to WhatsApp).
2. **Hero** — badge ("Wholesale & Retail Footwear"), headline + subcopy describing footwear sourcing across every category. Primary CTA: WhatsApp. Secondary CTA: call (`tel:+919502545219`). No phone-app mockup (nothing live to show). Simple icon-based visual accent in place of it.
3. **Categories strip** — 5 cards: General, Sports, School, Health, Indoor.
4. **"Why partner with us"** — bento grid of real platform capabilities reframed as retailer benefits:
   - GST-compliant invoicing on every order
   - Case-based bulk ordering built for retailers
   - Tiered wholesale pricing based on order volume
   - Simple style → color → size catalog structure
5. **"How to get started"** — 3 steps: reach out via call/WhatsApp/email → we set up your retailer account & discuss pricing → place your first bulk order with GST invoice included. (Manual process, described honestly — no self-serve flow exists yet.)
6. **Contact block** — closing CTA panel (same visual pattern as Lucky Stop's `store-cta`): phone/WhatsApp, email, address, each as a real `tel:` / `wa.me` / `mailto:` link.
7. **Footer** — "© 2026 Royal Footwear · Built by Cliff Industries" linking to `https://cliffindus.com`, plus footer links (Categories, Contact, Cliff Industries).

## Explicitly out of scope

- No staff/admin login link (admin dashboard is internal Admin/Staff only — not a retailer-facing feature)
- No app store buttons or app mockup (mobile app not built yet)
- No loyalty/tier section (not applicable to a B2B wholesale catalog business)
- No fabricated stats, counts, or years-in-business claims
- No privacy policy page (no forms, no data collection on this page — only static links)
- No project card added to `cliffindus.com`'s portfolio
- No DNS/hosting setup — this repo's existing deploy mechanism for `cliffindus.com` / `luckystop.cliffindus.com` is not visible in-repo (no CNAME file or CI config), so pointing `royalfootwear.cliffindus.com` at this folder is a manual step on the account owner's end after the page is built.

## Visual design

Same build quality and mechanics as `luckystop.cliffindus.com` (Inter font, scroll-reveal via `IntersectionObserver`, card hover spotlight effect via mouse-tracked CSS custom properties, same responsive breakpoints at 900px) but a distinct palette:

- Background: warm espresso-brown (dark, not navy — e.g. `#1A1410` family)
- Accent: leather-tan/amber (e.g. `#C8853A` family), replacing Lucky Stop's red
- Text: warm off-white / muted tan-grays (parallel to Lucky Stop's slate grays, warmed slightly)

Category and feature cards use emoji icons (👟 🏫 🏥 🏠 ⚽ or similar), matching the lightweight-iconography approach already used across both sibling sites — no stock photography.

## Technical approach

- Single self-contained `index.html` (inline `<style>` and `<script>`, no build step, no dependencies) at `S:\WEBSITES\royalfootwear.cliffindus.com\index.html` — matches the existing two-folder convention in this repo exactly.
- Standard meta tags: title, description, canonical URL (`https://royalfootwear.cliffindus.com/`), Open Graph + Twitter card tags, `theme-color`, inline SVG data-URI favicon (footwear-themed, tan/brown).
- `og.png` (1200×630 social preview image) generated to match the new palette, alongside the HTML file.
- No forms, no JS framework, no backend calls — pure static links (`tel:`, `wa.me`, `mailto:`).

## Testing

Static HTML with no build step — verify by opening the file directly in a browser and checking: all sections render, scroll-reveal animations fire, responsive layout holds at mobile width, and all `tel:`/`wa.me`/`mailto:` links carry the correct real contact details.
