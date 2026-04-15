# DESIGN.md — Time Killed v3 (Updated from Figma overhaul)

## What changed from v2

This is a significant visual overhaul. Key differences:
- Tier sections are now color-coded backgrounds (not text dividers)
- Cards are center-column (654px wide), not full-width
- Bar charts moved into their own rounded white cards below each comparison
- Left/right alternation applies to the whole card layout
- New "And FINALLY" closing section with dark olive background, rotated 2deg, large type
- Decorative spiral vector graphics bleed in from the right on each section

---

## Color Palette

```css
--white:        #ffffff   /* daily section bg, card backgrounds */
--lime-light:   #ebffc2   /* weekly section background */
--lime:         #d3ff7c   /* yearly section bg + accent everywhere */
--olive-dark:   #404339   /* "And FINALLY" closer section bg */
--dark-badge:   #232520   /* count badge background */
--bar-task:     #d3ff7c   /* task duration bar fill */
--bar-time:     #434549   /* your screen time bar fill */
--gray-mid:     #888880   /* body text, chart border, input placeholder */
--gray-light:   #d8d5cc   /* dividers */
--black:        #0f0f0d   /* primary text */
--input-bg:     #f6f9f2   /* hour/minute field backgrounds */
```

---

## Typography

```
Instrument Serif — all display, headings, badge numbers, input numbers
Instrument Sans  — all body copy, labels, bar chart labels
```

Google Fonts:
```
https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=Instrument+Sans:ital,wdth,wght@0,75..125,400..700;1,75..125,400..700&display=swap
```

Type scale:
- "INSTEAD!" hero display: 128px serif, letter-spacing: 2.56px
- Hero eyebrow: 48px serif
- Section headers: 72px serif, line-height 76px, tracking -1.44px
- "And FINALLY,": 128px serif, white, tracking -2.56px, line-height 76px
- Activity titles: 36px serif, line-height 42px, tracking -0.72px
- Body copy: 16px sans weight 400, color #888880, tracking -0.64px
- Badge number: 64px serif + "Times" at 36px, color #d3ff7c
- Final badge "4 days": 96px + 64px serif, color #d3ff7c
- Bar labels: 16px sans weight 400, color #0f0f0d, fixed width 150px
- Input numbers: 48px serif, color #888880

---

## Section Structure (top to bottom)

### Section 1 — Hero (bg: white, height ~873px)
- "Oh, The Things You Could Be Doing" — 48px serif centered
- "INSTEAD!" — 128px serif centered, tracking 2.56px
- Seuss spiral illustration — absolute, left: 0, top: 337px, 536×536px (Figma asset imgImage2)
- Time breakdown list (Hours / Minutes / Seconds / Milliseconds) — 20px serif, positioned left of input
- "Enter Your Screen Time" label — 16px sans, above input
- Input widget — centered, lime offset shadow rotated -2.49deg, skewX -0.88deg
  - Hours field: 48px serif, #888880, bg #f6f9f2, padding 7px 72px
  - Colon separator (Figma asset imgFrame6)
  - Minutes field: same as hours, width 200px
  - Arrow button: → at 40px, border #999, color #888880

### Section 2 — Daily (bg: white)
- Padding: 64px sides, 24px top/bottom
- Sticky time display: top-right, smaller version of input widget
- "In this time, you could have" — 72px serif centered
- Comparison cards: 654px wide, centered, 96px gap between cards
- Spiral vector (imgVector): absolute right bleed

### Section 3 — Weekly (bg: #ebffc2)
- Same card layout
- "Now imagine this repeats every day for a WEEK..." — 72px serif, line-height 76px
- Spiral vector (imgVector1): absolute right, rotated -3deg
- White collage placeholder boxes: 508×437px, alternating left/right

### Section 4 — Yearly (bg: #d3ff7c)
- "...or for a YEAR" — 72px serif centered
- Spiral vector (imgVector2): absolute right, rotated -3deg
- Collage placeholders: 508×437px alternating, bg #ebffc2 and white

### Section 5 — "And FINALLY" (bg: #404339, entire section rotate(2deg))
- "And FINALLY," — 128px serif, white, centered, line-height 76px
- Large badge (378px wide, rotate(-6deg)): "4" at 96px + "  days" at 64px, #d3ff7c on #232520
- "doing something you love" — 72px serif, white, centered
- Spiral vector (imgVector3): absolute left: -476px, rotate(-45.22deg)

