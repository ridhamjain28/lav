# Dynamic Cloud Progress & Hidden Admin Studio Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Implement resilient cloud-synced game progress with an on-screen save button and initial Level 5 checkpoint, build a secret PIN-protected admin studio (`hidden.html`) to publish new phrases and inspect puzzles/solutions without redeploying code, and completely replace all raw emojis with bespoke vector SVGs.

**Architecture:** A zero-account client-side key-value cloud store (`kvdb.io` / JSON store) manages live `phrases_and_levels` and `player_progress`. `index.html` dynamically consumes live cloud phrases while falling back to local presets, maintaining variable progress starting at Level 5. `hidden.html` acts as a Shadcn-styled admin dashboard for generating Sudoku puzzles, publishing phrases, and managing checkpoints.

**Tech Stack:** Vanilla JavaScript (ES6+), HTML5, CSS3 Glassmorphism, SVG vector graphics, `kvdb.io` / REST API.

## Global Constraints
- **Zero Raw Emojis**: Never use native OS emojis (💋, 💾, 💖, ✨, etc.). Use bespoke SVGs and refined typography only.
- **Variable Progress Checkpoint**: Baseline set to Level 5 (index 4, Levels 1–4 completed), dynamically advancing as levels are solved.
- **Zero Redeploy Updates**: New phrases added via `hidden.html` must become active on `index.html` via cloud sync without code changes.

---

### Task 1: Eliminate All Raw Emojis from `index.html` & Upgrade to Premium SVGs

**Files:**
- Modify: `index.html:1-717` (CSS, HTML, particle generators, buttons, and popups)
- Modify: `index.html:1500-1660` (Kiss counter, particle emitters, celebration overlay)

**Interfaces:**
- Produces: Clean SVG-based kiss counter (`#kissBtn`), celebration overlay (`#yayOverlay`), and particle generator `spawnHearts()` / `spawnKissParticle()` using vector paths instead of unicode emojis.

- [ ] **Step 1: Replace raw emojis in HTML markup with vector SVGs**
Replace `💋` in `#kissBtn`, `💖 ✨ 💕` in `#yayOverlay`, and any other unicode emoji glyphs with inline SVG icons and glowing SVG paths.

- [ ] **Step 2: Upgrade particle generator functions to spawn vector SVGs**
Update `spawnHearts()` and `spawnKissParticle()` to generate `<svg viewBox="0 0 24 24">` heart and sparkle elements with glowing CSS drop-shadows instead of text emojis.

- [ ] **Step 3: Verify visual rendering and commit**
```bash
git add index.html
git commit -m "style: replace all raw emojis with bespoke vector SVGs"
```

---

### Task 2: Cloud Sync Client & Dynamic Level Loader in `index.html`

**Files:**
- Modify: `index.html:720-752` (`LOVE_CONFIG` definition)
- Modify: `index.html:1015-1105` (Game Controller state initialization & loading)

**Interfaces:**
- Produces: `CloudSync` helper (`fetchPhrases()`, `fetchProgress()`, `saveProgress()`).
- Sets initial variable checkpoint: `currentLevel = 4` (Level 5), `levelCompleted = [true, true, true, true, false, false]`.
- Populates `userGrids` for levels 0..3 with their solution grids.

- [ ] **Step 1: Add CloudSync helper in `index.html`**
Implement async `fetchPhrasesAndLevels()`, `fetchPlayerProgress()`, and `pushPlayerProgress()` using `kvdb.io` with local fallback.

- [ ] **Step 2: Update state initialization to start at Level 5 with Levels 1–4 completed**
Set the default checkpoint to Level 5. Fill `userGrids` for levels 0..3 with preset solutions, unlock Diary entries 0..3, and start board on Level 5.

- [ ] **Step 3: Support dynamic phrase expansion**
Update `renderDots()` and `renderDiary()` to dynamically accommodate any number of levels fetched from the cloud.

- [ ] **Step 4: Verify Level 5 starts with 4 completed lines and commit**
```bash
git add index.html
git commit -m "feat: add cloud sync engine and initialize Level 5 progress checkpoint"
```

---

### Task 3: "Save Progress" UI Button & Silent Cloud Autosave in `index.html`

