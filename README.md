# BellaFina Outdoors — Outdoor Fire Pits & Fire Tables

Single-page static landing site for **Outdoor Fire Pits & Fire Tables**, deployed on Vercel at
**firepits.bellafinaoutdoors.com**.

Standalone gathering features on a patio — gas fire pits, fire tables, built-in and
custom fire features. Deliberately scoped away from pool-side fire bowls, which are
covered by a separate site, so the two don't compete for the same searches.

## Structure

```
index.html      the whole page — content, nav, forms, JSON-LD
assets/
  site.css      all styles
  site.js       lazy split-section video, UTM passthrough, hero lead form, quote popup
  logo.webp     brand logo and favicons
  fire-pit-loop.mp4    split-section loop, gas fire pits
  fire-table-loop.mp4  split-section loop, fire tables
  hero-fire-table.jpg  hero photograph (1500x1001)
vercel.json     cleanUrls + no trailing slash
robots.txt      allow-all + sitemap reference
sitemap.xml     the single URL
```

Everything is at the repository root, so Vercel deploys it with no Root Directory
setting and no build step.

## Deploying

1. vercel.com → **Add New → Project** → import this repository
2. **Framework Preset → Other**. Leave Build Command and Output Directory empty —
   these are plain static files, there is nothing to build
3. Deploy
4. **Settings → Domains** → add `firepits.bellafinaoutdoors.com`, then add the
   `CNAME` record Vercel shows to your DNS (`cname.vercel-dns.com`)

Production deploys from `main`. Every push redeploys automatically.

## This site is standalone

It does not link to the other BellaFina landing sites (fire bowls, outdoor kitchens and pizza ovens).
The header nav is on-page anchors only:

`#top` Fire Pits · `#types` Types · `#tables` Fire Tables · `#who` Homeowners & Pros · `#areas` Service Area · `#faq` FAQ

Every outbound link goes to **bellafinaoutdoors.com**, deep-linked to the matching
page rather than the homepage, and carries UTM tags:

- `utm_source=fire-pits-lp`
- `utm_medium=landing_page`
- `utm_campaign=fire_pits`
- `utm_content=<placement>` — e.g. `hero_shop_fire_pits`, `card_fire_tables`,
  `showroom_directions`, `footer_contact`

Adding a nav item means adding an `id` to the section it points at.

## Leads

The hero form and the quote popup both use the GoHighLevel form
`iD7GLxxCdv51i6umUCJF`. `<body data-lead-source="Fire pits and fire tables page">`
is what marks the CRM record as coming from this site, so keep it if you copy this
page anywhere.

Until `LEAD_WEBHOOK_URL` in `assets/site.js` is set, the hero form opens the hosted
version of that form in a new tab with the fields prefilled, rather than posting
directly. Set the webhook URL to capture leads without the extra step.

The popup opens by itself 5 seconds after load, once per session, and on
desktop exit intent.

## Design

Navy / gold / orange tokens, Georgia headings, sticky header, hero with a lead
card, trust strip, split sections, category grid, homeowner/trade split, spec
table, three-step process, FAQ, service area, showroom band, final CTA, footer.

The two split sections play vertical video loops (`#split-loop-video` for gas fire
pits, `#split-loop-video-2` for fire tables), lazy-loaded by `assets/site.js` — they
are only fetched within 200px of the viewport and pause once scrolled past. Both are
H.264/AAC, 720x1280, faststart, and muted so autoplay is allowed. Because the source
is 9:16 phone video, `.visual-video` overrides the 4:3 panel rather than cropping most
of each frame away. Under `prefers-reduced-motion` they do not autoplay and gain
controls instead. Swapping a loop is a `src` change; keep the ids, they are what
`site.js` looks for.

The hero carries `assets/hero-fire-table.jpg` — a lit linear gas fire table with
seating around it. It is wired as `class="hero hero-photo"` plus
`style="--hero-image:url('/assets/hero-fire-table.jpg')"` on the `<section>`. Use a
root-relative path there — a relative `url()` inside a custom property resolves
against `site.css`, not the page. The shared `.hero-photo` focal point of
`center 60%` is overridden to `center 42%` for this particular shot; re-tune that
if the photo is ever swapped. The image is also `rel="preload"`ed in the head,
because it is the LCP element and a CSS background behind a custom property is
invisible to the preload scanner.

The same photo backs the social preview (`og:image` / `twitter:image`) with a
`summary_large_image` card.

## Geographic coverage

Rather than a thin page per city, the service-area section carries the whole
Southern California footprint at once, grouped into clusters by county (Orange,
Los Angeles, San Diego), with copy on what actually differs by area. The same city
list feeds `areaServed` in the page's JSON-LD.

To extend the footprint, edit the `.areas` section — don't spawn new URLs.

## Before launch

- Point `firepits.bellafinaoutdoors.com` at the Vercel project and confirm HTTPS
- Consider re-encoding `hero-fire-table.jpg` (357KB) to WebP/AVIF for a lighter LCP
- Add this site as its own property in Search Console and submit `sitemap.xml` —
  subdomains are separate properties
