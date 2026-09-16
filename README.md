# Ragus site

The marketing site for [Ragus](https://ragus.io) — home, FAQ and blog. None of
the app's functionality lives here.

Plain HTML and one stylesheet. No framework, no build step, no dependencies:
every file is something a browser opens directly.

## Preview

Open `index.html` in a browser. That's it.

To check it on a phone on the same Wi-Fi:

```bash
python3 -m http.server 4321
# then visit http://<your-mac's-ip>:4321 on the phone
```

## Layout

```
index.html      home -- wide, centred sections with product shots
faq.html        questions
privacy.html    the privacy policy App Store Connect points at
support.html    the support page App Store Connect points at
styles.css      the whole design; palette matches the app
img/            phone screens, cropped from the App Store compositions
blog/
  index.html    post list
  *.html        one file per post
```

The home page is laid out like a modern product landing page: hero with one
italic accent phrase, a "you say / Ragus records" demo card, a comparison,
three how-it-works cards, alternating feature rows with a phone frame, a
privacy block, a short FAQ and a final call to action. There is deliberately
no invented social proof -- no logos, quotes or statistics -- until there are
real ones to show.

## Screens in `img/`

Each is the phone screen alone, cropped out of the 1290x2796 App Store
compositions (the frame and caption are drawn by the site, not the image):

```bash
sips --cropOffset 475 160 -c 2245 970 in.png --out img/name.png && sips -Z 1300 img/name.png
```

Redo them when the app's screens change.

## Adding a blog post

Copy an existing post, change the content, and add an entry to the list in
`blog/index.html`. Posts are deliberately hand-written files rather than
generated from markdown — at this volume a build step would cost more than it
saves.

## Before launch

The App Store button is **inert on purpose** while the app is TestFlight-only.
It renders greyed as "Coming soon to the App Store".

When the app is approved, search for `aria-disabled="true"` (four places:
three in `index.html` -- the nav pill and two App Store buttons -- and one in
`faq.html`), remove that attribute, set `href` to the App Store URL, and change
the nav pill's text to "Get the app".

## Search

Every page carries a canonical URL, Open Graph tags and JSON-LD: the home
page as a `MobileApplication`, the FAQ as a `FAQPage` (built from the
`<details>` blocks, so keep questions in that form), each post as a
`BlogPosting`. `sitemap.xml` lists the pages by hand -- add a line when you
add a post. `robots.txt` points at it. `_redirects` sends `www` to the apex
so there is one host, and `_headers` sets cache lifetimes for the images.

The home title leads with "voice calorie tracker" on purpose: it is the
phrase people search, and the app's name is not yet one.

## Deploying

The site is a Cloudflare Worker with static assets (`ragus-site`), serving
`ragus.io` and `www.ragus.io`. It builds on push: commit to `main`, push to
GitHub, and the new version is live within about a minute.

The host serves clean URLs and redirects the `.html` forms (`/faq.html` ->
`/faq`, `/blog/index.html` -> `/blog/`), so links between pages use the clean
form. `og.png` is the link-preview card (1200x630); rebuild it if the headline
changes. `favicon.png` and `apple-touch-icon.png` are the app icon resized.

## Design

Colours are taken from the app's `controls.tsx` so the two stay consistent:

| Token | Value |
| --- | --- |
| ink | `#2B2724` |
| accent | `#C4552F` |
| muted | `#7D736A` |
| rule | `#E7DED2` |
| background | `#FBF8F4` |

The Sugar mark is inlined SVG traced from the app's `sugar.tsx`. A dark-mode
palette is defined at the bottom of `styles.css`.
