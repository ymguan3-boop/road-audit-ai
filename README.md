# Road Audit AI — 運用生成式AI加強道路管理維護查核

Reveal.js 互動式簡報，展示審計機關如何運用生成式 AI 工具鏈（ChatGPT、NotebookLM、Excel AI、QGIS + Python）加強道路管理維護查核。

- **線上簡報：** https://ymguan3-boop.github.io/road-audit-ai/
- **部署方式：** GitHub Actions 自動部署至 GitHub Pages（`main` 分支推送即部署）

## 內容架構

簡報共 14 頁，主題包括：

| 頁 | 主題 |
|----|------|
| 1 | 封面 |
| 2 | AI 工具鏈 |
| 3 | 問題意識 |
| 4 | 把 AI 放進審計流程 |
| 5 | 三條驗證線 |
| 6 | 收費標準 |
| 7 | 重複缺失 |
| 8 | 改善進制度 |
| 9 | 改善成果 |
| 10 | AI 均衡 |
| 11 | 何時導入 |
| 12 | 六步驟審計方法 |
| 13 | AI 放大價值 |
| 14 | 結束 |

## 功能

### 📱 QR Code 手機遙控（Slide Remote）

最後一頁（結束頁）左上角顯示 QR Code，觀眾手機掃碼後可遙控簡報翻頁與跳頁。

**運作原理：**
- 主簡報（`index.html`）透過 **Ably Realtime** 建立即時通道，每頁載入產生一組隨機 `room` ID。
- QR Code 內容為手機遙控頁網址（`mobile.html?room=...&total=...`），固定指向 GitHub Pages，即使本機開檔掃碼仍可連線。
- 手機端（`mobile.html`）掃碼進入後顯示「上一頁 / 下一頁」按鈕與完整頁面清單，點擊即發布 `nav` 指令。
- 主簡報收到指令後以 `Reveal.slide(h)` 跳頁；主簡報翻頁時亦發布 `sync` 同步頁碼給手機端。

**連線規則：**
- 主簡報左下角有連線狀態指示燈：`📱 手機已連線 / 手機未連線`（以 Ably presence 偵測，每 10 秒確認一次）。
- 簡報（主機端）關閉即自動斷線，手機端立即顯示斷線狀態。
- 任一端閒置逾 10 分鐘自動斷線。
- 連線規則同時寫在 QR Code 下方供現場查看。

**實測驗證：**
- 手機連線後主簡報狀態燈由「未連線」轉「已連線」。
- 手機發布翻頁指令，主簡報正確跳至指定頁。
- 主簡報關閉後手機端自動斷線；手機離開後主簡報狀態燈自動熄滅。

**使用步驟：**
1. 以無痕視窗開啟線上簡報。
2. 翻到最後一頁，手機掃描左上角 QR Code。
3. 手機進入遙控頁後，即可按「◀ 上一頁 / ▶ 下一頁」或點擊頁面名稱跳頁。

**技術細節：**
- Realtime 通道：`slide-remote-<roomId>`（Ably Realtime，`https://cdn.ably.com/lib/ably.min-2.js`）
- QR Code 產生：`qrcodejs`（`https://cdn.jsdelivr.net/npm/qrcodejs@1.0.0/qrcode.min.js`）
- 通道權限：API Key 為唯讀發佈用金鑰（具發布/訂閱權限）

### ⛶ 全螢幕投放

右上角「⛶ 全螢幕」按鈕可進入瀏覽器全螢幕模式投放（多瀏覽器兼容）。若仍見 Windows 工具列，請改按 F11 或使用 Chrome/Edge。

### 📎 佐證附件

右上角「📎 佐證附件」按鈕，依目前頁面顯示對應的附件清單，可跳至 `attachments.html` 檢視全部 17 份附件（圖片、圖資、函文等）。

## 專案結構

```
├── index.html          # 主簡報（Reveal.js）
├── mobile.html         # 手機遙控頁（掃 QR 進入）
├── attachments.html    # 佐證附件總覽
├── attachments/        # 附件圖片
├── images/             # 簡報素材與流程圖
└── .github/workflows/  # GitHub Pages 自動部署
```

## 開發備註

- 簡報使用 Reveal.js 5.1（jsDelivr CDN）。
- 若需替換 Ably API Key，請同時更新 `index.html` 與 `mobile.html` 中的金鑰。
- 連線狀態、閒置逾時與斷線規則的實作邏輯位於 `index.html` 與 `mobile.html` 的 `<script>` 區塊。
