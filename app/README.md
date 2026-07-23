# Ascend 🏔️ — a gamified digital-SAT website

Studying for the SAT as a climb: pick a target score, and every question you master lifts your climber toward the summit. Adaptive practice, a **full-length adaptive test** that predicts your 400–1600, streaks / XP / levels, achievements, a friends leaderboard, and an AI tutor — all in a **self-contained static website**.

Built the same way as *The Grocery Gap*: one `index.html` holds the design system + UI + logic; the content lives in `data.js` (`window.ASCEND_DATA`) loaded via `<script>` so it runs straight from `file://`. No build step, no server.

## Run it

**Locally:** double-click `index.html` (or `Open Ascend.command`). It runs fully offline — content loads from `data.js`, and everything single-player works with no connection.

**As an installable app (mobile + desktop):** Ascend is a **PWA**. Installing (service worker + the "Install app" button) needs http/https, so:
- **On your computer:** double-click **`Install Ascend (local server).command`** — it serves the folder at `http://localhost:8756` and opens Chrome. Click the install icon in the address bar (or Dashboard → "Install Ascend as an app") to add it as a desktop app.
- **On your phone:** host it (below), open the URL in the phone's browser, then **Add to Home Screen** (iOS Safari) or **Install app** (Android Chrome). It launches full-screen like a native app and works offline.

**Host it (free):** drag this folder onto [Netlify Drop](https://app.netlify.com/drop), or push to GitHub and turn on Pages, or deploy to Cloudflare Pages / Vercel. No build step, no server, near-zero cost.

## Accounts & login
The first screen is a **login / sign-up** gate: create an account with a username + email or phone + password, or **Continue with Google**. Each account keeps its own progress on the device, and you can log out from the Dashboard. **For now this auth is local to the device** — accounts don't sync across devices, and Google sign-in is simulated. Real accounts + true Google OAuth turn on when Ascend is connected to its cloud backend (Supabase), which also needs the site to be hosted (see `../ascend-production-plan.md`).

## Files
| File | What it is |
|------|-----------|
| `index.html` | The whole site — design system, all screens (home/mountain, **Learn & Practice** modules, full-length adaptive test, dashboard/profile, friends, tutor, review), and the app engine, in one file. |
| `data.js` | The content — the **OpenSAT question bank** (~2,485 questions) + the 25-skill taxonomy + strategy lessons, as `window.ASCEND_DATA`. Add or swap questions here; the site picks them up on reload. |
| `generators.js` | The **Quiz Generator** — procedural question generators for all 25 skills as `window.ASCEND_GEN`. Math is computed fresh (infinite, always-correct); Reading & Writing draws new combinations from curated template banks. Powers the *Generate* tab. |
| `manifest.webmanifest`, `sw.js`, `icon-*.png` | **PWA** app manifest, offline service worker, and app icons — make Ascend installable on phones and computers. |
| `Install Ascend (local server).command` | Double-click to serve Ascend over `http://localhost` so it can be installed as a desktop app. |
| `README.md` | This file. |

## Add or swap questions
Open `data.js` and add objects to `ASCEND_DATA.questions`:
```js
{ id:"m99", sec:"math", skill:"lin-eq", diff:2, type:"mc",
  passage:"", stem:"If 2x = 10, what is x?",
  options:[{k:"A",t:"3"},{k:"B",t:"5"},{k:"C",t:"8"},{k:"D",t:"10"}],
  correct:"B", exp:"Divide both sides by 2: x = 5." }
```
Grid-ins use `type:"grid"`, `options:[]`, and `correct:"5"` (plus optional `accept:["5"]`). Question text may contain LaTeX/markdown math (`$\frac{a}{b}$`, `x^2`, `\sqrt{}`, `\le`, `*italics*`) — the app renders it to real fractions, exponents, radicals, and symbols at display time, so you can paste notation as-is.

## Questions & credits
The question bank comes from **[OpenSAT](https://github.com/Anas099X/OpenSAT)** (by Anas099X), an open SAT question database — its license explicitly permits reuse of the *database*. Questions are tagged only by the 8 official SAT domains, so Ascend sorts them into its 25 finer skill-modules by keyword rules within each domain (`src:"opensat"`). A few skills OpenSAT doesn't cover (e.g. transitions, paired-passage cross-text, inequalities) keep Ascend's own verified questions so every module still has a working quiz (`src:"ascend"`). The Claude generate→verify pipeline in `../content-pipeline/` remains available for authoring more.

## Quiz Generator (endless practice)
The **Generate** tab builds brand-new questions on demand for when you've exhausted a module's bank. Pick a section, a skill, and a count, and the app generates that many fresh questions — each with a worked explanation — then runs them through the normal practice flow. Because the site is fully offline (no server, no live LLM), generation is **procedural**: math questions are computed from templates (so they're unlimited and always correct), and reading questions are assembled in new combinations from curated banks in `generators.js`. Generated items are ephemeral (they don't fill your Review list). A live-AI generator can be added later on top of the planned Supabase/Claude backend.

## Real vs. simulated
All single-player features are real and offline. **Friends / leaderboard** use local demo data and the **AI tutor** is a built-in rule-based coach — both become live, cross-user features when you add the Supabase backend described in `../ascend-production-plan.md` (the "make it real" step).

## Design
Deep night-sky that warms to a **sunrise as your mastery grows**, a rising sun and animated climb, a sticky header nav, a **day / night toggle**, and a layout responsive down to phones.
