# TextMark — 文字轉 Markdown 工具

專為 Windows 桌面與瀏覽器設計的雙欄文字轉 Markdown 工作台，支援匯入 `.pdf` 與 `.txt` 檔案、即時手動編修、一鍵預設／程式智慧排版轉換、即時預覽與匯出 `.md` 檔案，並可下載單一離線 HTML 執行檔。

## 主要功能

- **左右雙欄即時編輯**
  - **左側（原始文字區）**：可直接輸入、貼上或修改文字，亦支援透過上方「匯入」按鈕或拖曳檔案至編輯區讀取資料。
  - **右側（Markdown 結果區）**：轉換結果顯示於右側等寬字型編輯框，可隨時手動修改，並提供「編輯 / 預覽」切換與「還原」上一步內容功能。
- **PDF 與 TXT 本地端匯入**
  - **TXT 編碼自動偵測**：支援 UTF-8、UTF-8 BOM、UTF-16（LE/BE），並自動相容繁體中文 Windows 常見的 Big5（ANSI）與 GB18030 編碼，統一正規化換行符號。
  - **PDF 空間文字抽取**：使用 `pdfjs-dist` 於瀏覽器本地解析各頁文字座標，依行距與水平間距還原段落與多欄結構，支援繁體中文 CMap。
- **兩種轉換模式**
  - **預設**：維持與左側相同的斷行與原始排版結構。
  - **排版**：程式自動識別章節標題（如 `第一章`、`一、`、`【標題】`、`1.1`）、項目與編號清單、核取方塊（`[ ]` / `[x]`）、Tab／多空白／`|` 分隔表格、引用註記、鍵值資訊及合併 PDF 異常斷行。
- **Windows 友善匯出與離線單檔**
  - **匯出 `.md`**：自訂輸出檔名，並可於右下角切換 Windows `CRLF` 或 `LF` 換行格式直接下載。
  - **離線單檔下載**：點擊左上角 **TextMark** LOGO（附帶下載圖示）即可透過瀏覽器本地產生並下載 `TextMark.html`，下載後於 Windows 直接雙擊即可在 Edge / Chrome 離線運行（不需伺服器或額外安裝環境）。

## 操作說明

1. **輸入來源文字**：點擊左側上方 **「匯入」** 選擇 `.pdf`／`.txt`（或直接拖曳檔案進左側框），亦可直接輸入文字或點擊頂部 **「載入範例」** 測試。
2. **執行轉換**：點擊兩欄中央的 **「轉換」** 按鈕（或按 `Ctrl + Enter`），將左側文字轉換至右側框。
3. **切換排版或編修**：
   - 點擊右側上方 **「預設」** 可維持與左側相同的排版。
   - 點擊右側上方 **「排版」** 可套用程式智慧排版。
   - 可直接在右側編輯框修改內容，或切換至 **「預覽」** 檢視渲染效果。
4. **匯出成品**：確認檔名與 `CRLF`／`LF` 格式後，點擊 **「匯出」** 下載 `.md` 檔案，或點擊右上角 **「複製結果」** 複製到剪貼簿。

## 技術架構

- **前端框架**：React 19、TypeScript、Vite 8、Tailwind CSS v4
- **圖示與字型**：`lucide-react`、`Plus Jakarta Sans`、`Noto Sans TC`、`JetBrains Mono`
- **PDF 解析**：`pdfjs-dist`（網頁版支援本地 CMap 與標準字型資源；離線單檔版自動切換為內嵌主執行緒 Worker）

## 本地開發與建置指令

```bash
# 安裝相依套件
npm install

# 啟動開發伺服器（預設 Port 3000）
npm run dev

# 僅建置可離線執行的 Windows 單檔（輸出至 src/generated/TextMark.html）
npm run build:singlefile

# 完整建置（依序產出離線單檔與 dist/ 網頁正式版）
npm run build

# 執行 TypeScript 型別檢查
npm run lint
```

## 專案目錄結構

```text
├── scripts/
│   └── build-singlefile.mjs          # 單檔離線版 TextMark.html 建置腳本
├── src/
│   ├── components/
│   │   └── MarkdownPreview.tsx       # 純 React 安全 Markdown 即時預覽元件
│   ├── generated/
│   │   └── TextMark.html             # 建置產出的獨立單檔執行頁
│   ├── utils/
│   │   ├── fileParsers.ts            # TXT 多編碼偵測、PDF 解析與 .md 匯出工具
│   │   ├── markdownConverter.ts      # 預設與程式智慧排版轉換核心
│   │   ├── pdfWorkerSetup.ts         # 一般網頁版 PDF.js Worker 設定
│   │   ├── pdfWorkerSinglefileSetup.ts # 離線單檔版內嵌 PDF.js Worker 設定
│   │   ├── standaloneDownload.ts     # 本地 Blob 下載離線單檔工具
│   │   └── standaloneDownloadEmpty.ts # 離線單檔自體建置用空替身模組
│   ├── App.tsx                       # 主雙欄工作台介面
│   ├── index.css                     # 全域樣式與編輯區捲軸設定
│   ├── main.tsx                      # 應用程式進入點
│   └── vite-env.d.ts                 # Vite 與 .mjs 模組型別宣告
├── index.html                        # 網頁進入點與字型設定
├── package.json                      # 相依套件與 npm 指令
└── vite.config.ts                    # Vite 設定與 PDF.js CMap 資源服務外掛
```
