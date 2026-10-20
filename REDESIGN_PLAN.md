# Redesign plan: ink on paper

Status: **plan only. No site files have been created or edited.** Waiting for James's approval and the answers in section 10.
Source: `Portfolio redesign brief ink on paper.md` (20 Oct 2026). Recon done on the three files in Downloads.

## 0. Recon findings that matter

| # | Finding | Effect on the plan |
| --- | --- | --- |
| 1 | `C:\dev\personal` is empty and **not a git repo**. The pages are loose files in Downloads (`index.html`, `atlasindex.html`, `complianceindex.html`). | I will `git init`, commit the originals as `baseline` on `main`, then branch `redesign/ink-on-paper`. Mapping assumed: `atlasindex.html` → `atlas/index.html`, `complianceindex.html` → `guardian/index.html`. **Q1** |
| 2 | No `tailwind.config.*` or `package.json`. The inlined bundle (v3.4.17) is pre-compiled and **byte-identical on all three pages** (same md5). | Move it into `assets/site.css` once. Only classes already in the bundle can be used; new classes go in hand-written CSS. |
| 3 | The case-study pages carry the thread, typing and `#avatar` scripts but **no `#threadWrap`, `#thread`, `#heroTitle` or `.tgap` markup**. Each script exits at its first guard. | Neither case study has a thread, so per brief requirement 10 none is added. The dead scripts are removed from both. **Q2** |
| 4 | The case studies have no problem/build/outcome blocks. They use the `.tldr` icon tiles, "Before/After" morph, demo frame, six or three diagram cards, a chain and a "Next case study" card. | Narrative-flow conversion applies to the home page only. The case studies get the tl;dr rebuild and component re-theme. |
| 5 | The case studies reveal diagrams on scroll (`data-reveal` plus an IntersectionObserver draws lines and bars). The brief bans generic scroll fade-ups and says reveals come from the thread. | These are diagram draw-ins, not fade-ups, and no thread exists there. I propose keeping them, re-themed, and disabled under reduced motion. **Q2** |
| 6 | QA check 2 searches for `accent`, which matches the CSS property `accent-color` on the range sliders (Guardian simulator). | I will set `accent-color: var(--ink)`. The check regex becomes `\baccent(-bright)?\b\|bg-accent\|to-accent` so it does not flag the property. **Q3** |
| 7 | QA check 3 (`gradient(`, `box-shadow`, `blur(`) also hits the compiled bundle: `.ring`, `.blur`, `.filter`, `.backdrop-blur-xl`, `.bg-gradient-to-b`, `:-moz-ui-invalid{box-shadow:none}`, the `--tw-*` shadow/blur variables. SVG `<linearGradient>` fills (`gBar`, `gSpec`) are also gradients. | Prune every unused utility and the `--tw-*` effect variables. Replace `gBar` and `gSpec` with flat ink and muted fills. The only exception left is the skeleton shimmer. |
| 8 | Clay on `--paper-2` is **4.34:1**, below 4.5. Muted on `--paper-2` is 4.79:1 (passes). | Clay text only on `--paper`. Inside `--paper-2` panels, status text is ink with a clay dot or stroke. |
| 9 | `--rule` is 1.37:1 on paper, fine for hairlines but not for controls. | Tappable diagram nodes, chain steps and chips use `--ink` or `--muted` outlines (3:1 or better). `--rule` is for dividers and panel borders only. |
| 10 | Amber is used as **text** today: `.eyebrow`, `.tw-hi`, `.cap.per`, `.lp-label`, `.sk-lab`, `.th-*`. | All become ink text. Where meaning is "a person decides" it moves to an amber fill with ink text (`.sk-lab` becomes an amber-filled label). |
| 11 | Counts differ slightly from the brief: home has **96 distinct hex literals** (the brief says 45; the avatar SVG accounts for most of the gap) and **120 `rgba(`** calls (brief: 113). | No effect; every literal goes either way. |
| 12 | Fonts: Google Fonts serves Besley 600 and 700 and Source Sans 3 at 400, 500 and 600. | No substitution needed. |

