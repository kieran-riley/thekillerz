<p align="center">
  <img src="images/band-shot.jpg" alt="The Killerz on stage" width="100%">
</p>

<h1 align="center">The Killerz</h1>

<p align="center">
  The UK's No&nbsp;1 tribute to The Killers. This repo holds the official website.<br>
  <a href="https://www.thekillerz.co.uk"><strong>thekillerz.co.uk</strong></a>
</p>

---

## Contents

- [What's here](#whats-here)
- [Running it locally](#running-it-locally)
- [Adding a gig](#adding-a-gig)
- [Gallery images](#gallery-images)
- [Styling and the CSS build](#styling-and-the-css-build)
- [Contact form](#contact-form)
- [Deploying](#deploying)

## What's here

A static, single-page site with no build step, apart from the Tailwind CSS (see below).

| Path | What it is |
| --- | --- |
| `index.html` | The whole page: hero slideshow, tour dates, gallery, about, band and contact sections |
| `main.js` | Loads the gigs, builds the tour ticker, mobile menu, gallery viewer and scroll effects |
| `gigs.json` | Tour dates. **This is the file you'll edit most** |
| `style.css` | Hand-written styles for things Tailwind can't express |
| `tailwind.css` | Precompiled Tailwind output. Don't edit it by hand |
| `images/` | Full-size photos; `images/thumbs/` holds the 800px gallery thumbnails |
| `CNAME` | Custom domain for GitHub Pages |

## Running it locally

Serve the folder rather than opening `index.html` directly. The tour dates are fetched
from `gigs.json`, and browsers block that on `file://`.

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Adding a gig

Add an entry to [`gigs.json`](gigs.json):

```json
{ "date": "14-03-2027", "venue": "The Roadhouse", "city": "Birmingham", "location": "England", "ticket_link": "" }
```

- **`date`** is `DD-MM-YYYY`. A gig stays on the site through the day of the show, then drops off by itself.
- **`ticket_link`**: leave it empty to show "Coming Soon" instead of a Tickets button.
- The scrolling ticker under the header picks up new dates automatically.

## Gallery images

The grid loads small thumbnails from `images/thumbs/`. Clicking one opens the full-size
original from `images/`. After adding a photo to the gallery in `index.html`, make its thumbnail:

```bash
# macOS
sips -Z 800 -s format jpeg -s formatOptions 72 images/new-photo.jpg --out images/thumbs/new-photo.jpg

# Linux / anywhere with ImageMagick
magick images/new-photo.jpg -resize 800x800 -quality 72 images/thumbs/new-photo.jpg
```

## Styling and the CSS build

Tailwind is precompiled into `tailwind.css`. **If you add or change a Tailwind class in
`index.html` or `main.js`, rebuild it**, or the new class won't have any styles:

```bash
npx tailwindcss@3 -c tailwind.config.js -i tailwind-input.css -o tailwind.css --minify
```

`style.css` loads after Tailwind, so it wins on conflicts. It covers the slideshow, gig cards,
band cards, form fields and the gallery viewer.

> **Cache busting:** stylesheets are linked with a `?v=YYYYMMDD` query in `index.html`.
> Bump it when you change the CSS so phones don't keep serving an old copy.

## Contact form

The form posts to [FormSubmit](https://formsubmit.co) and delivers enquiries to
`info@thekillerz.co.uk`. After deploying it for the first time, send one test enquiry and
click the activation link FormSubmit emails to that inbox. Nothing is delivered until that
one-time confirmation is done.

## Deploying

The site is served by GitHub Pages from the `main` branch on the custom domain in `CNAME`.
Pushing to `main` publishes it, usually within a minute or two.
