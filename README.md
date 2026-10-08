# Ali Safari Kenya — Homepage

A single-file, high-converting safari tour homepage for a local Kenyan operator.
One HTML file, Tailwind CSS via CDN, vanilla JS only — no build step, no framework.

**Goal:** build trust in 5 seconds, sort visitors by travel intent, and drive one
action — a safari enquiry (form or WhatsApp).

## Live URLs

| Environment | URL | Status |
|---|---|---|
| **Production** | https://site-cf52e19283a24161bc67f9739e0d11de.freebuff.page | published |
| Preview / staging | https://site-ad41107380414b7b8e183327ec96cea5.freebuff.page | published |
| GitHub | https://github.com/olitechs/ali-safari-kenya (`main`) | pushed |

## Run locally

```bash
python3 -m http.server 3000
# open http://localhost:3000
```

Or just open `index.html` in a browser — it only needs network access for the
Tailwind CDN, Google Fonts, Unsplash images and the hero video.

## Repository layout

| Path | Purpose |
|---|---|
| `index.html` | **The site.** Everything lives here — edit this. |
| `public/index.html` | Deployable copy consumed by the Workers asset pipeline. Keep in sync with `index.html`. |
| `worker.js` | Cloudflare Worker fetch handler — serves `env.ASSETS`. |
| `manifest.json` | Deploy manifest (entrypoint, compatibility date, assets dir). |
| `.gitignore` | Ignores `node_modules/`, logs, local `tools/`. |

## ⚠️ Swap checklist before going live

The page ships with realistic demo content. Find every marker with:

```bash
grep -n "SWAP:" index.html      # 13 markers
```

| Line | Item | Current demo value |
|---|---|---|
| 6 | GA4 + Meta Pixel snippet | events already pushed to `dataLayer`/`fbq`; add IDs |
| 184 | Phone number | `+254 712 345 678` |
| 189 | WhatsApp number | same number (used in 11 links incl. `wa.me/`) |
| 192 | Email address | `hello@alisafarikenya.com` |
| 272 | Founding year | `2009` |
| 367 | Memberships / ratings badges | TripAdvisor, KTF, KATO, SafariBookings, Google |
| 419 | Happy-travellers count | `4,800+` |
| 585 | Packages, durations & prices | 3 signature packages (`$1,890 / $2,450 / $1,240`) |
| 802 | TripAdvisor review profile URL | generic tripadvisor.com |
| 900 | Traveller-stories video URL | falls back to the hero MP4 |
| 1080 | Form endpoint | `data-endpoint=""` → built-in success state |
| 1354 | Registered address | Riverside Drive, Westlands, Nairobi |
| 1367 | Tour operator licence no. | `KTB licence no. KTPH/0417/2024` |

**The two that matter most:** the phone/WhatsApp number (WhatsApp taps would
reach a stranger) and the form endpoint (currently demo-only).

### Enabling live form delivery

```html
<!-- index.html, enquiry form -->
<form id="enquiryForm" action="#" method="POST" data-endpoint="" novalidate>
```

Paste a Formspree/EmailJS URL into `data-endpoint`. The JS submits via `fetch`
and shows the success state; with an empty endpoint it shows the built-in
demo success state.

## Brand tokens (Tailwind config, top of `index.html`)

| Token | Hex | Use |
|---|---|---|
| `night` | `#0F1A14` | Dark sections, text on gold |
| `gold` | `#D9A441` | **Primary CTAs only** |
| `terracotta` | `#B5472C` | Eyebrows, accents, links on light |
| `cream` | `#F6F0E4` | Light section backgrounds |
| `baobab` | `#3B2A1E` | Body text on light |
| `sage` | `#7C8B6F` | Park labels, secondary accents |

Buttons are `rounded-full` with `hover:scale-105` + stronger shadow.

## Page structure

Top bar → fixed nav → 100vh video hero + intent filter → trust marquee →
4 intent cards → 3 signature packages → 7 destinations → why-us + founder →
3-step how-it-works → review carousel → 7-day itinerary accordion →
enquiry form → FAQ → final CTA → footer.
Persistent: floating WhatsApp, mobile bottom bar, exit-intent prompt.

**Tech notes**

- Scrollbars hidden globally; smooth scroll; `.reveal` fade-in-up
  (`translate-y-10` + `opacity-0` → visible over 1000 ms, IntersectionObserver).
- All non-hero media lazy-loaded with explicit `width`/`height`.
- FAQ/itinerary accordions use CSS-grid `0fr → 1fr`; the `+` rotates 135°.
- JSON-LD: `TravelAgency` + `FAQPage` (8 questions); hreflang `en`/`it`.
- WCAG AA contrast audited — all text pairs pass (4.5:1, large text 3:1).
- Copy rules: specific over sensory, no clichés, every section ends with an action.

## Deploying

Push to `main`, then publish the workspace build:

```bash
git add index.html && git commit -m "Update homepage" && git push origin main
```

The deploy manifest points at `public/` — copy `index.html` there when the
page changes:

```bash
cp index.html public/index.html
```
