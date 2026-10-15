# FitTrack 3.1

A free, offline, private fitness and weight-loss tracker for a **strict vegetarian (dairy-friendly) beginner**. It's one file: open `index.html` in any modern browser (phone or laptop). Your data stays in that browser's localStorage.

## What's inside
- **Today**: Activity-style rings for calories, protein and water; calories left and burned; streak; protein nudge with concrete veg suggestions; "Up next" meal from your plan; water glasses; steps; level card.
- **Log**: food diary by meal with a **smart search sheet**. It's fuzzy and forgives typos (panir → paneer). It knows Hindi and regional names (dahi, bhindi, chaas, makhana, phulka, parantha…). Before you type, it shows recents, favourites (★) and *Repeat yesterday's breakfast*. Each food has a **serving picker** with Indian units (katori, roti/piece, glass, cup, tbsp, tsp, plate, g), quick ½/1/1½/2 buttons, a stepper, an **oil/ghee level** for cooked dishes and a live kcal/macro preview. You can also log workouts, **custom recipes** (built from ingredients, logged in one tap) and custom foods.
- **Plan**: a meal timetable built from your wake/sleep times (default 6:00 wake → 21:30 milk). It auto-generates **day / week / month** menus that fit your calorie target (±10%, usually ±3%), with no dish repeating within 3 days. You can swap or lock a meal, regenerate, log a planned meal in one tap, view the week or month calendar, and turn on meal-time reminders. The **grocery list** comes from the week's plan, with quantities grouped by category, checkboxes, and copy/share.
- **Progress**: raw weigh-ins with a **7-day moving-average trend line**, a goal projection, a **weekly check-in**, an **adaptive calorie suggestion** (±100 kcal based on your trend; you choose whether to apply it), body measurements (waist, chest, hips, arms and notes) and history.
- **More**: Rewards (XP, levels, 18 badges, weekly challenges, **streak freezes**: 1 earned per week, up to 2 banked, used automatically), Learn (beginner guides and the 4-week low-impact plan), Profile and targets (Mifflin-St Jeor), Reminders, Backup (export/import JSON, v1 files supported), and Appearance (Auto/Light/True-black dark).
- A floating **+** button opens quick log: Food, Water, Weight, Workout.

## Notes
- **Food values are approximate estimates** for typical home-style recipes, not lab data. Restaurant food usually has more oil. No IFCT/INDB data is bundled. There are 268 foods, and none contain meat, egg or fish.
- Notifications and reminders work only while the app is open in a tab.
- Install: on iPhone, use Safari → Share → *Add to Home Screen*; on Android, use Chrome → *Add to Home screen*. The manifest and icon are inline. There's no service worker (a single local file can't register one), so it's "installable-ish" and works offline because it's one file.
- Data from v1 (`fittrack_v1` key) migrates automatically: meals, workouts, weights, XP and badges are kept, and old waist entries move to Body measurements.
- Back up regularly (More → Backup & data), because clearing browser data deletes everything.

## Development
Source is in `src/` (`style.css`, `foods.js`, `app1–5.js`). Run `python3 build.py` to regenerate `index.html`. The v1 backup is at `v1/index.html`.

## Changelog

### 3.1 (2026-10-14): installable app and motion
- **Installable PWA** in `pwa/` (zip: `fittrack-pwa.zip`). Includes a manifest (standalone, portrait), PNG icons (192/512, maskable, 180 apple-touch), 22 iOS launch screens (light and dark), and a service worker that works offline first, uses a versioned cache, and shows an "Update" prompt. Chrome's installability check reports no errors.
- **Install button:** Android/Chrome uses the browser's own install prompt. iPhone shows a "Share → Add to Home Screen" guide sheet. Both are hidden once installed. Shortcuts: `?action=log`, `?action=water`.
- **Motion:** tab and page transitions (View Transitions, with a CSS fallback), the food name carries into the serving sheet, spring sheets with a blurred backdrop, list insert and delete animations, press feedback, an animated tab-bar pill, and haptics. Reduced motion is respected.
- **Dogfooding fixes:** see the list in the report. "Up next" skips meals already logged; the Hindi quick log has −/+ amount controls and a "check amount" flag; the typing box sits at the top and expands the sheet; toasts no longer collide with the + button; confetti stays below the bar; the backup nag waits for 3 days of use; import keeps your last-backup date.
- The single-file `index.html` is still built for backup. Build with `python3 build.py && python mkpwa.py`.

