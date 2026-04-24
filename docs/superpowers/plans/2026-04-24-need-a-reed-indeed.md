# Need a Reed Indeed — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a 5-page static HTML website for "Need a Reed Indeed," a professional home organizing service, with animated gradient backgrounds, FontAwesome icons, and a Formspree-powered contact form.

**Architecture:** Five self-contained HTML files sharing a consistent nav, footer, CSS variable set, and animated gradient pattern. No build tools — Tailwind CDN + custom `<style>` blocks. Gallery filtering and form submission handled by inline JavaScript.

**Tech Stack:** HTML5 · Tailwind CSS (CDN) · Custom CSS (keyframe animations, CSS variables) · Google Fonts · FontAwesome Free (CDN) · Formspree (form backend) · Node.js serve.mjs (local dev server)

---

## Parallel Execution Note

- **Task 1** must complete first (establishes shared patterns).
- **Tasks 2, 3, 4, 5, 6** are fully independent — dispatch all five in parallel after Task 1.
- **Task 7** runs after all page tasks complete.

## Model Assignments

| Task | Model | Reason |
|------|-------|--------|
| Task 1 — Foundation | `sonnet` | Design judgment for shared CSS, animations, nav/footer |
| Task 2 — Home | `sonnet` | Most visually complex page — hero, service grid, CTA |
| Task 3 — Services | `haiku` | Repetitive card layout, mechanical structure |
| Task 4 — Gallery | `sonnet` | Filter JS logic + hover animation layer |
| Task 5 — About | `haiku` | Content layout, straightforward structure |
| Task 6 — Contact | `sonnet` | Formspree integration, async form JS, error states |
| Task 7 — Polish | `sonnet` | Visual review, nav active states, screenshot verification |

---

## File Structure

```
/
├── index.html          # Home page
├── services.html       # Services page
├── gallery.html        # Gallery with filter
├── about.html          # About / story page
├── contact.html        # Contact form (Formspree)
└── serve.mjs           # Local dev server (port 3000)
```

---

## Shared Patterns (defined in Task 1, used verbatim in all pages)