### Baseline numbers (home page, Chromium 1440×900, 30 px instant-scroll steps, 3 frames each)

| Metric | Before |
| --- | --- |
| Path | 3,623 polyline points, 14,478 px long |
| Line drawn per 30 px scroll | median 32 px, **max 1,207 px** |
| Steps over 120 px / zero growth | **31 of 154** / 3 |
| Thread layer | `z-index: 5`, above content |
| Closing-loop target | 1,088 × 50 px full-width row (buttons are about 511 px) |

| Metric (all pages) | Home | Atlas | Guardian |
| --- | --- | --- | --- |
| File bytes | 137,413 | 115,072 | 95,389 |
| `gradient(` / `box-shadow`+`drop-shadow` / `backdrop-filter`+`blur(` | 37 / 18 / 16 | 37 / 18 / 16 | 37 / 18 / 16 |
| `Inter` references | 16 | 15 | 18 |
| `slate-` or `accent` matches | 39 | 30 | 31 |
| Middle dots in markup | 6 | 5 | 4 |

I measured the pacing myself, so the "9 of 157" in the brief differs from my "31 of 154". My test used the page's own eased tip. Both sets will be reported as "before". Baseline screenshots go to `qa/before` at the start of the build (the dev server is already set up).

## 1. File inventory

| Path | Action |
| --- | --- |
| `index.html` | Rewrite: link `assets/site.css`; keep inline page layout CSS, `window.SITE`, page scripts, `noscript` |
| `atlas/index.html` | From `atlasindex.html`; link `../assets/site.css`; case-study layout and copy edits |
| `guardian/index.html` | From `complianceindex.html`; same |
| `assets/site.css` | New. Tokens, base, typography, buttons, nav, chips, draft markers, diagram, demo, simulator, chain, pipeline, morph, pruned Tailwind bundle |
| `assets/thread.js` | **Not created.** The thread exists only on the home page, so it stays inline (brief rule) |
| `REDESIGN_PLAN.md` | This file |
| `qa/before`, `qa/after` | Screenshots, not committed |
| Not touched | `og.png` (not in the repo), CV link, `YOUR-DOMAIN` placeholders |

## 2. Old to new colour token map

| Old | New |
| --- | --- |
| `#0B0F19` (bg, `theme-color`, thread node fill) | `--paper` `#ECEEE8` |
| `#0F1524`, `#0E1424`, `#0D1426`, `#131B2E`, `rgba(19,27,46,*)`, `rgba(15,21,36,*)` (panels, cards, demo) | `--paper-2` `#E3E7DF`, opaque |
| `#CBD5E1`, `#E2E8F0`, `#D6DFEE`, `#fff` (text) | `--ink` `#1A2420` |
| `#94A3B8`, `#64748B`, `#475569`, `text-slate-300/400` | `--muted` `#5A665F` |
| `#1E293B`, `#334155`, `#2A364D`, `#3A4A66`, `#1B263A`, `#243049`, `rgba(51,65,85,*)` | `--rule` `#C8CEC3` (dividers); `--ink`/`--muted` for control outlines |
| `#2F6BFF`, `#5B93FF`, `#9CC2FF`, `#5A94FF`, `#3B5BA5`, `#C3D8FF`, `#C9DBFF` (blue family) | `--ink` for lines, nodes and primary buttons; `--moss` for links and focus |
| `#F5B84B`, `#FADA9E`, `#B8801B` (amber) | `--amber` `#F2A900`, **fills only** |
| `#4ADE9A`, `#A4EFCB`, `#1FA971` (mint) | `--moss` `#2F5D46` |
| `#F4756B`, `#FBC9C4` (coral) | `--clay` `#B04A2F` |
| Alpha washes `rgba(R,G,B,a)` | Removed (flat). Remaining alphas are `color-mix(in srgb, var(--token) N%, transparent)`, used only for the 28% ghost line and 22% selection |
| `--ink-hover` `#26342D` | Primary button hover |

