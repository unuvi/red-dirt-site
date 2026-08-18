# Red Dirt Exterior Solutions — website

A single-page site. No frameworks, no build step, no npm. One HTML file and a folder of
media. You can edit it in any text editor.

```
index.html          the whole site — structure, styling and behaviour
images/             logo, photos, and the hero video
README.md           this file
```

---

## 1. The one thing you must change

Open `index.html`, scroll to the very bottom, and find this line inside the `<script>`:

```js
var CONTACT_EMAIL = 'REPLACE-WITH-YOUR-EMAIL@example.com';
```

Put your real email address there. The quote form opens the visitor's email app
addressed to whatever is on that line, so until you change it the form goes nowhere.

---

## 2. What's in `images/`

| File | What it is |
|---|---|
| `logo.png` | Your logo, background removed, for the white header |
| `logo-light.png` | Same logo recoloured white-and-red, for the dark footer and over the video |
| `hero.mp4` / `hero.webm` | The 8-second hero background loop, cut from the clip you sent |
| `hero-poster.jpg` | The still shown while the video loads, and instead of it on slow connections |
| `caleb.jpg` | You in the branded polo — the trust section |
| `band-driveway.jpg` | You with the surface cleaner and the truck — the full-width band |
| `before-1-house.jpg` / `after-1-house.jpg` | Comparison pair 1 — house front & driveway |
| `before-2-porch.jpg` / `after-2-porch.jpg` | Comparison pair 2 — covered porch & patio |
| `before-3-carport.jpg` / `after-3-carport.jpg` | Comparison pair 3 — carport slab |

All of these are your real photos. The comparison pairs were cropped to an identical
3:2 so the slider handle tracks across matching geometry.

Replace any file, **keep the exact same filename**, and the site picks it up with no
other edits.

---

## 3. The hero video

