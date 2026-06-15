# WD-CABBY: CABBY Online Gaming Portal

---

## Context

CABBY is a browser-based online gaming portal where visitors can browse and play free HTML5 games directly in their browser. The portal hosts third-party games via iframe embedding rather than building the games themselves. The audience is casual gamers looking for quick, no-download play sessions across desktop and tablet browsers.

The portal adds four engagement features on top of standard browsing: a daily missions system that gives all users the same 3 missions per calendar day, a streak counter that rewards consecutive days of completing all 3 missions, a pause bookmarks system that lets users save where they left off in any game, and an automatic highest score tracker that records each user's best score per game. The portal is strictly frontend, no backend server, no database, no third-party APIs. The platform target is the latest versions of Chrome, Firefox, and Edge on desktop and tablet.

---

## Project Information

| Field | Details |
|---|---|
| Project ID | WD-CABBY |
| Project Type | Online Gaming Portal Website |
| Client / Brand | CABBY |
| Platform / Medium | Browser, desktop and tablet |
| Deliverable Type | Source code on GitHub plus live deployed URL |
| Primary Goal | Functional gaming portal with daily missions, streaks, bookmarks, and best-score tracking |

---

## Main Goal

Build a complete, responsive, frontend-only gaming portal called CABBY that lets users browse and play free HTML5 games in the browser, complete daily missions to grow a streak, save bookmarks to resume games later, and track their personal best score per game. All user data persists to browser localStorage with no backend, no database, and no API keys.

The portal has four pages:

1. **Home** with a daily missions section above a searchable, category-filtered game grid
2. **Game** that loads any selected game in an iframe with score saving, bookmark saving, and personal-best display
3. **Leaderboard** with two tabs (All Scores and Your Best) for viewing saved scores
4. **Profile** with display name, four stat cards, favourites list, bookmarks list, and reset

---

## About CABBY

CABBY is a new gaming portal brand. There is no prior product, no existing site, and no existing user base. The brand uses a yellow steering wheel icon paired with the wordmark "CABBY" in matching yellow. The logo file is provided as logo.png.

**Brand tone:** playful, dark-themed. Visual identity is defined by the colour palette in the Visual & Technical Specs section.
**Brand line / tagline:** none required for this build

---

## Requirements

The freelancer will build a four-page frontend-only website that browses, embeds, and tracks HTML5 games using localStorage for all persistence, with a daily mission system, streak counter, pause bookmarks, and automatic best-score tracking.

### Pages and Navigation

All pages share a top navigation bar that includes the CABBY logo, search bar, current streak counter (flame icon + number), credits indicator, notification bell, user icon, and Log in button. The Home, Leaderboard, and Profile pages also share a left sidebar with these 10 genre category icons in order: All Games (active by default), Trending, Favourites, then a dot separator, then Racing, Puzzle, Action, Runner, Sports, New, Settings.

- **Home page:** "Today's Missions" section with exactly 3 mission cards above the game grid. The grid shows minimum 9 game cards in 3 columns at 1024px+ or 2 columns at 768 to 1023px. Search filters cards in real time by name or genre. Clicking a sidebar category filters the grid.
- **Game page:** Back button, game title, best score display ("Your best: N" or "Your best: none"), iframe of minimum height 400px with sandbox="allow-scripts allow-same-origin", score input + Save Score button, Save Bookmark button, Add to Favourites toggle. If the iframe fails to load, show "Game could not be loaded. Try another one."
- **Leaderboard page:** Tab switcher between "All Scores" and "Your Best" views. All Scores table has 4 columns (Rank, Game Name, Score, Date) sorted by Score descending. Your Best table has 4 columns (Rank, Game Name, Highest Score, Date achieved) sorted by Highest Score descending. Clear All Scores button with confirmation prompt. Empty state message when the active tab has no data.
- **Profile page:** Avatar circle (initials on green background, "?" if no name), display name (or "Guest"), Edit Name button, four stat cards (Games Played, Total Scores Saved, Favourites, Best Streak), favourites list with remove buttons, bookmarks list with delete buttons and clickable game names, Reset Profile button with confirmation.

### Daily Missions System

