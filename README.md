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
index.html      home
faq.html        questions
styles.css      the whole design; palette matches the app
blog/
  index.html    post list
  *.html        one file per post
```

## Adding a blog post

Copy an existing post, change the content, and add an entry to the list in
`blog/index.html`. Posts are deliberately hand-written files rather than
generated from markdown — at this volume a build step would cost more than it
saves.

## Before launch

The App Store button is **inert on purpose** while the app is TestFlight-only.
It renders greyed as "Coming soon to the App Store".

When the app is approved, search for `aria-disabled="true"` (three places:
twice in `index.html`, once in `faq.html`), remove that attribute, and set
`href` to the App Store URL.

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
