# 國印口譯 Oral Translator

手機網頁工具（PWA）：國語（繁中）⇄ 印尼文 雙向語音對話翻譯。

## 使用
1. 按「說國語」或「Bicara Indonesia」→ 說話 → 自動辨識、翻譯並朗讀。
2. 雙方輪流按各自的按鈕即可對話。可用「↻ 對面」把印尼文氣泡轉 180°，方便面對面。
3. 語音辨識不可用時，可用下方輸入框打字。
4. 加入主畫面：Android Chrome「安裝應用程式」／iPhone Safari「分享 → 加入主畫面」。

## 技術
- 無需建置：純 HTML/JS。語音辨識 Web Speech API、朗讀 speechSynthesis、翻譯使用 Gemini API（`gemini-flash-lite-latest`）。
- 金鑰：按右上「🔑 金鑰」貼上，只存在該手機的 localStorage，不會進 repo。未設定時自動用 MyMemory 免費 API 備援。
- 麥克風需 HTTPS 或 localhost。本機測試：`python3 -m http.server 8000`；部署可用 GitHub Pages。
- 需要網路；Android Chrome 支援最佳，iPhone 請用 Safari。
## 取得並保護 Gemini API 金鑰
1. 到 [Google AI Studio](https://aistudio.google.com/apikey) 建立金鑰（有免費額度）。
2. 按右上「🔑 金鑰」貼上。金鑰只存在該手機的 localStorage，不進 repo。
3. 建議到 Google Cloud Console →「API 和服務 → 憑證」編輯該金鑰：API 限制只勾 Generative Language API；應用程式限制選「HTTP 參照位址」，填 `https://jimjeng50-source.github.io/*`。
4. 若 `AQ.` 開頭的新式金鑰回 401，請在 Cloud Console 憑證頁建立傳統 `AIza` 開頭的 API 金鑰（並啟用 Generative Language API）。
