# Royal Footwear Landing Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build `royalfootwear.cliffindus.com` — a single self-contained static HTML landing page for Royal Footwear, matching the design spec.

**Architecture:** One dependency-free `index.html` (inline CSS + inline JS, no build step), following the exact folder convention already used by `cliffindus.com/` and `luckystop.cliffindus.com/` in this repo. Built incrementally: each task appends a complete section (CSS + HTML) to an already-valid HTML document, so the page renders correctly after every task. A matching `og.png` social-preview image is generated separately via a Python script.

**Tech Stack:** Plain HTML/CSS/JS (Inter font via Google Fonts CDN, no framework), Python 3 + Pillow for image generation.

## Global Constraints

- Spec: `docs/superpowers/specs/2026-07-17-royalfootwear-landing-page-design.md`
- Real contact details (use verbatim, never alter): phone/WhatsApp `+91 9502545219` (WhatsApp link `https://wa.me/919502545219`, tel link `tel:+919502545219`), email `Saaju1017@gmail.com`, address `Shamshekhan Street, Islampet, Tenali`
- Categories (use verbatim, exactly these five, no more/fewer): General, Sports, School, Health, Indoor
- No fabricated stats, counts, or years-in-business claims anywhere on the page
- No staff/admin login link, no app-store buttons/mockup, no loyalty-tier section, no privacy policy page, no forms
- No project card added to `cliffindus.com`'s portfolio (out of scope for this plan)
- Palette: background `#1A1410` (`--brown`) / `#2A1F17` (`--brown2`), accent `#C8853A` (`--amber`) / `#E0A868` (`--amber-lt`), text `#FAF6F0` (`--text`), muted `#B8A895` (`--muted`) / `#D9CBB8` (`--muted2`)
- File location: `S:\WEBSITES\royalfootwear.cliffindus.com\index.html` and `S:\WEBSITES\royalfootwear.cliffindus.com\og.png`

---

### Task 1: Scaffold — head, base styles, nav, hero, footer, script

**Files:**
- Create: `S:\WEBSITES\royalfootwear.cliffindus.com\index.html`

