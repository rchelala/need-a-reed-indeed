# Need a Reed Indeed — Website Design Spec
_Created: 2026-04-24_

## Business Overview

- **Business name:** Need a Reed Indeed
- **Service:** Professional home organizing
- **Spaces served:** Closets, Pantries, Garages, Kitchens, Bathrooms, Bedrooms, Unique Projects
- **Deposit policy:** 30% deposit required to hold project date
- **Phone:** (323) 212-0384
- **Emails:** reeddesmond22@gmail.com · ebonyb540@gmail.com

---

## Goals & Primary CTA

The website's main job is to generate inquiries. Every page drives visitors toward the **contact/inquiry form**. No pricing is shown — the message is "contact us for a custom quote."

---

## Site Architecture

Five pages, classic service-site layout:

| Page | URL | Purpose |
|------|-----|---------|
| Home | index.html | Hero + services grid + gallery teaser + CTA banner |
| Services | services.html | Full breakdown of all 7 space types |
| Gallery | gallery.html | Filterable before/after photo grid |
| About | about.html | Personal story + mission/values |
| Contact | contact.html | Formspree inquiry form + contact info |

**Navigation:** Sticky top nav — logo left, page links right. Hamburger menu on mobile. Footer repeats key links + both email addresses + phone.

---

## Visual Design

### Aesthetic Direction
Fun & Playful — warm pinks, peachy corals, and gold accents. Bubbly and inviting. The signature element is a slow animated gradient background that flows between all brand colors. Professional but unmistakably feminine.

### Color Palette
| Role | Color | Hex |
|------|-------|-----|
| Primary | Deep Rose | `#e8527a` |
| Hot Pink | Hot Pink | `#ff8fab` |
| Blush | Blush Pink | `#ffb3c6` |
| Coral | Coral | `#ff8c69` |
| Peach | Peach | `#ffab91` |
| Accent | Gold | `#f4c430` |
| Background | Cream | `#fff8f5` |
| Text | Dark Rose-Brown | `#3d2a2a` |

### Typography
| Role | Font | Source |
|------|------|--------|
| Logo / Display | Dancing Script 700 | Google Fonts |
| Section Headings | Cormorant Garamond 600 | Google Fonts |
| Body / UI | Nunito 400/700/800 | Google Fonts |

- Headings: tight tracking (`-0.02em`), large size
- Body: generous line-height (`1.7`)
- Labels/tags: uppercase, wide tracking (`0.1em`), Nunito 800

### Icons
FontAwesome Free (CDN) — used throughout for service icons, nav, UI elements. No emojis.

---

## Animated Gradient Background

```css
background: linear-gradient(-45deg, #ff8fab, #ffb3c6, #ffcba4, #ffd700, #ff6b9d, #ffab91);
background-size: 400% 400%;
animation: gradientShift 20s ease infinite;

@keyframes gradientShift {
  0%   { background-position: 0% 50%; }
  50%  { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
```

Used on: hero sections across all pages, CTA banners, page header strips. Pure CSS, no JS.

---

## Page-by-Page Content

### Home (index.html)
1. **Hero** — Full-width animated gradient · Logo in Dancing Script · Tagline: "Your Space, Beautifully Organized" · Primary CTA button "Get Organized Today"
2. **Services Grid** — 7 cards (Closets, Pantries, Garages, Kitchens, Bathrooms, Bedrooms, Unique Projects) · FontAwesome icon + name + 1-line description each
3. **Gallery Teaser** — 3 placeholder before/after images in a row · "See Our Work" link to gallery.html
4. **CTA Banner** — Animated gradient banner: "Ready to transform your space? A 30% deposit holds your date." + "Request a Consultation" button

### Services (services.html)
1. **Page Hero** — Short animated gradient banner with title "Our Services"
2. **Service Cards** — One card per space type (7 total) · FontAwesome icon + name + description + placeholder image
3. **Deposit Callout** — Highlighted box: "A 30% deposit is required to reserve your project date. Contact us for pricing."
4. **CTA** — "Ready to get started? Fill out our inquiry form →"