**Files:**
- Modify: `index.html:525-565` (Controls layout & CSS)
- Modify: `index.html:680-690` (Controls HTML)
- Modify: `index.html:1420-1490` (Check button & save event listeners)

**Interfaces:**
- Produces: `#saveBtn` button in the game controls.
- Emits toast / status feedback: `"progress saved safely ♥"` with floating vector sparkles.
- Automatically triggers cloud save on successful level unlock in `checkBtn`.

- [ ] **Step 1: Add the Save Progress button to HTML controls**
Add `<button class="btn" id="saveBtn"><svg ...> save progress</button>` next to `clearBtn` and `checkBtn`.

- [ ] **Step 2: Connect click listener to save locally and to cloud**
When clicked, call `saveGameState()` and `CloudSync.pushPlayerProgress()`, show status `"progress saved safely"`, and spawn celebratory vector particles.

- [ ] **Step 3: Wire silent cloud autosave to level completion**
In `checkBtn` click handler, update `levelCompleted` and `currentLevel`, then automatically push to cloud.

- [ ] **Step 4: Verify manual save and autosave work, then commit**
```bash
git add index.html
git commit -m "feat: add on-screen save progress button with cloud persistence"
```

---

### Task 4: Secret Admin Studio Scaffold & PIN Authentication (`hidden.html`)

**Files:**
- Create: `hidden.html`

**Interfaces:**
- Produces: Complete, responsive admin page with dark glassmorphic Shadcn-style design, gold/rose color palette, and PIN gate modal.
- Consumes: `window.LOVE_CONFIG.adminPin` (default: `"sweetaa"`).

- [ ] **Step 1: Create `hidden.html` with base styles, navigation tabs, and PIN authentication modal**
Implement glassmorphic styling, header with live connection badge, PIN unlock dialog, and session persistence in `sessionStorage`.

- [ ] **Step 2: Verify PIN gate prevents unauthorized view and unlocks on correct PIN, then commit**
```bash
git add hidden.html
git commit -m "feat: scaffold hidden admin studio with secure PIN authentication"
```

---

### Task 5: Phrase Publishing, Sudoku Generator, Records Viewer & Checkpoint Tool in `hidden.html`

**Files:**
- Modify: `hidden.html`

**Interfaces:**
- Produces:
  - Add Phrase Form with 1-click **"Publish to Live Game"** button.
  - Interactive Level Inspector with side-by-side Sudoku & Solution grids.
  - **"Copy Markdown for solutions.md"** export tool.
  - **"Set Player Checkpoint"** dropdown & button to adjust her current level anytime.
- Uses: Inlined `SudokuEngine` from `index.html` to generate puzzles and solutions.

- [ ] **Step 1: Add Sudoku generation and cloud publishing engine**
Allow entering a phrase and clue count, generating the puzzle + solution via `SudokuEngine`, and pushing to the cloud `phrases_and_levels` key.

- [ ] **Step 2: Build the Puzzle & Solution inspector with Markdown exporter**
Display all current levels in an interactive table with side-by-side grid previews and a 1-click button to copy formatted Markdown for `solutions.md`.

- [ ] **Step 3: Build the Player Checkpoint control panel**
Show current player progress fetched live from cloud, with a dropdown to override/set her checkpoint (e.g. Level 1..N) and push to cloud.

- [ ] **Step 4: Verify phrase addition and checkpoint adjustments sync to cloud, then commit**
```bash
git add hidden.html
git commit -m "feat: implement phrase publishing, sudoku generator, and checkpoint controls in hidden.html"
```

---

### Task 6: End-to-End Verification & Documentation Update

**Files:**
- Modify: `README.md`
- Test: Browser verification of `index.html` and `hidden.html`

- [ ] **Step 1: Audit code for zero emojis**
Verify all files have 0 unicode emojis and use vector SVGs.

- [ ] **Step 2: Test live synchronization between `hidden.html` and `index.html`**
Verify that publishing a new phrase in `hidden.html` updates the level list on `index.html`, and that saving progress on `index.html` updates the checkpoint in `hidden.html`.

- [ ] **Step 3: Update `README.md` with admin guide and solutions record instructions**
Document how to access `hidden.html`, how to add phrases, and how cloud sync works.

- [ ] **Step 4: Commit all final updates**
```bash
git add README.md
git commit -m "docs: document hidden admin studio and cloud progress sync"
```
