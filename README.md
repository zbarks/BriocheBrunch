# Brioche — demo site

Built by Barker Digital for Hamilton Harman.
10B Queensferry Street, West End, Edinburgh. Opening September 2026.

Static, no build step, no dependencies. Open `index.html` or drop the folder on Vercel, Netlify or any host.

---

## Contents

```
index.html          the whole site, one file
assets/
  brioche-mark.svg        VECTOR loaf lockup, this is what the site uses
  brioche-wordmark.svg    VECTOR wordmark only, no loaf
  brioche-logo-cream.svg  vector, square, on brand cream
  brioche-mark.png        1588x1073 raster fallback, native res
  brioche-wordmark.png    2404x566 raster fallback
  brioche-logo.png        2000x2000 transparent, native res
  brioche-logo-cream.png  2000x2000 on cream, native res
  favicon.svg / .png / .ico
  og-image.png            1200x630 social share card
```

---

## Pages

Nav now matches what the business actually sells, taken from the two menus.

| Route | Nav label | What is on it |
|---|---|---|
| `#/` | Home | Logo intro, positioning, three routes into the menus, afternoon tea feature, find us |
| `#/menus` | Menus | All day food, then afternoon tea as its own section further down |
| `#/matcha-and-coffee` | Matcha & coffee | Matcha lattes, coffee and tea, pure smoothies |
| `#/visit` | Visit | Address, hours, phone, getting here |

The old Shop and Our Story pages are gone. There was nothing real to put on them, and empty pages hurt more than missing ones. Add them back when there is content.

---

## Positioning

The site leads on three things from the launch copy, in this order:

1. **A café and bar**, not just a café. The bar side is stated in the footer, the opening section and the day section.
2. **French and Japanese fusion.** Teriyaki french toast and duck a L'orange dumplings are named on the home page because they are the two dishes that explain the concept fastest.
3. **The first sober bar in Edinburgh.** This has its own full-width section on the home page, ending on the line "East Asian coffee. French fusion menus. No hangover."

The sober bar claim is a strong one and it is stated as fact on the site. Worth Hamilton confirming he is comfortable being first on record with it, since a competitor could dispute it.

Note on the heading: it reads "The first sober bar in Edinburgh" rather than "Edinburgh's first sober bar" because the display font sets a curly apostrophe with a visible gap. Avoid apostrophes in display-size headings.

---

## The menus page

The all day menu and afternoon tea now live on one page at `#/menus`. All day sits on the brown block, afternoon tea follows on cream as its own clearly separated section with an anchor at `#afternoon-tea`.

Afternoon tea is compressed into three columns rather than the four stacked blocks it had before: surf and turf, vegetarian, then scone, bon bons and the tea list together. Same content, roughly half the scroll.

The old routes `#/all-day` and `#/afternoon-tea` still resolve to the menus page, so any link already shared keeps working.

---

## Photo slots

There are two on the home page: one in the opening section and one beside the afternoon tea block. Until a file exists each renders as a labelled frame, so the space looks designed rather than broken.

To switch one on, drop the image in and add the class `has-image` to the figure:

```html
<figure class="photo has-image" id="photoRoom">   <!-- assets/room.jpg -->
<figure class="photo has-image" id="photoTea">    <!-- assets/afternoon-tea.jpg -->
```

The image then fills the frame with `object-fit: cover` and the placeholder disappears. More slots can be added anywhere by copying the block.

---

## About the copy

Every word is written for this business off the two menus. It is not placeholder any more.

House rules used throughout, worth keeping to if you edit:

- No em dashes anywhere. Commas, full stops and the occasional colon. There is a check in the build notes below.
- Plain words. No "curated", "elevated", "nestled", "artisanal", "journey", "experience".
- Slightly formal register. Full sentences, no fragments, no contractions where they can be avoided. "You will find us" rather than "we're at".
- Say the thing rather than the feeling. "Sixteen canapés rather than a plate of sandwiches" instead of "an unforgettable afternoon".
- British spelling.

Facts used in the copy all come from the menus: DOP Isigny Sainte-Mère butter, ceremonial grade single origin matcha, rare single origin Yunnan coffee, Pekoe tea, the 49 and 52 afternoon tea prices, the canapé counts, and the allergen and service charge lines. Nothing about the business has been invented.

### Prices

Written the way the menus write them, without a pound sign, so the site matches the printed menu. Say the word if you would rather show £14.90 on the web.

### Two things to check with Hamilton

1. The all day menu prints "Cappucino". It is spelled "Cappuccino" on the site. Worth fixing on the printed menu too.
2. The all day menu says "Petit or Wee or Little" under Petit Plates. That reads like a joke rather than a description, so the site keeps it as "Petit, or wee, or little". Confirm that is deliberate.

