# FitTrack audit (v4.3, 15 Oct 2026)

**Method.** I went through FEATURES.md (112 features, 50 user-journey questions) one section at a time. For each feature I asked four things: why does it exist, what is it used for, what more could it do, and where does it cause friction? I checked by tapping through the app as a user would, in WebKit with iPhone 14 emulation and a 90-day heavy seed, and then in Chromium. Clear wins were built in 4.3. Everything else is listed as a suggestion.

## 1. Feature-by-feature audit (grouped by FEATURES.md section)

| Area | Why it exists / what it's used for | What more it could do | Bugs and friction found | 4.3 action |
|---|---|---|---|---|
| Onboarding & personalisation | Turns age, height, weight, diet and conditions into a calorie and protein target and a plan in about 2 minutes | Let the user edit an answer from the summary step | None blocking. Long names were already fixed in 4.1 | — |
| Today (calorie ring, Up next, water, steps, Movement) | One glance at "how am I doing today" | Fold all the daily goals into one summary | Steps, workouts and water were three separate rows with no shared "done" state | **Built: Move / Exercise / Steps rings, with rest-day pause** |
| Log / Food (search, NL quick-log, tray, estimate, photo, custom) | Fast Indian veg logging | Edit and move entries, search history, jump to a date | **A logged meal could only be deleted**, so fixing a wrong amount meant re-adding it. Going back to a date three weeks ago meant tapping ◀ 21 times. There was no "when did I last eat rajma?" | **Built: edit sheet (amount, unit, slot, date, copy to today, delete); diary search with "last eaten N days ago"; date jump; Today button** |
| Plan / Meals (±5% kcal, protein as a minimum, swaps, grocery) | A ready-made veg plan that fits the target | Swap a whole day; let users pick favourites | OK in 4.0.1 | — |
| Exercise (plan workouts, info sheets, injury swaps, history) | Beginner-safe routine | Manual exercise swap | No way to judge whether a week was hard or easy | **Built: daily Effort 0–21 and 7-day effort bars** |
| Progress / Weight (trend, projection, measurements, weigh-ins) | Shows whether the plan is working | Monthly view, editing | "How much did I lose this month?" meant reading the chart by eye. **A wrong weigh-in could only be deleted** | **Built: per-month header (avg kg, change in the month); tap a weigh-in to edit kg or date (blocks duplicate dates)** |
| Nutrition history | Answers questions Q9–12 and Q35 | Micronutrients (fibre, iron, B12) | Missing: no charts beyond today's totals | **Built: 7/30/90-day screen (avg kcal and protein, protein days hit, days within ±10% of target, kcal and protein charts with target lines, top foods by share of calories, water)** |
| Reports (weekly card, PNG) | A shareable weekly summary | Effort and goal-completion lines | — | **Built: report shows weekly Effort and the number of days all rings closed** |
| Rewards (XP, badges) | Motivation | — | Copying a meal now awards meal XP the same way logging does | — |
| Challenges (codes, no server) | Social motivation without accounts | — | Code exchange is manual by design | — |
| Health import (Shortcuts, export.zip) | Brings in iPhone steps, workouts and weight | Sleep records (HKCategoryTypeIdentifierSleepAnalysis) to feed Readiness | — | Suggested |
| Learn / Reminders | Education and nudges | Wind-down reminder at night | — | Suggested |
| Settings / Profiles / Backup | Multi-user on one device; JSON backup | CSV and doctor export | Only JSON existed, which is useless in Excel or at the doctor (Q42, Q43) | **Built: Daily-summary, Meals, Workouts and Weigh-ins CSV (UTF-8 BOM, so Excel opens it correctly); one-page Doctor summary via print / Save as PDF** |
| Install / PWA | Offline, home-screen use | — | **Bug (desktop/iPad width):** a closed sheet stayed invisible (opacity 0) but still caught taps in the middle of the screen | **Fixed: `.sheet:not(.on){pointer-events:none}`** |

Journey questions newly answered: 2, 3, 4, 5, 6 (copy a single entry; whole-day copy still suggested), 9, 10, 11, 12, 15, 19, 35, 36 (sleep check-in), 37 (energy check-in, partial), 42, 43.

## 2. Apple Fitness: how it works, what people love and hate