All hex literals will live in `:root`. Inline SVG hex fills become classes using `currentColor` or `var()`.

## 3. Component re-theme list (all in `site.css`)

| Component | Change |
| --- | --- |
| Base, `html`, `body` | Paper ground, ink text, `color-scheme: light`, Source Sans 3 body at 1.0625rem/1.6, 68ch measure, selection moss 22% |
| Type scale | Per brief table. Besley 600 on h1, h2, project titles, wordmark and stage labels; 700 on numerals. Besley tracking never tighter than -0.015em (heading tracking is set per row in the brief's table). `text-wrap: balance` kept |
| Focus | `2px solid --moss`, 3px offset, 2px radius |
| `.btn` | 2px radius. Primary: ink fill, paper text. Secondary (`.btn-ghost`): 1px ink outline, transparent, paper-2 on hover. Hover lift `translateY(-1px)` 200ms; no brightness filter, no shadow |
| `.link-ext`, `.inline-link`, `.link-back` | Moss, underlined, ink on hover. Arrow icons removed from these links as well as buttons (open question Q4) |
| Nav | Solid paper, `1px --rule` bottom border once scrolled, no blur. Wordmark "James Wheeler" in Besley, no tile. Scroll-spy underline ink, flat. Mobile menu solid paper |
| `.chip` | 2px radius, 1px outline, sentence case. Live: filled moss dot. In development: solid outline. Building now: dotted outline. `chip-blue`, `chip-mint`, `chip-per` collapse into these |
| `.tbc`, `.draft-pill` | Dashed 1px muted outline, paper-2 fill, ink text. Pill loses its blur |
| `.card`, `.dgc`, `.pgrid`, `.pcol`, `.tldr` | Cards removed on the home page. `.dgc` (case-study diagram panels) become paper-2 panels, 2px radius, 1px rule, no hover |
| Diagram (`.nd`, `.ln`, `.cap`, `.halo`, `.val`, `.lp-*`) | Lines ink, `.dim` muted. Boxes paper-2 with ink or muted outline. Hover/selected = paper fill with ink 2px outline (no blue). `.per` = amber fill, ink text. `.ok` = moss stroke; `.bad` = clay stroke with ink text. `.halo` stroke is paper-2. Text fill ink |
| Morph (`.mhub`, `.mchip`) | Hub: filled ink disc, paper label (no gradient or glow). Chips: 2px radius. "Warn" chips use a clay outline with ink text, turning ink after the morph |
| Demo frame | Flat paper-2, 1px rule border, no coloured shadow. `.demo-intro` flat paper-2; play button filled ink; ping ring ink. Skeleton shimmer kept, built from paper-2 and rule only. `.hotspot` amber fill with ink outline |
| Chain and pipeline (`.cn`, `.cc`, `.pl`) | Boxes 2px radius. Connectors become static 2px ink lines (animated gradient flow removed). Numbered discs ink; `.per` amber fill |
| Simulator (`.sim`, `.meter`) | Fill ink (amber when a threshold is hit, as "person decides"); threshold marker ink; range `accent-color: var(--ink)` |
| Proof strip, career strip, `.band`, `.deal`, `.dots-viz`, `.pct-ring` | Unused today, kept and re-themed to tokens (flat) as the brief requires |
| Reduced motion block | Kept; extended to the thread and reveals |
| Deleted | `.avatar*`, `av-head`, `av-eye`, `avNod`, `avBlink`, `.bg-stage`, `.cta` glow/grid, `.eyebrow`, `.tw-hi`, glow `drop-shadow`s, `.th-bead`, `.dolist`, `.pcol`/`.pgrid`, `.tldr` tiles, `.steps` (replaced) |

## 4. Home page wireframes

Desktop ≥1024 (rail at left margin, `x` about 122 px at 1440):

```
HERO  (7 / 5 columns)
 o  Hi! I'm James.                          .Paper   Memory.
 |  I'm a curious builder, using AI to           .  Excel sheets
 |  find out what's possible.             Personal                 (6 hand-placed nodes,
 |  [See the projects] [CV] [LinkedIn]    devices  .  Legacy systems  dotted -> solid -> filled)
 |                                        Third-party software.
 +-----line leaves the field, passes below the button row, joins the rail

ABOUT
 o  What I do.            (Besley h2, lights ink when thread arrives)
 |  Product Manager (Compliance & AI)
 |  RBW Systems, London
 |  [lead paragraph 1]                     950 live sites  | caption
 |  [lead paragraph 2]                     ---------------
 |                                         One system      | caption
 |                                         ---------------
 |                                         One agent       | caption

PROJECTS  (h2 "Projects")
 o  RBW Atlas   [* Live] [Teams agent in development]
 |  Internal AI Native company operating system
 |  ------------------------------------ | 5/12 text            | 7/12 diagram panel
 |  (o) Problem | paragraph               | (paper-2, hub +     |
 |  ------------                          |  8 modules, thread  |
 |  ( ) Build   | paragraph (halved)      |  passes behind)     |
 |  ------------                          |                     |
 |  (*) Outcome | paragraph + 3 "add:"    |                     |
 |  [View the case study]  (primary, no arrow)
 o  Guardian Compliance  (title, tag)
 |  950  sites live as of 19 Oct 2026      35%  of the sites in the market   (numerals count up)
 |  Problem / Build / Outcome rows, 140px label column + paragraph
 o  Project three  [dotted: Building now]   all "add:" markers, disabled button

HOW I WORK
 o  h2 + intro paragraph
 |  1 Plan(amber)  ->  2 Review  ->  3 Build  ->  4 Critique(amber)   (thread runs through, then back to rail)
 |  "add: link a sanitised plan and its review"

EXPERIENCE
 o  May 2024 - now | Product Manager (Compliance & AI)      (date column | role in Besley)
 |                   RBW Systems
 |                   bullets (unchanged)
 o  Dec 2023 - May 2024 | Marketing Projects Lead ...
 |  Education list, one per line

CONTACT
 o  h2 + paragraph
 |  ( [Email me] [LinkedIn] [CV] )   <- inline-flex group, data-thread-end, closing loop 24px padding
```

Mobile <768/700: the rail stays in the left margin at the existing 2.75rem padding. The hero field sits under the buttons (full width, about 280 px). Stage rows stack (label above paragraph). The Atlas diagram sits under the text. About facts stack. The four How I work steps become a **vertical numbered list on the rail** (the line passes down through them instead of horizontally, so it cannot exceed 390 px). Experience: the date sits above the role.

Wireframes for the case-study tl;dr rebuild (both pages), replacing `.tldr` icon tiles with a definition list:

```
Problem  | Paper, scattered data, no field alerts      <- hairline above each row, 140px term column
Move     | One system of record, AI on top
Agent    | 80% benchmark, about 70% live
Ops      | 1,200 locations watched 24/7
Role     | Solo build with AI coding agents
```

Guardian leads with large numerals instead: `950` (Besley 700) with "sites live as of 19 October 2026" and a smaller offset `35%` "of the sites in the market", then Problem, Move, Role, Timeline as definition rows ("Aug 2024 to Oct 2026"; middle dots removed).

## 5. Thread architecture (home page only, inline script)

**Path construction.** Anchors read from the DOM at build: hero node centres, the rail `x` (`first heading x - 54`, or `-26` under 700 px), each `[data-thread-node]` heading, `[data-thread-hit]` elements, the Atlas diagram panel box, the four step nodes, and `[data-thread-end]`. The path is one SVG `<path>` of cubic Bézier segments, **at most 60 in total**, in this order:

1. Hero: Paper, Memory, Excel sheets, Personal devices, Legacy systems, Third-party software (5 segments).
2. Exit: below the button row (button-bottom + 48 px), across to the rail `x`, down (2 to 3 segments). It never crosses the headline or buttons.
3. Rail runs between headings: one segment per gap with a sine sway of 10 px or less.
4. Excursions, each with a reason: behind the Atlas diagram panel (through the hub); horizontally through the four How I work steps and back to the rail; around the closing buttons.
5. **One loop only**, the closing ellipse: 24 px padding each side, bounding box at most 1.3× the buttons' box, drawn last. All mid-page loops and the bead are gone.

**Layering.** `#thread` gets `z-index: 0`; a `#threadContent` stacking context (`position: relative; z-index: 1`) wraps the sections. Anything the line passes behind (the Atlas panel, node labels) has an opaque paper or paper-2 background.

**Scroll mapping (replaces running-max-y).** On build I place a knot at each anchor: `(scrollY at which the anchor sits at 55% of the viewport, path length at that anchor)`. Between knots, length maps linearly, so each section's segment completes over a fixed scroll distance and no step can surge. Per frame: binary search over about 20 knots, then a lerp (about 0.18) toward the target length, driven by `requestAnimationFrame` with passive scroll listeners. No `getPointAtLength` in the scroll path.

**Length tables, built once and on resize.** I sample the path every 2 px (`getPointAtLength`, build only) into arrays, then compute for each `data-thread-hit` the first length that enters its box expanded by 12 px. If the line never enters it, I use the first sample whose `y` is at or below the element's top, so every flagged element lights. Rebuild on `resize`, a `ResizeObserver` on `#threadWrap`, and `document.fonts.ready`.

**Time-based parts.** The hero draw (about 1.6 s, ease-in-out) starts when typing ends, and the closing loop (about 900 ms, ease-in-out) starts when the line reaches it. Both are exempt from the 120 px/step cap. If the user scrolls during the hero draw, its state persists and scroll mapping takes over afterwards.

**Lighting.** `.is-lit` is added when the drawn length passes the precomputed value. Lit means: headings muted → ink (plus an 8 px → 0 rise over 350 ms), stage markers and nodes hollow → filled, hairlines `scaleX(0 → 1)` from the left, stat numerals count up, diagram edges draw by `stroke-dashoffset`. Unlit styles apply only under `html.js` (set by an inline script) and `prefers-reduced-motion: no-preference`; unlit text stays `--muted` (5.13:1) in place, so nothing shifts. Never flagged: body paragraphs, buttons, links, nav.

**Look.** Ghost path: dotted ink at 28% opacity. Live path: solid ink, 2 px, round caps. Nodes: 14 px rings (paper fill, 2 px ink stroke) that fill ink when reached; amber only for the Plan and Critique nodes. No `filter` on the SVG.

## 6. Hero node spec

Inline SVG, `aria-hidden="true"`, no hover. Field 480 × 440 with labels in Source Sans 3 at 15 px.

| Node | Position (x, y) |
| --- | --- |
| Paper | (70, 50) top-left |
| Memory | (390, 70) top-right |
| Excel sheets | (410, 200) mid-right |
| Personal devices | (60, 210) mid-left |
| Legacy systems | (350, 320) lower-right |
| Third-party software | (110, 395) bottom-left |

All labels are at least 40 px from any other node. States: dotted ring + muted label (before the line arrives), solid ring (line touching), filled ink disc + ink label (passed). Connection order: Paper, Memory, Excel sheets, Personal devices, Legacy systems, Third-party software over about 1.6 s. The thread's start point is the Paper node. Below 1024 px the field sits under the buttons, full width, about 280 px tall (same `viewBox`, scaled). With reduced motion or no JS the connected end state is shown.

Typing: start delay 250 ms; line one 55 ms + up to 10 ms jitter per character; 350 ms pause; line two 22 ms + up to 10 ms jitter; caret fades 1.2 s after finishing (about 2.5 s end to end). Typing waits for `document.fonts.load` of both families, with a 1 s timeout. Caret and both lines are ink. The `aria-label` and `data-type` approach is unchanged.

## 7. Case-study copy changes (proposed; applied only after approval)

Rules: only facts already on the page, "I" for what James did or decided, "we" for the company, and a dashed "add:" marker where a specific is missing. **Each "before" is the current text.**

### Home page (from the brief, with on-page sources)

| Selector | Before | After | Source on page |
| --- | --- | --- | --- |
| `#about` subtitle | Product Manager (Compliance & AI) at RBW Systems · London | Two lines: "Product Manager (Compliance & AI)" / "RBW Systems, London" | Same line |
| `#about` triplet | Product / AI / Judgement boilerplate | 950 live sites; One system; One agent (brief captions) | About lead, Guardian outcome, Atlas outcome, tag "Teams agent in development" |
| `#atlas` problem | "RBW ran on paper and memory. Data was scattered across personal devices, Excel sheets, third-party software, and pretty much any place you could imagine. The team had no central source of truth…" | Brief text: "…Data sat on personal devices, in Excel sheets and in third-party software, with no single source of truth to build on as we grew." | Same paragraph, trimmed |
| `#atlas` build | "As we looked at the rapid advancements in AI, we recognised the need… (47 words)" | "Foundation first, AI second. I built one system for the company's core functions and data with AI coding agents, then put the AI workflows on top." | Same build paragraph; "I" and "with AI coding agents" from Atlas tl;dr "Solo build with AI coding agents" and the Experience bullet |
| `#atlas` outcome | "A single system covering CRM, onboarding…" | "One system now covers CRM, onboarding, installations and inventory, customer support, QBRs, the employee hub and server telemetry, plus an AI agent with custom knowledge bases, telemetry, multiple models and observability." | Same list |
| `#guardian` build | 95 words | About 50 words (brief text) | Same paragraph |
| `#guardian` outcome | "**950 sites live** as of 19 October 2026, 35% of the sites in the market. Unseated…" | The 950 and 35% move to the stat block; outcome keeps "Unseated the market incumbent's largest customer, and sub-licensed the solution to the trade body for use across its membership base." | Same paragraph |
| Experience education | one line joined with middle dots | One item per line | Same four items |

### `atlas/index.html` (proposed, **not in the brief**; for approval)

| Selector | Before | After | Source on page |
| --- | --- | --- | --- |
| `.tldr` | 5 icon tiles, "Agent 80% benchmark, ~70% live", etc. | Definition rows, same facts, no icons | Same tiles |
| `#ba-h` caption | "The question: could RBW run on AI workflows it controls? First: where does our data live?" | Unchanged (already specific). It stays in the file, only restyled | n/a |
| Decision "Keep Jira" cap | "Buy the commodity. Build where you need control." | Unchanged; already a decision statement | n/a |
| Decision "Ask first" row | `Dev team varies · mostly the employee hub` | Dev team: "varies, mostly the employee hub" (middle dot removed) | Same tip text |
| Tips mixing voice | "My leading hypothesis…", "I re-ran…" and "the team wrote…" | Already "I" for James and "the team" for staff. No edit; I will list any I find while building | n/a |

### `guardian/index.html` (proposed)

| Selector | Before | After | Source on page |
| --- | --- | --- | --- |
| `.tldr` "Result" | "950 sites live · 35% of the market" | Stat block: 950 / 35% (see section 4) | Same tile |
| `.tldr` "Role" | "End-to-end PM · Merkur account owner" | "End-to-end product manager" and "Merkur account owner" as two lines | Same tile; diagram tip "I was the account owner…" |
| `.tldr` "Timeline" | "Aug 2024 → Oct 2026" | "Aug 2024 to 19 Oct 2026" (arrow dropped) | Timeline diagram |
| Section lede `#ba-h` | "The question: what does staff on the floor need…" | Grammar fix only: "what do staff on the floor need…" | Same sentence |
| `#dec-h` cap | "Operators set the lines. Staff get the prompt. Configurable rules, not AI." | Unchanged | n/a |

I found no boilerplate beyond the tl;dr and the grammar slip, so the case-study copy changes stay small. **Q5**

## 8. Build order and commits (branch `redesign/ink-on-paper`)

1. Tokens and shared stylesheet: `assets/site.css` (pruned bundle, `:root`), pages link it; screenshot diff proves no layout shift.
2. Palette, amber rule, flat surfaces (gradients, shadows, blur, `.bg-stage` removed).
3. Typography (Besley, Source Sans 3, scale, Inter gone including the bundle base).
4. Nav and button chrome.
5. Hero (avatar removed, node field, typing timings, fonts gate).
6. Thread (new path, mapping, lighting, closing loop).
7. Home layouts and copy (About facts, stage rows, Atlas panel, Guardian stats, How I work, Experience, Contact, project three).
8. Case-study pages (tl;dr, components, dead-script removal, approved copy).
9. QA fixes.

Optional, only if approved: the Guardian market dot-field with 35% lit, as its own commit. Not built otherwise.

## 9. Risks and tests

| Risk | Mitigation |
| --- | --- |
| Bundle utilities cannot be regenerated | Hand-written CSS for new needs; check every class used exists |
| Thread path must clear headline, buttons and body text at 1440, 820 and 390 | Anchors from the DOM; automated "line over text" check using `elementsFromPoint` on sampled path points |
| Unlit text could hurt readability | Unlit state is muted (5.13:1) and in place |
| Mobile excursions overflow | Steps stack vertically under 768 px; check scrollWidth equals clientWidth |
| Sub-pixel or font swap shifts the path | Rebuild on `document.fonts.ready` and `ResizeObserver` |
| Stats count-up shifts layout | Reserve width with `tabular-nums` and `min-width` in `ch` |
| Browsers | Chromium run here; WebKit and Firefox only if installed (will say so) |

Test plan: the 16 QA checks in the brief with measured values, with the thread pacing test (30 px instant steps, 3 frames per step, median/max/zero-growth) run against the same baseline method used above. Screenshots at 1440×900, 820×1180, 390×844 into `qa/before` and `qa/after`.

## 10. Questions for James (blocking unless you accept my default)

1. **Layout and git.** OK to `git init` in `C:\dev\personal`, and map `atlasindex.html` → `atlas/`, `complianceindex.html` → `guardian/`? *Default: yes.*
2. **Case-study reveals and dead scripts.** The case studies have no thread. May I (a) keep the diagram draw-in-on-scroll animations, re-themed, and (b) delete the dead thread/typing/avatar scripts from both pages? *Default: yes to both.*
3. **QA check 2 regex.** Accept the narrower `accent` pattern so `accent-color` on sliders does not count? *Default: yes.*
4. **Arrow icons on text links** ("All case studies", "Read it"): the brief names buttons only. Remove them too, or keep? *Default: remove.*
5. **Case-study copy.** The table in section 7 is small. Do you want me to look harder for rewrites, or approve as is? *Default: approve as is.*
6. **[CONFIRM] First-person wording** in the home copy: "I built one system… with AI coding agents" and the Guardian build paragraph. Is each accurate as James's own claim?
7. **[CONFIRM] Optional amber "A person decides" node on the AI agent** in the Atlas diagram. Include it? *Default: leave out.*
8. **Optional Guardian dot-field** (35% lit as the thread arrives). Build it as a separate commit? *Default: no.*
9. **Draft marker count.** Home page gets the brief's three Atlas "add:" markers plus the existing ones. OK?

Say "approved" (with any changes to the defaults above) and the build starts at step 1.
