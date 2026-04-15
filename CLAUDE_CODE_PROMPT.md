# CLAUDE_CODE_PROMPT.md — Exact instructions for Claude Code

## How to use this file

Run `claude` in your project terminal and paste the prompt below.
This file also contains the full Figma-extracted component code as reference.

---

## Opening prompt (paste this first)

```
I'm building a single-page screen time data visualization called 
"Oh, The Things You Could Be Doing Instead!" 

Read DESIGN.md for the full visual spec. The Figma design has been 
extracted and the component code is in CLAUDE_CODE_PROMPT.md.

Your task: Build index.html as a single self-contained HTML+CSS+JS file 
that faithfully implements the Figma design. Make it fully functional 
with a working hours+minutes input that dynamically updates all 
comparison counts and bar chart widths.

Do NOT use any frameworks, npm, or build tools. Pure HTML, CSS, JS only.

Key rules:
1. Follow DESIGN.md exactly for all colors, spacing, typography
2. The layout is a centered 654px column for comparison cards
3. Section backgrounds change by tier: white → #ebffc2 → #d3ff7c → #404339 → white
4. Bar chart widths update dynamically based on user input
5. Download the Figma asset images listed in DESIGN.md immediately 
   and save to /images/ folder before building
6. The "And FINALLY" section rotates 2deg as a whole
```

---

## Figma component reference (paste this if Claude Code needs more detail)