### Section 6 — End closer (bg: white, height: 740px)
- "it's not too late.." — 48px serif, left ~25% from center, top 152px
- "you can still" — 48px serif, right ~50% from center, top 272px
- Lime bar — absolute, 959px wide, centered, top 494px, bg #d3ff7c, border black
  - "FIX YOUR ATTENTION SPAN" — 96px serif, underlined, centered
- "today." — 48px serif, far right, top 563px

---

## Comparison Card Layout

### Left-variant (odd index — 1st, 3rd, 5th...)
```
[Title left (407px)]     [Badge right (-6deg)]
[Description left  ]
[Bar Chart Card — 654px wide, left-anchored bars]
```

### Right-variant (even index — 2nd, 4th, 6th...)
```
[Badge left (+6deg)]     [Title right (407px)]
                         [Description right  ]
[Bar Chart Card — 654px wide, right-anchored bars]
```

Card gap: 96px between cards
Title/description gap: 24px
Card to chart gap: 36px

---

## Bar Chart Card

```css
background: white;
border: 1px solid #888880;
border-radius: 16px;
backdrop-filter: blur(2px);
width: 654px;
padding: 24px 0;
```

Row structure (padding: 8px 24px, gap: 19px):
- Label: 150px fixed width, 16px sans, #0f0f0d
- Bar: height 20px, border 1px solid black, dynamic width

Left-variant: label → bar (bars grow rightward)
Right-variant: bar → label (bars grow leftward, justify-content: flex-end)

Task bar color: #d3ff7c
Time bar color: #434549

---

## Count Badge

```css
/* Wrapper */
width: 234.952px;
padding: 12px 0;

/* Rotated box */
background: #232520;
padding: 12px;
transform: rotate(-6deg);  /* left badges: rotate(+6deg) */

/* Text */
font: Instrument Serif;
color: #d3ff7c;
text-align: center;
letter-spacing: -1.28px;
/* "20 " at 64px + "Times" at 36px, inline */
```

---

## Hero Input Widget

```css
/* Shadow (behind) */
position: absolute;
top: offset, left: offset;
background: #d3ff7c;
border: 1px solid black;
transform: rotate(-2.49deg) skewX(-0.88deg);

/* Box (front) */
position: relative;
background: white;
border: 1px solid black;
padding: 18px 38px;
display: flex;
gap: 28px;
align-items: center;

/* Hour/minute fields */
background: #f6f9f2;
font: 48px Instrument Serif;
color: #888880;
padding: 7px 72px;

/* Arrow → button */
flex: 1;
border: 1px solid #999;
display: flex;
align-items: center;
justify-content: center;
font: 40px serif;
color: #888880;
```

---

## Decorative Vector Assets

Four spiral vectors from Figma. Asset URLs expire in 7 days — download immediately.

imgImage2 (hero spiral): https://www.figma.com/api/mcp/asset/3e26f08d-373c-47a2-9d0e-074c26340129
imgFrame6 (colon separator): https://www.figma.com/api/mcp/asset/aa28cf4b-93ec-4312-b591-ca580215171c
imgVector (daily spiral): https://www.figma.com/api/mcp/asset/05ec09b8-c292-44e9-8e62-a97c487db5fe
imgFrame7 (mini colon): https://www.figma.com/api/mcp/asset/e5bdb95d-1636-41c0-99e8-8aae2d220fb8
imgVector1 (weekly spiral): https://www.figma.com/api/mcp/asset/e2ce3158-ad33-40de-baa8-46b381407556
imgVector2 (yearly spiral): https://www.figma.com/api/mcp/asset/f6b983c5-46ef-48b3-8f71-83d57011766d
imgVector3 (closer spiral): https://www.figma.com/api/mcp/asset/cde50019-9b97-4a17-8813-08768cdb7bd3

Save locally as:
- images/spiral-hero.png
- images/colon.png
- images/spiral-daily.png
- images/colon-sm.png
- images/spiral-weekly.png
- images/spiral-yearly.png
- images/spiral-closer.png

---

## Grain overlay (same as v2)

```css
body::before {
  content: '';
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 100;
  opacity: 0.045;
  background-image: url("data:image/svg+xml,...noise filter...");
  background-size: 256px 256px;
}
```
