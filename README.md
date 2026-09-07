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
  site.css      all styles (shared pattern with the outdoor kitchens page)
  site.js       lazy split-section video, UTM passthrough, hero lead form, quote popup
  logo.webp     current flame-and-wave brand mark
  brands/       partner logos shown in the design center strip
  hero-fire-table.jpg       hero photograph
  bellafina-design-center.webp  showroom band background
  fire-pit-loop.mp4 / fire-table-loop.mp4  split-section loops
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

`#types` Explore · `#tables` Fire Tables · `#why` Why BellaFina · `#showroom` Design Center · `#faq` FAQ · `#trade` For Trade

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

## Page order

Matches the outdoor kitchens page pattern, homeowner-first:

hero → trust strip → design center → lede → product splits → product grid →
how it works → why homeowners choose BellaFina → specs → FAQ → service areas →
visit the design center → trade → final CTA

Contractor content sits in its own quiet `#trade` section just before the final
CTA rather than competing with the homeowner journey; the trust strip is
homeowner-facing for the same reason.

`Request My Free Quote` is the only primary CTA. Every primary button uses
`href="#top" data-quote-modal` and opens the quote modal — no off-site primary
CTAs. The header carries one CTA, not a Shop ghost button plus a quote button,
which overflowed the nav.

Nav is seven items and fits alongside the CTA down to 1120px, below which the
CTA label shortens to "Free Quote" (`.cta-long` / `.cta-short`).

## Design

`assets/site.css` is the outdoor kitchens stylesheet, with this page's own
additions appended at the end (question-led spec table, `.aud-lead`, the
responsive header CTA label, and a three-up brand strip since this page carries
six logos rather than eight).

Brand tokens — no hardcoded hex outside `:root`:

| Token | Value |
| --- | --- |
| `--navy` / `--ink` | `#0d1b2a` |
| `--navy-2` | `#114a73` |
| `--blue` | `#3d7da8` |
| `--blue-pale` | `#b8d0e3` |
| `--gold` | `#c8a25b` |
| `--orange` | `#e5672a` |
| `--cream` / `--paper` | `#fdf5ec` |
| `--cream-2` | `#fbebda` |
| `--radius` | `10px` |

Cormorant Garamond for `h1`/`h2`, Montserrat for `h3`, nav, buttons and
eyebrows, Roboto for body — via Google Fonts. Buttons are flat accent fill at
10px radius, not pills or gradients.

Two traps worth remembering: `.hero-photo` is restated *after* the `.hero-fire`
theme rule because that rule uses the `background` shorthand and would otherwise
reset the image; and a base `.cta-row{display:flex}` exists so a new section's
buttons lay out without re-declaring it.

## Partner logos

`assets/brands/` holds trimmed webp, grayscale at rest and colour on hover. Only
brands published on bellafinaoutdoors.com are used, and the subset is chosen for
*this* page's topic: The Outdoor Plus for fire features, plus the stone and tile
brands that clad built-in fire features (this page has a Veneer, Stone & Tile
category). The kitchen page's eight are deliberately not reused verbatim.

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