### Gallery (gallery.html)
1. **Filter Bar** — Pill buttons: All · Closets · Pantries · Garages · Kitchens · Bathrooms · Bedrooms
2. **Photo Grid** — CSS columns masonry grid (no JS library) · placeholder images (placehold.co) · hover reveals soft pink overlay with space type label
3. **Structure** — Built for drop-in real photos with no layout changes

### About (about.html)
1. **Our Story** — Personal narrative section · team photo placeholder · owner copy goes here
2. **Mission Pillars** — 3 value cards (e.g., Personalized, Reliable, Transformative) · FontAwesome icon + statement
3. **CTA** — "Let's work together" → links to contact.html

### Contact (contact.html)
1. **Inquiry Form** — Formspree-powered (see below) · Fields: Name, Phone, Email, Space Type (dropdown), Project Description · Success/error inline states
2. **Contact Info** — Phone: (323) 212-0384 · reeddesmond22@gmail.com · ebonyb540@gmail.com
3. **Deposit Note** — "A 30% deposit is required to hold your project date."
4. **Response Time** — "We typically respond within 24–48 hours."

---

## Contact Form — Formspree

- **Provider:** Formspree (formspree.io) — free tier, 50 submissions/month
- **Setup:** Create account → create form → paste endpoint URL into HTML `action` attribute
- **Submissions sent to:** reeddesmond22@gmail.com and ebonyb540@gmail.com
- **Fields:** Name · Phone · Email · Space Type (dropdown) · Project Description
- **No backend required** — pure HTML form, Formspree handles delivery
- **Success/error states** — small inline JS snippet shows a thank-you message on submit; Formspree handles the actual email delivery

---

## Animations & Interactions

| Element | Animation |
|---------|-----------|
| Background gradient | CSS keyframes, 20s loop, `ease infinite` |
| Hero text / CTA | Staggered fade-in on page load (`animation-delay`) |
| Service cards | Lift on hover (`box-shadow` + `translateY`) |
| Buttons | Slight scale on hover + active press |
| Nav links | Pink underline sweep on hover |
| Gallery photos | Soft pink overlay on hover |

**Rules:**
- Only animate `transform` and `opacity`
- Never use `transition-all`
- Spring-style easing (`cubic-bezier(0.34, 1.56, 0.64, 1)`) on interactive elements

---

## Technical Spec

| Item | Decision |
|------|----------|
| Format | 5 plain HTML files — no build tools, no framework |
| Styling | Tailwind CSS via CDN + custom CSS for animations |
| Fonts | Google Fonts CDN |
| Icons | FontAwesome Free CDN |
| Images | placehold.co now, real photos later (drop-in, no layout changes) |
| Form | Formspree (HTML action attribute, no JS required) |
| Hosting | Any static host — Netlify, GitHub Pages, etc. |
| Responsive | Mobile-first · hamburger nav · 1→2→3 column grids |

### Deliverables
```
index.html
services.html
gallery.html
about.html
contact.html
```

---

## Responsive Breakpoints

| Breakpoint | Services Grid | Gallery Grid | Nav |
|-----------|--------------|--------------|-----|
| Mobile (<768px) | 1 column | 2 columns | Hamburger |
| Tablet (768–1024px) | 2 columns | 2 columns | Hamburger |
| Desktop (>1024px) | 3 columns | 3 columns | Full horizontal |

---

## Verification Checklist

- [ ] Animated gradient runs on all page heroes at 20s speed
- [ ] All icons are FontAwesome — no emojis anywhere
- [ ] Contact form submits successfully via Formspree → email received at both addresses
- [ ] Space Type dropdown includes all 7 options
- [ ] 30% deposit mention appears on: Home CTA banner, Services page callout, Contact page
- [ ] Both email addresses and phone number appear on Contact page and footer
- [ ] Placeholder images on Gallery and Services pages render correctly
- [ ] Gallery filter buttons correctly show/hide cards by space type
- [ ] Mobile nav hamburger opens/closes correctly
- [ ] All buttons have hover, focus-visible, and active states
- [ ] No `transition-all` used anywhere
- [ ] Site serves correctly on localhost:3000 via `node serve.mjs`
