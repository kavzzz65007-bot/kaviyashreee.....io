# ResumeLens

**ResumeLens shows you exactly which edits moved your resume's ATS score.**

You paste (or upload) your old resume (v1) and your edited resume (v2). ResumeLens splits the change into individual edits, applies each one *on its own* to v1, re-scores it, and tells you what every edit was really worth - including the edits that made your score **worse**, with a one-click **Undo** for just that edit.

- 100% in your browser. Nothing is uploaded, there is no server, and there are no paid APIs.
- The score is **deterministic**: the same input always gives the same score.
- Has a **3D login**, a 3D animated score ring, 3D impact bars and floating 3D particles - with automatic flat 2D fallbacks if a device cannot run WebGL.

---

## Contents

1. [What's in the box (your "index / style / js / readme" files)](#1-whats-in-the-box)
2. [Run it on your computer](#2-run-it-on-your-computer-step-by-step)
3. [Put it on GitHub and host it for free](#3-put-it-on-github-and-host-it-for-free-github-pages)
4. [Using the app (3-minute tour)](#4-using-the-app-3-minute-tour)
5. [How the score works](#5-how-the-score-works)
6. [How the Edit Impact Tracker works](#6-how-the-edit-impact-tracker-works)
7. [Important: the login is local-only](#7-important-the-login-is-local-only)
8. [Limits and what happens at them](#8-limits-and-what-happens-at-them)
9. [Folder structure](#9-folder-structure)
10. [Troubleshooting](#10-troubleshooting)
11. [Manual QA checklist](#11-manual-qa-checklist)

---

## 1. What's in the box

You asked for an `index.html`, a CSS file, JavaScript and a README. This project has all four, organised the way modern websites with React, Tailwind and 3D are built:

| You asked for | In this project | What it does |
|---|---|---|
| `index.html` | [`index.html`](index.html) (project root) | The one HTML page. It has an empty `<div id="root">` that the app fills in. |
| `style.css` | [`src/index.css`](src/index.css) | All styling: Tailwind CSS plus our dark theme, glass cards, 3D tilt, animations. |
| `js` | [`src/main.jsx`](src/main.jsx) and everything in `src/` | The JavaScript: `src/engine/` (scoring, diff, attribution), `src/three/` (3D scenes), `src/components/` and `src/pages/` (screens). |
| `README` | this file | Setup, hosting, usage, QA. |

**Why not just one `.html`, one `.css` and one `.js`?** React, Tailwind and Three.js need a *build step* that bundles them into plain files. You run that step with one command (`npm run build`, or GitHub does it for you automatically). The result is a folder called `dist/` that *does* contain plain `index.html` + CSS + JS files - that folder is what gets published on the web.

---

## 2. Run it on your computer (step by step)

You only do the setup once.

### Step 1 - Install Node.js

Node.js is the tool that runs the build. Download the **LTS** version from <https://nodejs.org> (you need **version 22.13 or newer**) and run the installer, accepting the defaults.

Check it worked: open a terminal (**Windows:** press the Windows key, type `cmd`, press Enter. **Mac:** press Cmd+Space, type `Terminal`, press Enter) and type:

```bash
node --version
```

You should see something like `v22.x.x` or `v24.x.x`. If you see "not recognized" or "command not found", close the terminal, open a new one and try again, or reinstall Node.js.

### Step 2 - Open the project folder in the terminal

Unzip the project somewhere you can find it (for example your Desktop), then in the terminal go into the folder:

```bash
cd Desktop/resumelens
```

(Use the real path to where you unzipped it. Tip: type `cd ` with a space, then drag the folder into the terminal window and press Enter.)

### Step 3 - Install the packages

```bash
npm install
```

This downloads React, Three.js and friends into a `node_modules` folder. It takes a minute or two and needs internet. Warnings in yellow are normal; red `ERR!` lines are not (see [Troubleshooting](#10-troubleshooting)).

### Step 4 - Start the app

```bash
npm run dev
```

The terminal prints a link like `http://localhost:5173/`. Open it in your browser (Chrome, Edge, Firefox or Safari). Leave the terminal open while you use the app. Press **Ctrl + C** in the terminal to stop it.

### Other commands

| Command | What it does |
|---|---|
| `npm test` | Runs the unit tests (scoring, diff, attribution, 3D maths, storage/login, file reading). |
| `npm run build` | Makes the production site in the `dist/` folder. |
| `npm run preview` | Serves the built `dist/` folder locally so you can check it before publishing. |

---

## 3. Put it on GitHub and host it for free (GitHub Pages)

You need a free account at <https://github.com>. The project already contains the automatic publishing recipe (`.github/workflows/deploy.yml`), so you only need to get the code onto GitHub and switch one setting on.

### Option A - with Git (recommended)

1. Install Git from <https://git-scm.com/downloads> (accept the defaults).
2. On GitHub click **+ -> New repository**. Name it `resumelens`. Choose **Public** (free GitHub Pages needs a public repository on free accounts). Do **not** tick "Add a README". Click **Create repository**.
3. In your terminal, inside the `resumelens` folder, run these commands one by one (replace `YOUR-USERNAME` with your GitHub username):

   ```bash
   git init
   git add .
   git commit -m "ResumeLens"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/resumelens.git
   git push -u origin main
   ```

   GitHub will ask you to sign in the first time (a browser window opens). If you get "src refspec main does not match any", run the `git commit` line again and then the rest.
4. On your repository page go to **Settings -> Pages**. Under **Build and deployment -> Source** choose **GitHub Actions**.
5. Click the **Actions** tab. You will see "Deploy to GitHub Pages" running (about 2-3 minutes). When it shows a green tick, your site is live at:

   **`https://YOUR-USERNAME.github.io/resumelens/`**

   If the run happened *before* you did step 4, open the run and press **Re-run all jobs**, or just make any small change and push again.

From now on, every time you run `git add . && git commit -m "update" && git push`, the site updates itself.

### Option B - no Git, upload in the browser

1. Create the repository as in step 2 above, then click **uploading an existing file** on the empty repository page.
2. Drag **the contents** of the `resumelens` folder in (not the folder itself) - but **do not** include `node_modules` or `dist`. Make sure the hidden `.github` folder goes up too (Mac: press **Cmd + Shift + .** in Finder to show hidden files; Windows: View -> Show -> Hidden items). Click **Commit changes**.
3. Continue from step 4 of Option A. (If the upload page refuses the `.github` folder, use Option A instead - it is far less fiddly.)

### Why the site works at any web address

`vite.config.js` sets `base: './'`, and the app uses `#/` style addresses (like `…/#/app`). That means it works on `https://user.github.io/repo/` without any server settings - refreshing the page never gives a 404.

### About `package-lock.json`

The first time you run `npm install`, a file called `package-lock.json` appears. Commit it (it is included automatically by `git add .`). It pins exact package versions so the online build matches your computer. The workflow uses it when present.

---

## 4. Using the app (3-minute tour)

1. **Home** - the animated 3D score ring and a summary. Click **Try it as a guest** (no sign-up) or **Sign in to start**.
2. **3D login** - the orb follows your mouse, spins while checking, shakes on a wrong password and glows green on success. Use **Create account**, **Sign in** or **Continue as guest**. (See [section 7](#7-important-the-login-is-local-only).)
3. **Tracker** - press **Load demo data** for a full example, or:
   - paste or **Upload** (PDF, DOCX or TXT - drag and drop works too) resume **v1** and **v2**;
   - optionally paste a **job description**;
   - press **Analyze edits**.
4. **Read the results**
   - **Overall ATS score** - v1, v2 and the change.
   - **What each edit was worth** - 3D bars (green up, red down, labelled with the edit), then a list of every edit with its points. Click *Show before / after text* on any edit.
   - **Regression alert** - edits that lowered your score are red, with **Undo this edit**, which reverts only that edit in v2 and re-analyzes.
   - **Interaction effect** - the row that makes the numbers add up exactly (explained below).
   - **What to improve next** - the top 3 improvements not yet applied, ranked by estimated gain.
   - **Which words moved the score** - v2 with green (raised the score) and red (lowered it) words. Hover or tap a word for its exact effect.
   - **Score breakdown**, **missing keywords** and **weak or vague bullets** with a one-line fix each.
5. **Score timeline** - use **Save v1 / v2 as version** to store versions (v1, v2, v3 ...). The line chart shows their scores. Use any saved version as the new v1 or v2 with one click.

Your text is auto-saved as a draft in this browser, so a reload never loses it.

---

## 5. How the score works

Fixed, transparent rules - no AI, no randomness. Total = 100 points.

| Category | With a job description | Without one | What is measured |
|---|---:|---:|---|
| Keyword match | 35 | - | Share of the job description's top 30 keywords (weighted by how often they appear, word-stem matched) found in the resume. |
| Quantified metrics | 20 | 31 | Share of bullets containing a number, %, $ amount, or unit (target: half of bullets). |
| Action verbs | 15 | 23 | Share of bullets that open with a strong verb such as *Led*, *Built*, *Reduced* (target: 80%). |
| Standard sections | 15 | 23 | Contact info, Summary, Experience, Education, Skills present. |
| Length, readability and weak phrases | 15 | 23 | Word count (best 350-800), bullets of 6-30 words, and phrases like "responsible for", "worked on", "various", "etc." |

With no job description the 35 keyword points are shared among the other four categories (so the total is still out of 100). Each category's score is rounded to one decimal, and the total is the sum of the rounded category scores - so the breakdown always adds up exactly.

The score is an *estimate of ATS-friendliness*. It is not a guarantee of how any employer's real system will rank you.

---

## 6. How the Edit Impact Tracker works

1. **Diff.** v1 and v2 are compared line by line (a longest-common-subsequence diff) and grouped into **chunks**: *changed* bullets (a rewritten bullet is one chunk, not a delete plus an add), *added*, *removed*, *moved* sections, and keyword/skills changes.
2. **Counterfactual attribution.** For each chunk, ResumeLens builds "v1 with only this chunk applied" and re-scores it. The difference from v1's score is that chunk's **delta** (for example "Added metric to bullet 3: +6").
3. **Interaction effect.** Edits can overlap. If two edits both add the same keyword, each looks worth full credit alone, but together they count once. So:

   `interaction effect = (v2 score - v1 score) - (sum of every individual delta)`

   The interaction row is shown explicitly, so **individual deltas + interaction = total change, always** (it is unit-tested, including on hundreds of random resume pairs). The attribution is labelled *approximate* in the app because credit for overlapping edits is shared by this rule rather than split.
4. **Regression + Undo.** A chunk with a negative delta is flagged. **Undo** rebuilds v2 with all chunks except that one.
5. **Next best edit.** Candidate improvements (add a missing JD keyword to Skills, add a number to a bullet, replace a weak phrase, add a strong verb, tighten a long bullet, add a missing section) are each *simulated* on v2 and re-scored; the top 3 gains are shown.
6. **Word-level highlight.** Each edited word is removed from v2 (or a deleted word restored) and the text re-scored; the difference decides green or red.

---

## 7. Important: the login is local-only

ResumeLens has **no server**, so the "3D login" is **not real security**:

- Accounts are saved only in *this browser* on *this device*, in `localStorage`.
- The password is salted and hashed (PBKDF2-SHA256, 100,000 rounds) so it is never stored as plain text, but anyone who can use your browser profile can still read or delete the data.
- The purpose is to keep **saved resume versions separate** on a shared computer.
- **Do not reuse a real password** here. The app says so on the login screen.

If you later want real accounts you would need a backend (for example a small server with a database) - that is deliberately outside this project.

---

## 8. Limits and what happens at them

| Situation | What ResumeLens does |
|---|---|
| Empty text | Friendly message under the box ("Paste or upload Resume v1 to continue"). |
| Very short text (< 40 words) | Still analyzed, with a warning that the score is not very meaningful. |
| Very long text (> 40,000 characters, or > 1,500 lines) | Blocked with a clear message (a normal resume is ~3,000-6,000 characters). Job description limit: 20,000 characters. |
| v1 and v2 identical | Both scores are shown, plus a note that there are no edits to measure; suggestions still work. |
| No job description | Keyword weight is shared among the other categories; a note explains it. |
| Wrong file type | "Not a supported file type. Upload a PDF, DOCX or TXT, or paste the text." (`.doc` gets its own "save as .docx" hint.) |
| File over 5 MB / empty file | Rejected with the size or "is empty" message. |
| Scanned / image-only PDF | "This PDF looks scanned: it has no selectable text" - export a text PDF or paste the text. |
| Password-protected or damaged PDF/DOCX | Friendly message; nothing crashes. |
| PDF with more than 15 pages | First 15 pages read, with a note. |
| Browser blocks `localStorage` | Everything still works; a notice says versions last only until the tab closes. |
| No WebGL / reduced-motion setting on | Flat 2D versions of every 3D element; with reduced motion the 3D scenes are still pictures and CSS animations are switched off. |
| Any unexpected error | An error boundary shows a "Something went wrong" card with *Try again*, *Back to home* and *Reload* - never a blank page. |

---

## 9. Folder structure

```text
resumelens/
├── index.html                  <- the single HTML page
├── package.json                <- packages + commands (dev, build, preview, test)
├── vite.config.js              <- build settings (base './' for GitHub Pages)
├── .npmrc  .gitignore          <- install / git settings
├── README.md                   <- this file
├── .github/workflows/
│   └── deploy.yml              <- automatic GitHub Pages publishing
├── public/
│   └── favicon.svg
├── src/
│   ├── main.jsx                <- starts React
│   ├── App.jsx                 <- shell + hash routing + route guard
│   ├── index.css               <- ALL the styling (Tailwind + our theme)
│   ├── engine/                 <- pure logic, no React, fully unit-tested
│   │   ├── constants.js        <- weights, keyword lists, limits
│   │   ├── text.js             <- parsing a resume into lines/bullets/sections
│   │   ├── scoring.js          <- the 0-100 scorer
│   │   ├── diff.js             <- v1 -> v2 chunks (LCS diff)
│   │   ├── attribution.js      <- counterfactual deltas + interaction + undo
│   │   ├── nextBest.js         <- "next best edit" simulator
│   │   ├── wordImpact.js       <- green/red word highlighting
│   │   ├── analysis.js         <- validation + one-call orchestrator
│   │   └── demoData.js         <- sample v1, v2 and job description
│   ├── lib/
│   │   ├── storage.js          <- safe localStorage, versions, drafts
│   │   ├── auth.js             <- local profiles (hashed passwords)
│   │   ├── AuthContext.jsx     <- shares the signed-in profile
│   │   ├── fileParsing.js      <- PDF / DOCX / TXT -> text, with friendly errors
│   │   └── hooks.js            <- reduced motion, WebGL check, hash router
│   ├── three/                  <- all 3D (loaded lazily)
│   │   ├── math.js             <- pure 3D maths (tested)
│   │   ├── SceneCanvas.jsx     <- the one <Canvas> wrapper
│   │   ├── BackdropScene.jsx   <- floating particles
│   │   ├── HeroRingScene.jsx   <- animated 3D score ring
│   │   ├── ImpactBarsScene.jsx <- 3D impact columns
│   │   ├── LoginOrbScene.jsx   <- the 3D login orb
│   │   └── lazy.js, usePointer.js, arcGeometry.js
│   ├── components/             <- ErrorBoundary, SafeScene, TiltCard, panels, ...
│   └── pages/                  <- Home.jsx, Login.jsx, Tracker.jsx
└── tests/                      <- node --test unit tests
```

---

## 10. Troubleshooting

| Problem | Fix |
|---|---|
| `npm` / `node` "not recognized" or "command not found" | Install Node.js (step 1), then **close and reopen** the terminal. |
| `npm install` fails with `EBADENGINE` or syntax errors | Your Node is too old. Install the current LTS (22.13+). |
| `npm install` fails with network errors (`ETIMEDOUT`, `ENOTFOUND`) | Check your internet/VPN/proxy and run it again. |
| Port 5173 already in use | Vite picks the next free port and prints it - use the link it prints. |
| Page is blank / old after `git push` | Open the **Actions** tab: the run must be green. Then check **Settings -> Pages -> Source = GitHub Actions**. Hard-refresh with Ctrl+Shift+R. |
| Actions run fails at "Run the unit tests" or "Build" | Click the failed step to read the message. If it is a package-version problem, run `npm install` locally, commit the new `package-lock.json` and push. |
| 3D looks flat / "simple circles" | Your browser has WebGL turned off or you enabled "reduce motion" - this is the intended fallback. In Chrome: Settings -> System -> "Use graphics acceleration when available". |
| PDF upload says "looks scanned" | The PDF is a picture of text. Export a text-based PDF from Word/Google Docs, or paste the text. |
| PDF/DOCX upload fails in `npm run dev` only | Stop the dev server and try `npm run build` then `npm run preview`. |
| I forgot my local password | There is no reset (no server). Use **Continue as guest**, or clear this site's data in browser settings (this deletes saved versions). |

---

## 11. Manual QA checklist

Run `npm test` first (all green), then tick through this in a browser. Each line says what you should see.

**Landing and navigation**
- [ ] Home loads with no blank flash; headline, buttons and the score ring are visible; the ring fills to ~85.6.
- [ ] Floating particles are behind the content and never block clicks.
- [ ] Cards tilt toward the mouse on hover (desktop) and do nothing on touch.
- [ ] Header: **Tracker** with no profile redirects to the login page; address bar shows `#/login`.
- [ ] Unknown address such as `#/nonsense` shows the home page. Refreshing any page never shows a 404.

**3D login**
- [ ] The orb follows the mouse. It spins while checking, shakes red on a wrong password, swells green on success, then opens the tracker.
- [ ] Empty username/password -> inline error; no crash.
- [ ] Create account with a taken name -> "already taken" message. Wrong password -> "Username or password is incorrect".
- [ ] Buttons are disabled while checking (no double submit). **Continue as guest** works. **Sign out** returns to Home.
- [ ] Create two accounts: each sees only its own saved versions and draft.

**Inputs and files**
- [ ] **Analyze edits** with empty boxes -> friendly messages under v1 and v2, focus jumps to the first one.
- [ ] Upload a `.png` -> "not a supported file type". Upload a `.doc` -> "save it as .docx or PDF".
- [ ] Upload a file over 5 MB -> size message. Upload an empty file -> "is empty".
- [ ] Upload a text PDF, a DOCX and a TXT -> the text appears and "Loaded ..." shows. Drag-and-drop a file onto a box works.
- [ ] Upload a scanned PDF -> "looks scanned" message.
- [ ] Paste 40,000+ characters -> "too long" message and the counter turns red. Paste 5 words -> a "very short" warning but analysis still runs.
- [ ] Leave the job description empty -> analysis works and an info note says the keyword points were shared.

**Results**
- [ ] **Load demo data** fills all three boxes and shows results (v1 39.9 -> v2 85.6, +45.7). Pressing it with existing text needs a second click.
- [ ] 3D bars: green up, red down, one violet "Interaction" bar, each labelled with its edit. (Flat bars if WebGL is off.)
- [ ] The line "Edits + interaction = total" adds up to the change shown in the summary.
- [ ] Two red **Regression** rows appear in the demo. **Undo this edit** changes v2, re-analyzes, and the toast says "Reverted ...".
- [ ] Identical v1 and v2 -> "identical" note, no bars, suggestions still shown.
- [ ] Editing a box after analysis shows "results are out of date" and disables Undo until you re-analyze.
- [ ] **What to improve next** shows up to 3 items, each with an estimated gain.
- [ ] Word highlight: green and red words, struck-through deleted words, hover shows points; the "only changed lines" checkbox works.
- [ ] Missing/found keywords and weak bullets (with suggestions) are listed.

**Versions and storage**
- [ ] **Save v1 / v2 as version** adds v1, v2 ... to the timeline chart; saving identical text says "Already saved".
- [ ] **Use as v1 / v2** loads that version; **Delete** needs a second click.
- [ ] Reload the page: your text and versions are still there. Clear all (second click) empties the boxes but keeps saved versions.
- [ ] Private/incognito window: the app still works (a notice may say versions last only until the tab closes).

**Responsive, motion and resilience**
- [ ] At phone width (375 px) there is no sideways scrolling on Home, Login or Tracker, and the buttons are tappable.
- [ ] With "Reduce motion" on in your OS: no tilt, no page slide, 3D scenes are still pictures, numbers appear instantly.
- [ ] Turn off hardware acceleration (or use a browser without WebGL): every 3D element has a flat replacement and nothing breaks.
- [ ] Open the browser console (F12): no red errors or yellow warnings during any of the above.
- [ ] Rapidly click Analyze and Load demo repeatedly: no double results, no freeze.

---

## Tech notes

- **Stack:** React 19, Vite, Tailwind CSS 4, Three.js with `@react-three/fiber` and `@react-three/drei`, `pdfjs-dist` (PDF text), `mammoth` (DOCX text).
- **Determinism:** scores are integers in tenths of a point; no randomness anywhere in scoring (particle positions use a seeded generator).
- **Performance:** 3D code is lazy-loaded, pixel ratio is capped, particle counts are reduced on phones, the score ring "fills" by changing a draw range instead of rebuilding geometry, and every geometry is disposed when its scene unmounts.
- **Tests:** `npm test` runs scoring, diff, attribution (including *individual deltas + interaction = total*, checked on random resume pairs), 3D maths, storage/login and file-reading tests.
