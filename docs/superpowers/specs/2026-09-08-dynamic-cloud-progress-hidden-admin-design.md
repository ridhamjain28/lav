# Dynamic Cloud Progress & Hidden Admin Studio Design Specification

## Overview
This specification details the architecture and implementation for:
1. **Dynamic, Cloud-Synced Progress System**: A resilient progress tracking system for the player (girlfriend) with variable cloud and local storage, a visible "Save Progress" button, silent autosaves on level clears, and an initial starting checkpoint set to Level 5 (index 4, 4 levels completed).
2. **Hidden Admin Studio (`hidden.html`)**: A private, PIN-protected admin dashboard allowing the creator to:
   - Add new romantic phrases dynamically and automatically generate valid 9x9 Sudoku puzzles & solutions.
   - Publish new phrases directly to the live website via zero-account cloud storage without redeploying code.
   - View/inspect Sudoku puzzle grids and solutions for personal records, with 1-click Markdown export for `solutions.md`.
   - Monitor and dynamically adjust the player's checkpoint level (e.g., set to Level 5 now, adjust to any level anytime).
3. **Strict Aesthetic Standard — Zero Emojis / Premium Romantic SVGs**:
   - **Zero Raw Emojis**: Replace all system emojis (💋, 💾, 💖, ✨, etc.) across `index.html` and `hidden.html` with bespoke, high-resolution vector SVG graphics, Lucide-style iconography, and smooth glowing CSS particle effects.

---

## 1. Design & Aesthetic Standards (No Emojis)
- **Icons**: Crisp SVG icons (Lucide / custom romantic vectors) with tailored stroke widths, gold (`var(--gold)`), rose (`var(--rose)`), and soft glowing filters.
- **Kisses & Particles**: Smooth animated SVG kiss/heart vector paths with radial gradient glow instead of unicode emoji glyphs.
- **Celebration & Badges**: Refined typographic badges and glowing gold/rose SVG ornaments.
- **Admin UI**: Styled like a sleek Shadcn/Tailwind dark theme with glassmorphic cards, gold/rose accents, and clean Lucide SVG icons.

---

## 2. Cloud Architecture & Data Model

### 2.1 Zero-Setup Cloud Key-Value Store
We use a lightweight, zero-account client-side key-value endpoint (`https://kvdb.io/` or JSON store) configured via `window.LOVE_CONFIG.cloudSyncKey`.

### 2.2 Data Schemas

#### A. `phrases_and_levels`
Stores the complete list of game levels:
```json
[
  {
    "id": 1,
    "phrase": "Hi sweetaa",
    "puzzle": [[... 9x9 array ...]],
    "solution": [[... 9x9 array ...]]
  },
  ...
]
```
- **Fallback / Baseline**: If cloud is not yet seeded or offline, the 6 standard preset levels defined in `index.html` act as the initial baseline.

#### B. `player_progress`
Stores the player's dynamic progress:
```json
{
  "currentLevel": 4,
  "levelCompleted": [true, true, true, true, false, false],
  "userGrids": [... optional saved active cell inputs ...],
  "currentSongIndex": 0,
  "lastUpdated": "2026-09-08T21:50:00.000Z"
}
```
- **Initial State**: Variable checkpoint set to Level 5 (index 4: Levels 0, 1, 2, 3 marked completed).
- **Dynamic Progression**: As the player clears Level 5, `levelCompleted[4]` becomes `true` and `currentLevel` advances to 5, updating both `localStorage` and the cloud store.

---

## 3. Player Experience (`index.html`)

### 3.1 Initialization & Merging
1. **Local & Cloud Sync**: On load, `index.html` loads from `localStorage` and immediately queries the cloud progress endpoint. The higher/latest progress wins.
2. **Dynamic Levels**: If new phrases have been published from `hidden.html`, the game dynamically loads the expanded list of levels, updating:
   - Top-bar level dots (e.g., 6 dots -> 7+ dots).
   - The Diary panel (adds new blank dashed lines for undiscovered phrases).
   - Note header progress text.
3. **Diary & Board State**: Completed levels (0..3) have their diary entries unlocked and their puzzle grids pre-filled with the known solution.

### 3.2 Save Progress Controls
- **"Save Progress" Button**: Added to the controls bar with a sleek SVG bookmark/heart icon.
  - Tapping saves `localStorage` + cloud KV.
  - Emits floating SVG heart/sparkle particles and displays status text: `"progress saved safely"`.
- **Autosave**: Triggered on `checkBtn` level completion and cell entries.
- **Accidental Refresh Protection**: Retained and updated for dynamic level sets.

---

## 4. Secret Admin Studio (`hidden.html`)

### 4.1 Security & Authentication
- PIN gate modal on entry (default: `"sweetaa"`, customizable in `LOVE_CONFIG.adminPin`).
- Session stored in `sessionStorage` so the creator stays logged in until tab close.

### 4.2 Features & UI Layout
1. **Live Stats & Progress Monitor**:
   - Displays player's current status: `"Currently playing Level 5 (4 levels cleared)"`.
   - **Set Checkpoint Tool**: Dropdown and button to set player's checkpoint to any level (1..N) with instant cloud sync.
2. **Add New Phrase Form**:
   - Phrase input text field.
   - Clue count selector (default: 34 clues - Romantic/Casual difficulty).
   - Button: **`Publish to Live Game`** (with sleek SVG sparkle/upload icon):
     - Client-side `SudokuEngine` generates a valid, unique Sudoku puzzle & solution.
     - Automatically appends the new level to the cloud store.
     - Live updates the level table without reload.
3. **Level Inspector & Personal Records**:
   - Interactive table showing all levels (ID, Phrase, Status, Clues).
   - Side-by-side Sudoku Grid and Full Solution Grid viewer.
   - **`Copy Markdown for solutions.md`**: Generates pre-formatted ASCII/Markdown tables for pasting into `solutions.md`.
   - **`Export Backup JSON`** / **`Import Backup JSON`**: For manual backup archives.

---

## 5. Verification & Testing Plan
- **Zero Emoji Audit**: Scan all HTML/CSS/JS in `index.html` and `hidden.html` to guarantee no raw unicode emojis remain, replaced by crisp SVG vectors.
- **Initial Baseline Test**: Confirm `index.html` starts at Level 5 with Levels 1–4 completed in Diary and filled in board history.
- **Save Progress Test**: Test clicking "Save Progress" and verifying both `localStorage` and cloud store update.
- **Level Clear Progression**: Simulate clearing Level 5; verify progress advances to Level 6 and persists across hard refresh.
- **Admin Phrase Publishing**: From `hidden.html`, add a test phrase (e.g., Level 7); confirm `index.html` loads Level 7 in the Diary and level dots.
- **Admin Checkpoint Override**: From `hidden.html`, adjust checkpoint and verify `index.html` reflects the change.
