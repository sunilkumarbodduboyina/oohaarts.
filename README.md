# Ooha Arts

A premium photography and photo-editing studio website — dark cinematic theme,
scroll choreography, an animated project gallery with a detail modal and
lightbox, and WhatsApp wired through every contact point.

Built with plain **HTML5, CSS3 and JavaScript**. No frameworks, no build step,
no external assets except the Google Fonts call (the site falls back to system
fonts without it).

---

## Project structure

```
oohaArts/
├── index.html            Home
├── editor.html           Editor introduction
├── about.html            About the studio
├── projects.html         Projects gallery + filters
├── services.html         Services, packages, process, FAQ
├── location.html         Address, hours, Google Maps
├── contact.html          Contact form → WhatsApp
│
├── css/
│   └── style.css         Whole design system (tokens → components → pages → motion)
│
├── js/
│   ├── config.js         ★ ALL editable settings, incl. the WhatsApp number
│   ├── data.js           Project archive + services (add projects here)
│   └── main.js           Scroll, gallery, modal, lightbox, form, WhatsApp
│
└── assets/
    └── img/              18 hand-built cinematic SVG images + favicon
```

---

## Changing the WhatsApp number

**One value, one file.** Open `js/config.js` and edit this line:

```js
window.OOHA_CONFIG = {
  WHATSAPP_NUMBER: '919876543210',   // ← change this
  ...
};
```

Format is **country code + number, digits only** — no `+`, spaces or dashes.
India `91XXXXXXXXXX` · UAE `971XXXXXXXXX` · USA `1XXXXXXXXXX`.

Everything else picks it up automatically:

- the floating button (bottom-right on every page)
- every green "WhatsApp" button in the header, hero, CTAs and footer
- the enquiry button inside every project modal, pre-filled with that project
- the contact form, which builds the message and hands it to WhatsApp
- the map / "book a visit" links

---

## Other settings (same file)

| Key | What it does |
| --- | --- |
| `WHATSAPP_DEFAULT_MESSAGE` | Opening line for links that don't set their own text |
| `BRAND`, `TAGLINE` | Name and strapline |
| `OWNER_NAME`, `OWNER_ROLE` | Fills the Editor page, About page and footer signature |
| `EMAIL`, `EMAIL_MAILTO` | Shown and linked anywhere an address appears |
| `PHONE_DISPLAY`, `PHONE_TEL` | Display form and the `tel:` link |
| `ADDRESS_LINE_1/2`, `PINCODE`, `CITY` | Footer, contact and location pages |
| `MAP_QUERY`, `MAP_EMBED_SRC`, `MAP_LINK` | The Google Map embed and the directions link |
| `SOCIAL` | Instagram / Behance / Pinterest / YouTube URLs |
| `HOURS` | Studio hours rows on the location page |

---

## Adding or editing a project

Everything in the gallery, the modal, the filters and the lightbox is generated
from `window.OOHA_PROJECTS` in `js/data.js`. Add an object with:

```js
{
  id: 'project-slug',
  title: 'Project Name',
  category: 'Editorial',        // must match a name in window.OOHA_FILTERS
  year: 2026,
  cover: 'assets/img/cover.svg',
  aspect: '3-2',                // 3-2 · 3-4 · 16-9 · 4-5 · 1-1 · 21-9
  span: 4,                      // columns out of 12 (use 6 or 12 for wide rows)
  featured: true,               // shows the gold "Featured" pill
  client: '…', location: '…',
  services: ['Photography', 'Colour grading'],
  summary: 'One line in the modal.',
  description: 'First paragraph.\n\nSecond paragraph.',
  images: [
    { src: 'assets/img/a.svg', caption: 'Plate 01 — …' },
    { src: 'assets/img/b.svg', caption: 'Plate 02 — …' }
  ]
}
```

Consecutive projects that share the same `aspect` are grouped into one grid row,
and each row is laid out to exactly 12 columns — so keep `span` values that add
up to 12 within a row. To change the home-page selection, edit
`window.OOHA_HOME` (a list of project ids).

---

## The project modal

Clicking any gallery tile opens a detail modal with:

- a large image stage with the full frame set
- **← / →** to move between frames, **`[` / `]`** to jump between projects
- title, category, year, client, location and what was delivered
- a **click-to-enlarge** lightbox with next/previous, keyboard arrows and
  swipe on touch
- an **Enquire on WhatsApp** button pre-filled with that project's name
- Esc to close, focus returns to the tile you opened

Gallery tiles are also hover-lift, zoom and desaturate-into-colour, and every
loose image across the site (`data-lb`) opens the same lightbox.

---

## Running it locally

Any static server works:

```bash
# from the project folder
python -m http.server 4321
# then open http://127.0.0.1:4321
```

Opening `index.html` directly from disk also works — there is no module or
`fetch` dependency.

---

## Notes

- Responsive at desktop, tablet (≤1080px) and mobile (≤700px) breakpoints.
- `prefers-reduced-motion` is respected: parallax, ken-burns, the animated
  grain and the custom cursor switch themselves off.
- The custom cursor only appears on fine pointers; the mobile drawer, the
  floating button and the form are all touch-friendly.
- With JavaScript disabled, a `<noscript>` rule hides the preloader and reveals
  the animated content.
- The Google Map is a plain embed (`output=embed`), so it needs no API key, and
  it is styled dark to match the theme.
