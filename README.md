# Love Note Sudoku

A clean, romantic personalized Sudoku game built for seamless play on iPhone and PC, ready for 1-click Vercel hosting.

---

### Secret Studio (`hidden.html`)
Manage your phrases, generate puzzles, and monitor player progress from your private admin studio:
- Visit `https://your-site.vercel.app/hidden.html` (or open `hidden.html` locally).
- Enter your creator PIN (default: `sweetaa`).
- **Add New Phrases Live**: Type any romantic phrase and click **"Generate & Publish to Live Game"**. It creates a valid Sudoku puzzle + solution and updates her game instantly with zero redeployment.
- **Player Checkpoint Manager**: View her current level and cleared count, or adjust her active checkpoint to any level.
- **Solutions & Records**: Inspect puzzle clues and solutions side-by-side, or copy Markdown formatted tables directly for records.

---

### Cloud Progress & Save Button
- **On-Screen Save Button**: The player can tap **"save"** in the controls anytime to lock in progress across `localStorage` and zero-account cloud storage (`kvdb.io`).
- **Autosave**: Level completions automatically sync to the cloud.
- **Starting Checkpoint**: Defaults to Level 5 ("I love you") with Levels 1–4 solved in The Diary.

---

### Pending Kisses Counter & External Pinging
On the top right of the screen is an interactive **"Pending Kisses"** counter button.
- **Global Permanent Storage**: The counter uses a live cloud counter service so the count persists permanently across all devices and reloads.
- **Interactive Animation**: Tapping the button increases the count by 1 and spawns glowing SVG heart particles.

#### How to Ping / Increment Externally:
1. **Via Link / URL Parameter**:
   Share or visit the link with `?kiss=1` (or `?addKiss=5`):
   ```
   https://your-site.vercel.app/?kiss=1
   ```
2. **Via JavaScript (Console / Developer Script)**:
   ```javascript
   window.addKiss(1); // Increments kisses count by 1
   ```
3. **Via HTTP API / Curl Ping**:
   ```bash
   curl https://api.counterapi.dev/v1/lav_love_kisses_app_v1/pending_kisses/up
   ```
4. **Outbound Webhook (Optional)**:
   In `window.LOVE_CONFIG`, set `kissWebhookUrl` to your Discord/Telegram/Slack/Ntfy webhook URL to get instant notifications whenever a kiss is tapped!

---

### Secret Reset Mechanism
Need to reset the counter back to 0? You have multiple secret options:
1. **Secret UI Gesture**: Triple-tap the header text **`"letter in progress"`** on the app page.
2. **Via URL Parameter**: Visit the page with `?secretReset=1` or `?resetKisses=1`:
   ```
   https://your-site.vercel.app/?secretReset=1
   ```
3. **Via Browser Developer Console**:
   ```javascript
   window.resetKisses();
   ```

---

### Audio Playlist
1. Place audio files (`song.mp3`, `song2.mp3`, `song3.mp3`) in this folder.
2. The built-in player features playlist navigation, play/pause, and an interactive seekbar.

---

### Puzzle Solutions Record
All preset level puzzle grids and their complete solutions are documented in [`solutions.md`](file:///d:/Github%20Repos/lav/solutions.md).
