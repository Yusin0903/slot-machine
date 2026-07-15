# 生日拉霸機 (Birthday Slot Machine)

一個單頁、手機優先的生日驚喜小遊戲。拉桿拉 4 次，結局完全預先寫死（不是隨機）：
前三次是搞笑「摃龜」（爛獎、差一點中獎、假當機），第四次中頭獎 **H B D**，
機台變成機場登機看板，最後滑出一張生日旅行的**登機證**。

- 收件人：**YUN YI YANG**
- 航班：**TPE → FUK**（台北 → 福岡）
- 日期：**OPEN / 無使用期限**

純 Vanilla HTML / CSS / JS，零依賴、零建置步驟，單一 `index.html` 檔案即可運行。

## 本機預覽

不需要安裝任何東西，直接用瀏覽器打開 `index.html`，或起一個簡單的 static server：

```bash
python3 -m http.server 8080
# 開啟 http://localhost:8080
```

## 玩法

1. 拉下右側拉桿。
2. 前三拉是安排好的橋段（爛獎 → 差一點中獎 → 假的 `ERR 404` 當機，當機後靜止 3
   秒是刻意設計，接著會自動重開機並直接進入第 4 拉）。
3. 第 4 拉中頭獎，機台展開成登機看板，最後滑出登機證。
4. 連續點擊標題 3 次（1.2 秒內）可以重置整個流程重玩。
5. 畫面下方有一個 `SOUND OFF/ON` 按鈕，音效預設關閉（用 WebAudio 即時合成，無音檔）。

## 部署到 Zeabur

這是純靜態網站（沒有 `package.json`），Zeabur 的 zbpack 會自動偵測為 Static
Site，不需要額外設定 build command。

1. 到 [dash.zeabur.com](https://dash.zeabur.com) 用 GitHub 帳號登入。
2. 建立一個新 Project → Add Service → **Deploy from GitHub**。
3. 若這個 repo 沒出現在清單（因為是 private repo），點清單裡的
   **"Configure GitHub App"**，到 GitHub 的 App 授權頁把 `slot-machine` 加進
   Zeabur GitHub App 的 repository access。
4. 回到 Zeabur 選這個 repo，直接部署，等建置完成即可拿到網址。

## 檔案

- `index.html` — 完整的頁面、樣式與互動邏輯（單檔、無外部 JS 依賴，字型透過
  Google Fonts CDN 載入：Anton / JetBrains Mono / Noto Sans TC）。
