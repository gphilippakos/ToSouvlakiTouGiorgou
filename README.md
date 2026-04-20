# Το Σουβλάκι του Γιώργου — Website

Single-page marketing website for the Gythio souvlaki restaurant, with the menu, story, and visit info baked in. One HTML file, one hero image, deployable to Vercel in seconds.

**Languages:** English · Ελληνικά · Deutsch · Français · Español · Italiano · Português (7 total)

## Project structure

```
index.html           Entire site — CSS + translations + menu all inline
images/
  hero.jpg           Hero photo (swap to change the landing image)
tools/
  generate-qr.js     QR-code generator
  optimize-images.js Resize/compress images in images/
  make-tent.js       A5 table-tent PDF generator
  package.json       Dev dependencies for the tools
vercel.json          Deploy config (caching headers)
```

## Deploy to Vercel

From the project folder:

```
npm i -g vercel       # only the first time
vercel --prod         # production deploy
```

Or connect the folder to a Git repo and import it in the Vercel dashboard — zero config needed.

After deploy, Vercel gives you a URL like `https://to-souvlaki-tou-giorgou.vercel.app`. That's what the QR code points to.

## Modifying content

Everything lives in **one file**: [index.html](index.html). Two places to know:

### To change a translation

Find the JavaScript block near the bottom of `index.html`:

```
const TRANSLATIONS = {"en": {...}, "el": {...}, "de": {...}, ...};
```

Each language is a dictionary of `key → text`. To change English "See the Menu", search for `"hero_btn_menu"` and edit its value in the `en` object. To change the Greek version, edit the same key in the `el` object.

**All 7 languages share the same key set.** If you add a new key, add it to every language (or at least `en`, which is the fallback).

### To change a price or menu item

Prices live in the HTML markup (not the translations). Search for `<span class="price">` and edit the number. For example:

```html
<span class="price">4.80</span>
```

If you want to rename the dish, edit the `data-i18n="item_..."` span text — or, better, update the corresponding key in `TRANSLATIONS` so every language stays consistent.

### To change the hero photo

Replace `images/hero.jpg` with a new JPG (any size, will be displayed as-is). For best results:
- Landscape orientation
- ~1200 × 800 px
- Under 200 KB (run `npm run optimize` in `tools/` to auto-compress)

### To change the restaurant name or location

Look for these in `index.html`:
- `<span>Το Σουβλάκι του Γιώργου · Gythio</span>` — nav and footer brand
- `<span class="brand-mark">Γ</span>` — the circular letter mark
- `Τζαννή Τζαννετάκη` / `Γύθειο · Gythio 232 00` — address in the Visit section

### To change opening hours, phone, or map link

Search for these in `index.html`:
- `Mon – Thu`, `Fri – Sat`, `Sunday` — hours table
- `tel:+30` — phone href
- `maps.app.goo.gl/...` — Google Maps link (appears twice)

## Tools

### Generate a QR code

From the project folder:

```
cd tools
npm install
npm run qr -- https://your-deployed-url.vercel.app
```

Outputs `qr/menu-qr.png` (for digital use) and `qr/menu-qr.svg` (for print). Uses highest error-correction level — still scans even with a logo overlay or smudges.

### Generate an A5 table-tent PDF

After the QR exists:

```
cd tools
npm run tent
```

(Or set a custom URL: `MENU_URL=souvlakitougiorgou.gr npm run tent`.)

Outputs `qr/menu-tent-a5.pdf` — print-ready A5 card with the brand, "Scan for menu" in 6 languages, and the QR. Print on 250–300 gsm cardstock.

### Optimize images

If you add new photos to `images/`, run:

```
cd tools
npm run optimize
```

Resizes everything to 800 × 800 max and re-encodes as JPG at quality 80 (typically ~85% file size savings).

## Known items to finalize before launch

1. **Phone number** — currently placeholder `tel:+30` with "call the shop". Search `data-i18n="visit_phone_val"` in `index.html`.
2. **Social links** — Instagram/Facebook links in the Visit section are `href="#"`. Replace or remove.
3. **Privacy link** — footer has `href="#"` for Privacy. Either write a `/privacy.html` page or remove the link.
4. **Favicon** — no favicon set; browser shows a default globe. Add a `favicon.ico` and a `<link rel="icon">` in the head.
5. **OpenGraph tags** — no meta tags for social sharing previews. When someone sends the URL in WhatsApp it'll show no image. Add OG tags if sharing matters.

## License / ownership

Site is bespoke for the restaurant. Images are not commercially licensed stock — confirm rights before using on other platforms (printed materials, paid ads, etc.).