---

## Private hire

There is now a fifth page at `#/private-hire`, linked from the nav, the footer and the Visit page. It is the one part of the site that does not go to Lightspeed.

Parties of eight and above, an area of the room, and whole venue hire go through a form there. The form posts to the portal, where a member of staff confirms or declines it by hand. Nothing books itself, which is what keeps this from clashing with the Lightspeed diary. Everything else, including afternoon tea at any party size, still goes to Lightspeed.

`#/private`, `#/groups` and `#/events` all resolve to the same page, so a guessed URL lands somewhere sensible.

The portal is a separate project. See its own README for the deployment.

---

## Before this goes live

**1. Config.** One block in the `<head>`, four values:

```js
window.BRIOCHE = {
  BOOKING_URL : "",   // the Lightspeed link, every Book button uses it
  PORTAL_API  : "",   // the deployed portal, no trailing slash
  POSTHOG_KEY : "",   // blank means no analytics script loads at all
  POSTHOG_HOST: "https://eu.i.posthog.com"
};
```

Every one of them is optional and the site works with all four blank. Without the booking URL the Book buttons show a toast. Without the portal URL the private hire form says it is not connected. Without the PostHog key no third party script loads.

The PostHog host has to match the region the project was created in, EU or US. Mixing them fails with an unhelpful error.

The portal also needs `PUBLIC_SITE_ORIGIN` set to this site's address, or the browser blocks the form.

**2. Still to confirm:**

- Opening hours. Currently 08:00 to 17:00 weekdays, 08:30 to 17:30 Saturday, 09:00 to 17:00 Sunday. These are assumed, not given.
- The opening month. The site now says "opening this September" with no year attached, taken from the launch copy.
- The afternoon tea booking rule. The site states "Afternoon tea must be booked a day ahead." in five places, worded identically each time. If that requirement changes, search for that sentence and it will find every instance.
- The minimum party size for private hire. The form is set to eight, stated on the page and enforced on both the form and the server. Changing it means editing the copy on `#/private-hire`, the `min` on the guests field, the check in the submit handler, and the same check in `api/enquiry.js` and `schema.sql`.
- Whether Lightspeed caps party size at its own end. If it does not, somebody will book fourteen through the widget and never see the private hire page.
- Whether the Matcha Sando link should be named on the site. Right now it is not.

**3. Real photography.** Two slots are waiting on the home page. See the photo slots section above.

---

## Brand

Sampled directly out of `BRIOCHE LOGO FINAL.png`, so these are exact.

| Token | Hex | Use |
|---|---|---|
| `--crust` | `#7A2F00` | logo brown, brown sections, primary buttons |
| `--butter` | `#FFD699` | logo lettering, accents, buttons on brown |
| `--cream` | `#FFE7C2` | page background |
| `--char` | `#4A1C00` | body text, footer |
| `--milk` | `#FFF3DF` | card surfaces |

Two accents on top, derived to sit next to the logo brown rather than fight it. Neither is in the logo, so they are a proposal, not brand law.

| Token | Hex | Use |
|---|---|---|
| `--matcha` | `#455C22` | the matcha and coffee page, deep green |
| `--matcha-lt` | `#CBDCA5` | VG tags, rules, bullets |
| `--jam` | `#9E3A4E` | card headings, deep berry |
| `--jam-lt` | `#F5C4CE` | V tags, rules, bullets |

Both deep tones clear 4.5:1 against the cream, so cream text on them passes AA.

### Type

Hamilton's brand font is **One Little Font** by Konstantina Louka, around £13 to £16.

The site uses **Gaegu** as a stand-in, free on Google Fonts and close to the hand-drawn feel of the wordmark. Body text is **Quicksand**.

Before swapping: a standard desktop licence does not usually cover `@font-face` web embedding. Check whether the licence includes webfont use. If not, either buy the webfont add-on or keep Gaegu on the site and use One Little Font in print and social only.

---

## The logo is vector

Traced from the original artwork with potrace and rebuilt as paths. It renders identically to the source: mean pixel difference of 0.66 out of 255 against the original at full size, with variance only on antialiased edges.

This matters because the load sequence blows the logo up to fill the screen. A raster file would need to be about 2600px wide to survive that on a retina display. The SVG is 4.8KB and stays sharp at any size.

Ask Hamilton whether the designer has the original vector, an AI, EPS or SVG. This trace is excellent but it is still a trace of a PNG. For signage and packaging he wants the real artwork.

---

## The load sequence

The logo fills the screen on first load, holds for a beat, then scales and travels into its resting place in the hero.