Your 23-second clip was trimmed to the best 4 seconds — the wand crossing the stained
concrete — and then mirrored so it plays forward, backward, forward, with no visible
jump at the loop point. Audio is stripped (browsers won't autoplay video with sound),
and it's encoded twice: WebM for Chrome and Firefox, MP4 for Safari. Together they're
about 1.6MB, which is small enough not to hurt the page on mobile data.

It's also handled carefully:

- Nothing downloads until the page decides it's worth it
- Visitors on Data Saver or a 2G connection get the poster image instead
- Visitors with reduced-motion turned on get the poster image instead
- The video pauses when it scrolls off screen so it isn't draining phone battery

### Swapping in different footage

Drop a new clip in and re-encode with these two commands (needs `ffmpeg`):

```bash
# trim to the best few seconds, then mirror it into a seamless loop
ffmpeg -ss 11.4 -t 4 -i YOUR-CLIP.mp4 -an -vf "scale=810:-2" -c:v libx264 -crf 30 seg.mp4
ffmpeg -i seg.mp4 -filter_complex "[0:v]split[a][b];[b]reverse[r];[a][r]concat=n=2:v=1" \
  -an -c:v libx264 -crf 30 -movflags +faststart images/hero.mp4
ffmpeg -i seg.mp4 -filter_complex "[0:v]split[a][b];[b]reverse[r];[a][r]concat=n=2:v=1" \
  -an -c:v libvpx-vp9 -crf 42 -b:v 0 images/hero.webm
ffmpeg -ss 11.4 -i YOUR-CLIP.mp4 -frames:v 1 -vf "scale=810:-2" images/hero-poster.jpg
```

Change `-ss 11.4` to wherever the good part of your clip starts. Keep it under about
2MB total or the page gets slow on phones.

---

## 4. The before/after photos

All three pairs are in and working. Two honest notes:

**Pair 1 (house front)** was shot from different distances at different times of day —
the before is wide and cloudy, the after is closer and in evening light. I cropped both
to match as closely as the source allows, but the slider is comparing two different
framings, and a careful visitor will notice.

**Pair 3 (carport)** is the strongest of the three, and the one I'd lead with if you
ever reorder them. Red dirt to bare concrete is exactly what your business name promises.

**When you shoot future pairs**, this is the whole trick: mark where you stand with a
chalk X or a cone. Take the before. Do the work. Stand on the same mark, same height,
same zoom, and take the after. Similar light for both — don't shoot the before in shade
and the after in full sun. Get that right and the slider does the selling for you.

Resize to about 1500px wide and save as JPG. A 6MB phone photo makes the page slow on
mobile data.

### Adding a fourth pair

Copy one whole `<article class="ba-item rv">…</article>` block in the Results section,
paste it after the third, change the two `src` filenames and the caption. The
JavaScript picks up any number of sliders automatically.

---

## 5. Things marked ⚠️ in the file

Search `index.html` for `⚠️` — there are two left:

1. **Pricing.** There's a spot to publish typical ranges if you're willing. Sites that
   show a range get noticeably more calls than sites that don't.
2. **The email address** (see section 1).

**Reviews are done.** Carson Berry, Brandy Harris and Zach Sutton are in, word for word
as you sent them, five stars each. Carson's leads because it's specific, mentions Yukon
by name, and names the things buyers actually worry about — showing up on time, easy to
communicate with, no stress.

Lucas's review is the one I left out. "Excellent best I have used yet" is kind, but with
no surname and no detail it reads thinner than the other three and would have weakened
the row. Swap it in any time by copying one of the three `<article class="card review">`
blocks.

One deliberate omission: there's **no review markup in the structured data**, and there
shouldn't be. Since 2019 Google ignores review snippets a business publishes about
itself, and marking them up anyway risks a manual action. Real review stars in search
results come from your Google Business Profile, not from your own website. Ask every
happy customer to leave one there — that's where it actually pays off.

Also check the hours — Mon–Sat 8am–6pm appears in three places: the box beside the
quote form, the footer, and the structured data near the bottom of the file.

---

## 6. Colours

The palette is defined once, at the top of the `<style>` block, so you can retune the
whole site from about ten lines:

```css
--maroon:  #8E1A1A   /* your logo red — buttons, headings, primary everything */
--clay:    #D6431E   /* the orange-red from your truck ad — secondary accent */
--ochre:   #B8820F   /* review stars, one of the four icon colours */
--water:   #146F8B   /* the cool accent, so the page isn't only warm tones */
--paper:   #FFFFFF   /* base */
--paper-2: #FAF6F2   /* alternating warm off-white sections */
--paper-3: #F4EDE6   /* the before/after section */
--dark:    #17110F   /* footer and the contact box */
```

Every text/background combination has been checked for WCAG AA contrast. If you change
these, re-check the ones you change — light red text on white is the usual casualty.

---

## 7. Putting it online

**Netlify Drop** is the fastest route and free:

1. Go to <https://app.netlify.com/drop>
2. Drag the whole `red-dirt-site` folder onto the page
3. It's live in about ten seconds at a random address
4. In Site settings you can point your own domain at it

Cloudflare Pages and GitHub Pages work identically. Any host that serves static files
will do — there's no server-side code to run.

### After it's live

- Replace `https://reddirtexteriorsolutions.com/` with your real domain everywhere it
  appears in `index.html` (canonical tag, social tags, and the two structured-data
  blocks near the bottom). Search and replace does it in one pass.
- Claim your **Google Business Profile** if you haven't. For a local service business it
  will out-perform this website for lead volume, and the two work together — make sure
  the business name, phone number and hours match this site *exactly*, including
  punctuation. Google treats "580-467-0087" and "(580) 467-0087" as a mismatch.
- Paste the live URL into <https://search.google.com/test/rich-results> to confirm the
  business and FAQ markup is being read.

---

## 8. The top of the page

Three stacked bars, the way the site you sent does it:

1. **Phone strip** — dark maroon, your number centred, tappable. Full width.
2. **White logo bar** — logo left, menu right. This one sticks to the top as you scroll.
3. **Service strip** — Concrete / Soft washing / Roof washing in bordered cells.

Then the hero: text sits directly on the video with a gradient behind it for legibility,
and the two big buttons — **Call Caleb** in white, **Get a free quote** in orange — run
edge to edge across the bottom of it. On a phone they land above the fold, so the first
screen a stranger sees has your number, what you do, and two ways to reach you.

To change the three service links in the strip, search `index.html` for
`class="strip"`.

---

## 9. What's already handled

- Mobile-first layout, tested down to 390px wide
- Sticky call/text bar that appears on phones once you scroll past the hero
- Every phone number is a real `tel:` link; the text buttons use `sms:`
- Before/after sliders work with mouse, touch **and** keyboard arrow keys
- Scroll-reveal animations that run once, and switch off for reduced-motion visitors
- `LocalBusiness` and `FAQPage` structured data for search results
- Open Graph tags so the link previews properly in a text message or on Facebook
- Visible focus outlines, labelled controls, and AA contrast throughout

## 10. Known trade-offs

- **Fonts load from Google Fonts.** One external request. If you'd rather have zero
  dependencies, download the Archivo family, drop the files in a `fonts/` folder and
  swap the `<link>` in the head for a `@font-face` block.
- **The form uses `mailto:`.** No signup, no monthly fee, nothing to maintain — but it
  depends on the visitor having an email app configured, and a few won't. That's why
  the call and text buttons sit right next to it. If you'd rather have a form that
  posts properly, Formspree's free tier takes about two minutes to wire up.
- **The hero video is portrait**, because that's how it was filmed. It fills a phone
  screen perfectly and gets cropped top and bottom on a desktop monitor. If you ever
  shoot replacement footage, film it landscape and it'll look sharper on desktop.
- **The four service cards use icons, not photos.** I have good photos of concrete work
  but nothing of a roof or a fence, and three photo cards next to one icon card looks
  worse than four icon cards. Send me a roof shot and a fence or deck shot and I'll
  switch all four to photographs — that's the single biggest remaining upgrade.
- **No reviews are real yet.** See section 5.
