---
name: tailwind-patterns
description: Technical utilities only. NO component templates — build every component from scratch based on the brand.
---

# Tailwind Technical Utilities

This file contains ONLY technical recipes (animations, effects, boilerplate). 
It does NOT contain component templates. Every nav, hero, card, section, and layout must be built from scratch for each competitor. Copying a "hero template" or "card template" is what makes every site look the same.

## HTML Boilerplate

```html
<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title><!-- competitor name --></title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <!-- Google Fonts — pick a pairing from design-agent.md Font Pairing Library -->
  <link href="https://fonts.googleapis.com/css2?family=HEADING_FONT:wght@400;600;700;800&family=BODY_FONT:wght@400;500;700&display=swap" rel="stylesheet">
  <!-- FlyonUI CSS (semantic component classes: card, btn, navbar, stat, mockups, etc.) -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/flyonui@2.4.1/flyonui.css">
  <!-- Tailwind CSS (utility classes for custom styling) -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
  tailwind.config = {
    theme: {
      extend: {
        colors: { /* per competitor */ },
        fontFamily: {
          display: ['HEADING_FONT', 'sans-serif'],
          body: ['BODY_FONT', 'sans-serif'],
        }
      }
    }
  }
  </script>
  <style>/* Custom keyframes here */</style>
</head>
<body class="font-body">
  <!-- Use font-display on headings, font-body is the default via body class -->
  <!-- FlyonUI JS (for interactive components: accordion, dropdown, modal, etc.) -->
  <script src="https://cdn.jsdelivr.net/npm/flyonui@2.4.1/flyonui.js"></script>
  <script>/* Interactions here */</script>
</body>
</html>
```

**Theme:** Set `data-theme="dark"` on `<html>` for dark competitors, `data-theme="light"` for light ones. FlyonUI handles base colors automatically. Override with Tailwind utilities for brand-specific colors.

**Browse all FlyonUI components:** https://flyonui.com/docs/component/

## Scroll-Triggered Animations

```js
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('animate-visible');
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.1, rootMargin: '0px 0px -50px 0px' });
document.querySelectorAll('[data-animate]').forEach(el => observer.observe(el));
```

```css
[data-animate] {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
[data-animate].animate-visible {
  opacity: 1;
  transform: translateY(0);
}
```

## Background Effect Recipes

Use only if the brand calls for it. Most sites need at most ONE of these.

### Gradient mesh
```css
.mesh::before {
  content: '';
  position: absolute;
  top: -50%; left: -50%;
  width: 200%; height: 200%;
  background:
    radial-gradient(ellipse at 20% 50%, var(--glow-1) 0%, transparent 50%),
    radial-gradient(ellipse at 80% 20%, var(--glow-2) 0%, transparent 40%);
  animation: drift 20s ease-in-out infinite alternate;
}
@keyframes drift {
  0% { transform: translate(0,0) rotate(0deg); }
  100% { transform: translate(-3%,-2%) rotate(2deg); }
}
```

### Dot grid
```css
background-image: radial-gradient(circle, currentColor 1px, transparent 1px);
background-size: 24px 24px;
opacity: 0.06;
```

### Noise grain
```css
.grain::after {
  content: '';
  position: fixed; inset: 0;
  pointer-events: none; opacity: 0.03; z-index: 9999;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
```

## Typography — Font Loading

Load fonts via Google Fonts `<link>` tags. Register in Tailwind config as `display` (headings) and `body` (text). Every competitor gets a different pairing — see the Font Pairing Library in `design-agent.md` for the full menu of 16 pairings.

```html
<!-- Example: loading + registering a font pairing -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700;800&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">

<script>
tailwind.config = {
  theme: {
    extend: {
      fontFamily: {
        display: ['Sora', 'sans-serif'],
        body: ['DM Sans', 'sans-serif'],
      },
      colors: { /* per competitor */ }
    }
  }
}
</script>
```

**Usage in HTML:**
```html
<h1 class="font-display text-5xl font-bold">Heading in display font</h1>
<p class="font-body text-lg">Body text in body font</p>
```