**Interfaces:**
- Produces: CSS custom properties `--brown`, `--brown2`, `--amber`, `--amber-lt`, `--text`, `--muted`, `--muted2`, `--border`, `--border2`, `--surface` on `:root`, used by every later task. Shared section CSS classes `.section`, `.section-divider`, `.eyebrow`, `.section-title`, `.section-sub`, `[data-reveal]` used by every later task. HTML anchor `</section>\n\n  <footer>` (the hero section's closing tag immediately followed by the footer) — Task 2 inserts new sections at this exact point. CSS anchor `    /* FOOTER */` — Tasks 2 and 3 insert new CSS rule blocks immediately before this comment. CSS anchor `.footer-links a:hover { color: var(--muted2); }\n  </style>` — Task 4 inserts the responsive media query immediately before this point.

- [ ] **Step 1: Create the directory and write the full scaffold file**

Create `S:\WEBSITES\royalfootwear.cliffindus.com\index.html` with this exact content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Royal Footwear · Wholesale &amp; Retail Footwear Supplier</title>
  <meta name="description" content="Royal Footwear supplies general, sports, school, health, and indoor footwear to retailers at wholesale prices, with GST-compliant invoicing and flexible case-based bulk ordering. Based in Tenali, Andhra Pradesh."/>
  <link rel="canonical" href="https://royalfootwear.cliffindus.com/"/>
  <meta name="theme-color" content="#1A1410"/>
  <link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Crect width='100' height='100' rx='22' fill='%231A1410'/%3E%3Ccircle cx='50' cy='50' r='24' fill='%23C8853A'/%3E%3C/svg%3E"/>
  <meta property="og:type" content="website"/>
  <meta property="og:site_name" content="Royal Footwear"/>
  <meta property="og:title" content="Royal Footwear · Wholesale &amp; Retail Footwear Supplier"/>
  <meta property="og:description" content="General, sports, school, health, and indoor footwear at wholesale prices. GST-compliant invoicing, flexible bulk ordering. Based in Tenali, Andhra Pradesh."/>
  <meta property="og:url" content="https://royalfootwear.cliffindus.com/"/>
  <meta property="og:image" content="https://royalfootwear.cliffindus.com/og.png"/>
  <meta property="og:image:width" content="1200"/>
  <meta property="og:image:height" content="630"/>
  <meta name="twitter:card" content="summary_large_image"/>
  <meta name="twitter:title" content="Royal Footwear · Wholesale &amp; Retail Footwear Supplier"/>
  <meta name="twitter:description" content="General, sports, school, health, and indoor footwear at wholesale prices. GST-compliant invoicing, flexible bulk ordering."/>
  <meta name="twitter:image" content="https://royalfootwear.cliffindus.com/og.png"/>
  <link rel="preconnect" href="https://fonts.googleapis.com"/>
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin/>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --brown:  #1A1410;
      --brown2: #2A1F17;
      --amber:  #C8853A;
      --amber-lt: #E0A868;
      --text:   #FAF6F0;
      --muted:  #B8A895;
      --muted2: #D9CBB8;
      --border: rgba(255,255,255,0.07);
      --border2:rgba(255,255,255,0.12);
      --surface:rgba(255,255,255,0.04);
    }

    html { scroll-behavior: smooth; }

    body {
      font-family: 'Inter', system-ui, sans-serif;
      background: var(--brown);
      color: var(--text);
      line-height: 1.6;
      overflow-x: hidden;
      -webkit-font-smoothing: antialiased;
    }

    ::-webkit-scrollbar { width: 6px; }
    ::-webkit-scrollbar-track { background: var(--brown); }
    ::-webkit-scrollbar-thumb { background: #3D2E20; border-radius: 3px; }

    /* NAV */
    nav {
      position: fixed; top: 0; left: 0; right: 0; z-index: 200;
      height: 60px;
      display: flex; align-items: center; justify-content: space-between;
      padding: 0 40px;
      background: rgba(26,20,16,0.85);
      backdrop-filter: blur(16px);
      border-bottom: 1px solid var(--border);
    }
    .nav-brand { display: flex; align-items: center; gap: 10px; text-decoration: none; color: var(--text); }
    .shoe-icon { font-size: 22px; line-height: 1; }
    .nav-wordmark { font-size: 16px; font-weight: 800; letter-spacing: -0.3px; }
    .nav-wordmark span { color: var(--amber-lt); }
    .nav-right { display: flex; align-items: center; gap: 8px; }
    .nav-link { color: var(--muted); font-size: 14px; font-weight: 500; text-decoration: none; padding: 6px 12px; border-radius: 8px; transition: color .15s; }
    .nav-link:hover { color: var(--text); }
    .nav-btn-cta { background: rgba(255,255,255,0.06); border: 1px solid var(--border2); color: var(--text); font-size: 13px; font-weight: 600; padding: 6px 14px; border-radius: 8px; text-decoration: none; transition: background .15s; }
    .nav-btn-cta:hover { background: rgba(255,255,255,0.1); }

    /* HERO */
    .hero {
      position: relative; min-height: 100vh;
      display: flex; align-items: center; justify-content: center;
      padding: 120px 24px 80px; overflow: hidden;
    }
    .hero::before {
      content: '';
      position: absolute; inset: 0;
      background-image: radial-gradient(circle, rgba(255,255,255,0.06) 1px, transparent 1px);
      background-size: 32px 32px;
      mask-image: radial-gradient(ellipse 80% 100% at 50% 0%, black 60%, transparent 100%);
      -webkit-mask-image: radial-gradient(ellipse 80% 100% at 50% 0%, black 60%, transparent 100%);
      pointer-events: none;
    }
    .hero-orb-1 { position: absolute; width: 700px; height: 700px; background: radial-gradient(circle, rgba(200,133,58,0.18), transparent 70%); top: -200px; left: -100px; border-radius: 50%; animation: oa 14s ease-in-out infinite; pointer-events: none; }
    .hero-orb-2 { position: absolute; width: 500px; height: 500px; background: radial-gradient(circle, rgba(224,168,104,0.10), transparent 70%); bottom: -100px; right: -100px; border-radius: 50%; animation: ob 10s ease-in-out infinite; pointer-events: none; }
    @keyframes oa { 0%,100%{transform:translate(0,0)} 50%{transform:translate(30px,-30px)} }
    @keyframes ob { 0%,100%{transform:translate(0,0)} 50%{transform:translate(-30px,20px)} }

    .hero-inner { position: relative; z-index: 1; display: grid; grid-template-columns: 1fr auto; gap: 64px; align-items: center; max-width: 1100px; width: 100%; }

    .hero-badge { display: inline-flex; align-items: center; gap: 8px; background: rgba(200,133,58,0.08); border: 1px solid rgba(200,133,58,0.2); border-radius: 100px; padding: 5px 14px 5px 8px; margin-bottom: 24px; }
    .badge-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--amber-lt); box-shadow: 0 0 0 3px rgba(224,168,104,0.2); animation: bpulse 2s ease-in-out infinite; }
    @keyframes bpulse { 0%,100%{box-shadow:0 0 0 3px rgba(224,168,104,0.2)} 50%{box-shadow:0 0 0 6px rgba(224,168,104,0)} }
    .hero-badge span { font-size: 12px; font-weight: 600; color: var(--amber-lt); letter-spacing: .3px; }

    .hero-text h1 { font-size: clamp(38px, 6vw, 72px); font-weight: 900; line-height: 1.05; letter-spacing: -2.5px; margin-bottom: 20px; }
    .amber-gradient { background: linear-gradient(135deg, #E0A868 0%, #C8853A 50%, #F0C088 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
    .hero-text p { font-size: 17px; color: var(--muted2); line-height: 1.7; max-width: 440px; margin-bottom: 36px; }

    .hero-ctas { display: flex; flex-wrap: wrap; gap: 12px; }
    .cta-whatsapp { display: inline-flex; align-items: center; gap: 10px; background: var(--text); color: var(--brown); font-size: 14px; font-weight: 700; padding: 12px 22px; border-radius: 12px; text-decoration: none; transition: transform .15s, background .15s; letter-spacing: -0.2px; }
    .cta-whatsapp:hover { background: #F0E6D8; transform: translateY(-2px); }
    .cta-icon { font-size: 18px; }
    .cta-call { display: inline-flex; align-items: center; gap: 10px; background: rgba(255,255,255,0.04); border: 1px solid var(--border2); color: var(--muted2); font-size: 14px; font-weight: 600; padding: 12px 22px; border-radius: 12px; text-decoration: none; transition: background .15s, color .15s; }
    .cta-call:hover { background: rgba(255,255,255,0.08); color: var(--text); }

    .hero-tags { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 32px; }
    .hero-tag { font-size: 12px; font-weight: 600; color: var(--muted2); background: var(--surface); border: 1px solid var(--border); border-radius: 100px; padding: 5px 12px; }

    /* HERO VISUAL */
    .hero-visual-wrap { position: relative; flex-shrink: 0; }
    .hero-visual-glow { position: absolute; inset: -40px; background: radial-gradient(circle, rgba(200,133,58,0.14), transparent 70%); border-radius: 50%; pointer-events: none; }
    .hero-visual { position: relative; width: 260px; height: 340px; background: linear-gradient(145deg, var(--brown2), var(--brown)); border-radius: 32px; border: 2px solid rgba(255,255,255,0.08); box-shadow: 0 32px 80px rgba(0,0,0,0.5), inset 0 1px 0 rgba(255,255,255,0.06); display: flex; flex-wrap: wrap; align-items: center; justify-content: center; align-content: center; gap: 22px; padding: 32px; animation: visualFloat 6s ease-in-out infinite; }
    @keyframes visualFloat { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-12px)} }
    .hero-visual span { font-size: 42px; filter: drop-shadow(0 6px 14px rgba(0,0,0,.4)); }

    /* SECTION (shared by all sections below) */
    .section { padding: 88px 24px; max-width: 1100px; margin: 0 auto; }
    .section-divider { border: none; border-top: 1px solid var(--border); }
    .eyebrow { display: inline-flex; align-items: center; gap: 6px; font-size: 12px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; color: var(--amber-lt); margin-bottom: 14px; }
    .eyebrow::before { content:''; width:14px;height:1px;background:var(--amber-lt);display:block; }
    .section-title { font-size: clamp(26px,4vw,42px); font-weight: 800; letter-spacing: -1.5px; line-height: 1.1; margin-bottom: 14px; }
    .section-sub { font-size: 16px; color: var(--muted2); max-width: 480px; line-height: 1.7; margin-bottom: 52px; }

    /* REVEAL */
    [data-reveal] { opacity: 0; transform: translateY(20px); transition: opacity .55s ease, transform .55s ease; }
    [data-reveal].revealed { opacity: 1; transform: translateY(0); }

    /* FOOTER */
    footer { border-top: 1px solid var(--border); padding: 32px 40px; display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 16px; }
    footer p { font-size: 13px; color: var(--muted); }
    .footer-links { display: flex; gap: 20px; }
    .footer-links a { color: var(--muted); text-decoration: none; font-size: 13px; transition: color .15s; }
    .footer-links a:hover { color: var(--muted2); }
  </style>
</head>
<body>

  <nav>
    <a href="/" class="nav-brand">
      <span class="shoe-icon">👞</span>
      <span class="nav-wordmark">Royal <span>Footwear</span></span>
    </a>
    <div class="nav-right">
      <a href="#categories" class="nav-link">Categories</a>
      <a href="#why-partner" class="nav-link">Why Partner</a>
      <a href="https://wa.me/919502545219" target="_blank" class="nav-btn-cta">Contact Us →</a>
    </div>
  </nav>

  <section class="hero">
    <div class="hero-orb-1"></div>
    <div class="hero-orb-2"></div>

    <div class="hero-inner">
      <div class="hero-text">
        <div class="hero-badge">
          <div class="badge-dot"></div>
          <span>Wholesale &amp; Retail Footwear</span>
        </div>

        <h1>
          Every category,<br/>
          <span class="amber-gradient">stocked and ready</span>
        </h1>

        <p>
          Royal Footwear supplies retailers with footwear across every category, general, sports, school, health, and indoor, at wholesale prices, with proper GST invoicing on every order.
        </p>

        <div class="hero-ctas">
          <a href="https://wa.me/919502545219" target="_blank" class="cta-whatsapp">
            <span class="cta-icon">💬</span>
            <div>
              <div style="font-size:10px;font-weight:500;opacity:0.7;margin-bottom:1px">MESSAGE US ON</div>
              <div>WhatsApp</div>
            </div>
          </a>
          <a href="tel:+919502545219" class="cta-call">
            <span class="cta-icon">📞</span>
            <div>
              <div style="font-size:10px;font-weight:500;opacity:0.6;margin-bottom:1px">CALL</div>
              <div>+91 95025 45219</div>
            </div>
          </a>
        </div>

        <div class="hero-tags">
          <span class="hero-tag">General</span>
          <span class="hero-tag">Sports</span>
          <span class="hero-tag">School</span>
          <span class="hero-tag">Health</span>
          <span class="hero-tag">Indoor</span>
        </div>
      </div>

      <div class="hero-visual-wrap">
        <div class="hero-visual-glow"></div>
        <div class="hero-visual">
          <span>👞</span>
          <span>👟</span>
          <span>🥾</span>
          <span>🩴</span>
          <span>👢</span>
          <span>🥿</span>
        </div>
      </div>
    </div>
  </section>

  <footer>
    <p>© 2026 Royal Footwear · Built by <a href="https://cliffindus.com" style="color:var(--muted2);text-decoration:none">Cliff Industries</a></p>
    <div class="footer-links">
      <a href="#categories">Categories</a>
      <a href="#contact">Contact</a>
      <a href="https://cliffindus.com" target="_blank">Cliff Industries</a>
    </div>
  </footer>

  <script>
    const reveals = document.querySelectorAll('[data-reveal]');
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('revealed'); observer.unobserve(e.target); } });
    }, { threshold: 0.08, rootMargin: '0px 0px -40px 0px' });
    reveals.forEach(el => observer.observe(el));

    document.querySelectorAll('.f-card').forEach(card => {
      card.addEventListener('mousemove', e => {
        const r = card.getBoundingClientRect();
        card.style.setProperty('--x', `${e.clientX - r.left}px`);
        card.style.setProperty('--y', `${e.clientY - r.top}px`);
      });
    });
  </script>