Below is the exact component structure extracted from Figma.
Use this as the ground truth for layout, not as code to run directly
(it's React/Tailwind — convert to plain HTML/CSS).

### TimeComparisonChartLeft component
Two-row bar chart, bars grow left-to-right:
- Row 1: 150px label | lime bar (#d3ff7c, border black, h:20px)
- Row 2: 150px label | dark bar (#434549, border black, h:20px)
- Container: white bg, border #888880, border-radius 16px, blur(2px), py-24px, w-654px

### TimeComparisonChartRight component  
Two-row bar chart, bars grow right-to-left (justify-end):
- Row 1: lime bar | 150px label (right-aligned)
- Row 2: dark bar | 150px label (right-aligned)
- Same container styling as Left variant

### ComparisonStat component (badge — right side, rotate -6deg)
- Outer: 234.952px wide, py-12px
- Inner box: bg #232520, p-12px, rotate(-6deg)
- Text: "20 " at 64px + "Times" at 36px, color #d3ff7c, tracking -1.28px

### ComparisonStat1 component (badge — left side, rotate +6deg)
- Same as above but rotate(+6deg) instead

### ActivityHeading component
- 407.048px wide
- 36px Instrument Serif, line-height 42px, tracking -0.72px
- Left variant: text-left
- Right variant: text-right

---

## All comparison data (build all these cards)

### DAILY SECTION (bg: white) — multiply duration by: totalMinutes / durationMin

| # | Variant | Title | Duration | Body copy |
|---|---------|-------|----------|-----------|
| 1 | Left | Taken out the trash AND Done the dishes | 15 min | Average time for both: 15 minutes. A complete act of domestic heroism you chose not to perform. |
| 2 | Right | Watched Sabrina Carpenter's full Coachella set | 75 min | Runtime: 75 minutes. She was right there. |
| 3 | Left | Rewatched Mean Girls (and made "so fetch" a thing) | 97 min | Runtime: 97 minutes. You could have single-handedly changed the cultural lexicon. |
| 4 | Right | Replied to that one text | 2 min | Average reply time: 2 minutes. You know which one. They are still waiting. |
| 5 | Left | Called your mom | 20 min | Average call: 20 minutes. She saw you being active on your socials though. |
| 6 | Right | Run a 5k | 35 min | Avg casual 5k: 35 minutes. Your dog would have loved it. |
| 7 | Left | Dyed your hair | 90 min | Full DIY dye job: 90 minutes. Still the same hair. |
| 8 | Right | Rewritten your New Year's goals (because you know you're not hitting them) | 15 min | 15 minutes per planning session. Fresh start, same results. |
| 9 | Left | Walked your dog | 20 min | Average walk: 20 minutes. Your dog noticed. |
| 10 | Right | Baked chocolate chip cookies from scratch | 47 min | Prep + bake = 47 minutes. Your kitchen is still clean. |
| 11 | Left | Applied to jobs on LinkedIn | 4 min | Easy Apply takes ~4 minutes. That's [n] rejections you haven't received yet. |
| 12 | Right | Decided what to watch on Netflix | 19 min | Average browse time: 18–20 minutes. [n] full cycles of "I don't know, you pick." |

### WEEKLY SECTION (bg: #ebffc2) — multiply duration by: (totalMinutes × 7) / durationMin

| # | Variant | Title | Duration | Body copy |
|---|---------|-------|----------|-----------|
| 1 | Left | Built an outdoor deck | 1800 min | DIY deck construction: ~30 hours avg. You've got the foundation at least. |
| 2 | Right | Finished a beginner crochet blanket | 720 min | Average completion time: 12 hours. You had time for one full blanket or the start of a scarf you'd never finish anyway. |
| 3 | Left | Written a short story | 480 min | Average first draft: ~8 hours. You had time for [n]. |
| 4 | Right | Watched every episode of The Bear S1 | 270 min | Runtime of The Bear S1: ~4.5 hours. |
| 5 | Left | Finally gotten your driver's license | 360 min | Required hours: ~6 hours. Still no license. |
| 6 | Right | Learned a TikTok dance. All the way through. | 180 min | Estimated learning time: ~3 hours. [n] dances you could've posted. |

### YEARLY SECTION (bg: #d3ff7c) — multiply duration by: (totalMinutes × 365) / durationMin

| # | Variant | Title | Duration | Body copy |
|---|---------|-------|----------|-----------|
| 1 | Left | Become a certified yoga instructor | 12000 min | Certification requirement: 200 hours. You could be certified, teaching, and already spiritually exhausted by your students. |
| 2 | Right | Read every book on Obama's reading list | 2400 min | Estimated reading time: ~40 hours for the full list. |
| 3 | Left | Learned conversational Spanish | 9000 min | FSI research: ~150 hours to conversational fluency. |
| 4 | Right | Walked the entire Camino de Santiago | 12000 min | The full pilgrimage: ~200 hours of walking. |

---

## Bar chart width calculation

```javascript
// Given: totalMins (user input), durationMin (per card)
// For bar chart widths:

const MAX_BAR_WIDTH = 387; // px — max bar width from Figma

function getBarWidths(totalMins, durationMin) {
  const ratio = totalMins / durationMin;
  if (ratio >= 1) {
    // Screen time is longer — time bar is full, task bar is proportional
    return {
      taskBarWidth: Math.min((durationMin / totalMins) * MAX_BAR_WIDTH, MAX_BAR_WIDTH),
      timeBarWidth: MAX_BAR_WIDTH
    };
  } else {
    // Screen time is shorter — task bar is full, time bar is proportional  
    return {
      taskBarWidth: MAX_BAR_WIDTH,
      timeBarWidth: Math.min((totalMins / durationMin) * MAX_BAR_WIDTH, MAX_BAR_WIDTH)
    };
  }
}
```

Bar widths animate on input change: `transition: width 0.5s ease`

---

## Count display format

```javascript
function formatCount(totalMins, durationMin) {
  const n = totalMins / durationMin;
  if (n >= 100) return Math.round(n).toLocaleString();
  if (n >= 10) return (Math.round(n * 10) / 10).toString();
  return (Math.round(n * 10) / 10).toString();
}
// Display as: "10×" or "2×" or "0.6×"
// Unit: singular if n === 1, plural otherwise
```

---

## Time unit conversions (hero display)

```javascript
const totalMins = (hours * 60) + minutes;
display.hours = hours;
display.minutes = totalMins;
display.seconds = totalMins * 60;
display.milliseconds = totalMins * 60 * 1000;
// Format with toLocaleString() for thousands separators
```

---

## Zero state

When totalMins === 0:
- Show italic serif placeholder under input: "enter your time to see what slipped away."
- Hide all comparison sections
- Show only hero

---

## Image asset instructions

Download these URLs immediately and save to /images/:
- https://www.figma.com/api/mcp/asset/3e26f08d-373c-47a2-9d0e-074c26340129 → images/spiral-hero.png
- https://www.figma.com/api/mcp/asset/aa28cf4b-93ec-4312-b591-ca580215171c → images/colon.png
- https://www.figma.com/api/mcp/asset/05ec09b8-c292-44e9-8e62-a97c487db5fe → images/spiral-daily.png
- https://www.figma.com/api/mcp/asset/e5bdb95d-1636-41c0-99e8-8aae2d220fb8 → images/colon-sm.png
- https://www.figma.com/api/mcp/asset/e2ce3158-ad33-40de-baa8-46b381407556 → images/spiral-weekly.png
- https://www.figma.com/api/mcp/asset/f6b983c5-46ef-48b3-8f71-83d57011766d → images/spiral-yearly.png
- https://www.figma.com/api/mcp/asset/cde50019-9b97-4a17-8813-08768cdb7bd3 → images/spiral-closer.png