**Quick reference — popular pairing families (see design-agent.md for all 16):**
- Tech: Sora + Inter, Outfit + IBM Plex Mono, Figtree + Space Mono
- Premium: Playfair Display + DM Sans, Cormorant Garamond + Archivo
- Bold: Red Hat Display + Space Grotesk, Unbounded + Work Sans
- Friendly: Nunito + Source Sans 3, Fredoka + Lexend
- Editorial: Libre Baskerville + Karla, Bitter + Raleway
- Corporate: Plus Jakarta Sans + Manrope, IBM Plex Sans + IBM Plex Mono
- Modern: Instrument Sans + Albert Sans, Bricolage Grotesque + DM Sans

## Icon Libraries

Load only if the design calls for icons. Not every site needs them.

- **Lucide**: `<script src="https://unpkg.com/lucide@latest"></script>` + `lucide.createIcons();`
- **Phosphor**: `<script src="https://unpkg.com/@phosphor-icons/web"></script>`
- **Inline SVG**: Often better — draw simple shapes yourself for maximum brand fit.
- **No icons**: Sometimes the strongest choice.

## Imagery

### Photo sources (direct URLs)
```html
<!-- Unsplash — append ?w=WIDTH&q=80 for sizing -->
<img src="https://images.unsplash.com/photo-1551434678-e076c223a692?w=800&q=80" alt="..." class="w-full object-cover">

<!-- Pexels -->
<img src="https://images.pexels.com/photos/3184292/pexels-photo-3184292.jpeg?auto=compress&w=800" alt="..." class="w-full object-cover">

<!-- Pixabay — use the direct image URL from their CDN -->
<img src="https://cdn.pixabay.com/photo/2023/XXXXX.jpg" alt="..." class="w-full object-cover">
```

### Image treatment
```html
<!-- Gradient overlay on photo -->
<div class="relative">
  <img src="..." class="w-full h-96 object-cover">
  <div class="absolute inset-0 bg-gradient-to-t from-black/80 to-transparent"></div>
  <div class="absolute bottom-8 left-8 text-white"><!-- content --></div>
</div>

<!-- Rounded with shadow -->
<img src="..." class="rounded-2xl shadow-2xl">

<!-- Blend into background -->
<img src="..." class="mix-blend-luminosity opacity-60">
```

### Device Mockups (FlyonUI built-in)

FlyonUI includes ready-made mockup components. Use these instead of building CSS frames from scratch.

#### Phone mockup
```html
<div class="mockup-phone">
  <div class="mockup-phone-display">
    <img src="https://images.unsplash.com/photo-XXXXX?w=400&q=80" alt="App screen" class="w-full h-full object-cover object-top">
  </div>
</div>
```

#### Browser mockup
```html
<div class="mockup-browser border-base-300 border">
  <div class="mockup-browser-toolbar">
    <div class="input">https://app.competitor-name.com</div>
  </div>
  <div class="border-base-300 border-t">
    <img src="..." alt="Dashboard" class="w-full">
  </div>
</div>
```

#### Window mockup (OS window frame)
```html
<div class="mockup-window border-base-300 border">
  <div class="border-base-300 border-t px-4 py-8">
    <!-- content inside window -->
  </div>
</div>
```

Docs: https://flyonui.com/docs/mockups/phone/ · https://flyonui.com/docs/mockups/browser/

#### When to use mockups
- Fintech / app → phone mockup with app screen
- B2B SaaS / platform → browser mockup with dashboard URL
- Desktop software → window mockup
- Physical product / logistics → skip mockup, use a photo instead

## FlyonUI Components — Quick Reference

Use semantic classes for structure, Tailwind utilities for custom styling.