It is one element for the whole sequence, animated with the Web Animations API. There is no second copy and no handover, so there is no frame where the logo is missing or dimmed.

Three things this depends on, so be careful editing them:

- `.page` must not have an opacity transition. Group opacity creates a stacking context, which both dims the logo and traps it below the curtain.
- `.hero .wrap` must not carry a `z-index`, for the same reason.
- The flying logo is `pointer-events:none` so clicks pass through to the skip layer beneath it.

Guardrails:

- About 1.9 seconds total.
- Click anywhere or press Escape to skip. Skipping calls `finish()` so the logo lands where it should rather than jumping.
- A 5 second failsafe finishes the intro even if the artwork never loads.
- `prefers-reduced-motion` skips it entirely.

It plays on every load, which is what you want when demoing. To make it once per visit, wrap `runIntro()` in a `sessionStorage` check.

---

## Design notes

The section dividers are the loaf silhouette from the logo. Every coloured block rises out of the cream in that shape, so scrolling reads as bread coming up. That is the signature device, do not swap it for a plain edge.

Menu rows use a dashed rule between items rather than dot leaders, which hold up better on mobile. Below 900px the price drops to its own line rather than squashing the dish name.

---

Barker Digital
zach@barkerdigital.co.uk / barkerdigital.co.uk

---

## Configuration

Everything the site needs is in one block at the top of `index.html`:

```js
window.BRIOCHE = {
  BOOKING_URL : "https://mylightspeed.app/reservation/db1f6f59-5914-4b53-9462-14591bee4da7/reservation",
  PORTAL_API  : "https://brioche-portal.vercel.app",
  POSTHOG_KEY : "phc_vu3cSykAkzBsJKP296Tv5SLvVvb8yHKfsiCorQ5ouiRC",
  POSTHOG_HOST: "https://us.i.posthog.com"
};
```

**PORTAL_API** points at the private hire portal. The portal only accepts the
form from origins listed in its own `PUBLIC_SITE_ORIGIN` variable, so if this
site moves to a new address, that variable has to be updated too or the browser
will block the request.

**POSTHOG_KEY** is a write-only project key. It is meant to be public and is
safe in this file and in git. The account is on **US** cloud, not EU. Pointing
EU keys at a US host, or the reverse, fails with an unhelpful error.

**BOOKING_URL** is set to the live Lightspeed reservation page. Every Book
button opens it inside the site, in a booking panel, rather than sending people
to another tab. On desktop it is a large centred panel over the page, on phones
it goes full screen. Close with the cross, the backdrop or Escape.

- The Lightspeed page only loads the first time someone presses Book, so it
  adds nothing to the initial page load.
- `#/book` (also `#/booking`, `#/reserve`) opens the panel directly. Use this
  link on Instagram, Google Business Profile and anywhere else that needs a
  straight booking link.
- The panel header has an "Open in a new tab" link, and if Lightspeed is slow
  to load a direct link appears after eight seconds, so there is always a way
  through.
- The panel footer points parties of eight or more at the private hire page.
- If Lightspeed ever starts refusing to be framed, the panel will show their
  error. Blank BOOKING_URL is not the fix; switch the click handler back to
  `window.open(BOOKING_URL)` in the booking section of the script.

If BOOKING_URL is emptied, every Book button shows "Booking link not connected
yet" rather than failing silently.

## Coming soon switch

The site can show a single holding page instead of everything else. It is one
line at the top of the config block in `index.html`:

```js
COMING_SOON : true,    // holding page on
COMING_SOON : false,   // full site live
```

Change it, push, and Vercel republishes. Nothing else needs touching, and no
code is removed while it is on.

- The holding page shows the logo (with the load animation), "Opening soon.",
  a short line on the café, a Book a table button that opens the live
  Lightspeed panel, a call button, and the address.
- The nav and footer are hidden while it is on.
- Every address on the site, including `#/menus` and `#/book`, lands on the
  holding page. `#/book` still opens the booking panel over it.
- To check the full site while the switch is on, add `?preview` to the
  address, for example `https://yoursite.co.uk/?preview` or
  `https://yoursite.co.uk/?preview#/menus`. Visitors without it see the
  holding page.

## Photographs

Two photographs are not yet supplied:

```
assets/room.jpg
assets/afternoon-tea.jpg
```

The page handles their absence on its own. When a file is missing the image
removes itself and the "Photo to be added" placeholder shows instead. Drop the
files in with those exact names and they appear, no code change needed.

## Deploying

Static, so there is no build step. Push to the connected repository and Vercel
publishes it. Nothing in this folder holds a secret.
