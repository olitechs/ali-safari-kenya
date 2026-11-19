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

## Pages

| Page | Contents |
|---|---|
| `index.html` | Homepage — trust, intent filter, packages, reviews, form, FAQ |
| `tours.html` | **Tours hub** — filterable grid of all safaris, blue safaris and excursions |
| `safari-masai-mara-signature.html` | 7-day Masai Mara + Lake Nakuru — day-by-day, inclusions, lodge hints |
| `safari-amboseli-tsavo-coast.html` | 9-day Amboseli → Tsavo East → Diani |
| `safari-northern-giants.html` | 5-day Samburu + Lake Nakuru (short safari) |
| `safari-great-migration.html` | 8-day Great Migration expedition, balloon + conservancy night drive |
| `blue-safari-watamu.html` | Watamu boat day: dolphins, coral gardens, Mida Creek + 4 excursion combos |
| `blue-safari-malindi.html` | Malindi boat day: dhow lunch, marine reserve, Marafa + combos |

Every detail page carries: itinerary (accordion/timeline), what's included **and** not included,
lodge hints by budget (Comfort/Premium/Luxury), when-to-go, WhatsApp deep link, `TouristTrip` +
`BreadcrumbList` JSON-LD. The language switcher's `/it/` links are placeholders for the future
Italian site.

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
grep -n "SWAP:" index.html      # 7 markers
```

| Line | Item | Current demo value |
|---|---|---|
| 6 | GA4 + Meta Pixel snippet | events already pushed to `dataLayer`/`fbq`; add IDs |
| 366 | Memberships / ratings badges | TripAdvisor, KTF, KATO, SafariBookings, Google |
| 418 | Happy-travellers count | `4,800+` |
| 584 | Packages, durations & prices | 3 signature packages (`$1,890 / $2,450 / $1,240`) |
| 801 | TripAdvisor review profile URL | generic tripadvisor.com |
| 899 | Traveller-stories video URL | falls back to the hero MP4 |
| 1079 | Form endpoint | `data-endpoint=""` → built-in success state |
| — | Italian site (`/it/` links) | language switcher points at future Italian URLs |

**Contact details are real:** phone/WhatsApp `+254 745 043 121` and
`sheddymae01@gmail.com`, address Watamu, Kenya, no licence number in the
footer. All eight pages use the same values.

**The one that matters most now:** the form endpoint (currently demo-only, so
enquiries are not emailed anywhere yet).

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
