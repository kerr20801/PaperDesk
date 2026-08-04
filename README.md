# 📄 PaperDesk

合併、分割、重排、轉正 PDF — 檔案完全不離開你的瀏覽器。

Merge, split, reorder and rotate PDFs — your files never leave the browser.

**[👉 直接使用 / Open the tool](https://kerr20801.github.io/PaperDesk/)**

---

## 為什麼做這個 / Why

線上 PDF 工具幾乎都要你**把檔案上傳到別人的伺服器**。但大家拿去處理的是什麼？合約、身分證、報稅資料、病歷、財務報表。

PaperDesk 全部在你的瀏覽器裡跑完，沒有後端、沒有上傳、沒有帳號、不用連網也能用。

Most online PDF tools require **uploading your file to someone else's server** — yet the documents people actually process are contracts, IDs, tax forms, medical records. PaperDesk does everything locally in your browser: no backend, no upload, no account, works offline.

---

## 功能 / Features

| 功能 | 說明 |
|---|---|
| **合併 / Merge** | 一次拖入多份 PDF，自動接成一份 |
| **分割 / Split** | 選取要的頁面，匯出成新檔 |
| **刪頁 / Delete** | 選取後一鍵移除 |
| **重排 / Reorder** | 直接拖曳縮圖調整順序 |
| **轉正 / Rotate** | 90° 為單位旋轉，疊加在頁面原有方向上（掃描件常見歪頁） |

所有操作都在同一個縮圖網格上完成 — 合併、分割、刪頁、重排本質上都是「重新安排頁面清單」，所以不需要在不同工具間切換。

Everything happens in one thumbnail grid — merging, splitting, deleting and reordering are all just "rearranging a list of pages", so there's no tool-switching.

---

## 使用方式 / How to use

1. 拖入一或多個 PDF（或點「＋ 加入 PDF」）
2. 點縮圖選取頁面，拖曳調整順序
3. 需要的話旋轉或刪除
4. 「匯出全部」或「匯出選取」下載

---

## 隱私 / Privacy

- 無後端、無資料庫、無追蹤、無帳號
- PDF 只存在瀏覽器記憶體中，關掉分頁就沒了
- 原始碼公開，可自行審查或下載離線使用

No backend, no database, no tracking, no account. PDFs live only in browser memory and are gone when you close the tab. Source is fully open — audit it or run it offline.

---

## 技術棧 / Tech stack

單一 HTML 檔案，零框架。

| 函式庫 | 用途 |
|---|---|
| [PDF.js](https://mozilla.github.io/pdf.js/) | 頁面縮圖渲染 |
| [pdf-lib](https://pdf-lib.js.org/) | 頁面複製、旋轉、輸出 |

### 架構筆記 / Design note

核心是一個**虛擬頁面清單**，而不是「一份被編輯的 PDF」：

```js
sources = { s1: bytes, s2: bytes }              // 載入過的原始檔
pages   = [ {uid, srcId:'s1', idx:0, rot:0},    // 目前文件的組成
            {uid, srcId:'s2', idx:3, rot:90} ]
```

匯出時照 `pages` 順序用 `copyPages()` 組一份新的。於是所有功能都收斂成陣列操作 — 合併是 push、分割是 filter、刪頁是移除、重排是換位置。每頁帶穩定 `uid`（不是頁碼），這樣之後加「在指定頁蓋簽名」之類的功能時，重排頁面不會讓標記跑到錯的頁上。

旋轉是**疊加**在頁面原有的 `/Rotate` 上，不是覆蓋 — 掃描件常常本身就帶 90°，覆蓋會把原本正常的頁弄歪。

---

## 開發路線圖 / Roadmap

| 狀態 | 功能 |
|---|---|
| ✅ | 多檔合併 / 分割匯出 / 刪頁 / 拖曳重排 / 旋轉 |
| ⬜ | 頁面範圍輸入（如 `1-3,7,9-12`） |
| ⬜ | 壓縮（重新編碼內嵌圖片） |
| ⬜ | 插入空白頁 |
| ⬜ | 浮水印 / 頁碼 |
| ⬜ | 打包成 Chrome 擴充功能（離線可用） |

簽名功能請見姊妹專案 [pdf-sign](https://github.com/kerr20801/pdf-sign)。

---

## License

MIT
