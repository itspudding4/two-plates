# Two Plates

A calorie, body-composition (InBody) and sleep tracker for two people. I built it for my girlfriend and me: we both want to lose fat and build muscle, and we use it every day on our iPhones.

**Try the demo:** https://itspudding4.github.io/two-plates/

The demo is the real app running with sample data for two made-up people, Jun and Mei. Nothing you do there is sent anywhere; it stays in your browser.

<p>
<img src="screenshots/today.png" width="200" alt="Today screen: calories left, protein, carbs and fat, partner's day">
<img src="screenshots/log-food.png" width="200" alt="Logging a meal described in words">
<img src="screenshots/progress.png" width="200" alt="Progress: InBody comparison of fat and muscle">
<img src="screenshots/coach.png" width="200" alt="Coach chat proposing a food entry with Apply and No thanks buttons">
</p>

## Why I made it

- Most tracking apps are for one person. We wanted one place where we can see each other's day and keep each other on track.
- Other apps don't read InBody result sheets, and they don't know Taiwanese food: bento boxes, 7-Eleven meals, bubble tea.
- I don't have a Mac, so I couldn't build a native iOS app. Instead I built a web app that you add to the iPhone home screen and use like a normal app, with no App Store needed.

## What it does

- **Log food fast:** snap a photo of the meal or describe it in words, and AI estimates calories, protein, carbs and fat. You can also scan a barcode (the first scan reads the nutrition label and saves it for both of us), use quick add, or tap a saved meal.
- **Today:** calories and macros left for the day, your partner's progress, last night's sleep, and meal ideas that fit what's left.
- **InBody check-ins:** photograph the result sheet and the AI reads every number. You confirm them before saving.
- **Progress:** weight, body fat, muscle and fat-mass trends, calories and protein for the last 14 days, and sleep.
- **Week in review:** after each new InBody test, the app compares the change with what we ate and how we slept since the previous test, and points out what helped and what held us back (weekend eating, late-night snacks, low protein, sugary drinks, short sleep).
- **Coach chat:** ask anything about food, fat loss, muscle or sleep. The coach sees your real data and can propose changes (log a meal, change targets, change sleep goals), shown as cards. Nothing changes until you tap **Apply**, and you can undo it.
- **Sleep:** an iPhone Shortcut sends last night's sleep from Apple Health (from a Xiaomi band through Mi Fitness) to the app, so nobody has to type it.
- **Safe targets:** daily calories and protein come from the InBody BMR (or the Katch-McArdle / Mifflin-St Jeor formulas), activity level and pace. Neither the app nor the AI can set calories below a safe minimum.

## How it works on a phone

- Open the link in Safari, tap **Share**, then **Add to Home Screen**. It gets its own icon and opens full screen like an app.
- Everything is laid out for one hand: one column, a switcher at the top for "my day / her day", and a bottom tab bar (Today · Progress · + · Coach · Profile) with a big + button for logging.
- It fits around the iPhone notch and home bar, works on small screens (tested down to 340 px wide) and has a dark mode.
- The photo, barcode and InBody features use the phone's camera through the normal photo picker.

## How it's built

- One page of HTML, CSS and plain JavaScript, no framework. The source is split into modules in [`source/src`](source/src): `core.js` (data, targets, sleep math), `render.js` (screens and charts), `sheets.js` (pop-up screens and AI prompts), `review.js` (week in review), `coach.js` (coach chat) and `main.js` (buttons, syncing, startup). [`source/build.js`](source/build.js) joins them into one file.
- The real app is hosted as a Claude artifact, which gives it a shared database (so both phones see the same data) and Claude for the AI features. There's no server to run and no API key to pay for: the AI runs on each person's own Claude account.
- Charts are hand-drawn SVG; barcodes are decoded with ZXing.
- I tested it with automated browser tests (Playwright). [`tests/demo-test.js`](tests/demo-test.js) checks this demo on a phone-sized screen: every tab, all the ways of logging food, an InBody check-in, the coach, dark mode on a narrow screen and the laptop page.
- Built over 15 versions in October 2026. Each version came from what we noticed using it on our own phones.

**My role:** I came up with the idea and the features, made the design decisions (a web app instead of a native app, a coach that asks before changing anything, Taiwan-specific food knowledge), tested every version on my iPhone with our real data and set up the Apple Health sleep link. I built it with an AI coding assistant (Claude), which wrote most of the code from my specifications while I directed, reviewed and tested it.

## About this demo

The real app needs Claude for its database and AI, so this public copy runs without them:

| | Real app | This demo |
|---|---|---|
| Data | Shared between our two phones | Sample data, saved only in your browser |
| Photo, InBody and label reading | Claude reads the actual photo | Sample results (type what's in the photo in the note box and it uses that) |
| Describe a meal | Claude understands any description | A built-in list of about 40 common foods |
| Coach chat and week review | Written by Claude from your data | Pre-written answers filled in with your numbers |

Everything else (targets, charts, check-ins, sleep, saved foods, barcodes) is the real app's code, from version 14. The newest version also has a fuller InBody report, which isn't in this demo yet. The demo layer is in [`app.html`](app.html): a stand-in for Claude at the top of the page and the welcome card at the bottom. **Reset sample data** is under the Demo badge.

## Files

- `index.html`: the landing page (on a phone it goes straight to the app)
- `app.html`: the demo app
- `source/`: the real app's source code
- `tests/demo-test.js`: the end-to-end test for the demo
- `demo/`: the demo layer (the stand-in for Claude, the welcome card, the landing page) and `build_site.py`, which rebuilds the demo from `source/` with `python3 demo/build_site.py`
- `icons/`, `manifest.webmanifest`: home-screen icon and settings

To run the demo on your own computer: open a terminal in this folder, run `python3 -m http.server 8765`, then open http://127.0.0.1:8765/.

Built by Kaixin, 2026.
