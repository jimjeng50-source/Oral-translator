# 國印口譯 Oral Translator

手機網頁工具（PWA）：國語（繁中）⇄ 印尼文 雙向語音對話翻譯。

## 使用
1. 按「說國語」或「Bicara Indonesia」→ 說話 → 自動辨識、翻譯並朗讀。
2. 雙方輪流按各自的按鈕即可對話。可用「↻ 對面」把印尼文氣泡轉 180°，方便面對面。
3. 語音辨識不可用時，可用下方輸入框打字。
4. 加入主畫面：Android Chrome「安裝應用程式」／iPhone Safari「分享 → 加入主畫面」。

## 技術
- 無需建置：純 HTML/JS。語音辨識 Web Speech API、朗讀 speechSynthesis、翻譯 MyMemory 免費 API（無需金鑰，有每日用量限制）。
- 麥克風需 HTTPS 或 localhost。本機測試：`python3 -m http.server 8000`；部署可用 GitHub Pages。
- 需要網路；Android Chrome 支援最佳，iPhone 請用 Safari。
- 想要更高品質／無用量限制：把 `translate()` 換成自己的後端（Claude／Google／DeepL API），金鑰不要放在前端。
