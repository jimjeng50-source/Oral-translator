# 國印口譯 Oral Translator — 專案總結與留存資料

最後更新：2026-10-11

## 1. 目標
手機上使用的雙向語音翻譯工具：**國語（繁體中文）⇄ 印尼文**，兩人面對面輪流說話，即時辨識、翻譯、朗讀。

## 2. 目前功能
| 功能 | 說明 |
|---|---|
| 語音辨識 | 瀏覽器 Web Speech API（`zh-TW` / `id-ID`） |
| 翻譯 | Gemini API（`gemini-flash-lite-latest`），口語口譯風格；未設金鑰時自動用 MyMemory 免費 API 備援 |
| 朗讀 | 瀏覽器 speechSynthesis，可關閉「自動朗讀」，每則可重聽 |
| 對面看（預設開啟） | 畫面上下分半：上半印尼文側整塊轉 180°，其麥克風按鈕在最上方；下半國語側；中間是工具列。兩側都看得到完整對話（大字＝自己語言） |
| 單人模式 | 關閉「對面看」後為單一對話列表＋打字輸入框（分半模式則每一側各有自己的輸入框） |
| 金鑰設定 | 「🔑 金鑰」按鈕，只存該手機 localStorage，不進 repo |
| PWA | 可加入主畫面；Service Worker 快取靜態檔 |
| 麥克風處理 | 按下時主動請求權限；LINE／FB 等內建瀏覽器會提示改用 Chrome；各種錯誤有中文說明 |

## 3. 檔案
- `index.html`：全部介面與邏輯（無需建置）
- `sw.js`：Service Worker（改版時需把檔內快取名稱的版本號 +1，目前 `oral-translator-v6`）
- `manifest.webmanifest`、`icon.svg`：PWA 設定
- `README.md`：使用與金鑰說明

## 4. 部署
- Repo：`jimjeng50-source/Oral-translator`，正式分支 `main`（開發分支 `ccr-2e4a2de8-v8y98d`，兩者內容同步）
- 網址：**https://jimjeng50-source.github.io/Oral-translator/**
- GitHub Pages：Settings → Pages → Deploy from a branch → `main` / `/ (root)`
- Private repo 免費帳號不能開 Pages，需改 public（repo 內無任何金鑰，可公開）
- 更新流程：改程式 → 推到開發分支與 `main` → 手機重新整理兩次載入新版

## 5. 金鑰
- 來源：Google AI Studio，或 Cloud Console「憑證 → API 金鑰」並啟用 Generative Language API
- 安全：金鑰在前端可被看到，務必在 Cloud Console 限制 API（只勾 Generative Language API）、HTTP 參照位址（`https://jimjeng50-source.github.io/*`），並設用量上限
- 使用者目前的金鑰為 `AQ.` 開頭（新式金鑰）。論壇有許多 `AQ.` 金鑰呼叫 Gemini API 回 401 的回報；使用者手機上的請求已通過驗證（錯誤變成參數問題，見下節），且已於使用者手機實測翻譯成功（2026-10-11，`AQ.` 金鑰可用）。若日後 401，改建 `AIza` 開頭傳統金鑰

## 6. 開發歷程與決策
1. 起初用 MyMemory 免費翻譯（無金鑰、品質普通）。
2. 改用 Google Cloud Translation v2 + API key → 使用者金鑰為 `AQ.`，回「API keys are not supported by this API」。
3. 改用 Gemini API（`x-goog-api-key` header）。首版加了 `thinkingConfig: {thinkingBudget: 0}` → 回 `Request contains an invalid argument`，已移除，目前不再有此參數。
4. 「對面」先只轉印尼文氣泡，不符需求 → 改成上下分半版面，並設為預設。
5. 另一支手機沒有麥克風權限 → 加入主動 `getUserMedia` 請求、內建瀏覽器偵測與錯誤提示。

## 7. 已知限制
- 需要網路；Android Chrome 最穩，iPhone 用 Safari
- 印尼文只支援標準印尼語（`id-ID`）；爪哇語、巽他語等地方語言不支援；口語縮寫（nggak、banget）翻譯較不穩
- 分半模式時狀態列文字不會轉向
- 對話記錄不保存，重新整理就清空
- Gemini 翻譯已由使用者在手機上實測成功（開發端雲端環境無法使用該金鑰，無法自行測試）

## 8. 待辦／可選改進
- （已完成）分半模式每一側都有打字輸入
- 記住「對面看」開關與自動朗讀設定（localStorage）
- 把翻譯失敗的錯誤文字改為「翻譯失敗：<原因>」，避免誤導成網路問題
- 對話記錄保存／匯出
- 想完全隱藏金鑰：加小型後端（例如 Cloudflare Worker）轉送請求
- 語速、語音（男女聲）選項；辨識錯字時可編輯後重譯

## 9. 疑難排解速查
| 狀況 | 處理 |
|---|---|
| 沒麥克風權限 | 用 Chrome 直接開網址；網址列 ⓘ → 權限 → 麥克風 → 允許；Android 設定 → 應用程式 → Chrome → 權限 |
| 從 LINE 開啟無法用麥克風 | 右上 ⋮ → 用其他瀏覽器開啟 → Chrome |
| 語音辨識服務關閉 | 設定 → 應用程式 → 預設應用程式 → 語音輸入 → Google |
| 看到舊版畫面 | 重新整理兩次，或清除該網站資料 |
| 翻譯 401／invalid argument | 檢查金鑰類型（`AIza` 最穩）、已啟用 Generative Language API、API／參照位址限制是否擋到 |
| Pages 沒有 Source 選項 | repo 是 private，改 public |
