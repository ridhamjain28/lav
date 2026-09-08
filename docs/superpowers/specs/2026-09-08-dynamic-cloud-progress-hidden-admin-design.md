# Dynamic Cloud Progress & Hidden Admin Studio Design Specification

## Overview
This specification details the architecture and implementation for:
1. **Dynamic, Cloud-Synced Progress System**: A resilient progress tracking system for the player (girlfriend) with variable cloud and local storage, a visible "💾 save progress" button, silent autosaves on level clears, and an initial starting checkpoint set to Level 5 (index 4, 4 levels completed).
2. **Hidden Admin Studio (`hidden.html`)**: A private, PIN-protected admin dashboard allowing the creator to:
   - Add new romantic phrases dynamically and automatically generate valid 9x9 Sudoku puzzles & solutions.
   - Publish new phrases directly to the live website via zero-account cloud storage without redeploying code.
   - View/inspect Sudoku puzzle grids and solutions for personal records, with 1-click Markdown export for `solutions.md`.
   - Monitor and dynamically adjust the player's checkpoint level (e.g., set to Level 5 now, adjust to any level anytime).

---

## 1. Cloud Architecture & Data Model

### 1.1 Zero-Setup Cloud Key-Value Store
We use a lightweight, zero-account client-side key-value endpoint (`https://kvdb.io/` or JSON store) configured via `window.LOVE_CONFIG.cloudSyncKey`.

### 1.2 Data Schemas

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
- **Initial State**: Preset to Level 5 (index 4: Levels 0, 1, 2, 3 marked completed).
- **Dynamic Progression**: As the player clears Level 5, `levelCompleted[4]` becomes `true` and `currentLevel` advances to 5, updating both `localStorage` and the cloud store.

---

## 2. Player Experience (`index.html`)

### 2.1 Initialization & Merging
1. **Local & Cloud Sync**: On load, `index.html` loads from `localStorage` and immediately queries the cloud progress endpoint. The higher/latest progress wins.
2. **Dynamic Levels**: If new phrases have been published from `hidden.html`, the game dynamically loads the expanded list of levels, updating:
   - Top-bar level dots (e.g., 6 dots -> 7+ dots).
   - The Diary panel (adds new blank dashed lines for undiscovered phrases).
   - Note header progress text.
3. **Diary & Board State**: Completed levels (0..3) have their diary entries unlocked and their puzzle grids pre-filled with the known solution.

### 2.2 Save Progress Controls
- **"💾 save progress" Button**: Added to the controls bar.
  - Tapping saves `localStorage` + cloud KV.
  - Emits floating heart/sparkle animation and displays status text: `"progress saved safely ♥"`.
- **Autosave**: Triggered on `checkBtn` level completion and cell entries.
- **Accidental Refresh Protection**: Retained and updated for dynamic level sets.

---

## 3. Secret Admin Studio (`hidden.html`)

### 3.1 Security & Authentication
- PIN gate modal on entry (default: `"sweetaa"`, customizable in `LOVE_CONFIG.adminPin`).
- Session stored in `sessionStorage` so the creator stays logged in until tab close.

### 3.2 Features & UI Layout
1. **Live Stats & Progress Monitor**:
   - Displays player's current status: `"Currently playing Level 5 (4 levels cleared)"`.
   - **Set Checkpoint Tool**: Dropdown and button to set player's checkpoint to any level (1..N) with instant cloud sync.
2. **Add New Phrase Form**:
   - Phrase input text field.
   - Clue count selector (default: 34 clues - Romantic/Casual difficulty).
   - Button: **`✨ Generate & Publish to Live Game`**:
     - Client-side `SudokuEngine` generates a valid, unique Sudoku puzzle & solution.
     - Automatically appends the new level to the cloud store.
     - Live updates the level table without reload.
3. **Level Inspector & Personal Records**:
   - Interactive table showing all levels (ID, Phrase, Status, Clues).
   - Side-by-side Sudoku Grid and Full Solution Grid viewer.
   - **`📋 Copy Markdown for solutions.md`**: Generates pre-formatted ASCII/Markdown tables for pasting into `solutions.md`.
   - **`💾 Export Backup JSON`** / **`📥 Import Backup JSON`**: For manual backup archives.

---

## 4. Verification & Testing Plan
- **Initial Baseline Test**: Confirm `index.html` starts at Level 5 with Levels 1–4 completed in Diary and filled in board history.
- **Save Progress Test**: Test clicking "💾 save progress" and verifying both `localStorage` and cloud store update.
- **Level Clear Progression**: Simulate clearing Level 5; verify progress advances to Level 6 and persists across hard refresh.
- **Admin Phrase Publishing**: From `hidden.html`, add a test phrase (e.g., Level 7); confirm `index.html` loads Level 7 in the Diary and level dots.
- **Admin Checkpoint Override**: From `hidden.html`, adjust checkpoint and verify `index.html` reflects the change.