</body>
</html>
```

- [ ] **Step 2: Verify the file is well-formed and contains the expected anchors**

Run: `grep -c "<section" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `1` (only the hero section exists so far)

Run: `grep -c "919502545219" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `3` (WhatsApp link in nav, WhatsApp link in hero, tel link in hero)

Run: `grep -c "</html>" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `1`

- [ ] **Step 3: Commit**

```bash
cd "S:\WEBSITES"
git add royalfootwear.cliffindus.com/index.html
git commit -m "feat: scaffold royalfootwear.cliffindus.com — head, nav, hero"
```

---

### Task 2: Categories and Why Partner sections

**Files:**
- Modify: `S:\WEBSITES\royalfootwear.cliffindus.com\index.html`

**Interfaces:**
- Consumes: CSS anchor `    /* FOOTER */` and HTML anchor `  </section>\n\n  <footer>` produced by Task 1.
- Produces: CSS classes `.categories`, `.cat-card`, `.cat-icon`, `.cat-name`, `.bento`, `.f-card`, `.f-card-icon` used by Task 4's responsive block and by Task 6's JS-interaction verification. HTML sections with ids `categories` and `why-partner`, linked from the nav (`#categories`) and footer (`#categories`) added in Task 1.

- [ ] **Step 1: Insert CSS for the categories grid and feature bento grid**