- The Home page shows a "Today's Missions" section above the game grid containing exactly 3 mission cards
- Missions are drawn from a fixed pool of 10 mission templates defined in the source code
- All users see the same 3 missions on the same calendar date, chosen deterministically from the pool based on the date
- Missions reset at local midnight based on the user's browser timezone
- Each mission card shows the mission text and a checkbox or icon indicating completion status
- Missions auto-complete when the user performs the required action, no manual check required

The fixed pool of 10 mission templates:

1. Play any game for 2 minutes total
2. Score above 500 in any game
3. Try a game you have never played before
4. Play games from 3 different categories
5. Add a game to favourites
6. Save 3 scores in one day
7. Set or change your display name
8. Play a game for 5 minutes total
9. Score above 1000 in any game
10. Visit the leaderboard page

### Streak System

- The current streak is stored in localStorage as part of the cabby_streak object
- Completing all 3 missions in a single day increments cabby_streak.current by 1 and updates lastCompletedDate to today's date in YYYY-MM-DD format
- If the user opens the site and the current date is more than one day after lastCompletedDate without all 3 missions having been completed the previous day, cabby_streak.current resets to 0
- The best streak (cabby_streak.best) is updated to max(best, current) whenever current changes, and is never reset
- The navigation bar on every page shows a flame icon followed by the current streak value

### Pause Bookmarks System