### Head Block
Every page uses this `<head>` (swap `<title>` per page):

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>PAGE TITLE — Need a Reed Indeed</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            display: ['"Dancing Script"', 'cursive'],
            heading: ['"Cormorant Garamond"', 'serif'],
            body: ['Nunito', 'sans-serif'],
          }
        }
      }
    }
  </script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;600&family=Dancing+Script:wght@700&family=Nunito:wght@400;600;700;800&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
  <style>
    :root {
      --rose:    #e8527a;
      --pink:    #ff8fab;
      --blush:   #ffb3c6;
      --coral:   #ff8c69;
      --peach:   #ffab91;
      --gold:    #f4c430;
      --cream:   #fff8f5;
      --text:    #3d2a2a;
      --spring:  cubic-bezier(0.34, 1.56, 0.64, 1);
    }
    body { font-family: 'Nunito', sans-serif; background: var(--cream); color: var(--text); }
    .grad-bg {
      background: linear-gradient(-45deg, #ff8fab, #ffb3c6, #ffcba4, #ffd700, #ff6b9d, #ffab91);
      background-size: 400% 400%;
      animation: gradientShift 20s ease infinite;
    }
    @keyframes gradientShift {
      0%   { background-position: 0% 50%; }
      50%  { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }
    .font-display  { font-family: 'Dancing Script', cursive; }
    .font-heading  { font-family: 'Cormorant Garamond', serif; }
    .nav-link {
      font-weight: 700; font-size: 0.85rem; letter-spacing: 0.06em;
      text-transform: uppercase; color: var(--text);
      position: relative; padding-bottom: 2px; transition: color 0.2s;
    }
    .nav-link::after {
      content: ''; position: absolute; bottom: 0; left: 0;
      width: 0; height: 2px; background: var(--rose);
      transition: width 0.25s var(--spring);
    }
    .nav-link:hover { color: var(--rose); }
    .nav-link:hover::after { width: 100%; }
    .nav-link.active { color: var(--rose); }
    .nav-link.active::after { width: 100%; }
    .btn-primary {
      display: inline-block; background: linear-gradient(135deg, var(--rose), var(--coral));
      color: white; font-weight: 800; font-size: 0.9rem; letter-spacing: 0.05em;
      padding: 14px 32px; border-radius: 50px; border: none; cursor: pointer;
      box-shadow: 0 6px 24px rgba(232,82,122,0.35);
      transition: transform 0.2s var(--spring), box-shadow 0.2s;
    }
    .btn-primary:hover  { transform: translateY(-2px) scale(1.03); box-shadow: 0 10px 32px rgba(232,82,122,0.45); }
    .btn-primary:active { transform: translateY(0) scale(0.98); }
    .btn-primary:focus-visible { outline: 3px solid var(--gold); outline-offset: 3px; }
    .btn-outline {
      display: inline-block; background: transparent;
      color: var(--rose); font-weight: 800; font-size: 0.9rem;
      padding: 12px 30px; border-radius: 50px; border: 2px solid var(--rose);
      cursor: pointer; transition: background 0.2s, color 0.2s, transform 0.2s var(--spring);
    }
    .btn-outline:hover  { background: var(--rose); color: white; transform: translateY(-2px); }
    .btn-outline:active { transform: translateY(0); }
    .btn-outline:focus-visible { outline: 3px solid var(--gold); outline-offset: 3px; }
    .card {
      background: white; border-radius: 20px;
      box-shadow: 0 4px 0 0 rgba(232,82,122,0.08), 0 8px 32px rgba(232,82,122,0.06);
      transition: transform 0.25s var(--spring), box-shadow 0.25s;
      overflow: hidden;
    }
    .card:hover {
      transform: translateY(-6px);
      box-shadow: 0 8px 0 0 rgba(232,82,122,0.12), 0 16px 48px rgba(232,82,122,0.12);
    }
  </style>
</head>
```

### Nav Block
```html
<nav class="sticky top-0 z-50 bg-white/90 backdrop-blur-md border-b border-pink-100">
  <div class="max-w-6xl mx-auto px-6 py-4 flex items-center justify-between">
    <a href="index.html" class="font-display text-2xl leading-none" style="color:var(--rose)">Need a Reed Indeed</a>
    <div class="hidden md:flex items-center gap-8">
      <a href="index.html"    class="nav-link">Home</a>
      <a href="services.html" class="nav-link">Services</a>
      <a href="gallery.html"  class="nav-link">Gallery</a>
      <a href="about.html"    class="nav-link">About</a>
      <a href="contact.html"  class="btn-primary text-sm py-3 px-6">Get in Touch</a>
    </div>
    <button id="menu-toggle" class="md:hidden focus-visible:outline-none" aria-label="Open menu">
      <i class="fa-solid fa-bars text-xl" style="color:var(--rose)"></i>
    </button>
  </div>
  <div id="mobile-menu" class="hidden md:hidden bg-white border-t border-pink-100 px-6 py-5 flex flex-col gap-4">
    <a href="index.html"    class="nav-link text-base">Home</a>
    <a href="services.html" class="nav-link text-base">Services</a>
    <a href="gallery.html"  class="nav-link text-base">Gallery</a>
    <a href="about.html"    class="nav-link text-base">About</a>
    <a href="contact.html"  class="btn-primary text-center mt-2">Get in Touch</a>
  </div>
  <script>
    document.getElementById('menu-toggle').addEventListener('click', () => {
      document.getElementById('mobile-menu').classList.toggle('hidden');
    });
    // Mark active nav link
    document.querySelectorAll('.nav-link').forEach(link => {
      if (link.href === location.href || location.pathname.endsWith(link.getAttribute('href'))) {
        link.classList.add('active');
      }
    });
  </script>
</nav>
```

### Footer Block
```html
<footer style="background:#fff0f4; border-top:1px solid #ffd6e0;" class="py-14 mt-20">
  <div class="max-w-6xl mx-auto px-6 grid md:grid-cols-3 gap-10">
    <div>
      <div class="font-display text-3xl mb-2" style="color:var(--rose)">Need a Reed Indeed</div>
      <p class="text-sm" style="color:var(--text); opacity:0.65;">Professional Home Organizing</p>
    </div>
    <div>
      <h3 class="font-body font-extrabold text-xs uppercase tracking-widest mb-4" style="color:var(--rose)">Quick Links</h3>
      <div class="flex flex-col gap-2 text-sm" style="color:var(--text); opacity:0.8;">
        <a href="services.html" class="hover:opacity-100 transition-opacity">Services</a>
        <a href="gallery.html"  class="hover:opacity-100 transition-opacity">Gallery</a>
        <a href="about.html"    class="hover:opacity-100 transition-opacity">About</a>
        <a href="contact.html"  class="hover:opacity-100 transition-opacity">Contact</a>
      </div>
    </div>
    <div>
      <h3 class="font-body font-extrabold text-xs uppercase tracking-widest mb-4" style="color:var(--rose)">Contact Us</h3>
      <div class="flex flex-col gap-3 text-sm" style="color:var(--text); opacity:0.8;">
        <a href="tel:3232120384" class="flex items-center gap-2 hover:opacity-100 transition-opacity">
          <i class="fa-solid fa-phone" style="color:var(--rose)"></i> (323) 212-0384
        </a>
        <a href="mailto:reeddesmond22@gmail.com" class="flex items-center gap-2 hover:opacity-100 transition-opacity">
          <i class="fa-solid fa-envelope" style="color:var(--rose)"></i> reeddesmond22@gmail.com
        </a>
        <a href="mailto:ebonyb540@gmail.com" class="flex items-center gap-2 hover:opacity-100 transition-opacity">
          <i class="fa-solid fa-envelope" style="color:var(--rose)"></i> ebonyb540@gmail.com
        </a>
      </div>
    </div>
  </div>
  <div class="max-w-6xl mx-auto px-6 mt-10 pt-6 border-t border-pink-200 text-xs text-center" style="opacity:0.5;">
    &copy; 2026 Need a Reed Indeed. All rights reserved.
  </div>
</footer>
```

---

## Task 1: Project Foundation
**Model:** `sonnet`
**Files:** `serve.mjs`

- [ ] **Step 1: Create serve.mjs**

```js
import { createServer } from 'http';
import { readFile } from 'fs/promises';
import { extname, join } from 'path';
import { fileURLToPath } from 'url';

const __dirname = fileURLToPath(new URL('.', import.meta.url));
const PORT = 3000;
const MIME = {
  '.html': 'text/html; charset=utf-8',
  '.css':  'text/css',
  '.js':   'application/javascript',
  '.png':  'image/png',
  '.jpg':  'image/jpeg',
  '.svg':  'image/svg+xml',
  '.ico':  'image/x-icon',
  '.mjs':  'application/javascript',
};

createServer(async (req, res) => {
  let urlPath = req.url.split('?')[0];
  if (urlPath === '/') urlPath = '/index.html';
  try {
    const data = await readFile(join(__dirname, urlPath));
    const ext  = extname(urlPath);
    res.writeHead(200, { 'Content-Type': MIME[ext] || 'text/plain' });
    res.end(data);
  } catch {
    res.writeHead(404, { 'Content-Type': 'text/plain' });
    res.end('404 Not Found');
  }
}).listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
```

- [ ] **Step 2: Start the server and verify it runs**

```bash
node serve.mjs
```
Expected output: `Server running at http://localhost:3000`
Open http://localhost:3000 in browser — should show 404 (no index.html yet — that's fine).
Stop with Ctrl+C.

- [ ] **Step 3: Commit**

```bash
rtk git add serve.mjs && rtk git commit -m "feat: add local dev server"
```

---

## Task 2: Home Page
**Model:** `sonnet`
**Files:** Create `index.html`
**Can run in parallel with Tasks 3, 4, 5, 6**

- [ ] **Step 1: Create index.html**

```html
<!DOCTYPE html>
<html lang="en">
[HEAD BLOCK — use Shared Head Pattern, title: "Home — Need a Reed Indeed"]
<style>
  .hero-text { animation: fadeUp 0.7s ease both; }
  .hero-sub  { animation: fadeUp 0.7s 0.15s ease both; }
  .hero-cta  { animation: fadeUp 0.7s 0.3s ease both; }
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(20px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  .service-card {
    background: white; border-radius: 20px; padding: 28px 24px;
    box-shadow: 0 4px 24px rgba(232,82,122,0.07);
    transition: transform 0.25s cubic-bezier(0.34,1.56,0.64,1), box-shadow 0.25s;
  }
  .service-card:hover {
    transform: translateY(-6px);
    box-shadow: 0 12px 40px rgba(232,82,122,0.14);
  }
</style>
</head>
<body>

[NAV BLOCK — use Shared Nav Pattern]

<!-- HERO -->
<section class="grad-bg relative overflow-hidden" style="min-height:88vh; display:flex; align-items:center;">
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at 30% 40%,rgba(255,255,255,0.28) 0%,transparent 60%),radial-gradient(ellipse at 70% 70%,rgba(244,196,48,0.18) 0%,transparent 55%);pointer-events:none;"></div>
  <div class="max-w-3xl mx-auto px-6 py-24 text-center relative">
    <p class="hero-text font-body font-extrabold text-xs uppercase tracking-widest text-white/80 mb-4">Professional Home Organizing</p>
    <h1 class="hero-text font-display text-white mb-6" style="font-size:clamp(2.8rem,7vw,5rem);line-height:1.1;text-shadow:0 2px 24px rgba(200,80,100,0.3);">
      Your Space,<br>Beautifully Organized
    </h1>
    <p class="hero-sub font-body text-white/90 text-lg leading-relaxed max-w-xl mx-auto mb-10">
      From cluttered garages to chaotic pantries — we transform every corner of your home into a peaceful, beautiful space you'll love.
    </p>
    <div class="hero-cta flex flex-col sm:flex-row gap-4 justify-center">
      <a href="contact.html" class="btn-primary">
        <i class="fa-solid fa-sparkles mr-2"></i>Get Organized Today
      </a>
      <a href="services.html" class="btn-outline" style="background:rgba(255,255,255,0.2);border-color:white;color:white;">
        View Our Services
      </a>
    </div>
  </div>
</section>

<!-- SERVICES GRID -->
<section class="max-w-6xl mx-auto px-6 py-20">
  <div class="text-center mb-14">
    <p class="font-body font-extrabold text-xs uppercase tracking-widest mb-3" style="color:var(--gold)">What We Organize</p>
    <h2 class="font-heading text-5xl font-semibold" style="color:var(--text)">Our Services</h2>
  </div>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
    <!-- Service cards (7 total) -->
    <div class="service-card">
      <div class="w-14 h-14 rounded-2xl flex items-center justify-center mb-5" style="background:var(--cream);">
        <i class="fa-solid fa-shirt fa-lg" style="color:var(--rose)"></i>
      </div>
      <h3 class="font-heading text-2xl font-semibold mb-2" style="color:var(--text)">Closets</h3>
      <p class="font-body text-sm leading-relaxed" style="color:var(--text);opacity:0.65;">Maximizing every inch — from wardrobe systems to shoe collections.</p>
    </div>
    <div class="service-card">
      <div class="w-14 h-14 rounded-2xl flex items-center justify-center mb-5" style="background:var(--cream);">
        <i class="fa-solid fa-jar fa-lg" style="color:var(--rose)"></i>
      </div>
      <h3 class="font-heading text-2xl font-semibold mb-2">Pantries</h3>
      <p class="font-body text-sm leading-relaxed" style="color:var(--text);opacity:0.65;">Categorized, labeled, and beautiful — your dream kitchen pantry awaits.</p>
    </div>
    <div class="service-card">
      <div class="w-14 h-14 rounded-2xl flex items-center justify-center mb-5" style="background:var(--cream);">
        <i class="fa-solid fa-car fa-lg" style="color:var(--rose)"></i>
      </div>
      <h3 class="font-heading text-2xl font-semibold mb-2">Garages</h3>
      <p class="font-body text-sm leading-relaxed" style="color:var(--text);opacity:0.65;">Reclaim your space with smart storage systems and clear zones.</p>
    </div>
    <div class="service-card">
      <div class="w-14 h-14 rounded-2xl flex items-center justify-center mb-5" style="background:var(--cream);">
        <i class="fa-solid fa-kitchen-set fa-lg" style="color:var(--rose)"></i>
      </div>
      <h3 class="font-heading text-2xl font-semibold mb-2">Kitchens</h3>
      <p class="font-body text-sm leading-relaxed" style="color:var(--text);opacity:0.65;">Streamlined workflows, clear countertops, and perfectly organized cabinets.</p>
    </div>
    <div class="service-card">
      <div class="w-14 h-14 rounded-2xl flex items-center justify-center mb-5" style="background:var(--cream);">
        <i class="fa-solid fa-bath fa-lg" style="color:var(--rose)"></i>
      </div>
      <h3 class="font-heading text-2xl font-semibold mb-2">Bathrooms</h3>
      <p class="font-body text-sm leading-relaxed" style="color:var(--text);opacity:0.65;">Spa-worthy organization for every product, towel, and toiletry.</p>
    </div>
    <div class="service-card">
      <div class="w-14 h-14 rounded-2xl flex items-center justify-center mb-5" style="background:var(--cream);">
        <i class="fa-solid fa-bed fa-lg" style="color:var(--rose)"></i>
      </div>
      <h3 class="font-heading text-2xl font-semibold mb-2">Bedrooms</h3>
      <p class="font-body text-sm leading-relaxed" style="color:var(--text);opacity:0.65;">Peaceful, restful spaces built around how you live and sleep.</p>
    </div>
    <div class="service-card sm:col-span-2 lg:col-span-1">
      <div class="w-14 h-14 rounded-2xl flex items-center justify-center mb-5" style="background:var(--cream);">
        <i class="fa-solid fa-star fa-lg" style="color:var(--gold)"></i>
      </div>
      <h3 class="font-heading text-2xl font-semibold mb-2">Unique Projects</h3>
      <p class="font-body text-sm leading-relaxed" style="color:var(--text);opacity:0.65;">Have something different in mind? We love a challenge — reach out and let's talk.</p>
    </div>
  </div>
  <div class="text-center mt-10">
    <a href="services.html" class="btn-outline">See All Services <i class="fa-solid fa-arrow-right ml-2"></i></a>
  </div>
</section>

<!-- GALLERY TEASER -->
<section style="background:#fff0f4;" class="py-20">
  <div class="max-w-6xl mx-auto px-6">
    <div class="text-center mb-12">
      <p class="font-body font-extrabold text-xs uppercase tracking-widest mb-3" style="color:var(--gold)">Before &amp; After</p>
      <h2 class="font-heading text-5xl font-semibold" style="color:var(--text)">Our Work</h2>
    </div>
    <div class="grid grid-cols-1 sm:grid-cols-3 gap-6">
      <div class="rounded-2xl overflow-hidden shadow-md" style="aspect-ratio:4/3;">
        <img src="https://placehold.co/600x450/ffb3c6/e8527a?text=Before+%26+After" alt="Organized closet" class="w-full h-full object-cover">
      </div>
      <div class="rounded-2xl overflow-hidden shadow-md" style="aspect-ratio:4/3;">
        <img src="https://placehold.co/600x450/ffcba4/e8527a?text=Before+%26+After" alt="Organized pantry" class="w-full h-full object-cover">
      </div>
      <div class="rounded-2xl overflow-hidden shadow-md" style="aspect-ratio:4/3;">
        <img src="https://placehold.co/600x450/ffd6e0/e8527a?text=Before+%26+After" alt="Organized garage" class="w-full h-full object-cover">
      </div>
    </div>
    <div class="text-center mt-10">
      <a href="gallery.html" class="btn-primary">View Full Gallery <i class="fa-solid fa-images ml-2"></i></a>
    </div>
  </div>
</section>

<!-- CTA BANNER -->
<section class="grad-bg py-20 relative overflow-hidden">
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at 20% 50%,rgba(255,255,255,0.22) 0%,transparent 55%);pointer-events:none;"></div>
  <div class="max-w-2xl mx-auto px-6 text-center relative">
    <i class="fa-solid fa-wand-magic-sparkles fa-2x text-white/80 mb-6"></i>
    <h2 class="font-display text-white text-5xl mb-4" style="text-shadow:0 2px 16px rgba(200,80,100,0.3);">Ready to Transform Your Space?</h2>
    <p class="font-body text-white/90 text-lg mb-2 leading-relaxed">A 30% deposit holds your project date. No pricing surprises — just beautiful results.</p>
    <p class="font-body text-white/70 text-sm mb-8">We'll respond within 24–48 hours.</p>
    <a href="contact.html" class="btn-primary" style="background:white;color:var(--rose);box-shadow:0 8px 32px rgba(0,0,0,0.15);">
      <i class="fa-solid fa-clipboard-list mr-2"></i>Request a Consultation
    </a>
  </div>
</section>

[FOOTER BLOCK — use Shared Footer Pattern]

</body>
</html>
```

- [ ] **Step 2: Start server and screenshot**

```bash
node serve.mjs &
node screenshot.mjs http://localhost:3000
```
Read the screenshot from `temporary screenshots/`. Verify: hero gradient visible and animating, 7 service cards in 3-column grid, gallery teaser shows 3 images, CTA banner visible.

- [ ] **Step 3: Commit**

```bash
rtk git add index.html && rtk git commit -m "feat: add home page"
```

---

## Task 3: Services Page
**Model:** `haiku`
**Files:** Create `services.html`
**Can run in parallel with Tasks 2, 4, 5, 6**

- [ ] **Step 1: Create services.html**

```html
<!DOCTYPE html>
<html lang="en">
[HEAD BLOCK — use Shared Head Pattern, title: "Services — Need a Reed Indeed"]
</head>
<body>

[NAV BLOCK — use Shared Nav Pattern]

<!-- PAGE HERO -->
<section class="grad-bg py-20 text-center relative overflow-hidden">
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at 50% 60%,rgba(255,255,255,0.22) 0%,transparent 55%);pointer-events:none;"></div>
  <div class="relative">
    <p class="font-body font-extrabold text-xs uppercase tracking-widest text-white/80 mb-3">What We Do</p>
    <h1 class="font-display text-white" style="font-size:clamp(2.4rem,6vw,4rem);text-shadow:0 2px 20px rgba(200,80,100,0.3);">Our Services</h1>
  </div>
</section>

<!-- SERVICE CARDS -->
<section class="max-w-6xl mx-auto px-6 py-20">
  <div class="grid grid-cols-1 md:grid-cols-2 gap-8">

    <!-- Closets -->
    <div class="card flex gap-6 p-8">
      <div class="w-16 h-16 rounded-2xl flex-shrink-0 flex items-center justify-center" style="background:var(--cream);">
        <i class="fa-solid fa-shirt fa-xl" style="color:var(--rose)"></i>
      </div>
      <div>
        <h2 class="font-heading text-2xl font-semibold mb-2">Closets</h2>
        <p class="font-body text-sm leading-relaxed mb-4" style="opacity:0.7;">We design and implement custom closet systems that maximize every inch. From wardrobe organization to shoe racks, accessories, and seasonal storage — your closet becomes a boutique.</p>
        <img src="https://placehold.co/480x240/ffb3c6/e8527a?text=Closet+Organizing" alt="Closet organizing" class="rounded-xl w-full object-cover" style="height:160px;">
      </div>
    </div>

    <!-- Pantries -->
    <div class="card flex gap-6 p-8">
      <div class="w-16 h-16 rounded-2xl flex-shrink-0 flex items-center justify-center" style="background:var(--cream);">
        <i class="fa-solid fa-jar fa-xl" style="color:var(--rose)"></i>
      </div>
      <div>
        <h2 class="font-heading text-2xl font-semibold mb-2">Pantries</h2>
        <p class="font-body text-sm leading-relaxed mb-4" style="opacity:0.7;">Categorized zones, matching containers, clear labels, and a layout that makes meal prep effortless. We make your pantry the most satisfying room in your kitchen.</p>
        <img src="https://placehold.co/480x240/ffcba4/e8527a?text=Pantry+Organizing" alt="Pantry organizing" class="rounded-xl w-full object-cover" style="height:160px;">
      </div>
    </div>

    <!-- Garages -->
    <div class="card flex gap-6 p-8">
      <div class="w-16 h-16 rounded-2xl flex-shrink-0 flex items-center justify-center" style="background:var(--cream);">
        <i class="fa-solid fa-car fa-xl" style="color:var(--rose)"></i>
      </div>
      <div>
        <h2 class="font-heading text-2xl font-semibold mb-2">Garages</h2>
        <p class="font-body text-sm leading-relaxed mb-4" style="opacity:0.7;">Clear zones for tools, sports gear, seasonal items, and your car. We turn overwhelming garages into functional, organized spaces you'll actually enjoy.</p>
        <img src="https://placehold.co/480x240/ffd6e0/e8527a?text=Garage+Organizing" alt="Garage organizing" class="rounded-xl w-full object-cover" style="height:160px;">
      </div>
    </div>

    <!-- Kitchens -->
    <div class="card flex gap-6 p-8">
      <div class="w-16 h-16 rounded-2xl flex-shrink-0 flex items-center justify-center" style="background:var(--cream);">
        <i class="fa-solid fa-kitchen-set fa-xl" style="color:var(--rose)"></i>
      </div>
      <div>
        <h2 class="font-heading text-2xl font-semibold mb-2">Kitchens</h2>
        <p class="font-body text-sm leading-relaxed mb-4" style="opacity:0.7;">From cabinet systems to drawer dividers and countertop clarity — we create kitchen workflows that make cooking a joy, not a chore.</p>
        <img src="https://placehold.co/480x240/ffab91/e8527a?text=Kitchen+Organizing" alt="Kitchen organizing" class="rounded-xl w-full object-cover" style="height:160px;">
      </div>
    </div>

    <!-- Bathrooms -->
    <div class="card flex gap-6 p-8">
      <div class="w-16 h-16 rounded-2xl flex-shrink-0 flex items-center justify-center" style="background:var(--cream);">
        <i class="fa-solid fa-bath fa-xl" style="color:var(--rose)"></i>
      </div>
      <div>
        <h2 class="font-heading text-2xl font-semibold mb-2">Bathrooms</h2>
        <p class="font-body text-sm leading-relaxed mb-4" style="opacity:0.7;">Spa-worthy organization for every product, towel, and toiletry. We bring calm and clarity to even the smallest bathroom.</p>
        <img src="https://placehold.co/480x240/ffb3c6/e8527a?text=Bathroom+Organizing" alt="Bathroom organizing" class="rounded-xl w-full object-cover" style="height:160px;">
      </div>
    </div>

    <!-- Bedrooms -->
    <div class="card flex gap-6 p-8">
      <div class="w-16 h-16 rounded-2xl flex-shrink-0 flex items-center justify-center" style="background:var(--cream);">
        <i class="fa-solid fa-bed fa-xl" style="color:var(--rose)"></i>
      </div>
      <div>
        <h2 class="font-heading text-2xl font-semibold mb-2">Bedrooms</h2>
        <p class="font-body text-sm leading-relaxed mb-4" style="opacity:0.7;">Peaceful, restful spaces where everything has a home. We organize furniture flow, nightstand surfaces, under-bed storage, and everything in between.</p>
        <img src="https://placehold.co/480x240/ffd6e0/e8527a?text=Bedroom+Organizing" alt="Bedroom organizing" class="rounded-xl w-full object-cover" style="height:160px;">
      </div>
    </div>

    <!-- Unique Projects — full width -->
    <div class="card flex gap-6 p-8 md:col-span-2">
      <div class="w-16 h-16 rounded-2xl flex-shrink-0 flex items-center justify-center" style="background:var(--cream);">
        <i class="fa-solid fa-star fa-xl" style="color:var(--gold)"></i>
      </div>
      <div class="flex-1">
        <h2 class="font-heading text-2xl font-semibold mb-2">Unique Projects</h2>
        <p class="font-body text-sm leading-relaxed" style="opacity:0.7;">Have a space that doesn't fit a category? A home office, a playroom, a utility room — we love a challenge. Reach out and we'll create a custom plan just for you.</p>
      </div>
    </div>

  </div>

  <!-- DEPOSIT CALLOUT -->
  <div class="mt-12 rounded-3xl p-8 text-center" style="background:linear-gradient(135deg,#fff0f4,#fff8f5);border:2px solid var(--blush);">
    <i class="fa-solid fa-circle-info fa-lg mb-3" style="color:var(--rose)"></i>
    <h3 class="font-heading text-2xl font-semibold mb-2">Booking & Pricing</h3>
    <p class="font-body text-sm leading-relaxed max-w-lg mx-auto" style="opacity:0.75;">A <strong>30% deposit</strong> is required to reserve your project date. Pricing is custom to your space — contact us for a quote. We'll never surprise you with hidden fees.</p>
    <a href="contact.html" class="btn-primary mt-6 inline-block">
      <i class="fa-solid fa-clipboard-list mr-2"></i>Request a Quote
    </a>
  </div>
</section>

[FOOTER BLOCK — use Shared Footer Pattern]

</body>
</html>
```

- [ ] **Step 2: Screenshot and verify**

```bash
node screenshot.mjs http://localhost:3000/services.html
```
Verify: 7 service cards render, deposit callout visible, no emojis.

- [ ] **Step 3: Commit**

```bash
rtk git add services.html && rtk git commit -m "feat: add services page"
```

---

## Task 4: Gallery Page
**Model:** `sonnet`
**Files:** Create `gallery.html`
**Can run in parallel with Tasks 2, 3, 5, 6**

- [ ] **Step 1: Create gallery.html**

```html
<!DOCTYPE html>
<html lang="en">
[HEAD BLOCK — use Shared Head Pattern, title: "Gallery — Need a Reed Indeed"]
<style>
  .filter-btn {
    font-family: 'Nunito', sans-serif; font-weight: 800; font-size: 0.78rem;
    letter-spacing: 0.06em; text-transform: uppercase;
    padding: 8px 20px; border-radius: 50px; border: 2px solid var(--blush);
    background: white; color: var(--text); cursor: pointer;
    transition: background 0.2s, color 0.2s, border-color 0.2s, transform 0.2s cubic-bezier(0.34,1.56,0.64,1);
  }
  .filter-btn:hover  { border-color: var(--rose); color: var(--rose); transform: translateY(-2px); }
  .filter-btn.active { background: var(--rose); border-color: var(--rose); color: white; }
  .filter-btn:focus-visible { outline: 3px solid var(--gold); outline-offset: 3px; }

  .gallery-grid {
    columns: 1; column-gap: 16px;
  }
  @media (min-width: 640px)  { .gallery-grid { columns: 2; } }
  @media (min-width: 1024px) { .gallery-grid { columns: 3; } }

  .gallery-item {
    break-inside: avoid; margin-bottom: 16px;
    position: relative; border-radius: 16px; overflow: hidden;
    cursor: pointer;
  }
  .gallery-item img { display: block; width: 100%; border-radius: 16px; }
  .gallery-overlay {
    position: absolute; inset: 0; border-radius: 16px;
    background: linear-gradient(to top, rgba(232,82,122,0.75) 0%, transparent 50%);
    opacity: 0; transition: opacity 0.3s;
    display: flex; align-items: flex-end; padding: 16px;
  }
  .gallery-item:hover .gallery-overlay { opacity: 1; }
  .gallery-overlay span {
    font-family: 'Nunito', sans-serif; font-weight: 800; font-size: 0.8rem;
    letter-spacing: 0.08em; text-transform: uppercase; color: white;
  }
</style>
</head>
<body>

[NAV BLOCK — use Shared Nav Pattern]

<!-- PAGE HERO -->
<section class="grad-bg py-20 text-center relative overflow-hidden">
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at 50% 60%,rgba(255,255,255,0.22) 0%,transparent 55%);pointer-events:none;"></div>
  <div class="relative">
    <p class="font-body font-extrabold text-xs uppercase tracking-widest text-white/80 mb-3">Before &amp; After</p>
    <h1 class="font-display text-white" style="font-size:clamp(2.4rem,6vw,4rem);text-shadow:0 2px 20px rgba(200,80,100,0.3);">Our Work</h1>
  </div>
</section>

<section class="max-w-6xl mx-auto px-6 py-16">
  <!-- FILTER BAR -->
  <div class="flex flex-wrap gap-3 justify-center mb-12" id="filter-bar">
    <button class="filter-btn active" data-filter="all"      onclick="filterGallery('all')">All</button>
    <button class="filter-btn"        data-filter="closets"  onclick="filterGallery('closets')">Closets</button>
    <button class="filter-btn"        data-filter="pantries" onclick="filterGallery('pantries')">Pantries</button>
    <button class="filter-btn"        data-filter="garages"  onclick="filterGallery('garages')">Garages</button>
    <button class="filter-btn"        data-filter="kitchens" onclick="filterGallery('kitchens')">Kitchens</button>
    <button class="filter-btn"        data-filter="bathrooms" onclick="filterGallery('bathrooms')">Bathrooms</button>
    <button class="filter-btn"        data-filter="bedrooms" onclick="filterGallery('bedrooms')">Bedrooms</button>
  </div>

  <!-- GALLERY GRID -->
  <div class="gallery-grid" id="gallery-grid">
    <div class="gallery-item" data-type="closets">
      <img src="https://placehold.co/600x750/ffb3c6/e8527a?text=Closet+After" alt="Organized closet">
      <div class="gallery-overlay"><span><i class="fa-solid fa-shirt mr-2"></i>Closet</span></div>
    </div>
    <div class="gallery-item" data-type="pantries">
      <img src="https://placehold.co/600x500/ffcba4/e8527a?text=Pantry+After" alt="Organized pantry">
      <div class="gallery-overlay"><span><i class="fa-solid fa-jar mr-2"></i>Pantry</span></div>
    </div>
    <div class="gallery-item" data-type="garages">
      <img src="https://placehold.co/600x600/ffd6e0/e8527a?text=Garage+After" alt="Organized garage">
      <div class="gallery-overlay"><span><i class="fa-solid fa-car mr-2"></i>Garage</span></div>
    </div>
    <div class="gallery-item" data-type="kitchens">
      <img src="https://placehold.co/600x480/ffab91/e8527a?text=Kitchen+After" alt="Organized kitchen">
      <div class="gallery-overlay"><span><i class="fa-solid fa-kitchen-set mr-2"></i>Kitchen</span></div>
    </div>
    <div class="gallery-item" data-type="bedrooms">
      <img src="https://placehold.co/600x700/ffd6e0/e8527a?text=Bedroom+After" alt="Organized bedroom">
      <div class="gallery-overlay"><span><i class="fa-solid fa-bed mr-2"></i>Bedroom</span></div>
    </div>
    <div class="gallery-item" data-type="bathrooms">
      <img src="https://placehold.co/600x550/ffb3c6/e8527a?text=Bathroom+After" alt="Organized bathroom">
      <div class="gallery-overlay"><span><i class="fa-solid fa-bath mr-2"></i>Bathroom</span></div>
    </div>
    <div class="gallery-item" data-type="closets">
      <img src="https://placehold.co/600x600/ffcba4/e8527a?text=Closet+Before" alt="Closet before">
      <div class="gallery-overlay"><span><i class="fa-solid fa-shirt mr-2"></i>Closet</span></div>
    </div>
    <div class="gallery-item" data-type="pantries">
      <img src="https://placehold.co/600x720/ffd6e0/e8527a?text=Pantry+Before" alt="Pantry before">
      <div class="gallery-overlay"><span><i class="fa-solid fa-jar mr-2"></i>Pantry</span></div>
    </div>
    <div class="gallery-item" data-type="garages">
      <img src="https://placehold.co/600x500/ffab91/e8527a?text=Garage+Before" alt="Garage before">
      <div class="gallery-overlay"><span><i class="fa-solid fa-car mr-2"></i>Garage</span></div>
    </div>
  </div>

  <p class="text-center font-body text-sm mt-10" style="opacity:0.5;">
    <i class="fa-solid fa-camera mr-1"></i>Real client photos coming soon — these are placeholders.
  </p>
</section>

[FOOTER BLOCK — use Shared Footer Pattern]

<script>
function filterGallery(type) {
  document.querySelectorAll('.filter-btn').forEach(btn => {
    btn.classList.toggle('active', btn.dataset.filter === type);
  });
  document.querySelectorAll('#gallery-grid .gallery-item').forEach(item => {
    const show = type === 'all' || item.dataset.type === type;
    item.style.display = show ? 'block' : 'none';
  });
}
</script>

</body>
</html>
```

- [ ] **Step 2: Screenshot and verify filter works**

```bash
node screenshot.mjs http://localhost:3000/gallery.html
```
Open in browser, click filter buttons, verify correct items show/hide. Verify hover overlay appears on photos.

- [ ] **Step 3: Commit**

```bash
rtk git add gallery.html && rtk git commit -m "feat: add gallery page with filter"
```

---

## Task 5: About Page
**Model:** `haiku`
**Files:** Create `about.html`
**Can run in parallel with Tasks 2, 3, 4, 6**

- [ ] **Step 1: Create about.html**

```html
<!DOCTYPE html>
<html lang="en">
[HEAD BLOCK — use Shared Head Pattern, title: "About — Need a Reed Indeed"]
</head>
<body>

[NAV BLOCK — use Shared Nav Pattern]

<!-- PAGE HERO -->
<section class="grad-bg py-20 text-center relative overflow-hidden">
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at 50% 60%,rgba(255,255,255,0.22) 0%,transparent 55%);pointer-events:none;"></div>
  <div class="relative">
    <p class="font-body font-extrabold text-xs uppercase tracking-widest text-white/80 mb-3">Our Story</p>
    <h1 class="font-display text-white" style="font-size:clamp(2.4rem,6vw,4rem);text-shadow:0 2px 20px rgba(200,80,100,0.3);">About Us</h1>
  </div>
</section>

<!-- OUR STORY -->
<section class="max-w-5xl mx-auto px-6 py-20">
  <div class="grid md:grid-cols-2 gap-16 items-center">
    <div>
      <p class="font-body font-extrabold text-xs uppercase tracking-widest mb-4" style="color:var(--gold)">How We Started</p>
      <h2 class="font-heading text-4xl font-semibold mb-6" style="line-height:1.2;">Meet the Reeds</h2>
      <div class="font-body text-base leading-relaxed space-y-4" style="opacity:0.75;">
        <p>
          Need a Reed Indeed was born out of a simple belief: everyone deserves a home that feels calm, beautiful, and completely their own. We started organizing spaces for friends and family and quickly realized how powerful a well-organized home could be — not just practically, but emotionally.
        </p>
        <p>
          What began as a passion project has grown into a full-service organizing business dedicated to transforming garages, pantries, closets, and every space in between. We bring both heart and expertise to every project.
        </p>
        <p>
          <em>Replace this section with your personal story!</em>
        </p>
      </div>
    </div>
    <div class="rounded-3xl overflow-hidden shadow-xl" style="aspect-ratio:4/5;">
      <img src="https://placehold.co/500x625/ffb3c6/e8527a?text=The+Reed+Team" alt="The Need a Reed Indeed team" class="w-full h-full object-cover">
    </div>
  </div>
</section>

<!-- MISSION PILLARS -->
<section style="background:#fff0f4;" class="py-20">
  <div class="max-w-5xl mx-auto px-6">
    <div class="text-center mb-14">
      <p class="font-body font-extrabold text-xs uppercase tracking-widest mb-3" style="color:var(--gold)">Our Values</p>
      <h2 class="font-heading text-4xl font-semibold">What Sets Us Apart</h2>
    </div>
    <div class="grid md:grid-cols-3 gap-8">
      <div class="card p-8 text-center">
        <div class="w-16 h-16 rounded-full flex items-center justify-center mx-auto mb-6" style="background:var(--cream);">
          <i class="fa-solid fa-heart fa-xl" style="color:var(--rose)"></i>
        </div>
        <h3 class="font-heading text-2xl font-semibold mb-3">Personalized</h3>
        <p class="font-body text-sm leading-relaxed" style="opacity:0.7;">Every home is different. We listen to how you live and design a system that works for you — not a generic template.</p>
      </div>
      <div class="card p-8 text-center">
        <div class="w-16 h-16 rounded-full flex items-center justify-center mx-auto mb-6" style="background:var(--cream);">
          <i class="fa-solid fa-shield-halved fa-xl" style="color:var(--rose)"></i>
        </div>
        <h3 class="font-heading text-2xl font-semibold mb-3">Reliable</h3>
        <p class="font-body text-sm leading-relaxed" style="opacity:0.7;">We show up on time, communicate clearly, and deliver exactly what we promise. Your trust means everything to us.</p>
      </div>
      <div class="card p-8 text-center">
        <div class="w-16 h-16 rounded-full flex items-center justify-center mx-auto mb-6" style="background:var(--cream);">
          <i class="fa-solid fa-wand-magic-sparkles fa-xl" style="color:var(--rose)"></i>
        </div>
        <h3 class="font-heading text-2xl font-semibold mb-3">Transformative</h3>
        <p class="font-body text-sm leading-relaxed" style="opacity:0.7;">We don't just tidy — we transform. The results are spaces you'll love coming home to, every single day.</p>
      </div>
    </div>
  </div>
</section>

<!-- CTA -->
<section class="py-20 text-center">
  <div class="max-w-xl mx-auto px-6">
    <i class="fa-solid fa-handshake fa-2x mb-6" style="color:var(--rose)"></i>
    <h2 class="font-heading text-4xl font-semibold mb-4">Let's Work Together</h2>
    <p class="font-body text-base leading-relaxed mb-8" style="opacity:0.7;">Ready to transform your space? We'd love to hear about your project. A 30% deposit holds your date.</p>
    <a href="contact.html" class="btn-primary">
      <i class="fa-solid fa-paper-plane mr-2"></i>Get in Touch
    </a>
  </div>
</section>

[FOOTER BLOCK — use Shared Footer Pattern]

</body>
</html>
```

- [ ] **Step 2: Screenshot and verify**

```bash
node screenshot.mjs http://localhost:3000/about.html
```
Verify: story section, team photo placeholder, 3 mission pillar cards, CTA at bottom. No emojis.

- [ ] **Step 3: Commit**

```bash
rtk git add about.html && rtk git commit -m "feat: add about page"
```

---

## Task 6: Contact Page
**Model:** `sonnet`
**Files:** Create `contact.html`
**Can run in parallel with Tasks 2, 3, 4, 5**

- [ ] **Step 1: Create contact.html**

> **Before going live:** Create a free account at formspree.io, create a form, and add both emails as notification recipients. Replace `YOUR_FORMSPREE_ENDPOINT` with your actual endpoint (e.g., `https://formspree.io/f/xyzxyzxyz`).

```html
<!DOCTYPE html>
<html lang="en">
[HEAD BLOCK — use Shared Head Pattern, title: "Contact — Need a Reed Indeed"]
<style>
  .form-field {
    width: 100%; font-family: 'Nunito', sans-serif; font-size: 0.95rem;
    padding: 14px 18px; border-radius: 14px; border: 2px solid var(--blush);
    background: white; color: var(--text); outline: none;
    transition: border-color 0.2s, box-shadow 0.2s;
  }
  .form-field:focus { border-color: var(--rose); box-shadow: 0 0 0 4px rgba(232,82,122,0.1); }
  .form-label {
    display: block; font-family: 'Nunito', sans-serif; font-weight: 800;
    font-size: 0.75rem; letter-spacing: 0.08em; text-transform: uppercase;
    color: var(--rose); margin-bottom: 6px;
  }
</style>
</head>
<body>

[NAV BLOCK — use Shared Nav Pattern]

<!-- PAGE HERO -->
<section class="grad-bg py-20 text-center relative overflow-hidden">
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at 50% 60%,rgba(255,255,255,0.22) 0%,transparent 55%);pointer-events:none;"></div>
  <div class="relative">
    <p class="font-body font-extrabold text-xs uppercase tracking-widest text-white/80 mb-3">Let's Connect</p>
    <h1 class="font-display text-white" style="font-size:clamp(2.4rem,6vw,4rem);text-shadow:0 2px 20px rgba(200,80,100,0.3);">Get in Touch</h1>
  </div>
</section>

<section class="max-w-5xl mx-auto px-6 py-20 grid md:grid-cols-2 gap-16 items-start">

  <!-- FORM -->
  <div>
    <h2 class="font-heading text-3xl font-semibold mb-2">Request a Consultation</h2>
    <p class="font-body text-sm leading-relaxed mb-8" style="opacity:0.65;">Tell us about your space and we'll get back to you within 24–48 hours with next steps.</p>

    <!-- Success message (hidden by default) -->
    <div id="form-success" class="hidden rounded-2xl p-6 mb-6 text-center" style="background:#f0fff4;border:2px solid #86efac;">
      <i class="fa-solid fa-circle-check fa-xl mb-3" style="color:#22c55e;"></i>
      <h3 class="font-heading text-xl font-semibold mb-1">Message Sent!</h3>
      <p class="font-body text-sm" style="opacity:0.7;">Thanks for reaching out — we'll be in touch within 24–48 hours.</p>
    </div>

    <!-- Error message (hidden by default) -->
    <div id="form-error" class="hidden rounded-2xl p-6 mb-6 text-center" style="background:#fff1f2;border:2px solid #fda4af;">
      <i class="fa-solid fa-circle-exclamation fa-xl mb-3" style="color:#f43f5e;"></i>
      <h3 class="font-heading text-xl font-semibold mb-1">Something went wrong</h3>
      <p class="font-body text-sm" style="opacity:0.7;">Please try again or email us directly at reeddesmond22@gmail.com.</p>
    </div>

    <form id="inquiry-form" action="YOUR_FORMSPREE_ENDPOINT" method="POST" class="space-y-5">
      <div>
        <label for="name" class="form-label"><i class="fa-solid fa-user mr-1"></i>Your Name</label>
        <input id="name" name="name" type="text" required placeholder="Jane Smith" class="form-field">
      </div>
      <div>
        <label for="phone" class="form-label"><i class="fa-solid fa-phone mr-1"></i>Phone Number</label>
        <input id="phone" name="phone" type="tel" placeholder="(555) 123-4567" class="form-field">
      </div>
      <div>
        <label for="email" class="form-label"><i class="fa-solid fa-envelope mr-1"></i>Email Address</label>
        <input id="email" name="email" type="email" required placeholder="jane@example.com" class="form-field">
      </div>
      <div>
        <label for="space" class="form-label"><i class="fa-solid fa-home mr-1"></i>Space Type</label>
        <select id="space" name="space_type" class="form-field" style="cursor:pointer;">
          <option value="" disabled selected>Select a space…</option>
          <option value="Closet">Closet</option>
          <option value="Pantry">Pantry</option>
          <option value="Garage">Garage</option>
          <option value="Kitchen">Kitchen</option>
          <option value="Bathroom">Bathroom</option>
          <option value="Bedroom">Bedroom</option>
          <option value="Unique Project">Unique Project</option>
        </select>
      </div>
      <div>
        <label for="message" class="form-label"><i class="fa-solid fa-comment mr-1"></i>Project Description</label>
        <textarea id="message" name="message" rows="4" required placeholder="Tell us about your space and what you're hoping to achieve…" class="form-field" style="resize:vertical;"></textarea>
      </div>
      <input type="hidden" name="_subject" value="New Organizing Inquiry — Need a Reed Indeed">
      <button type="submit" id="submit-btn" class="btn-primary w-full justify-center" style="width:100%;">
        <i class="fa-solid fa-paper-plane mr-2"></i>Send Inquiry
      </button>
    </form>
  </div>

  <!-- CONTACT INFO -->
  <div class="space-y-8">
    <div class="card p-8">
      <h3 class="font-heading text-2xl font-semibold mb-6">Contact Details</h3>
      <div class="space-y-5">
        <a href="tel:3232120384" class="flex items-center gap-4 group">
          <div class="w-12 h-12 rounded-xl flex items-center justify-center flex-shrink-0" style="background:var(--cream);">
            <i class="fa-solid fa-phone" style="color:var(--rose)"></i>
          </div>
          <div>
            <div class="font-body font-extrabold text-xs uppercase tracking-widest mb-1" style="color:var(--gold)">Phone</div>
            <div class="font-body font-semibold group-hover:text-rose-500 transition-colors">(323) 212-0384</div>
          </div>
        </a>
        <a href="mailto:reeddesmond22@gmail.com" class="flex items-center gap-4 group">
          <div class="w-12 h-12 rounded-xl flex items-center justify-center flex-shrink-0" style="background:var(--cream);">
            <i class="fa-solid fa-envelope" style="color:var(--rose)"></i>
          </div>
          <div>
            <div class="font-body font-extrabold text-xs uppercase tracking-widest mb-1" style="color:var(--gold)">Email</div>
            <div class="font-body font-semibold group-hover:text-rose-500 transition-colors">reeddesmond22@gmail.com</div>
          </div>
        </a>
        <a href="mailto:ebonyb540@gmail.com" class="flex items-center gap-4 group">
          <div class="w-12 h-12 rounded-xl flex items-center justify-center flex-shrink-0" style="background:var(--cream);">
            <i class="fa-solid fa-envelope" style="color:var(--rose)"></i>
          </div>
          <div>
            <div class="font-body font-extrabold text-xs uppercase tracking-widest mb-1" style="color:var(--gold)">Email</div>
            <div class="font-body font-semibold group-hover:text-rose-500 transition-colors">ebonyb540@gmail.com</div>
          </div>
        </a>
      </div>
    </div>

    <div class="rounded-3xl p-8" style="background:linear-gradient(135deg,#fff0f4,#fff8f5);border:2px solid var(--blush);">
      <i class="fa-solid fa-circle-info mb-3" style="color:var(--rose)"></i>
      <h3 class="font-heading text-xl font-semibold mb-2">About Your Deposit</h3>
      <p class="font-body text-sm leading-relaxed" style="opacity:0.75;">A <strong>30% deposit</strong> is required to hold your project date. We'll send deposit details after reviewing your inquiry. Pricing is customized to your space.</p>
    </div>

    <div class="card p-8">
      <i class="fa-solid fa-clock mb-3" style="color:var(--gold)"></i>
      <h3 class="font-heading text-xl font-semibold mb-2">Response Time</h3>
      <p class="font-body text-sm leading-relaxed" style="opacity:0.75;">We typically respond within <strong>24–48 hours</strong>. For faster response, give us a call at (323) 212-0384!</p>
    </div>
  </div>
</section>

[FOOTER BLOCK — use Shared Footer Pattern]

<script>
  const form = document.getElementById('inquiry-form');
  const btn  = document.getElementById('submit-btn');

  form.addEventListener('submit', async (e) => {
    e.preventDefault();
    btn.disabled = true;
    btn.innerHTML = '<i class="fa-solid fa-spinner fa-spin mr-2"></i>Sending…';

    try {
      const res = await fetch(form.action, {
        method: 'POST',
        body: new FormData(form),
        headers: { Accept: 'application/json' }
      });
      if (res.ok) {
        document.getElementById('form-success').classList.remove('hidden');
        document.getElementById('form-error').classList.add('hidden');
        form.reset();
        form.classList.add('hidden');
      } else {
        throw new Error('Non-OK response');
      }
    } catch {
      document.getElementById('form-error').classList.remove('hidden');
      btn.disabled = false;
      btn.innerHTML = '<i class="fa-solid fa-paper-plane mr-2"></i>Send Inquiry';
    }
  });
</script>

</body>
</html>
```

- [ ] **Step 2: Screenshot and verify form renders**

```bash
node screenshot.mjs http://localhost:3000/contact.html
```
Verify: form fields all present, contact details visible, deposit note visible, no emojis.

- [ ] **Step 3: Commit**

```bash
rtk git add contact.html && rtk git commit -m "feat: add contact page with Formspree form"
```

---

## Task 7: Integration & Polish
**Model:** `sonnet`
**Runs after Tasks 2–6 complete**

- [ ] **Step 1: Screenshot all 5 pages and check for issues**

```bash
node screenshot.mjs http://localhost:3000 home
node screenshot.mjs http://localhost:3000/services.html services
node screenshot.mjs http://localhost:3000/gallery.html gallery
node screenshot.mjs http://localhost:3000/about.html about
node screenshot.mjs http://localhost:3000/contact.html contact
```
Read each screenshot. Check: gradient animating, FontAwesome icons loading, nav present, footer present, no broken layout.

- [ ] **Step 2: Verify mobile nav on home page**

Open http://localhost:3000 in browser at a narrow viewport (mobile). Verify hamburger icon appears, tapping it opens the mobile menu, links are tappable.

- [ ] **Step 3: Verify gallery filter**

Open http://localhost:3000/gallery.html. Click each filter button. Verify only matching cards show. Click "All" to restore full grid.

- [ ] **Step 4: Test contact form (after Formspree endpoint is set)**

Open http://localhost:3000/contact.html. Fill in all fields. Submit. Verify:
- Button shows spinner during submission
- Success message appears after submit
- Form hides after success
- Email arrives at reeddesmond22@gmail.com and ebonyb540@gmail.com

If Formspree endpoint not yet configured, verify the form UI only (spinner, error state with a bad URL).

- [ ] **Step 5: Run verification checklist from spec**

Check each item in `docs/superpowers/specs/2026-04-24-need-a-reed-indeed-design.md` verification section. Fix any outstanding issues.

- [ ] **Step 6: Final commit**

```bash
rtk git add -A && rtk git commit -m "feat: complete Need a Reed Indeed website"
```

---

## Post-Build: Formspree Setup

1. Go to [formspree.io](https://formspree.io) and create a free account
2. Click **New Form** — give it the name "Need a Reed Indeed Inquiry"
3. In form settings, add notification emails: `reeddesmond22@gmail.com` and `ebonyb540@gmail.com`
4. Copy the form endpoint URL (e.g. `https://formspree.io/f/xxxxxxxx`)
5. In `contact.html`, replace `YOUR_FORMSPREE_ENDPOINT` with your endpoint
6. Test a live submission and confirm both emails receive it
