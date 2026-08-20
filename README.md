# 💌 Love Note Sudoku

A clean, romantic personalized Sudoku game built for seamless play on iPhone and PC, ready for 1-click Vercel hosting.

---

### 🧩 Puzzle Solutions
All level puzzle grids and their complete step-by-step solutions are documented in [`solutions.md`](file:///d:/Github%20Repos/lav/solutions.md).

---

### 💋 Pending Kisses Counter & External Pinging
On the top right of the screen is an interactive **"Pending Kisses"** counter button (`💋 0 kisses`).
- **Global Permanent Storage**: The counter uses a free, public cloud counter service (`counterapi.dev`) so the count persists permanently across all devices and reloads! (e.g. if left at 10, it shows 10 everywhere).
- **Interactive Animation**: Tapping the button increases the count by 1 and spawns floating kiss emoji particles (`💋`).

#### 🌐 How to Ping / Increment Externally:
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

### 🤫 Secret Reset Mechanism
Need to reset the counter back to 0? You have multiple secret options:
1. **Secret UI Gesture**: Triple-tap the header text **`"letter in progress"`** on the app page. A secret prompt will confirm resetting kisses to 0.
2. **Via URL Parameter**: Visit the page with `?secretReset=1` or `?resetKisses=1`:
   ```
   https://your-site.vercel.app/?secretReset=1
   ```
3. **Via Browser Developer Console**:
   ```javascript
   window.resetKisses();
   ```

---

### 🎵 How to Add Your Song (Plays Automatically)
1. Download or copy your song into this folder and name it **`song.mp3`**.
2. When opened or deployed on Vercel, tapping anywhere or pressing **"our song"** plays it smoothly in the background.

---

### 🤫 How to Change Secret Messages
Open [`index.html`](file:///d:/Github%20Repos/lav/index.html) and edit `window.LOVE_CONFIG`:

```javascript
window.LOVE_CONFIG = {
  phrases: [
    "Hi sweetaa",
    "How are you",
    "I miss you",
    "When will we meet",
    "I love you",
    "Forever yours"
  ],

  finaleMessage: "Every moment with you is my favorite puzzle solved. I love you endlessly ♥",
  songFile: "song.mp3"
};
```

---

### 📱 How It Works on iPhone & PC
- **On iPhone / Android:** Tapping any cell opens the native phone numeric keypad directly. No bulky on-screen buttons.
- **On PC:** Use keyboard numbers `1`–`9`, `Backspace` to clear, or Arrow keys to move.