Using the Edit tool on `S:\WEBSITES\royalfootwear.cliffindus.com\index.html`, replace:

```
    /* FOOTER */
    footer { border-top: 1px solid var(--border); padding: 32px 40px; display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 16px; }
```

with:

```
    /* CATEGORIES */
    .categories { display: grid; grid-template-columns: repeat(5,1fr); gap: 10px; }
    .cat-card { background: var(--surface); border: 1px solid var(--border); border-radius: 14px; padding: 28px 16px; text-align: center; transition: transform .2s, border-color .2s; }
    .cat-card:hover { transform: translateY(-4px); border-color: var(--border2); }
    .cat-icon { font-size: 32px; margin-bottom: 12px; display: block; }
    .cat-name { font-size: 14px; font-weight: 700; letter-spacing: -0.2px; }

    /* FEATURES */
    .bento { display: grid; grid-template-columns: repeat(2,1fr); gap: 10px; }
    .f-card { background: var(--surface); border: 1px solid var(--border); border-radius: 16px; padding: 28px; transition: border-color .2s, transform .2s, box-shadow .2s; position: relative; overflow: hidden; }
    .f-card::after { content:''; position:absolute; inset:0; border-radius:inherit; background: radial-gradient(400px circle at var(--x,50%) var(--y,50%), rgba(200,133,58,0.06), transparent 60%); opacity:0; transition: opacity .3s; pointer-events:none; }
    .f-card:hover { border-color: var(--border2); transform: translateY(-3px); box-shadow: 0 16px 48px rgba(0,0,0,0.4); }
    .f-card:hover::after { opacity: 1; }
    .f-card-icon { font-size: 32px; margin-bottom: 16px; display: block; }
    .f-card h3 { font-size: 19px; font-weight: 700; letter-spacing: -0.4px; margin-bottom: 8px; }
    .f-card p { font-size: 14px; color: var(--muted2); line-height: 1.7; }

    /* FOOTER */
    footer { border-top: 1px solid var(--border); padding: 32px 40px; display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 16px; }
```