```html
<!-- Buttons -->
<button class="btn btn-primary">Get started</button>
<button class="btn btn-outline">Learn more</button>

<!-- Cards -->
<div class="card">
  <div class="card-body">
    <h5 class="card-title">Feature</h5>
    <p>Description</p>
  </div>
</div>

<!-- Stats -->
<div class="stat">
  <div class="stat-title">Revenue</div>
  <div class="stat-value">€4.2M</div>
  <div class="stat-desc">↗ 24% vs last year</div>
</div>

<!-- Badges -->
<span class="badge badge-primary">New</span>
<span class="badge badge-success">Live</span>

<!-- Navbar -->
<nav class="navbar">
  <div class="navbar-start">...</div>
  <div class="navbar-end">...</div>
</nav>

<!-- Timeline -->
<ul class="timeline">
  <li><div class="timeline-start">Step 1</div></li>
  <li><div class="timeline-start">Step 2</div></li>
</ul>
```

Full component list: https://flyonui.com/docs/component/

## FlyonUI Blocks — Landing Page Building Blocks

FlyonUI provides 500+ ready-made blocks specifically for marketing pages. Use these as structural starting points, then customize heavily with brand colors, copy, and imagery. **Never use a block unchanged** — always adapt it to the competitor's brand and proposition.

### Available block categories (browse at https://flyonui.com/blocks):

**Marketing UI blocks (most relevant for competitor landing pages):**
- **Hero Section** (22 blocks) → https://flyonui.com/blocks/marketing-ui/hero-section
- **Features Section** (20 blocks) → https://flyonui.com/blocks/marketing-ui/features-section
- **Pricing Component** (20 blocks) → https://flyonui.com/blocks/marketing-ui/pricing-component
- **Testimonials Component** (20 blocks) → https://flyonui.com/blocks/marketing-ui/testimonials-component
- **Social Proof** (9 blocks) → https://flyonui.com/blocks/marketing-ui/social-proof
- **CTA Section** (9 blocks) → https://flyonui.com/blocks/marketing-ui/cta-section
- **Logo Cloud** (9 blocks) → https://flyonui.com/blocks/marketing-ui/logo-cloud
- **FAQ Component** (17 blocks) → https://flyonui.com/blocks/marketing-ui/faq-component
- **Navbar Component** (14 blocks) → https://flyonui.com/blocks/marketing-ui/navbar-component
- **Footer Component** (5 blocks) → https://flyonui.com/blocks/marketing-ui/footer-component
- **About Us Page** (20 blocks) → https://flyonui.com/blocks/marketing-ui/about-us-page

**Dashboard blocks (for competitors with platform/SaaS products):**
- **Statistics Component** (8 blocks) → https://flyonui.com/blocks/dashboard-and-application/statistics-component
- **Widgets Component** (11 blocks) → https://flyonui.com/blocks/dashboard-and-application/widgets-component
- **Charts Component** (17 blocks) → https://flyonui.com/blocks/dashboard-and-application/charts-component

**Bento Grid** (6 blocks) → https://flyonui.com/blocks/bento-grid/bento-grid

### FlyonUI Templates (full page references):

Use as structural inspiration for how a complete landing page flows:
- **Free Restaurant Landing Page** → https://flyonui.com/templates (free, HTML download)
- **SaaS Landing Page** → https://flyonui.com/templates (section flow reference)
- **Mobile App Landing Page** → https://flyonui.com/templates (app-focused layout)
- **AI Tool Landing Page** → https://flyonui.com/templates (tech product layout)

### How to use blocks in the build process:

1. After the 5 feeling questions, decide which block categories fit the competitor type
2. Browse the relevant block pages to select structural patterns
3. Combine blocks: typically Hero + Features/Stats + Social Proof/Testimonials + CTA + Footer
4. **Strip all placeholder content** — replace with the approved copywriter text
5. **Override all colors** — apply the competitor's brand palette via Tailwind config
6. **Swap all images** — use real photos from Pexels/Unsplash/Pixabay
7. **Customize layout** — rearrange, remove, or extend sections to match the proposition
8. Add scroll animations, hover effects, and interactions on top

The blocks give you professional structure fast. The customization makes it feel like a real company, not a template.

## That's it.

Everything else — layout, components, sections, navigation, typography sizing, spacing rhythm, color application — must be invented from scratch for each competitor. Do not template it.