- The Game page contains a Save Bookmark button next to the Save Score button
- Clicking the Save Bookmark button opens an inline text input that accepts up to 80 characters, with a Save action
- Saving stores a bookmark object to the cabby_bookmarks localStorage array with the fields: id (unique string), gameId, gameName, note (the user-entered text or empty string), date (YYYY-MM-DD), and lastScore (the user's last entered score for that game, or 0)
- The Profile page contains a Bookmarks section listing every saved bookmark with game name, note, and date
- Each bookmark in the list has a delete button that removes that bookmark from cabby_bookmarks
- Clicking the game name of any bookmark navigates to the Game page for that bookmarked game

### Highest Score Tracker

- Each user has a cabby_best_scores object in localStorage mapping game ID to the highest score ever entered for that game
- When the user clicks Save Score, the system reads the current best for that game ID, and updates cabby_best_scores only if the new score is greater than the stored value
- The Game page displays the current best score above the iframe in the format "Your best: N", or "Your best: none" if no best is recorded for that game ID
- The Leaderboard page provides a tab switcher between "All Scores" and "Your Best" views
- The "Your Best" view shows one row per game that has a recorded best score, with columns: Rank, Game Name, Highest Score, Date achieved
- Rows in "Your Best" are sorted by Highest Score descending

### Game Data

- Games are not built by the developer. They are defined as a static JavaScript array or JSON file
- A minimum of 9 game entries is required
- Each entry has fields: id (string), name (string), genre (string), rating (number), ageTag (string or null), url (string), description (string)
- Game URLs may be placeholder URLs during development as long as the iframe loading mechanism works

### Data Persistence

- All persistence uses localStorage only
- All keys must be prefixed with `cabby_`
- `cabby_username`: string, the user's display name
- `cabby_scores`: JSON array of objects with fields `game`, `score`, and `date` in YYYY-MM-DD format
- `cabby_favourites`: JSON array of game ID strings
- `cabby_missions_state`: object with fields `date` (YYYY-MM-DD), `completed` (array of 3 booleans)
- `cabby_streak`: object with fields `current` (number), `best` (number), `lastCompletedDate` (YYYY-MM-DD or null)
- `cabby_played_games`: array of game ID strings the user has ever played
- `cabby_session_times`: object mapping game ID strings to total seconds played
- `cabby_bookmarks`: JSON array of bookmark objects with fields `id`, `gameId`, `gameName`, `note`, `date`, `lastScore`
- `cabby_best_scores`: JSON object mapping game ID strings to the highest score recorded for that game

> **Rule for this section:** Every requirement above is testable. No fuzzy language. Every count, breakpoint, key name, and field type is exact.

---

## Visual & Technical Specs

### Dimensions / Sizing

- **Page width:** responsive, target range 768px to 1440px wide
- **Nav bar height:** approximately 48px, padding 10px 16px
- **Sidebar width:** 48px
- **Sidebar icon buttons:** 34 × 34 px, border radius 8px
- **Game card aspect ratio:** 16:10
- **Game card border radius:** 8px
- **Game card grid gap:** 10px
- **Game card bolt icon:** top-left of card, colour #1D9E75
- **Game card age tag:** bottom-right of card, text "16+" in colour #999999 at 9px, present only on age-restricted games
- **Mission card height:** approximately 80px, border radius 8px
- **Mission card grid:** 3 columns at all viewport widths above 768px, 10px gap
- **Rating badge:** border radius 4px, padding 1px 6px
- **Streak indicator:** flame icon at 14px next to number text at 13px in the nav bar
- **Info icon circle:** 18px diameter
- **Button border radius:** 6px
- **Content padding:** 14px 16px

### Typography

| Usage | Font | Notes |
|---|---|---|
| Logo text "CABBY" | System sans-serif, weight 500 | 16px, colour #F5C842, letter spacing 1px |
| Page headings | System sans-serif, weight 500 | 15px, colour #EEEEEE |
| Mission card title | System sans-serif, weight 500 | 12px, colour #DDDDDD |
| Game card name | System sans-serif, weight 500 | 12px, colour #DDDDDD |
| Rating badge | System sans-serif, weight 500 | 10px, colour #FFFFFF |
| Streak number | System sans-serif, weight 500 | 13px, colour #F5C842 |
| Best score label | System sans-serif, weight 500 | 13px, colour #F5C842 |
| Search placeholder | System sans-serif, weight 400 | 13px, colour #777777 |
| Table text | System sans-serif, weight 400 | 13px, colour #CCCCCC |
| Stat card number | System sans-serif, weight 500 | 20px, colour #FFFFFF |
| Stat card label | System sans-serif, weight 400 | 12px, colour #999999 |
| Profile display name | System sans-serif, weight 500 | 18px, colour #FFFFFF |

**Not allowed:** decorative fonts, script fonts, web fonts that require external CDN loading.

### Colors

| Purpose | Color name | Hex |
|---|---|---|
| Logo and brand | Logo yellow | `#F5C842` |
| Primary action | Primary green | `#1D9E75` |
| Destructive action | Danger red | `#E24B4A` |
| Streak flame | Flame orange | `#EF9F27` |
| Nav bar background | Nav background | `#2A2A2A` |
| Sidebar background | Sidebar background | `#252525` |
| Page background | Page background | `#1E1E1E` |
| Mission card background | Card surface | `#2A2A2A` |
| Mission card completed | Completed green tint | `#1D3328` |
| Border colour | Border | `#333333` |
| Text primary | Text primary | `#EEEEEE` |
| Text secondary | Text secondary | `#999999` |
| Search input bg | Input background | `#3A3A3A` |

**Color rules:**

1. No gradients on UI components except for the game card backgrounds, which use dark genre-specific gradients
2. No drop shadows, glow effects, or transparency effects on text
3. Completed mission cards use the completed green tint as background and the primary green as a checkmark icon
4. All colours must match the hex values above within a close visual range

---

## Technical Requirements

- **Engine / Framework:** Vanilla HTML, CSS, and JavaScript. React is acceptable but not required
- **Language:** HTML, CSS, JavaScript (ES6+)
- **Platform / Target:** Browser, desktop and tablet
- **Storage:** Browser localStorage only, all keys prefixed `cabby_`
- **Browser support:** Latest Chrome, Firefox, and Edge
- **Code quality:** Code is split across multiple files matching the Folder Structure section below. No single JS file exceeds 800 lines. Each function includes a one-line comment describing its purpose

### Allowed Tech Stack

1. Vanilla HTML, CSS, and JavaScript
2. React with Create React App or Vite
3. Static site generators that produce client-only output

### Not Allowed

- Backend servers of any kind (Node, Python, PHP, etc.)
- Databases (Firebase, Supabase, MongoDB, Postgres, etc.)
- Third-party authentication services (Google sign-in, Auth0, etc.)
- Any feature that requires an API key to function
- Paid plugins, paid themes, or paid third-party services

---

## Design Direction

**Do:**

- Use a dark theme with the navy and charcoal background colours specified
- Apply 14px 16px content padding throughout (per Visual & Technical Specs) with a single H1 per page and H2 section headings beneath it
- Use solid colour fills on UI elements, with gradients only on game card backgrounds
- Place the "Today's Missions" section directly above the game grid on the Home page, clearly separated from the grid
- Show completed missions distinctly from pending ones using the completed green tint background and a checkmark icon
- Show the user's best score for the current game prominently above the iframe on the Game page
- Match the layout shown in `reference_image.png`

**Do not:**

- Use light theme or white backgrounds
- Add decorative animations beyond simple hover states and completion feedback
- Use stock illustrations or photographic backgrounds
- Add any branding elements other than the CABBY logo

---

## Reference Materials

| Asset | File / URL |
|---|---|
| CABBY brand logo | `assets/logo.png` |
| Portal layout reference | `assets/reference_image.png` |

**How to use these references:** Place `logo.png` in the top-left corner of the navigation bar on every page at its specified size of 28 × 28 px. Use `reference_image.png` as the visual target for the overall page layout, sidebar style, nav bar arrangement, mission card placement, and game card design. Match the structure, colour palette, and proportions. Where the reference conflicts with a written specification in this document, the written specification wins.

---

## Deliverables

| # | Item | Format | Notes |
|---|---|---|---|
| 1 | Source code | Public GitHub repository | All source files organised by the Folder Structure section below |
| 2 | Live deployed site | Public URL | GitHub Pages or Vercel free tier |
| 3 | README.md | `.md` | Tech stack, setup instructions, live URL |

### File Naming Convention

- Lowercase kebab-case for HTML, CSS, JS files (e.g., `game-card.js`, `missions.js`, `bookmarks.js`)
- PascalCase for React component files (e.g., `GameCard.jsx`, `BookmarkCard.jsx`)
- No spaces in file names
- No backup files (`copy.js`, `final.js`, `final-v2.js`, etc.) in the final commit

### Folder Structure

Recommended structure for a vanilla JS build:

```
cabby/
  index.html
  css/
    style.css
  js/
    app.js
    games.js
    missions.js
    bookmarks.js
    storage.js
  assets/
    logo.png
  README.md
```

Recommended structure for a React build:

```
cabby/
  public/
    logo.png
  src/
    App.jsx
    pages/
      Home.jsx
      Game.jsx
      Leaderboard.jsx
      Profile.jsx
    components/
      Navbar.jsx
      Sidebar.jsx
      GameCard.jsx
      MissionCard.jsx
      BookmarkCard.jsx
      StreakBadge.jsx
    data/
      games.js
      missions.js
    utils/
      storage.js
      missionLogic.js
  package.json
  README.md
```

---

## Build / Export Specifications

- **Build:** the live site must be deployable as static files only, no server runtime required
- **Source files:** all source code committed to the GitHub repository in its raw, editable form
- **Production build:** if using React, the production build output must also be deployable on a static host

---

## Scope Boundaries

### DO

- Build all four pages: Home, Game, Leaderboard, Profile
- Build the daily missions system with the 10 mission templates listed
- Build the streak counter with current and best values stored in localStorage
- Build the pause bookmarks system on Game and Profile pages
- Build the highest score tracker with display on Game page and Your Best tab on Leaderboard
- Use localStorage with the exact key names specified
- Include at least 9 game entries in the game data file
- Match the layout shown in the reference image
- Deploy to a public URL and provide the link in the README

### DO NOT

- Build any games from scratch
- Add multiplayer, chat, or social features
- Add ads, in-app purchases, or any monetisation
- Add login via Google, Facebook, or any third-party identity provider
- Add a backend, server, database, or any feature needing API keys
- Add support for screens below 768px wide
- Add additional missions beyond the 10 templates listed, or change the deterministic selection logic

---

## Tool Requirements

- **Required tools:** any code editor that saves UTF-8 plain text (VS Code, Sublime, Notepad++, etc.), Git
- **Acceptable alternatives:** any text editor that can save UTF-8 plain text
- **Forbidden:** no-code site builders, AI-generated full-site code generators that produce unreadable output

---

## Acceptance Checklist

These are the binary delivery-gate criteria. Every item must pass for the submission to be accepted. Granular per-feature scoring lives in the separate Rubric.

1. [ ] All 4 pages exist and are reachable via navigation: Home, Game, Leaderboard, Profile.
2. [ ] Daily missions render exactly 3 cards per day, are deterministic by calendar date, auto-complete on the matching user action, and reset at local midnight.
3. [ ] Streak counter increments only when all 3 daily missions complete, persists across sessions, and resets correctly when a day is skipped.
4. [ ] Pause bookmarks can be saved from the Game page, list on the Profile page, and clicking a bookmark's game name navigates back to that Game page.
5. [ ] Best score tracker writes to `cabby_best_scores`, updates only when the new score is higher, and displays as `Your best: N` or `Your best: none` on the Game page.
6. [ ] All 9 documented `cabby_`-prefixed localStorage keys are implemented with the exact schemas specified in the Data Persistence section.
7. [ ] No backend, database, third-party auth, API keys, or paid services are present in the codebase or at runtime.
8. [ ] Game embedding uses an `<iframe>` with `sandbox="allow-scripts allow-same-origin"` and shows the documented error fallback message when the game URL fails to load.
9. [ ] Visual implementation honors the documented colour palette (12 hex values), typography table, and layout dimensions (page width 768-1440px, sidebar 48px, game card 16:10 aspect ratio with 8px border radius).
10. [ ] Project is deployed to GitHub Pages or Vercel free tier with a working public URL, and the repository contains a `README.md` at the root.
11. [ ] No placeholder text appears in the final delivery: no Lorem ipsum, no `TODO`, no `FIXME`, and no sample-text artifacts in user-visible UI or game data.

---

## Evaluation Criteria

The Acceptance Checklist above is the binary delivery gate: all 11 items must pass for the submission to be accepted. There is no partial-credit threshold at this gate. Granular per-feature scoring (40-item rubric) is applied separately to the accepted build and lives in the project's rubric file. Visual fidelity to `reference_image.png`, correctness of localStorage behaviour, deterministic missions, accurate streak logic, working bookmarks, and accurate best-score tracking are the highest-weighted areas of rubric judgement.

---

## Designer's / Developer's Choices

| Decision | Accepted Options |
|---|---|
| Framework | Vanilla JS, React, or any static-only stack |
| Routing approach | Hash-based routing, single-page JS switching, or multi-page navigation |
| Build tool | None, Vite, Create React App, or any free static builder |
| Hosting platform | GitHub Pages or Vercel free tier |
| Icon library | Any free icon library (Lucide, Tabler, Heroicons) or inline SVG |
| Genre gradient palettes | Developer chooses tones, must remain dark and genre-suggestive |
| Game data source | Static array in JS file or static JSON file in repo |
| Mission selection formula | Any deterministic function of date that yields 3 distinct missions from the pool of 10 |
| Bookmark ID generation | Any unique string approach (timestamp, UUID, random hash) |

---

## Delivery Terms

- **Revisions included:** 2 rounds of minor revisions after delivery
- **Allowed revision types:** colour adjustments to match the palette, element repositioning, copy edits, bug fixes for documented features
- **What does not count as a revision:** adding new features outside this document, changing the page count, changing the tech stack, adding any feature that requires a backend or API key
- **Delivery method:** GitHub repository link plus the live deployed URL, both included in the final message

---

## Final Goal

When a visitor opens the CABBY URL, they see a dark-themed gaming portal with a yellow CABBY logo in the top-left, a search bar in the centre of the nav, a flame icon with their current streak number in the nav, and a sidebar of category icons on the left. The main area opens with today's 3 missions clearly visible at the top, followed by a grid of at least 9 game cards they can browse, filter by search, or filter by category. Clicking any card opens the game in an iframe with their personal best score shown above it, where they can play, save a score, save a bookmark to return to later, and mark it as a favourite. As they play, their missions auto-complete and their streak grows. They can visit the Leaderboard to switch between all their saved scores and their personal best per game, or visit the Profile page to set a display name, view their play stats and best streak, manage their favourites, and revisit any bookmark they saved. Everything they do is saved to their browser instantly, with no sign-up, no payment, and no waiting.
