# Cloud.md — Shivansh's Graduation Party Invite

## Project Overview

Build a **single self-contained HTML file** — a premium digital party invitation + RSVP page for Shivansh Bansal's graduation celebration. The theme is **"The World Awaits"** — adventurous, global, aspirational. The visual language blends a starry night sky with travel/explorer motifs: compass roses, passport stamps, dotted flight paths, globe outlines, and world map silhouettes. Think luxury airline lounge meets celestial observatory.

**Deliverable:** One `.html` file. All CSS and JS inline. No frameworks, no build tools, no npm. Only external dependency: Google Fonts CDN. Deployable as a static page on GitHub Pages, Netlify Drop, AWS S3, or Vercel with zero configuration.

---

## Party Details

| Field | Value |
|---|---|
| **Guest of Honor** | Shivansh Bansal |
| **Occasion** | Graduation Party |
| **Theme** | "The World Awaits" |
| **Tagline** | "One chapter closes. The whole world opens." |
| **Date** | Sunday, August 2nd, 2026 |
| **Time** | 5:30 PM onwards |
| **Venue** | Royale Sakoon |
| **Venue Address** | 5200 Mowry Ave Suite K, Fremont, CA 94538 |
| **Google Maps Link** | `https://maps.google.com/?q=Royale+Sakoon+5200+Mowry+Ave+Suite+K+Fremont+CA+94538` |
| **Dress Code** | Indo-Western |
| **Hosted By** | Sanjeev, Pooja & Shanaya Bansal |
| **RSVP Contact Email** | *(placeholder — owner will fill in)* |
| **RSVP Contact Phone** | *(placeholder — owner will fill in)* |

---

## Color Palette

Navy + gold + silver. The palette should feel like a midnight atlas illuminated by candlelight.

```css
:root {
  --navy:         #060e22;       /* deep dark background */
  --navy-mid:     #0c1a3a;       /* slightly lighter navy for cards */
  --gold:         #c9a84c;       /* primary gold accent */
  --gold-light:   #e8d08a;       /* lighter gold for headings */
  --gold-pale:    #f5e9c0;       /* palest gold — hover states */
  --silver:       #c0c8d8;       /* silver accent — secondary to gold */
  --silver-light: #dce3ed;       /* light silver for subtle elements */
  --crystal:      #b8d4f0;       /* soft blue accent */
  --text:         #e8e0d0;       /* warm off-white body text */
  --muted:        #7a8aaa;       /* muted grey-blue for captions */
  --success:      #5cb85c;       /* green for form success */
}
```

---

## Typography (Google Fonts)

```
Cinzel            — Display headings, labels, uppercase tracked text, navigation
Cormorant Garamond — Body/paragraph text, descriptions, italic details
Great Vibes        — Cursive script: name, "With Love", "Kindly Reply", tagline
Inter              — UI fallback: form inputs, buttons, small labels
```

Import URL:
```
https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600;700;900&family=Cormorant+Garamond:ital,wght@0,400;0,600;1,400;1,600&family=Great+Vibes&family=Inter:wght@400;500;600&display=swap
```

---

## Background Effects (all pure CSS + Canvas)

Layer these from bottom to top with `position: fixed` and increasing `z-index`:

### Layer 0 — Deep Radial Glow
`div.bg-glow` — full-screen fixed div with layered `radial-gradient` in navy/deep blue tones. Creates a subtle vignette that's darker at edges, slightly warmer/lighter near center-bottom.

### Layer 1 — Static Star Field
`div.stars-static` — uses many small `radial-gradient(1px 1px ...)` positioned pseudo-randomly. ~120 star positions. White and very pale gold dots.

### Layer 2 — Twinkling Stars (two layers)
- `div.stars-twinkle` — 40–50 stars, `@keyframes twinkleA` opacity 0.3→1 over 5s, `alternate-reverse`, `ease-in-out`
- `div.stars-twinkle-2` — 30–40 stars, `@keyframes twinkleB` opacity 0.2→0.8 over 7s, offset timing

