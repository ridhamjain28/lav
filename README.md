# 💌 Love Note Sudoku

A clean, romantic personalized Sudoku game built for seamless play on iPhone and PC, ready for 1-click Vercel hosting.

---

### 🎵 How to Add Your Song (Plays Automatically)
1. Download or copy your song into this folder and name it **`song.mp3`**.
2. When opened or deployed on Vercel, tapping anywhere or pressing **"our song"** plays it smoothly in the background.
*(If no `song.mp3` is provided, a romantic soft music melody plays automatically).*

---

### 🤫 How to Change Secret Messages (Without Her Knowing)
Open [`index.html`](file:///d:/Github%20Repos/lav/index.html) and edit lines 294–312:

```javascript
window.LOVE_CONFIG = {
  phrases: [
    "Hi sweetaa",
    "How are you",
    "I miss you",
    "When will we meet",
    "I love you",
    "Forever yours"
    // 👉 Add or change any lines here!
  ],

  // Grand finale message shown when all levels are solved:
  finaleMessage: "Every moment with you is my favorite puzzle solved. I love you endlessly ♥",

  songFile: "song.mp3"
};
```

Whenever you add or change lines, the Sudoku engine automatically generates matching playable puzzles for every phrase you set.

---

### 📱 How It Works on iPhone & PC
- **On iPhone / Android:** Tapping any cell opens the native phone numeric keypad directly. No bulky on-screen buttons.
- **On PC:** Use keyboard numbers `1`–`9`, `Backspace` to clear, or Arrow keys to move.
