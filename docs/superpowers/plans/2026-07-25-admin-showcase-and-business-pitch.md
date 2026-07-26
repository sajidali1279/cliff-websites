# Admin Showcase + Business Pitch Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace cliffindus.com's generic Capabilities section with a hover-expand showcase of real Lucky Stop admin features, and replace luckystop.cliffindus.com's small store-owner CTA with a full "bring this to your business" pitch section, both using the same custom-animated-SVG-icon, hover-to-expand card pattern.

**Architecture:** Two independent single-file edits (`cliffindus.com/index.html`, `luckystop.cliffindus.com/index.html`). No build step, no shared code between the two files (matches this repo's existing pattern of fully self-contained HTML files per site). Each task adds CSS (card grid, absolute-positioned hover/tap detail panel, per-icon CSS keyframe animations), hand-built inline SVG icons, real content, and a small touch-detection script, then removes the CSS/HTML it replaces.

**Tech Stack:** Plain HTML/CSS/vanilla JS. CSS `:has()` for hover-dimming (progressive enhancement, no fallback needed). `matchMedia('(hover: none)')` for touch tap-to-toggle.

## Global Constraints

- No new pages, no build tooling, no framework, no external requests added (from spec Non-goals).
- No Flaticon or other third-party icon/animation service. Icons are hand-built inline SVG animated with CSS only (from spec Part A / user decision).
- No em dash character (`—`) anywhere in new copy: titles, taglines, bullets, CTAs. Use periods, colons, or separate lines instead (from spec Non-goals / explicit user instruction).
- Card expand mechanism must not push/reflow the grid: use an absolutely-positioned detail panel, not a height-changing card (from spec Part A).
- Push to `origin/main` only after explicit user confirmation, since Cloudflare Workers auto-deploys straight to production on push (from spec Rollout).

---

## Task 1: cliffindus.com. Inside the Admin Dashboard

**Files:**
- Modify: `S:\WEBSITES\cliffindus.com\index.html:341-363` (replace `.cap-*` CSS with `.admin-*` CSS + icon animation CSS)
- Modify: `S:\WEBSITES\cliffindus.com\index.html:494` and `:500` (media queries: `.cap-grid` selectors become `.admin-grid`)
- Modify: `S:\WEBSITES\cliffindus.com\index.html:730-777` (replace Capabilities section HTML with the new admin showcase HTML)
- Modify: `S:\WEBSITES\cliffindus.com\index.html:914-930` (append touch tap-to-toggle script inside the existing `<script>` block)

**Interfaces:**
- Consumes: existing CSS custom properties already defined in this file's `:root` (`--z950`, `--z900`, `--z800`, `--z700`, `--z600`, `--z400`, `--z300`, `--z200`, `--z100`, `--z50`, `--b1`, `--b2`, `--b3`) and the existing `.max-w`, `.section`, `.eyebrow`, `.s-title`, `.s-sub`, `[data-r]` reveal classes. Keeps the `id="capabilities"` anchor so the existing nav link `<a href="#capabilities">Capabilities</a>` (line 528) keeps working.
- Produces: nothing consumed by Task 2 (fully independent file).

- [ ] **Step 1: Replace the Capabilities CSS block with the new admin showcase CSS**

Open `S:\WEBSITES\cliffindus.com\index.html`. Find this block (lines 341-363):

```css
    /* ── CAPABILITIES ──────────────────────────────────── */
    .cap-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 1px;
      background: var(--b1);
      border: 1px solid var(--b1);
      border-radius: 16px;
      overflow: hidden;
    }
    .cap-cell {
      background: var(--z950);
      padding: 28px 24px;
      transition: background .2s;
    }
    .cap-cell:hover { background: rgba(255,255,255,0.02); }
    .cap-num {
      font-size: 11px; font-weight: 700; letter-spacing: 2px;
      color: var(--z700); text-transform: uppercase; margin-bottom: 14px;
    }
    .cap-icon { font-size: 22px; margin-bottom: 12px; display: block; }
    .cap-cell h4 { font-size: 14px; font-weight: 700; color: var(--z200); margin-bottom: 6px; }
    .cap-cell p { font-size: 13px; color: var(--z600); line-height: 1.6; }
```

Replace it entirely with:

```css
    /* ── ADMIN SHOWCASE ────────────────────────────────── */
    .admin-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
      position: relative;
    }
    .admin-card {
      background: var(--z900);
      border: 1px solid var(--b1);
      border-radius: 16px;
      padding: 24px 22px;
      position: relative;
      cursor: default;
      transition: border-color .25s, box-shadow .25s, transform .25s, opacity .25s;
      z-index: 1;
    }
    .admin-grid:has(.admin-card:hover) .admin-card:not(:hover) { opacity: .82; }
    .admin-card:hover, .admin-card.is-open {
      border-color: var(--b3);
      transform: translateY(-3px);
      box-shadow: 0 20px 50px rgba(0,0,0,0.45);
      z-index: 30;
    }
    .admin-card-icon { width: 30px; height: 30px; color: var(--z300); margin-bottom: 14px; display: block; transition: color .25s; }
    .admin-card:hover .admin-card-icon, .admin-card.is-open .admin-card-icon { color: var(--z50); }
    .admin-card-title { font-size: 15px; font-weight: 700; color: var(--z100); margin-bottom: 6px; letter-spacing: -0.2px; }
    .admin-card-tag { font-size: 13px; color: var(--z600); line-height: 1.6; }

    .admin-detail {
      position: absolute;
      top: calc(100% - 8px); left: 0; right: 0;
      background: var(--z800);
      border: 1px solid var(--b3);
      border-radius: 14px;
      padding: 16px 18px 18px;
      box-shadow: 0 24px 60px rgba(0,0,0,0.55);
      opacity: 0;
      transform: translateY(-6px);
      pointer-events: none;
      transition: opacity .28s cubic-bezier(.16,1,.3,1), transform .28s cubic-bezier(.16,1,.3,1);
      z-index: 40;
    }
    .admin-card:hover .admin-detail, .admin-card.is-open .admin-detail {
      opacity: 1; transform: translateY(0); pointer-events: auto;
    }
    /* Bottom row (cards 4-6 of 6): flip the panel to open upward so it doesn't
       spill into the next section down. */
    .admin-grid > .admin-card:nth-child(n+4) .admin-detail {
      top: auto; bottom: calc(100% - 8px);
      transform: translateY(6px);
    }
    .admin-grid > .admin-card:nth-child(n+4):hover .admin-detail,
    .admin-grid > .admin-card:nth-child(n+4).is-open .admin-detail {
      transform: translateY(0);
    }
    .admin-detail ul { list-style: none; display: flex; flex-direction: column; gap: 8px; }
    .admin-detail li { font-size: 12.5px; color: var(--z300); line-height: 1.55; padding-left: 14px; position: relative; }
    .admin-detail li::before {
      content: ''; position: absolute; left: 0; top: 7px;
      width: 4px; height: 4px; border-radius: 50%; background: var(--z600);
    }

    /* Icon: box (Inventory) */
    .icon-box .flap-l, .icon-box .flap-r { transform-box: fill-box; transition: transform .35s cubic-bezier(.34,1.56,.64,1); }
    .icon-box .flap-l { transform-origin: 0% 100%; }
    .icon-box .flap-r { transform-origin: 100% 100%; }
    .admin-card:hover .icon-box .flap-l, .admin-card.is-open .icon-box .flap-l { transform: rotate(-22deg); }
    .admin-card:hover .icon-box .flap-r, .admin-card.is-open .icon-box .flap-r { transform: rotate(22deg); }

    /* Icon: badge (Employee & Access) */
    .icon-badge .check { stroke-dasharray: 22; stroke-dashoffset: 22; transition: stroke-dashoffset .4s ease .05s; }
    .admin-card:hover .icon-badge .check, .admin-card.is-open .icon-badge .check { stroke-dashoffset: 0; }

    /* Icon: trophy (Rates & Cashback) */
    .icon-trophy .cup { transform-box: fill-box; transform-origin: 50% 100%; transition: transform .4s cubic-bezier(.34,1.56,.64,1); }
    .admin-card:hover .icon-trophy .cup, .admin-card.is-open .icon-trophy .cup { transform: rotate(-5deg); }
    .icon-trophy .shine { opacity: 0; transform-box: fill-box; transform-origin: center; transform: scale(.3); transition: opacity .3s ease .12s, transform .3s ease .12s; }
    .admin-card:hover .icon-trophy .shine, .admin-card.is-open .icon-trophy .shine { opacity: 1; transform: scale(1); }

    /* Icon: bar chart (Analytics & Reporting) */
    .icon-chart .bar { transform-box: fill-box; transform-origin: 50% 100%; transition: transform .3s ease; }
    .icon-chart .bar-1 { transition-delay: 0s; }
    .icon-chart .bar-2 { transition-delay: .06s; }
    .icon-chart .bar-3 { transition-delay: .12s; }
    .admin-card:hover .icon-chart .bar, .admin-card.is-open .icon-chart .bar { transform: scaleY(1.18); }

    /* Icon: shield (Fraud Protection & Audit Trail) */
    .icon-shield .outline { transform-box: fill-box; transform-origin: 50% 50%; transition: transform .3s ease; }
    .admin-card:hover .icon-shield .outline, .admin-card.is-open .icon-shield .outline { transform: scale(1.06); }
    .icon-shield .check { stroke-dasharray: 16; stroke-dashoffset: 16; transition: stroke-dashoffset .35s ease .1s; }
    .admin-card:hover .icon-shield .check, .admin-card.is-open .icon-shield .check { stroke-dashoffset: 0; }

    /* Icon: bell (Notices & Alerts) */
    .icon-bell .body { transform-box: fill-box; transform-origin: 50% 0%; }
    .admin-card:hover .icon-bell .body, .admin-card.is-open .icon-bell .body { animation: bellRing .4s ease; }
    @keyframes bellRing { 0%,100%{transform:rotate(0)} 25%{transform:rotate(-12deg)} 50%{transform:rotate(10deg)} 75%{transform:rotate(-6deg)} }
    .icon-bell .ping { opacity: 0; transition: opacity .2s ease .1s; }
    .admin-card:hover .icon-bell .ping, .admin-card.is-open .icon-bell .ping { opacity: 1; }
```

- [ ] **Step 2: Update the two media queries that reference `.cap-grid`**

At line 494 (inside `@media (max-width: 860px)`), change:

```css
      .cap-grid { grid-template-columns: repeat(2, 1fr); }
```

to:

```css
      .admin-grid { grid-template-columns: repeat(2, 1fr); }
```

At line 500 (inside `@media (max-width: 560px)`), change:

```css
      .cap-grid { grid-template-columns: 1fr; }
```

to:

```css
      .admin-grid { grid-template-columns: 1fr; }
```

- [ ] **Step 3: Replace the Capabilities section HTML with the admin showcase HTML**

Find this block (lines 730-777):

```html
  <!-- ── CAPABILITIES ───────────────────────────────── -->
  <hr class="divider"/>
  <div class="max-w">
    <section id="capabilities" class="section">
      <div class="eyebrow" data-r>Capabilities</div>
      <h2 class="s-title" data-r data-d="1">What we build</h2>
      <p class="s-sub" data-r data-d="2">Every layer of the stack handled. No coordinating between teams. One builder, full ownership.</p>

      <div class="cap-grid" data-r>
        <div class="cap-cell">
          <div class="cap-num">01</div>
          <span class="cap-icon">📱</span>
          <h4>Mobile Apps</h4>
          <p>iOS and Android from one codebase. React Native + Expo EAS builds and submissions.</p>
        </div>
        <div class="cap-cell">
          <div class="cap-num">02</div>
          <span class="cap-icon">🖥️</span>
          <h4>Web Dashboards</h4>
          <p>Admin portals, management tools, and data dashboards built for every browser.</p>
        </div>
        <div class="cap-cell">
          <div class="cap-num">03</div>
          <span class="cap-icon">⚙️</span>
          <h4>Backend Systems</h4>
          <p>REST APIs, databases, authentication, and cloud infrastructure that scales.</p>
        </div>
        <div class="cap-cell">
          <div class="cap-num">04</div>
          <span class="cap-icon">🔔</span>
          <h4>Push Notifications</h4>
          <p>Firebase Cloud Messaging real-time alerts for customers and staff across platforms.</p>
        </div>
        <div class="cap-cell">
          <div class="cap-num">05</div>
          <span class="cap-icon">🔐</span>
          <h4>Auth &amp; Security</h4>
          <p>Phone OTP, role-based access control, JWT, and secure API design at every layer.</p>
        </div>
        <div class="cap-cell">
          <div class="cap-num">06</div>
          <span class="cap-icon">📊</span>
          <h4>Analytics &amp; Reporting</h4>
          <p>Store dashboards, transaction history, inventory reports, and operational insights.</p>
        </div>
      </div>
    </section>
  </div>
```

Replace it entirely with:

```html
  <!-- ── ADMIN SHOWCASE ─────────────────────────────── -->
  <hr class="divider"/>
  <div class="max-w">
    <section id="capabilities" class="section">
      <div class="eyebrow" data-r>Admin Platform</div>
      <h2 class="s-title" data-r data-d="1">What the people running the store actually see</h2>
      <p class="s-sub" data-r data-d="2">Lucky Stop's admin isn't a settings page bolted onto an app. It's the operational core for 12+ stores. Hover to look inside.</p>

      <div class="admin-grid" data-r>

        <div class="admin-card">
          <svg class="admin-card-icon icon-box" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="4" y="11" width="16" height="9" rx="1.2" stroke="currentColor" stroke-width="1.6"/>
            <path class="flap-l" d="M4 11L11.5 11L8 5.5Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/>
            <path class="flap-r" d="M20 11L12.5 11L16 5.5Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/>
          </svg>
          <div class="admin-card-title">Inventory Management</div>
          <p class="admin-card-tag">Every SKU tracked from request to restock.</p>
          <div class="admin-detail">
            <ul>
              <li>Scanned-product catalog with a manual "Add Product" fallback for anything without a barcode</li>
              <li>Per-store Order Lists with a Quick Add panel for fast reordering</li>
              <li>Auto-reopens on close, plus a standing per-store instructions field so nothing falls through the cracks</li>
            </ul>
          </div>
        </div>

        <div class="admin-card">
          <svg class="admin-card-icon icon-badge" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M12 2.5L19 6V12C19 16.5 15.9 19.8 12 21C8.1 19.8 5 16.5 5 12V6L12 2.5Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/>
            <path class="check" d="M8.3 12L10.8 14.5L15.7 9.3" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" fill="none"/>
          </svg>
          <div class="admin-card-title">Employee &amp; Access Management</div>
          <p class="admin-card-tag">Role-based access, enforced at the API. Not just hidden UI.</p>
          <div class="admin-detail">
            <ul>
              <li>5-level role hierarchy: DevAdmin, SuperAdmin, Store Manager, Employee, Customer</li>
              <li>Every permission checked server-side, not just hidden in the interface</li>
              <li>Weekly shift scheduling with employee-submitted shift-change requests</li>
            </ul>
          </div>
        </div>

        <div class="admin-card">
          <svg class="admin-card-icon icon-trophy" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path class="cup" d="M6.5 4h11v6a5.5 5.5 0 0 1-11 0V4Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/>
            <path d="M6.5 5H3.5v2a3.8 3.8 0 0 0 3 3.7" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
            <path d="M17.5 5h3v2a3.8 3.8 0 0 1-3 3.7" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
            <path d="M12 15.5v3" stroke="currentColor" stroke-width="1.6"/>
            <path d="M8.5 21h7M9.3 19h5.4" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
            <circle class="shine" cx="9.5" cy="7" r="1" fill="currentColor"/>
          </svg>
          <div class="admin-card-title">Rates &amp; Cashback Engine</div>
          <p class="admin-card-tag">5 loyalty tiers, per-category bonuses, live-configurable.</p>
          <div class="admin-detail">
            <ul>
              <li>Bronze through Diamond tiers, each with its own base cashback rate</li>
              <li>Category bonus rates stack on top: groceries, hot foods, tobacco, and more</li>
              <li>Optional flat cents-per-gallon mode for gas and diesel instead of a percentage</li>
            </ul>
          </div>
        </div>

        <div class="admin-card">
          <svg class="admin-card-icon icon-chart" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M4 20V4" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
            <path d="M4 20H20" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
            <rect class="bar bar-1" x="7" y="14" width="3" height="6" rx="0.6" fill="currentColor"/>
            <rect class="bar bar-2" x="12" y="10" width="3" height="10" rx="0.6" fill="currentColor"/>
            <rect class="bar bar-3" x="17" y="7" width="3" height="13" rx="0.6" fill="currentColor"/>
          </svg>
          <div class="admin-card-title">Analytics &amp; Reporting</div>
          <p class="admin-card-tag">Daily numbers, broken down by store.</p>
          <div class="admin-detail">
            <ul>
              <li>Daily transaction and revenue charts, including platform-fee breakdown</li>
              <li>Inventory Intelligence: most-ordered items, by category, by store</li>
              <li>Auto-generated monthly invoices per store</li>
            </ul>
          </div>
        </div>

        <div class="admin-card">
          <svg class="admin-card-icon icon-shield" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path class="outline" d="M12 2.5L19.5 5.5V11C19.5 16 16.3 19.8 12 21.5C7.7 19.8 4.5 16 4.5 11V5.5L12 2.5Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/>
            <path class="check" d="M8.3 11.8L10.8 14.3L15.7 9.2" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" fill="none"/>
          </svg>
          <div class="admin-card-title">Fraud Protection &amp; Audit Trail</div>
          <p class="admin-card-tag">Every grant, edit, and override is logged.</p>
          <div class="admin-detail">
            <ul>
              <li>Receipt photo mandatory before any points grant</li>
              <li>Full activity log across point grants, offer edits, staff and schedule changes</li>
              <li>Customer-initiated disputes resolved against the receipt on file</li>
            </ul>
          </div>
        </div>

        <div class="admin-card">
          <svg class="admin-card-icon icon-bell" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <g class="body">
              <path d="M6 11C6 7.5 8.5 5 12 5C15.5 5 18 7.5 18 11V14.5L19.5 17H4.5L6 14.5V11Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/>
              <path d="M9.5 19.5C10 20.5 11 21 12 21C13 21 14 20.5 14.5 19.5" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
            </g>
            <circle class="ping" cx="18" cy="6" r="2.6" fill="#EF4444"/>
          </svg>
          <div class="admin-card-title">Notices &amp; Alerts</div>
          <p class="admin-card-tag">One pinned message, seen by every store instantly.</p>
          <div class="admin-detail">
            <ul>
              <li>Admin-authored notices pinned to every employee's chat</li>
              <li>Push notifications (Firebase Cloud Messaging) across iOS, Android, and web</li>
              <li>Built-in support chat between store staff and HQ</li>
            </ul>
          </div>
        </div>

      </div>
    </section>
  </div>
```

- [ ] **Step 4: Add touch tap-to-toggle behavior**

Find the closing `</script>` tag at line 930 (end of the existing script block that starts at line 914). Immediately before that closing tag, insert:

```javascript

    // Admin cards: tap-to-toggle on touch devices (no hover available)
    if (window.matchMedia('(hover: none)').matches) {
      document.querySelectorAll('.admin-card').forEach(card => {
        card.addEventListener('click', () => {
          const wasOpen = card.classList.contains('is-open');
          document.querySelectorAll('.admin-card.is-open').forEach(c => c.classList.remove('is-open'));
          if (!wasOpen) card.classList.add('is-open');
        });
      });
    }
```

- [ ] **Step 5: Verify in a browser**

Run a local static server from the site folder and open it:

```bash
cd /s/WEBSITES/cliffindus.com && python -m http.server 8811
```

Open `http://localhost:8811/` in a browser. Confirm:
- The six admin cards render with visible icons at rest (box, badge, trophy, bar chart, shield, bell)
- Hovering each card lifts it, dims the other five, plays that card's icon animation, and drops down (top row) or up (bottom row) a detail panel with the right bullets, without shifting any other content on the page
- Clicking `Capabilities` in the nav still scrolls to this section
- At a narrow viewport (browser devtools responsive mode, e.g. 375px wide), tapping a card toggles its detail panel open/closed and tapping a different card closes the previous one
- Resize to ~700px and ~500px wide and confirm the grid drops to 2 then 1 column with no overlap

- [ ] **Step 6: Confirm no em dashes were introduced**

```bash
cd /s/WEBSITES && grep -n "—" cliffindus.com/index.html
```

Expected: no output (grep finds nothing). If it finds a match, fix that line before continuing.

- [ ] **Step 7: Commit**

```bash
cd /s/WEBSITES && git add cliffindus.com/index.html && git commit -m "$(cat <<'EOF'
feat: replace Capabilities with a hover-expand admin dashboard showcase

Swaps the generic agency-language capabilities grid on cliffindus.com for
six real, hover-expandable features pulled from the live Lucky Stop admin
(inventory, employee access, rates engine, analytics, fraud protection,
notices), each with a small hand-built animated SVG icon.
EOF
)"
```

---

## Task 2: luckystop.cliffindus.com. Bring This To Your Store

**Files:**
- Modify: `S:\WEBSITES\luckystop.cliffindus.com\index.html:207-211` (replace dead `.store-cta`/`.store-cta::before`/`.store-cta-text` CSS with `.biz-*` CSS + icon animation CSS; keep the two `.store-cta-btn` rules that follow)
- Modify: `S:\WEBSITES\luckystop.cliffindus.com\index.html:241` (media query: replace the now-unused `.store-cta` rule with a `.biz-grid` rule)
- Modify: `S:\WEBSITES\luckystop.cliffindus.com\index.html:519-528` (replace the store-cta section HTML with the new business pitch section)
- Modify: `S:\WEBSITES\luckystop.cliffindus.com\index.html:539-553` (append touch tap-to-toggle script inside the existing `<script>` block)

**Interfaces:**
- Consumes: existing CSS custom properties already defined in this file's `:root` (`--navy`, `--navy2`, `--red`, `--red-lt`, `--text`, `--muted`, `--muted2`, `--border`, `--border2`, `--surface`) and the existing `.section`, `.eyebrow`, `.section-title`, `.section-sub`, `.store-cta-btn`, `[data-reveal]` reveal classes.
- Produces: nothing consumed by Task 1 (fully independent file). Adds one outbound link to `https://cliffindus.com/#capabilities`, which Task 1 must keep working at that anchor (it does: Task 1 preserves `id="capabilities"`).

- [ ] **Step 1: Replace the dead STORE CTA layout CSS with the business pitch CSS**

Find this block (lines 207-211):

```css
    /* STORE CTA */
    .store-cta { border-radius: 20px; overflow: hidden; position: relative; background: linear-gradient(135deg, #0F1C2E, #1A2E45); border: 1px solid var(--border); padding: 56px 48px; display: flex; align-items: center; justify-content: space-between; gap: 32px; }
    .store-cta::before { content: ''; position: absolute; top: 0; left: 0; right: 0; height: 1px; background: linear-gradient(to right, transparent, var(--red), transparent); }
    .store-cta-text h2 { font-size: clamp(24px,3vw,36px); font-weight: 800; letter-spacing: -1px; margin-bottom: 12px; }
    .store-cta-text p { font-size: 15px; color: var(--muted2); max-width: 400px; line-height: 1.7; }
```

Replace it with (this keeps the `.store-cta-btn` rules on the next two lines untouched, since the new CTA button reuses that class):

```css
    /* BUSINESS PITCH */
    .biz-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; position: relative; }
    .biz-card {
      background: var(--surface); border: 1px solid var(--border); border-radius: 16px;
      padding: 22px 20px; position: relative; cursor: default;
      transition: border-color .25s, box-shadow .25s, transform .25s, opacity .25s;
      z-index: 1;
    }
    .biz-grid:has(.biz-card:hover) .biz-card:not(:hover) { opacity: .82; }
    .biz-card:hover, .biz-card.is-open {
      border-color: var(--border2); transform: translateY(-3px);
      box-shadow: 0 20px 50px rgba(0,0,0,0.45); z-index: 30;
    }
    .biz-card-icon { width: 28px; height: 28px; color: var(--red-lt); margin-bottom: 12px; display: block; transition: color .25s; }
    .biz-card-title { font-size: 14.5px; font-weight: 700; color: var(--text); margin-bottom: 5px; letter-spacing: -0.2px; }
    .biz-card-tag { font-size: 12.5px; color: var(--muted); line-height: 1.55; }

    .biz-detail {
      position: absolute; top: calc(100% - 8px); left: 0; right: 0;
      background: var(--navy2); border: 1px solid var(--border2); border-radius: 14px;
      padding: 14px 16px 16px; box-shadow: 0 24px 60px rgba(0,0,0,0.55);
      opacity: 0; transform: translateY(-6px); pointer-events: none;
      transition: opacity .28s ease, transform .28s ease; z-index: 40;
    }
    .biz-card:hover .biz-detail, .biz-card.is-open .biz-detail { opacity: 1; transform: translateY(0); pointer-events: auto; }
    .biz-detail ul { list-style: none; display: flex; flex-direction: column; gap: 7px; }
    .biz-detail li { font-size: 12px; color: var(--muted2); line-height: 1.5; padding-left: 13px; position: relative; }
    .biz-detail li::before { content: ''; position: absolute; left: 0; top: 6px; width: 4px; height: 4px; border-radius: 50%; background: var(--red-lt); }

    .biz-proof { display: flex; gap: 40px; margin-top: 40px; padding-top: 28px; border-top: 1px solid var(--border); flex-wrap: wrap; }
    .biz-proof-num { font-size: 24px; font-weight: 800; color: var(--red-lt); letter-spacing: -0.5px; }
    .biz-proof-lbl { font-size: 12px; color: var(--muted); margin-top: 2px; }

    .biz-cta-row { margin-top: 36px; display: flex; align-items: center; gap: 20px; flex-wrap: wrap; }
    .biz-cta-link { font-size: 13px; color: var(--muted); text-decoration: none; border-bottom: 1px solid var(--border2); padding-bottom: 1px; transition: color .15s; }
    .biz-cta-link:hover { color: var(--muted2); }

    /* Icon: box (Inventory) */
    .icon-box .flap-l, .icon-box .flap-r { transform-box: fill-box; transition: transform .35s cubic-bezier(.34,1.56,.64,1); }
    .icon-box .flap-l { transform-origin: 0% 100%; }
    .icon-box .flap-r { transform-origin: 100% 100%; }
    .biz-card:hover .icon-box .flap-l, .biz-card.is-open .icon-box .flap-l { transform: rotate(-22deg); }
    .biz-card:hover .icon-box .flap-r, .biz-card.is-open .icon-box .flap-r { transform: rotate(22deg); }

    /* Icon: badge (Team) */
    .icon-badge .check { stroke-dasharray: 22; stroke-dashoffset: 22; transition: stroke-dashoffset .4s ease .05s; }
    .biz-card:hover .icon-badge .check, .biz-card.is-open .icon-badge .check { stroke-dashoffset: 0; }

    /* Icon: trophy (Loyalty rules) */
    .icon-trophy .cup { transform-box: fill-box; transform-origin: 50% 100%; transition: transform .4s cubic-bezier(.34,1.56,.64,1); }
    .biz-card:hover .icon-trophy .cup, .biz-card.is-open .icon-trophy .cup { transform: rotate(-5deg); }
    .icon-trophy .shine { opacity: 0; transform-box: fill-box; transform-origin: center; transform: scale(.3); transition: opacity .3s ease .12s, transform .3s ease .12s; }
    .biz-card:hover .icon-trophy .shine, .biz-card.is-open .icon-trophy .shine { opacity: 1; transform: scale(1); }

    /* Icon: shield (Fraud protection) */
    .icon-shield .outline { transform-box: fill-box; transform-origin: 50% 50%; transition: transform .3s ease; }
    .biz-card:hover .icon-shield .outline, .biz-card.is-open .icon-shield .outline { transform: scale(1.06); }
    .icon-shield .check { stroke-dasharray: 16; stroke-dashoffset: 16; transition: stroke-dashoffset .35s ease .1s; }
    .biz-card:hover .icon-shield .check, .biz-card.is-open .icon-shield .check { stroke-dashoffset: 0; }
```

- [ ] **Step 2: Update the media query that references `.store-cta`**

At line 241 (inside `@media (max-width: 900px)`), change:

```css
      .store-cta { flex-direction: column; padding: 32px 24px; }
```

to:

```css
      .biz-grid { grid-template-columns: repeat(2,1fr); }
      .biz-proof { gap: 24px; }
```

- [ ] **Step 3: Replace the store-cta section HTML with the business pitch section**

Find this block (lines 519-528):

```html
  <hr class="section-divider"/>
  <section class="section">
    <div class="store-cta" data-reveal>
      <div class="store-cta-text">
        <h2>Are you a Lucky Stop store owner?</h2>
        <p>Manage your rewards program, push offers to customers, track sales, and run your team, all from the web admin portal. Contact us to get your store set up.</p>
      </div>
      <a href="mailto:sksajidali1279@gmail.com" class="store-cta-btn">Get your store set up →</a>
    </div>
  </section>
```

Replace it entirely with:

```html
  <hr class="section-divider"/>
  <section class="section">
    <div class="eyebrow" data-reveal>For Business Owners</div>
    <h2 class="section-title" data-reveal>Your store. Your brand. This system.</h2>
    <p class="section-sub" data-reveal>Lucky Stop Rewards is a full operations platform running live across 12+ locations, built and maintained by Cliff Industries. We can build the same caliber of system for your business, under your own name.</p>

    <div class="biz-grid" data-reveal>

      <div class="biz-card">
        <svg class="biz-card-icon icon-box" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect x="4" y="11" width="16" height="9" rx="1.2" stroke="currentColor" stroke-width="1.6"/>
          <path class="flap-l" d="M4 11L11.5 11L8 5.5Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/>
          <path class="flap-r" d="M20 11L12.5 11L16 5.5Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/>
        </svg>
        <div class="biz-card-title">Inventory, handled.</div>
        <p class="biz-card-tag">Track stock requests and restocks per store.</p>
        <div class="biz-detail">
          <ul>
            <li>Your staff always know what to reorder, no guessing</li>
            <li>Per-store restock lists, updated in real time</li>
          </ul>
        </div>
      </div>

      <div class="biz-card">
        <svg class="biz-card-icon icon-badge" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M12 2.5L19 6V12C19 16.5 15.9 19.8 12 21C8.1 19.8 5 16.5 5 12V6L12 2.5Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/>
          <path class="check" d="M8.3 12L10.8 14.5L15.7 9.3" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" fill="none"/>
        </svg>
        <div class="biz-card-title">Your team, organized.</div>
        <p class="biz-card-tag">Role-based logins for managers and cashiers.</p>
        <div class="biz-detail">
          <ul>
            <li>Shift scheduling built in, no spreadsheets</li>
            <li>Each role sees only what it needs to</li>
          </ul>
        </div>
      </div>

      <div class="biz-card">
        <svg class="biz-card-icon icon-trophy" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path class="cup" d="M6.5 4h11v6a5.5 5.5 0 0 1-11 0V4Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/>
          <path d="M6.5 5H3.5v2a3.8 3.8 0 0 0 3 3.7" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
          <path d="M17.5 5h3v2a3.8 3.8 0 0 1-3 3.7" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
          <path d="M12 15.5v3" stroke="currentColor" stroke-width="1.6"/>
          <path d="M8.5 21h7M9.3 19h5.4" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
          <circle class="shine" cx="9.5" cy="7" r="1" fill="currentColor"/>
        </svg>
        <div class="biz-card-title">Loyalty rules you control.</div>
        <p class="biz-card-tag">Set your own tiers and cashback rates.</p>
        <div class="biz-detail">
          <ul>
            <li>Configure rates by product category, change them anytime</li>
            <li>Run bonus events whenever you want, no code required</li>
          </ul>
        </div>
      </div>

      <div class="biz-card">
        <svg class="biz-card-icon icon-shield" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path class="outline" d="M12 2.5L19.5 5.5V11C19.5 16 16.3 19.8 12 21.5C7.7 19.8 4.5 16 4.5 11V5.5L12 2.5Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/>
          <path class="check" d="M8.3 11.8L10.8 14.3L15.7 9.2" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" fill="none"/>
        </svg>
        <div class="biz-card-title">Fraud protection built in.</div>
        <p class="biz-card-tag">Every point grant requires a receipt photo.</p>
        <div class="biz-detail">
          <ul>
            <li>Every action logged and tied to the employee who did it</li>
            <li>Customer disputes resolved against the receipt on file</li>
          </ul>
        </div>
      </div>

    </div>

    <div class="biz-proof" data-reveal>
      <div class="biz-proof-item">
        <div class="biz-proof-num">12+</div>
        <div class="biz-proof-lbl">Locations live today</div>
      </div>
      <div class="biz-proof-item">
        <div class="biz-proof-num">iOS · Android · Web</div>
        <div class="biz-proof-lbl">Every platform covered</div>
      </div>
      <div class="biz-proof-item">
        <div class="biz-proof-num">100%</div>
        <div class="biz-proof-lbl">Of point grants receipt-verified</div>
      </div>
    </div>

    <div class="biz-cta-row" data-reveal>
      <a href="mailto:sksajidali1279@gmail.com" class="store-cta-btn">Get a system built for your business →</a>
      <a href="https://cliffindus.com/#capabilities" target="_blank" class="biz-cta-link">Want the full technical breakdown? See what's inside the dashboard on cliffindus.com</a>
    </div>
  </section>
```

- [ ] **Step 4: Add touch tap-to-toggle behavior**

Find the closing `</script>` tag at line 553 (end of the existing script block that starts at line 539). Immediately before that closing tag, insert:

```javascript

    // Business pitch cards: tap-to-toggle on touch devices (no hover available)
    if (window.matchMedia('(hover: none)').matches) {
      document.querySelectorAll('.biz-card').forEach(card => {
        card.addEventListener('click', () => {
          const wasOpen = card.classList.contains('is-open');
          document.querySelectorAll('.biz-card.is-open').forEach(c => c.classList.remove('is-open'));
          if (!wasOpen) card.classList.add('is-open');
        });
      });
    }
```

- [ ] **Step 5: Verify in a browser**

```bash
cd /s/WEBSITES/luckystop.cliffindus.com && python -m http.server 8812
```

Open `http://localhost:8812/` in a browser. Confirm:
- The four business-pitch cards render with visible icons at rest (box, badge, trophy, shield), red/navy themed
- Hovering each card lifts it, dims the other three, plays that card's icon animation, and drops down a detail panel with the right two bullets
- The proof strip (12+, iOS/Android/Web, 100%) renders below the cards
- Both CTAs work: the mailto button and the text link to `cliffindus.com/#capabilities` (opens in a new tab)
- At ~700px wide the grid drops to 2 columns without overlap; tapping a card toggles it and tapping another closes the first

- [ ] **Step 6: Confirm no em dashes were introduced**

```bash
cd /s/WEBSITES && grep -n "—" luckystop.cliffindus.com/index.html
```

Expected: no output. If it finds a match, fix that line before continuing.

- [ ] **Step 7: Commit**

```bash
cd /s/WEBSITES && git add luckystop.cliffindus.com/index.html && git commit -m "$(cat <<'EOF'
feat: replace store-owner CTA with a full B2B pitch section

Lucky Stop Rewards is now positioned as a case study: the small
'are you a Lucky Stop store owner' box becomes a proper section pitching
other business owners on getting their own branded version of the same
platform, with hover-expand cards and a proof strip of real stats.
EOF
)"
```

---

## After both tasks: manual cross-check, then push

- [ ] Open both local previews side by side one more time and confirm the visual language feels related (same card mechanic, each site's own palette) but not identical/copy-pasted.
- [ ] Confirm `git log --oneline -2` shows both commits on `main`.
- [ ] Ask the user for explicit confirmation before running `git push`, since both sites auto-deploy to production via Cloudflare Workers on push to `origin/main`.