- [ ] **Step 2: Insert the Categories and Why Partner sections**

Using the Edit tool, replace:

```
  </section>

  <footer>
```

with:

```
  </section>

  <hr class="section-divider"/>
  <section id="categories" class="section">
    <div class="eyebrow" data-reveal>What We Stock</div>
    <h2 class="section-title" data-reveal>Every kind of footwear, one supplier</h2>
    <p class="section-sub" data-reveal>From daily wear to specialized needs, we carry across every category retailers ask for.</p>

    <div class="categories" data-reveal>
      <div class="cat-card">
        <span class="cat-icon">👞</span>
        <div class="cat-name">General</div>
      </div>
      <div class="cat-card">
        <span class="cat-icon">👟</span>
        <div class="cat-name">Sports</div>
      </div>
      <div class="cat-card">
        <span class="cat-icon">🎒</span>
        <div class="cat-name">School</div>
      </div>
      <div class="cat-card">
        <span class="cat-icon">🩺</span>
        <div class="cat-name">Health</div>
      </div>
      <div class="cat-card">
        <span class="cat-icon">🩴</span>
        <div class="cat-name">Indoor</div>
      </div>
    </div>
  </section>

  <hr class="section-divider"/>
  <section id="why-partner" class="section">
    <div class="eyebrow" data-reveal>Why Partner With Us</div>
    <h2 class="section-title" data-reveal>Built for how retailers actually order</h2>
    <p class="section-sub" data-reveal>Real wholesale infrastructure behind every order, not just a price list.</p>

    <div class="bento" data-reveal>
      <div class="f-card">
        <span class="f-card-icon">🧾</span>
        <h3>GST-compliant invoicing</h3>
        <p>Every order comes with a proper GST tax invoice, CGST/SGST or IGST calculated correctly for your state, ready for your books.</p>
      </div>
      <div class="f-card">
        <span class="f-card-icon">📦</span>
        <h3>Case-based bulk ordering</h3>
        <p>Order by the case, not one pair at a time. Built around how retailers actually buy, in bulk, by style and size run.</p>
      </div>
      <div class="f-card">
        <span class="f-card-icon">💰</span>
        <h3>Tiered wholesale pricing</h3>
        <p>Pricing scales with your order volume, so larger retailers get better rates automatically.</p>
      </div>
      <div class="f-card">
        <span class="f-card-icon">🗂️</span>
        <h3>Simple catalog structure</h3>
        <p>Every product organized by style, color, and size, so finding and ordering exactly what you need takes seconds.</p>
      </div>
    </div>
  </section>

  <footer>
```

