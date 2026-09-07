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

This site has no photography of its own yet, so the hero uses a brand gradient
(`.hero-fire` in `assets/site.css`) and the split sections use gradient panels with an
SVG mark (`.visual-fire`, `.visual-water`). To use a real photo: drop it into
`assets/`, swap the gradient class on the hero `<section>` for `hero-photo`, and
set `style="--hero-image:url('/assets/your-photo.webp')"`. Use a root-relative
path there — a relative `url()` inside a custom property resolves against
`site.css`, not the page.

For the same reason the social preview (`og:image` / `twitter:image`) points at
`assets/icon-512.png` and the Twitter card is `summary`, not
`summary_large_image`. When real photography lands, point both at the wide
photo and switch the card back to `summary_large_image`.

## Geographic coverage

Rather than a thin page per city, the service-area section carries the whole
Southern California footprint at once, grouped into clusters by county (Orange,
Los Angeles, San Diego), with copy on what actually differs by area. The same city
list feeds `areaServed` in the page's JSON-LD.

To extend the footprint, edit the `.areas` section — don't spawn new URLs.

## Before launch

- Point `firepits.bellafinaoutdoors.com` at the Vercel project and confirm HTTPS
- Add real photography (see **Design**)
- Swap `og:image` / `twitter:image` to that photo and restore `summary_large_image`
- Add this site as its own property in Search Console and submit `sitemap.xml` —
  subdomains are separate properties
