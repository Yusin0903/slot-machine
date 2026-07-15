# Birthday Slot Machine

A single-page, mobile-first birthday surprise: a Showa-era slot machine you
pull 4 times. The outcome is **fully scripted** (not random). The first
three pulls are gag "losses" (a joke prize, a near-miss, and a fake system
crash); the fourth pull hits the jackpot, the machine transforms into an
airport departure board, and a **boarding pass** slides out for a trip.

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

1. Pull the lever on the right.
2. The first three pulls are scripted beats (a joke prize → a near-miss →
   a fake `ERR 404` crash). The crash is followed by exactly 3 seconds of
   total silence/stillness — that's intentional, not a bug — then the
   machine reboots itself and auto-advances into pull 4.
3. Pull 4 is the jackpot: the machine expands into a departure board, then
   slides out a boarding pass.
4. Tap the title 3 times within 1.2s to reset and replay.
5. A `SOUND OFF/ON` pill at the bottom toggles audio, off by default
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
  errorCaption: 'ERR 404 — PRIZE NOT FOUND',

  loseWord1: ['N', 'O', '!'],       // pull 1 result (exactly 3 chars)
  joke1: '恭喜獲得：你欠我的那杯珍奶（本人親自兌換）',

  loseSymbol2: '💩',                 // pull 2 near-miss final symbol
  joke2: '差一個字。獎品：一句「欸差一點欸」',

  crashWord: ['4', '0', '4'],       // pull 3 fake-crash result

  winWord: ['H', 'B', 'D'],         // pull 4 jackpot result (exactly 3 chars)
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

Every field is a plain string (or a 3-element array for the reel results —
`winWord`, `destWord`, `loseWord1`, and `crashWord` must each be exactly 3
characters/emoji, one per reel). Change the jokes, the prize, the trip, the
passenger name, whatever — save the file and reload.

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