- [ ] **Step 3: Verify**

Run: `grep -c "<section" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `3`

Run: `grep -c "cat-card" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `7` (5 `<div class="cat-card">` usages + `.cat-card {` rule + `.cat-card:hover {` rule)

Run: `grep -c -E "General|Sports|School|Health|Indoor" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `12` (5 hero-tag spans + 5 category-grid names, both from Task 1/2, plus the capitalized "General" that opens the `og:description` and `twitter:description` meta content strings from Task 1 — 2 + 5 + 5)

- [ ] **Step 4: Commit**

```bash
cd "S:\WEBSITES"
git add royalfootwear.cliffindus.com/index.html
git commit -m "feat: add Categories and Why Partner sections to royalfootwear.cliffindus.com"
```

---

### Task 3: How It Works and Contact sections

**Files:**
- Modify: `S:\WEBSITES\royalfootwear.cliffindus.com\index.html`

**Interfaces:**
- Consumes: CSS anchor `    /* FOOTER */` and HTML anchor `  </section>\n\n  <footer>` (now immediately after the Why Partner section from Task 2).
- Produces: CSS classes `.steps`, `.step`, `.store-cta`, `.contact-address`, `.contact-links`, `.store-cta-btn`, `.contact-link-sub` used by Task 4's responsive block. HTML element `id="contact"` on the contact CTA panel, linked from the nav and footer anchors added in Task 1.

- [ ] **Step 1: Insert CSS for the steps grid and contact CTA panel**

Using the Edit tool, replace:

```
    /* FOOTER */
    footer { border-top: 1px solid var(--border); padding: 32px 40px; display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 16px; }
