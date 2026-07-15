# Birthday Slot Machine

A single-page, mobile-first birthday surprise: a Showa-era slot machine.
Every pull of the lever is a scripted "loss" (a joke prize, a near-miss, a
fake system crash — looping forever) — the jackpot never happens on its
own. It only fires when you press a **hidden trigger** (an invisible
hotspot in the top-right corner of the screen), so whoever's operating the
machine controls exactly when the machine expands into an airport
departure board and slides out a **boarding pass** for a trip.

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

1. Pull the lever on the right — it always loses. It cycles through a set
   of scripted beats forever, looping back to the start: a joke prize → a
   near-miss → a fake `ERR 404` system crash (followed by exactly 3
   seconds of total silence/stillness — intentional, not a bug — then the
   machine reboots itself and is ready for another pull).
2. To make the jackpot happen, tap the **invisible button in the top-right
   corner of the screen** (a 72×72px hotspot, `#secretBtn` in the HTML —
   there's nothing to see, you just have to know it's there). The machine
   expands into a departure board, then slides out a boarding pass.
   - If you tap it while a loss is still playing out, it doesn't cut the
     animation off — the jackpot plays right after that loss finishes.
   - Once the jackpot has played, the lever and the hidden trigger both go
     quiet until you reset.
3. Tap the title 3 times within 1.2s to reset and replay from the top.
4. A `SOUND OFF/ON` pill at the bottom toggles audio, off by default
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

  // The lever cycles through these forever (loops back to index 0).
  // type: 'plain' (reels land on `word`) | 'tease' (reels tease winWord,
  // then slip to `symbol`) | 'crash' (fake system crash + reboot).
  losses: [
    { type: 'plain', word: ['N', 'O', '!'], caption: '恭喜獲得：你欠我的那杯珍奶（本人親自兌換）' },
    { type: 'tease', symbol: '💩', caption: '差一個字。獎品：一句「欸差一點欸」' },
    { type: 'crash', word: ['4', '0', '4'], caption: 'ERR 404 — PRIZE NOT FOUND' },
  ],

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

`winWord`, `destWord`, and every `word` in `losses` must be exactly 3
characters/emoji (one per reel). Add, remove, or reorder entries in
`losses` freely — the cycle length adjusts automatically. Change the
jokes, the prize, the trip, the passenger name, whatever — save the file
and reload.

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