### 3.0 (2026-10-14)
- **All 21 REVIEW.md issues fixed.** Input validation (workout 1–600 min with no XP for bad entries; weigh-ins 30–300 kg, no future dates, confirm when >3 kg off trend; steps 0–100,000; profile age 13–100 with target below current; serving > 0 with a message). Midnight rollover via visibilitychange, focus and a timer. Safe Notification detection with an iOS "Add to Home Screen" fallback. Tap targets ≥ 44 px. Redesigned water card. Undo snackbar on deletes. Import preview before replacing data. Weigh-in history grouped by month (last 7 + Show all). Calmer one-line toasts; confetti only for milestones and kept below the title. Desktop left rail with the FAB inside it. Sheets use role=dialog, a focus trap, Esc to close, half/full detents with drag. Collapsing large titles, animated rings, count-up numbers.
- **Meal plans hit calories and ~144 g protein within ±5%** (tested over 7 and 31 days, whey on and off). New "Include whey (optional)" toggle, on by default. Plan screen shows calorie and protein meters.
- **New:** a 60-second skippable onboarding; a weekly backup reminder with one-tap export; "Estimate" labels and editable food values (saved as overrides); Hindi/English quick log ("2 roti aur 1 katori dal", "ek glass chaas") with optional voice; Calm mode; small/medium/large meal estimates; notification preferences per type with quiet hours; 579 vegetarian foods; log a meal in 2–3 taps from the + button.
- Data migrates automatically from v1 and v2. The 2.0 build is kept in `v2/index.html`.
- QA: `review/qa3.py` (Playwright) runs 70 checks.

## 4.0 (Oct 2026)
- One-question-per-card onboarding (goal, body, diet incl. Jain/vegan/eggetarian, routine, sleep, health conditions, experience, home/gym, equipment, time, dislikes/allergies, cuisine); skippable and editable later; multiple local profiles with a switcher.
- Rule-based smart plan engine: analysis screen (BMI with Asian cut-offs, kcal/protein, safe weekly rate, condition risks, activity advice), plus "why this plan" notes.
- Diet-aware meal planner (allergies, dislikes, regional bias); 12-week exercise program with injury-safe swaps, SVG illustrations, timer player, logging and weekly adaptation.
- Goal accent colours, calm progress-first UI, daily next-step card, small wins.
- NL parser: fuzzy units (katorri…), longest-name food match, tap an item to change the food; iOS keyboard handling uses visualViewport.

## 4.1 (Oct 2026)
- Global iOS keyboard handling: any focused field (sheets, onboarding, pages, all food inputs) stays above the keyboard; sheets go full visible height while typing; numeric keypad for number fields.
- Weekly report card (auto Sundays + on demand), PNG export, Web Share, history.
- Apple Health import via iOS Shortcuts (clipboard or URL), preview + dedupe merge, pasted-text fallback, 9 PM automation guide.
- Friends challenges without a server: invite codes, tamper-evident progress codes, local leaderboard.
- Photo food log: compressed thumbnails in IndexedDB, smart suggestions (plan/recent/frequent).
- Stress fixes: double-submit guard, view-transition race, storage-full safety, no duplicate active-profile copy, per-day meal index, long-name wrapping.

## 4.1.1 (Oct 2026)
- More › Apple Health › Import Health export: pick export.zip/export.xml from Files; read fully on-device in chunks (zip central directory + DecompressionStream('deflate-raw'), bundled inflate fallback; streaming XML scanner). Range picker (default 90 days), progress + cancel, preview, merge replacing imported days.
- Per day: steps & walking/running distance (per-hour max across sources so iPhone+Watch aren't double counted), active energy (kcal/kJ), latest body mass (lb/st→kg), workouts (duration, kcal, distance; old and iOS 16+ formats). New 30-day steps chart on Progress; weekly report uses imported steps/workouts.