```

with:

```
    /* HOW IT WORKS */
    .steps { display: grid; grid-template-columns: repeat(3,1fr); gap: 1px; background: var(--border); border: 1px solid var(--border); border-radius: 16px; overflow: hidden; }
    .step { background: var(--brown); padding: 32px 24px; transition: background .2s; }
    .step:hover { background: rgba(255,255,255,0.03); }
    .step-num { font-size: 11px; font-weight: 700; letter-spacing: 2px; color: var(--amber-lt); text-transform: uppercase; margin-bottom: 14px; }
    .step-icon { font-size: 28px; margin-bottom: 14px; display: block; }
    .step h4 { font-size: 15px; font-weight: 700; margin-bottom: 6px; }
    .step p { font-size: 13px; color: var(--muted2); line-height: 1.6; }

    /* CONTACT CTA */
    .store-cta { border-radius: 20px; overflow: hidden; position: relative; background: linear-gradient(135deg, var(--brown), var(--brown2)); border: 1px solid var(--border); padding: 56px 48px; display: flex; align-items: flex-start; justify-content: space-between; gap: 32px; flex-wrap: wrap; }
    .store-cta::before { content: ''; position: absolute; top: 0; left: 0; right: 0; height: 1px; background: linear-gradient(to right, transparent, var(--amber), transparent); }
    .store-cta-text h2 { font-size: clamp(24px,3vw,36px); font-weight: 800; letter-spacing: -1px; margin-bottom: 12px; }
    .store-cta-text p { font-size: 15px; color: var(--muted2); max-width: 400px; line-height: 1.7; }
    .contact-address { margin-top: 20px; font-size: 13px; color: var(--muted); }
    .contact-links { flex-shrink: 0; display: flex; flex-direction: column; align-items: flex-start; gap: 12px; }
    .store-cta-btn { display: inline-flex; align-items: center; gap: 8px; background: var(--amber); color: #fff; font-size: 14px; font-weight: 700; padding: 12px 24px; border-radius: 12px; text-decoration: none; transition: background .15s, transform .15s; white-space: nowrap; }
    .store-cta-btn:hover { background: #B37530; transform: translateY(-2px); }
    .contact-link-sub { color: var(--muted2); text-decoration: none; font-size: 14px; transition: color .15s; }
    .contact-link-sub:hover { color: var(--text); }

    /* FOOTER */
    footer { border-top: 1px solid var(--border); padding: 32px 40px; display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 16px; }
```

- [ ] **Step 2: Insert the How It Works and Contact sections**

Using the Edit tool, replace:

```
  </section>

  <footer>
```

with:

```
  </section>

  <hr class="section-divider"/>
  <section id="how-it-works" class="section">
    <div class="eyebrow" data-reveal>Process</div>
    <h2 class="section-title" data-reveal>How to get started</h2>
    <p class="section-sub" data-reveal>Three steps from first contact to your first bulk order.</p>

    <div class="steps" data-reveal>
      <div class="step">
        <div class="step-num">Step 01</div>
        <span class="step-icon">📞</span>
        <h4>Reach out</h4>
        <p>Call, WhatsApp, or email us to start the conversation.</p>
      </div>
      <div class="step">
        <div class="step-num">Step 02</div>
        <span class="step-icon">🤝</span>
        <h4>We set you up</h4>
        <p>We set up your retailer account and walk through pricing and minimum order quantities together.</p>
      </div>
      <div class="step">
        <div class="step-num">Step 03</div>
        <span class="step-icon">📦</span>
        <h4>Place your first order</h4>
        <p>Order by the case across any category. We prepare it and send a GST invoice with your shipment.</p>
      </div>
    </div>
  </section>

  <hr class="section-divider"/>
  <section class="section">
    <div class="store-cta" id="contact" data-reveal>
      <div class="store-cta-text">
        <h2>Ready to stock Royal Footwear?</h2>
        <p>Reach out and we'll set up your retailer account, pricing, minimum orders, and next steps, sorted out directly with us.</p>
        <div class="contact-address">📍 Shamshekhan Street, Islampet, Tenali</div>
      </div>
      <div class="contact-links">
        <a href="https://wa.me/919502545219" target="_blank" class="store-cta-btn">WhatsApp Us →</a>
        <a href="tel:+919502545219" class="contact-link-sub">📞 +91 95025 45219</a>
        <a href="mailto:Saaju1017@gmail.com" class="contact-link-sub">✉️ Saaju1017@gmail.com</a>
      </div>
    </div>
  </section>

  <footer>
```

- [ ] **Step 3: Verify**

Run: `grep -c "<section" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `5`

Run: `grep -c "Saaju1017@gmail.com" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `1`

Run: `grep -c "Shamshekhan Street, Islampet, Tenali" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `1`

Run: `grep -c "919502545219" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `5` (3 from Task 1 — nav link, hero WhatsApp CTA, hero call CTA — plus 2 new lines in the contact panel: the WhatsApp button and the tel link)

- [ ] **Step 4: Commit**

```bash
cd "S:\WEBSITES"
git add royalfootwear.cliffindus.com/index.html
git commit -m "feat: add How It Works and Contact sections to royalfootwear.cliffindus.com"
```

---

### Task 4: Responsive layout

**Files:**
- Modify: `S:\WEBSITES\royalfootwear.cliffindus.com\index.html`

**Interfaces:**
- Consumes: all CSS classes produced by Tasks 1–3 (`.hero-inner`, `.hero-visual-wrap`, `.categories`, `.bento`, `.steps`, `.store-cta`, `.contact-links`, `footer`).
- Produces: nothing consumed by later tasks — this is the last CSS addition.

- [ ] **Step 1: Insert the responsive media query at the end of the stylesheet**

Using the Edit tool, replace:

```
    .footer-links a:hover { color: var(--muted2); }
  </style>
```

with:

```
    .footer-links a:hover { color: var(--muted2); }

    /* RESPONSIVE */
    @media (max-width: 900px) {
      nav { padding: 0 20px; }
      .hero-inner { grid-template-columns: 1fr; text-align: center; }
      .hero-text p { max-width: none; margin-left: auto; margin-right: auto; }
      .hero-ctas { justify-content: center; }
      .hero-tags { justify-content: center; }
      .hero-visual-wrap { display: none; }
      .categories { grid-template-columns: repeat(2,1fr); }
      .bento { grid-template-columns: 1fr; }
      .steps { grid-template-columns: 1fr; }
      .store-cta { flex-direction: column; padding: 32px 24px; }
      .contact-links { align-items: flex-start; }
      footer { flex-direction: column; align-items: flex-start; padding: 24px 20px; }
    }
  </style>
```

- [ ] **Step 2: Verify**

Run: `grep -c "@media (max-width: 900px)" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `1`

Run: `grep -c "</style>" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `1`

- [ ] **Step 3: Commit**

```bash
cd "S:\WEBSITES"
git add royalfootwear.cliffindus.com/index.html
git commit -m "feat: add responsive layout to royalfootwear.cliffindus.com"
```

---

### Task 5: Generate the og.png social preview image

**Files:**
- Create: `S:\WEBSITES\royalfootwear.cliffindus.com\og.png`
- Create (temporary, delete after use): `S:\WEBSITES\generate_og.py`

**Interfaces:**
- Consumes: palette values from the Global Constraints section (`#1A1410`, `#C8853A`, `#E0A868`, `#FAF6F0`, `#B8A895`).
- Produces: `og.png` at `S:\WEBSITES\royalfootwear.cliffindus.com\og.png`, referenced by the `og:image` and `twitter:image` meta tags already present in `index.html` from Task 1.

- [ ] **Step 1: Write the image-generation script**

Create `S:\WEBSITES\generate_og.py`:

```python
from PIL import Image, ImageDraw, ImageFont

W, H = 1200, 630
bg = (26, 20, 16)
amber = (200, 133, 58)
amber_lt = (224, 168, 104)
text_color = (250, 246, 240)
muted = (184, 168, 149)

img = Image.new("RGB", (W, H), bg)
draw = ImageDraw.Draw(img)

draw.rectangle([0, 0, W, 6], fill=amber)

title_font = ImageFont.truetype(r"C:\Windows\Fonts\segoeuib.ttf", 72)
sub_font = ImageFont.truetype(r"C:\Windows\Fonts\segoeui.ttf", 32)
tag_font = ImageFont.truetype(r"C:\Windows\Fonts\segoeuib.ttf", 26)

draw.text((80, 210), "Royal Footwear", font=title_font, fill=text_color)
draw.text((80, 310), "Wholesale & Retail Footwear Supplier", font=sub_font, fill=amber_lt)
draw.text((80, 380), "General  ·  Sports  ·  School  ·  Health  ·  Indoor", font=tag_font, fill=muted)

img.save(r"S:\WEBSITES\royalfootwear.cliffindus.com\og.png")
print(f"saved {img.size} to og.png")
```

- [ ] **Step 2: Run the script**

Run: `python "S:\WEBSITES\generate_og.py"`
Expected output: `saved (1200, 630) to og.png`

- [ ] **Step 3: Verify the image file**

Run: `python -c "from PIL import Image; im = Image.open(r'S:\WEBSITES\royalfootwear.cliffindus.com\og.png'); print(im.size, im.format)"`
Expected output: `(1200, 630) PNG`

- [ ] **Step 4: Delete the temporary script and commit the image**

```bash
rm "S:/WEBSITES/generate_og.py"
cd "S:\WEBSITES"
git add royalfootwear.cliffindus.com/og.png
git commit -m "feat: add og.png social preview image for royalfootwear.cliffindus.com"
```

---

### Task 6: Final verification pass

**Files:**
- None (verification only)

**Interfaces:**
- Consumes: the complete `S:\WEBSITES\royalfootwear.cliffindus.com\index.html` and `og.png` from Tasks 1–5.

- [ ] **Step 1: Confirm every real contact detail appears correctly, exactly once where expected**

Run: `grep -n "919502545219\|Saaju1017@gmail.com\|Shamshekhan Street, Islampet, Tenali" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`

Expected: every match line shows the phone number formatted as either `wa.me/919502545219` or `tel:+919502545219` or the display text `+91 95025 45219` — never a mistyped digit. The email must read exactly `Saaju1017@gmail.com` and the address exactly `Shamshekhan Street, Islampet, Tenali`. Manually read each matched line and confirm no typos crept in during the earlier tasks.

- [ ] **Step 2: Confirm no excluded content was accidentally added**

Run: `grep -ic "login\|sign up\|signup\|app store\|google play\|privacy policy\|bronze\|silver\|gold\|platinum\|diamond" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"`
Expected: `0`

- [ ] **Step 3: Open the page in a browser and visually confirm**

Run: `start "" "S:\WEBSITES\royalfootwear.cliffindus.com\index.html"` (Windows — opens in the default browser)

Manually confirm:
- Nav, hero, categories, why-partner, how-it-works, and contact sections all render without visual glitches
- Scrolling triggers the fade-in reveal animation on each section
- Hovering a "Why Partner" feature card shows the amber spotlight-glow effect following the cursor
- Resizing the window below 900px width collapses to the single-column mobile layout
- The WhatsApp, call, and email links all have the correct real values (click to confirm the browser offers to open WhatsApp / dial / compose email with the right number and address)

- [ ] **Step 4: Note remaining manual follow-up (not part of this plan)**

No code changes needed for this step — it's a note for the account owner. Per the spec's "Explicitly out of scope" section, pointing `royalfootwear.cliffindus.com` at this folder requires: (a) adding the subdomain to whichever host currently serves `cliffindus.com`/`luckystop.cliffindus.com` (mechanism not visible in this repo — check the relevant hosting dashboard), and (b) adding the corresponding DNS record at the domain registrar for `cliffindus.com`. This is outside the scope of this implementation plan.