- **How it works.** Three rings. Move is active kcal, Exercise is 30 brisk minutes, Stand is standing in 12 separate hours. Rings fill all day from Watch sensors and are meant to be closed daily. Around them sit Trends (90-day vs 365-day comparisons, with arrows), Awards and streaks, Sharing (see friends' rings, competitions), and Fitness+ (paid guided classes). iOS 26.1 also infers activity, so a workout you forgot to start still counts.
- **USP.** Effortlessly passive: the "close your rings" loop is the simplest goal system in fitness.
- **What reviewers love.** It is simple. The rings are very motivating, and the social nudges work.
- **What reviewers hate.** Streak pressure and ring obsession: rest days were punished until watchOS 11 added pausing rings and per-day goals. Award and streak bugs, with awards lost after a restore and wrong awards that can't be deleted. Sharing breaks. Trends feel naggy. Some users move to "Gentler Streak".

## 3. WHOOP: how it works, what people love and hate

- **How it works.** A screenless strap. Recovery (0–100%, green/yellow/red) comes from HRV, resting heart rate, respiration and sleep. Strain is a logarithmic 0–21 cardiovascular load with a recommended daily target set by Recovery. Sleep Coach suggests a bedtime and sleep need. Journal yes/no habits (alcohol, late meals) are correlated with Recovery. Weekly and monthly Performance Assessments were later reframed as Weekly Plans.
- **USP.** It tells you how hard to go today instead of only counting what you did.
- **What reviewers love.** The recovery-first framing, sleep insight, and how the journal links behaviour to how you feel.
- **What reviewers hate.** A $199–359/yr subscription. Data overload. "Insights" that are only correlations. Weak strength tracking. No screen.

## 4. What FitTrack can adopt within its limits (PWA, no wearable, no login, free on-device engine, Indian veg beginner) and how to do it better

| Idea | Their version | FitTrack's better or fair version | Status |
|---|---|---|---|
| Daily goal rings | Watch sensors; streak pressure | Move = workout kcal + step kcal, Exercise minutes, Steps (Stand needs a sensor, so it is dropped). Goals are editable with calm copy. **Rest day pauses the rings with no streak loss** (fixes Apple's main complaint). Gold tint on full-close days, plus a 7-day mini-ring row | **Built** |
| Strain / Effort 0–21 | Heart-rate based | Load = Σ minutes × MET × intensity + steps, mapped to 21·(1−e^(−load/900)). Shown honestly as an estimate "from workouts + steps" | **Built** |
| Recovery / Readiness | HRV sensor, paid | 0–100 from an optional 5-second check-in (sleep hours chips, energy Low/Okay/High), yesterday's effort, and a 7-day vs 28-day load ratio (a jump of more than 1.5× lowers it). Gives a plain advice line (Push / Steady / Easy). Explains *why* (e.g. "6 h sleep · hard day yesterday") instead of giving a black-box number. Free | **Built** |
| Journal | Habit correlations | Sleep and energy are stored per day and exported in the CSV | Built (lite); correlations suggested |
| Weekly performance report | Paid | Weekly report now includes Effort and rings-closed days | **Built** |
| Trends (90 vs 365 days) | Arrows | Nutrition history at 7/30/90 days; monthly weight summary | Built (partial); arrows suggested |
| Sleep coach | Sensor | Bedtime reminder from a target wake time; import sleep from Health export | Suggested |
| Awards | Bugged, can't be deleted | Existing XP and badges; add milestone awards (first 5 kg, 30 protein days) that are rebuilt from the data, so they can never be "lost" | Suggested |
| Sharing / competitions | iCloud | Already have code-based challenges; add a weekly rings-closed challenge type | Suggested |
| Fitness+ classes | Paid video | Free guided beginner routines with timers (e.g. a 10-minute home walk, yoga for knees) | Suggested |

## 5. Still open / suggested next
Copy a whole day; barcode scanning; before/after photos; BP and sugar logs; cycle tracking; sleep from Health export feeding Readiness; correlation hints ("on days you slept 7 h+ you ate 180 kcal less"); manual exercise swap; waist chart.

## 6. 4.4 follow-up (built)
- Onboarding → guided tour (6 steps, show-once, replay, existing users offered once) and a Getting-started checklist with XP.
- Rings redesign modelled on Apple Fitness screenshots: weekday ring row, arrow end-caps, ring detail with hourly chart, Change Goal / Pause Ring, Summary card grid, Awards row, and 90-vs-365-day Trends. Awards are derived from logged data, so they can't be "lost" after a restore (a common Apple complaint).
- Hourly data comes from timed workouts plus hourly steps and energy stored by the Health export import. Days without timestamps say so instead of making up hours.
