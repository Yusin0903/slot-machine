# Birthday Slot Machine

A single-page, mobile-first birthday surprise: a Showa-era slot machine.
Every pull of the lever randomly draws 2 real prizes from a configurable
list (occasionally a near-miss tease or a fake system crash plays
instead) — the jackpot never happens on its own. It only fires when you
press a **hidden trigger** (an invisible hotspot in the top-right corner
of the screen), so whoever's operating the machine controls exactly when
it expands into an airport departure board and slides out a **boarding
pass** for a trip. A **🎁 獎品清單** button lets you check everything
that's been won so far.

Pure vanilla HTML / CSS / JS — zero dependencies, zero build step. It's a
single `index.html` file.

## Preview locally

No install needed — just open `index.html` in a browser, or spin up a
simple static server:

```bash
python3 -m http.server 8080
# open http://localhost:8080
```

## How it plays

1. Pull the lever on the right — it always loses, but usually not empty
   handed. Each pull randomly draws **1 prize** from the prize list and
   adds it to the 🎁 獎品清單. There's no fixed order — it's a fresh
   random draw every time.
2. Occasionally (25% of pulls by default, split evenly) a near-miss tease
   or a fake `ERR 404` system crash plays instead of a prize draw. The
   crash is followed by exactly 3 seconds of total silence/stillness —
   intentional, not a bug — then the machine reboots itself and is ready
   for another pull.
3. Tap **🎁 獎品清單** (bottom-right) any time to see everything won so
   far this session, tallied by prize.
4. To make the jackpot happen, tap the **invisible button in the top-right
   corner of the screen** (a 72×72px hotspot, `#secretBtn` in the HTML —
   there's nothing to see, you just have to know it's there). The machine
   expands into a departure board, then slides out a boarding pass.
   - If you tap it while a draw is still playing out, it doesn't cut the
     animation off — the jackpot plays right after that draw finishes.
   - Once the jackpot has played, the lever and the hidden trigger both go
     quiet until you reset.
5. Tap the title 3 times within 1.2s to reset and replay from the top
   (this also clears the 獎品清單).
6. A `SOUND OFF/ON` pill at the bottom toggles audio, off by default
   (synthesized live with WebAudio — no sound files).

## Making it your own

Everything you'd want to change for a different recipient, trip, or set of
jokes lives in one `CONFIG` object at the top of the `<script>` block in
`index.html` — no HTML editing required. Open the file and look for:

```js
const CONFIG = {
  titleLine1: 'BIRTHDAY',
  titleLine2: 'JACKPOT',

  idleCaption: '拉下拉桿，開始你的生日抽獎 🎂',
  rebootCaption: '…系統回復中…',

  // Real prizes — each pull randomly draws 1 of these and logs it to the
  // 獎品清單. A prize with `stock` stops being drawn once it's been won
  // that many times total.
  prizes: [
    { emoji: '💆', label: '按摩券一張' },
    { emoji: '🧼', label: '洗碗券一張' },
    { emoji: '🎂', label: '楊寶春一個' },
    { emoji: '🍞', label: '麵包一個' },
    { emoji: '🍳', label: '煮飯券一張' },
    { emoji: '🧋', label: '珍奶一杯' },
    { emoji: '📖', label: 'my book 兌換券一本' },
    { emoji: '🍚', label: '肉燥飯一碗' },
    { emoji: '📛', label: '黃嘟嘟為你取名頂級券', stock: 3 },  // limited: only 3 in the whole session
  ],

  // Chance (0–1) that a pull is a special event instead of a prize draw,
  // split evenly between the near-miss tease and the fake crash.
  specialEventChance: 0.25,
  tease: { symbol: '💩', caption: '差一個字。獎品：一句「欸差一點欸」' },
  crash: { word: ['4', '0', '4'], caption: 'ERR 404 — PRIZE NOT FOUND' },

  // Shown instead of a prize draw once fewer than 2 prizes have stock left
  // (i.e. every `stock`-capped prize has been fully claimed).
  soldOut: { word: ['4', '0', '4'], caption: 'ERR 404 — 獎品沒了' },

  winWord: ['H', 'B', 'D'],         // jackpot result (exactly 3 chars), only reachable via the hidden trigger
  destWord: ['F', 'U', 'K'],        // shown after winWord on the departure board
  departLabel: 'ANYTIME',
  gate: 'B7',

  passengerName: 'YUN YI YANG',
  fromCode: 'TPE',
  fromCity: '台北桃園',
  toCode: 'FUK',
  toCity: '福岡',
  date: 'OPEN',
  dateNote: '無使用期限',
  flightNumber: 'BD-2026',
  seat: '1A',
  boardingTime: '09:25',
  passMessage: '生日快樂。這次換我帶你去。',
};
```

`winWord`, `destWord`, and `crash.word` must be exactly 3 characters (one
per reel). Add, remove, or edit entries in `prizes` freely — the draw pool
adjusts automatically (needs at least 1 entry with stock left). Each prize
just needs an `emoji` (fills all 3 reels when drawn) and a `label` (the
full text, any length); add `stock: N` to cap how many times a prize can
ever be won in one session — once it's out, it silently stops appearing in
draws and the 獎品清單 shows "限量款，已送完" next to it. If you cap
enough prizes that none are left with stock (e.g. you give every prize a
`stock`), the machine shows an `ERR 404 — 獎品沒了` sold-out message on
every pull instead of drawing — right now only `黃嘟嘟為你取名頂級券` is
capped, so this state isn't reachable unless you add `stock` to more
prizes too. Change the prizes, the trip, the passenger name, whatever —
save the file and reload.

Want the hidden trigger somewhere other than the top-right corner? It's
the `#secretBtn` element right after the `SOUND OFF` button in the HTML —
move it, resize it, or restyle it (it's fully transparent by default).

## Deploying to Zeabur

This is a static site (no `package.json`), so Zeabur's zbpack auto-detects
it as a Static Site — no build command needed.

1. Sign in to [dash.zeabur.com](https://dash.zeabur.com) with GitHub.
2. Create a new Project → Add Service → **Deploy from GitHub**.
3. If this repo doesn't show up in the list (it's private), click
   **"Configure GitHub App"** in that list, and on the GitHub authorization
   page add this repo to the Zeabur GitHub App's repository access.
4. Back in Zeabur, select the repo and deploy — you'll get a URL once the
   build finishes.

## Files

- `index.html` — the entire page: markup, styles, and interaction logic in
  one file (fonts loaded from Google Fonts CDN: Anton / JetBrains Mono /
  Noto Sans TC).