### Layer 3 — Shooting Stars
`div.shoot-wrap` containing 4 `div.shooting-star` elements. Each is a thin `linear-gradient` line (gold-to-transparent) animated with `@keyframes shoot1–shoot4`:
- Translate diagonally across the screen
- Stagger: durations 3s/4s/3.5s/5s, delays 0s/7s/12s/18s
- Total cycle feels natural, not synchronized

### Layer 4 — Floating Particles Canvas
`<canvas id="particles-canvas">` — full-screen fixed, z-index 0.
- ~80 small circular particles drifting slowly upward
- Colors: mix of white, gold (#e8d08a), and silver (#c0c8d8) with varying opacity
- Gentle sine-wave horizontal drift
- Particles wrap when they leave the viewport
- Vanilla Canvas 2D API, `requestAnimationFrame` loop

### Layer 5 — Fireworks Canvas (hidden until RSVP submit)
`<canvas id="fireworks-canvas">` — fixed full-screen, z-index 1.
- Triggered by calling `window.launchFireworks()`
- Burst: random origin, 30–50 particles per burst, radiate outward with gravity + friction
- Gold and silver color palette for burst particles
- Multiple bursts staggered with `setTimeout`
- Canvas cleared each frame with semi-transparent rect for trail effect
- Runs for ~5 seconds then stops

---

## Theme-Specific Animations & Motifs

These are **in addition** to the starry night background — they tie the "World Awaits" travel theme into the visual design:

### A. Dotted Flight Path SVG
An animated SVG arc (curved dotted line) that traces across the hero section behind the name. Uses `stroke-dasharray` and `stroke-dashoffset` animation to "draw" a flight path on scroll or on page entry. A small airplane silhouette (✈ or inline SVG) follows the end of the path. Color: gold with low opacity.

### B. Compass Rose Ornament
Replace generic `✦` ornaments with a small inline SVG compass rose (N/S/E/W). Used as:
- The header ornament above the hero
- Divider ornaments between sections
- Subtle watermark behind the RSVP form
Gold stroke, no fill, ~40–60px.

### C. Rotating Globe (CSS only)
A subtle CSS-only wireframe globe animation in the welcome overlay background. Built with circular `border` elements and `@keyframes rotate` (slow spin, 30s duration, linear, infinite). Very low opacity (0.08–0.12) so it reads as atmosphere, not a focal point. Silver/crystal color.

### D. Passport Stamp Effect on RSVP Success
When the user submits their RSVP, in addition to fireworks, show a "passport stamp" overlay:
- A circular/rectangular bordered stamp graphic (CSS borders + rotated text)
- Text inside: "CONFIRMED ✦ AUG 2 2026 ✦ FREMONT, CA"
- Animated: scales from 0 → 1 with a slight overshoot (`cubic-bezier(0.175, 0.885, 0.32, 1.275)`), rotates ~-12deg
- Red-ish ink color (#c94c4c) or gold — whichever looks better against the navy

### E. Floating World Landmark Silhouettes (optional, subtle)
In the particle canvas, occasionally mix in tiny (~8–12px) silhouette shapes of world landmarks (Eiffel Tower, Taj Mahal, Statue of Liberty, Big Ben — keep to 4–5 simple path shapes). Very low opacity (0.15), drift upward with the particles. These should be barely noticeable — an easter egg, not a distraction.

---

## Page Structure (top to bottom)

### 1. Welcome/Intro Overlay (`#welcome-overlay`)

Full-screen fixed overlay, `z-index: 9999`, navy background.

Contents:
- Background: rotating globe wireframe (CSS) + twinkling stars
- Top: small compass rose SVG ornament
- Main title: **"The World Awaits"** in `Great Vibes`, `clamp(42px, 10vw, 72px)`, gold gradient text (`#f5e9c0 → #c9a84c → #f5e9c0`)
- Subtitle: **"You are invited to celebrate Shivansh's graduation"** in `Cinzel`, spaced uppercase, silver
- Tagline: **"One chapter closes. The whole world opens."** in `Cormorant Garamond`, italic, muted color
- Enter button: **"✦ Enter ✦"** — gold pill button with pulsing `box-shadow` glow animation (`@keyframes welcomeBtnPulse`, 2s, infinite)
- Skip button: **"SKIP INTRO ›"** — tiny `Cinzel`, muted gold, below enter button

**JS behavior:**
```js
function enterRSVP() {
  overlay.classList.add('fade-out'); // CSS opacity 1→0, transition 1.2s
  setTimeout(() => overlay.remove(), 1300);
  // Start background music if an audio element exists
}
```

### 2. Invite Header (`.invite-header`)
- Centered compass rose SVG ornament (~50px)
- Gold colored, `filter: drop-shadow(0 0 12px rgba(201,168,76,0.4))`

### 3. Hero Section (`.invite-hero`)

- **Eyebrow:** "You are invited to celebrate" — `Cinzel`, 12px, spaced uppercase (`letter-spacing: 4px`), `--muted` color
- **Name:** "Shivansh Bansal" — `Cinzel`, `clamp(36px, 8vw, 64px)`, `font-weight: 700`, gold gradient, `letter-spacing: 4px`, `text-shadow: 0 0 40px rgba(201,168,76,0.3)`
- **Animated flight path SVG** — arcs behind/below the name, draws in on load
- **Milestone line:** Horizontal rule + graduation cap emoji (🎓) + horizontal rule — flex layout, gold color
- **Occasion:** "Graduation Celebration" — `Cormorant Garamond`, italic, `clamp(20px, 4vw, 28px)`, `--text` color
- **Date line:** "Sunday, August 2nd, 2026  ✦  Fremont, California" — `Cinzel`, 13px, spaced uppercase, `--silver` color

### 4. Countdown Timer (`.countdown`)

Container: `.page` (max-width: 700px, centered, padding: 0 24px)

4 glassmorphism cards in a row (`.cd-unit`):
```css
.cd-unit {
  background: rgba(10, 20, 50, 0.65);
  border: 1px solid rgba(201, 168, 76, 0.2);
  backdrop-filter: blur(6px);
  border-radius: 12px;
  padding: 20px 16px;
  min-width: 70px;
  text-align: center;
}
```
- Large number: `Cinzel`, 32px, `--gold-light`
- Label: `Cinzel`, 9px, `letter-spacing: 3px`, uppercase, `--muted`

**JS:**
```js
const target = new Date('2026-08-02T17:30:00-07:00'); // 5:30 PM PDT
(function tick() {
  const diff = target - Date.now();
  if (diff <= 0) { /* show "The celebration has begun!" */ return; }
  const d = Math.floor(diff / 86400000);
  const h = Math.floor((diff % 86400000) / 3600000);
  const m = Math.floor((diff % 3600000) / 60000);
  const s = Math.floor((diff % 60000) / 1000);
  document.getElementById('cd-days').textContent = String(d).padStart(2, '0');
  document.getElementById('cd-hours').textContent = String(h).padStart(2, '0');
  document.getElementById('cd-mins').textContent = String(m).padStart(2, '0');
  document.getElementById('cd-secs').textContent = String(s).padStart(2, '0');
  setTimeout(tick, 1000);
})();
```

### 5. Details Cards (`.details`)

3-column flex row (wraps on mobile to single column):

| Card | Icon | Label | Value |
|------|------|-------|-------|
| Time | 🕠 | WHEN | 5:30 PM Onwards |
| Dress Code | 👗 | ATTIRE | Indo-Western |
| Venue | 📍 | WHERE | Royale Sakoon *(linked to Google Maps)* |

Card styling matches countdown units — glassmorphism, gold border, hover: `transform: translateY(-4px)` with `transition: 0.3s ease`.

### 6. Add to Calendar Button

`a.cal-btn` — gold outlined pill button, centered.

**JS generates .ics file:**
```
BEGIN:VCALENDAR
VERSION:2.0
PRODID:-//ShivanshGradParty//EN
BEGIN:VEVENT
DTSTART:20260802T173000
DTEND:20260802T230000
SUMMARY:Shivansh's Graduation Party — The World Awaits
DESCRIPTION:Join us to celebrate Shivansh Bansal's graduation! Dress code: Indo-Western. Hosted by Sanjeev, Pooja & Shanaya Bansal.
LOCATION:Royale Sakoon, 5200 Mowry Ave Suite K, Fremont, CA 94538
END:VEVENT
END:VCALENDAR
```
Create as `Blob('text/calendar')`, set `href` and `download="shivansh-grad-party.ics"`.

### 7. Divider
`div.divider > span` — compass rose icon or `✦`, thin gold horizontal rule with centered ornament.

### 8. RSVP Form Card (`.form-card`)

```css
.form-card {
  background: rgba(10, 20, 50, 0.55);
  border: 1px solid rgba(201, 168, 76, 0.25);
  backdrop-filter: blur(8px);
  border-radius: 16px;
  padding: 48px 36px;
  max-width: 560px;
  margin: 0 auto;
}
```

- **Title:** "Kindly Reply" — `Great Vibes`, 46px, `--gold-light`
- **Subtitle:** "PLEASE RESPOND BY JULY 20, 2026" — `Cinzel`, 10px, spaced, `--muted`

**Form fields:**

| Field | Type | Name | Details |
|-------|------|------|---------|
| First Name | text | `first_name` | required, side-by-side with Last Name |
| Last Name | text | `last_name` | required |
| Email | email | `email` | required, full width |
| Phone | tel | `phone` | optional, full width |
| Attendance | select | `attendance` | Options: "Joyfully Accepts ✦" (value: `yes`), "Regretfully Declines" (value: `no`) |
| Number of Guests | number | `guests` | min=1, max=10, default=1, note: "including yourself" |
| Dietary Restrictions | text | `dietary` | optional, placeholder: "Vegetarian, allergies, etc." |
| Message | textarea | `message` | optional, placeholder: "A note for Shivansh..." |
| Honeypot | text | `bot-field` | `display: none` — spam protection |
| Subject (hidden) | hidden | `subject` | value: "RSVP — Shivansh's Graduation Party" |

**Input styling:**
```css
.form-card input, .form-card select, .form-card textarea {
  background: rgba(6, 14, 34, 0.7);
  border: 1px solid rgba(201, 168, 76, 0.2);
  border-radius: 8px;
  color: var(--text);
  font-family: 'Cormorant Garamond', serif;
  font-size: 16px;
  padding: 14px 16px;
  width: 100%;
  transition: border-color 0.3s;
}
.form-card input:focus, .form-card select:focus, .form-card textarea:focus {
  outline: none;
  border-color: var(--gold);
}
```

**Submit button:**
```css
.submit-btn {
  width: 100%;
  padding: 16px;
  background: linear-gradient(135deg, var(--gold), #b8952e);
  color: var(--navy);
  font-family: 'Cinzel', serif;
  font-weight: 700;
  font-size: 14px;
  letter-spacing: 3px;
  text-transform: uppercase;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}
```
Text: **"Send RSVP ✦"**

### 9. Success Message (`#success-msg`)

Hidden until form submission. Replaces the form.

**For "Joyfully Accepts":**
- Passport stamp animation (CSS, see Theme-Specific Animations §D)
- 🥂 emoji or ✈ emoji (large, 48px)
- Heading: "Bon Voyage — See You There!" in `Great Vibes`, gold
- Body: "Your RSVP has been received. We can't wait to celebrate with you on August 2nd!"

**For "Regretfully Declines":**
- No passport stamp, no fireworks
- Heading: "You'll Be Missed!" in `Great Vibes`, silver
- Body: "Thank you for letting us know. We'll raise a glass in your honor."

### 10. Footer (`.footer`)

- "With Love" — `Great Vibes`, 38px, `--gold-light`, `filter: drop-shadow(0 0 20px rgba(201,168,76,0.3))`
- "THE BANSAL FAMILY" — `Cinzel`, 12px, `letter-spacing: 5px`, `--gold`
- "Sanjeev, Pooja & Shanaya" — `Cormorant Garamond`, 16px, `--silver`
- Contact links (phone and email) in `Cinzel`, 11px, `--muted`, `a:hover` → `--gold`
- Use `tel:` and `mailto:` href values — **leave phone/email as placeholders for the owner to fill in**

---

## RSVP Backend — Google Apps Script + Google Sheets

This is the free, zero-server backend for collecting RSVPs.

### Setup Steps (for the owner)

1. **Create a Google Sheet** at sheets.google.com
   - Name it "Shivansh Grad Party RSVPs"
   - Add header row: `Timestamp | First Name | Last Name | Email | Phone | Attendance | Guests | Dietary | Message`

2. **Create a Google Apps Script**
   - Go to `Extensions > Apps Script` from the sheet (or script.google.com)
   - Paste this code:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = e.parameter;

    // Honeypot check
    if (data['bot-field'] && data['bot-field'].length > 0) {
      return ContentService.createTextOutput(JSON.stringify({status: 'ok'}))
        .setMimeType(ContentService.MimeType.JSON);
    }

    sheet.appendRow([
      new Date().toISOString(),
      data.first_name || '',
      data.last_name || '',
      data.email || '',
      data.phone || '',
      data.attendance || '',
      data.guests || '',
      data.dietary || '',
      data.message || ''
    ]);

    // Optional: send confirmation email
    if (data.email && data.attendance === 'yes') {
      try {
        GmailApp.sendEmail(
          data.email,
          "You're Confirmed! — Shivansh's Graduation Party 🎓",
          "Hi " + data.first_name + ",\n\n" +
          "We've received your RSVP! The World Awaits on August 2nd.\n\n" +
          "📍 Royale Sakoon, 5200 Mowry Ave Suite K, Fremont, CA 94538\n" +
          "🕠 5:30 PM onwards\n" +
          "👗 Dress code: Indo-Western\n\n" +
          "See you there!\n" +
          "— The Bansal Family"
        );
      } catch (emailErr) {
        // Email send failed — log but don't break the RSVP
        Logger.log('Email error: ' + emailErr);
      }
    }

    return ContentService.createTextOutput(JSON.stringify({status: 'ok'}))
      .setMimeType(ContentService.MimeType.JSON);

  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({status: 'error', message: err.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

// Handle CORS preflight
function doGet(e) {
  return ContentService.createTextOutput(JSON.stringify({status: 'ok', message: 'RSVP endpoint active'}))
    .setMimeType(ContentService.MimeType.JSON);
}
```

3. **Deploy as Web App**
   - Click `Deploy > New deployment`
   - Type: Web app
   - Execute as: **Me**
   - Who has access: **Anyone**
   - Click Deploy, authorize, copy the `/exec` URL

4. **Paste the URL** into the HTML file:
```js
const SHEETS_URL = 'YOUR_GOOGLE_APPS_SCRIPT_URL_HERE';
```

### Frontend Form Submission JS

```js
const SHEETS_URL = 'YOUR_GOOGLE_APPS_SCRIPT_URL_HERE';

document.getElementById('rsvp-form').addEventListener('submit', async (e) => {
  e.preventDefault();

  const btn = e.target.querySelector('.submit-btn');
  const originalText = btn.textContent;
  btn.textContent = 'Sending...';
  btn.disabled = true;

  const formData = new FormData(e.target);

  try {
    await fetch(SHEETS_URL, {
      method: 'POST',
      body: formData
    });

    // Hide form, show success
    document.getElementById('rsvp-form').style.display = 'none';
    const successMsg = document.getElementById('success-msg');
    successMsg.style.display = 'block';

    const attendance = formData.get('attendance');
    if (attendance === 'yes') {
      // Show passport stamp + fireworks
      document.getElementById('passport-stamp').classList.add('stamped');
      if (window.launchFireworks) window.launchFireworks();
    } else {
      // Show decline message variant
      successMsg.querySelector('h2').textContent = "You'll Be Missed!";
      successMsg.querySelector('p').textContent =
        "Thank you for letting us know. We'll raise a glass in your honor.";
      // Hide passport stamp for declines
      const stamp = document.getElementById('passport-stamp');
      if (stamp) stamp.style.display = 'none';
    }
  } catch (err) {
    btn.textContent = originalText;
    btn.disabled = false;
    alert('Something went wrong — please try again or contact us directly.');
  }
});
```

---

## Hosting Instructions

This is a **single static HTML file** — no server required.

### Option A: GitHub Pages (recommended, free)
1. Create a repo (e.g. `shivansh-grad-invite`)
2. Add the HTML file as `index.html`
3. Settings → Pages → Source: main branch → Save
4. Access at `https://username.github.io/shivansh-grad-invite/`
5. Use a URL shortener (rebrand.ly, bit.ly) for a clean share link

### Option B: Netlify Drop (fastest, free)
1. Go to `app.netlify.com/drop`
2. Drag the HTML file in
3. Get a live URL instantly
4. Optionally set a custom subdomain

### Option C: AWS S3 Static Website
1. Create an S3 bucket
2. Upload the HTML file
3. Enable static website hosting
4. Set bucket policy to public read
5. Access via the S3 website endpoint

### Option D: Vercel
1. `npx vercel` in the folder containing the file, or drag-drop in the Vercel dashboard

**After hosting,** create a clean short link like `rebrand.ly/shivansh-grad` to share via text/WhatsApp.

---

## Responsive Design Requirements

- **Mobile first.** The invite will primarily be opened on phones via text/WhatsApp links.
- Welcome overlay: full-screen, buttons large enough for thumb tapping (min 48px touch target)
- Hero name: `clamp()` for fluid typography
- Countdown cards: flex-wrap, 2×2 grid on very narrow screens
- Detail cards: stack to single column below 600px
- Form: single column throughout, generous padding
- All touch targets ≥ 44px
- Test at 375px (iPhone SE), 390px (iPhone 14), and 768px (iPad)

---

## Performance Notes

- No images to load (all visual effects are CSS + canvas)
- Google Fonts loaded with `display=swap` to prevent FOIT
- Canvas animations should use `requestAnimationFrame` and stop when tab is not visible (`document.hidden`)
- Fireworks canvas only activates on RSVP submit — not running in background
- Total file size target: under 30KB

---

## Accessibility Baseline

- All form inputs have associated `<label>` elements
- Color contrast: gold on navy passes WCAG AA for large text
- Form error states: red border + text message (not just color)
- Reduced motion: wrap heavy animations in `@media (prefers-reduced-motion: no-preference)` — users with motion sensitivity see a static version
- Skip to content: welcome overlay has skip button
- `aria-live="polite"` on success message container

---

## File Checklist for Claude Code

When building, ensure the final HTML file contains all of these in a single file:

- [ ] `<style>` block with all CSS (variables, backgrounds, typography, layout, animations, responsive)
- [ ] Google Fonts `<link>` in `<head>`
- [ ] Welcome overlay with enter/skip buttons
- [ ] Starry night background layers (glow, static stars, twinkle layers, shooting stars)
- [ ] Particles canvas with floating gold/silver/white dots
- [ ] Fireworks canvas (inactive until triggered)
- [ ] Compass rose SVG ornaments (header + dividers)
- [ ] Flight path SVG animation in hero
- [ ] Rotating wireframe globe in welcome overlay
- [ ] Hero section with name, milestone, occasion, date
- [ ] Countdown timer (targeting Aug 2, 2026 5:30 PM PDT)
- [ ] 3 detail cards (time, dress code, venue)
- [ ] Add to Calendar button (.ics download)
- [ ] RSVP form with all fields
- [ ] Passport stamp animation on RSVP success
- [ ] Success/decline message states
- [ ] Footer with "With Love" + Bansal family + contact placeholders
- [ ] `SHEETS_URL` placeholder for Google Apps Script
- [ ] Honeypot spam field
- [ ] `prefers-reduced-motion` media query
- [ ] Mobile responsive (375px–768px+)