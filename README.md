# LocalConvert

純前端的檔案轉換網頁，部署在 GitHub Pages 即可使用。所有檔案都在使用者自己的裝置上處理，不會上傳到任何伺服器。

## 原理

整個網站只有一個 `index.html`，轉換工作依檔案類型交給不同的瀏覽器端工具：

| 類型 | 使用的技術 |
| --- | --- |
| 影片、音訊 | ffmpeg.wasm（單執行緒版，不需要特殊標頭，GitHub Pages 可直接執行） |
| 圖片 | 瀏覽器內建的 Canvas 解碼與編碼；較少見的格式交給 ffmpeg.wasm |
| PDF 讀取 | pdf.js，把每頁畫成圖片或擷取文字 |
| PDF 產生 | jsPDF；文字內容先畫成圖片頁再嵌入，因此支援中文但文字無法選取 |
| Word | mammoth.js，轉成文字或 HTML |
| 表格 | SheetJS，各種表格格式互轉 |
| Markdown | marked |
| 壓縮 | JSZip |

補充說明：

- 轉換引擎與各函式庫從 jsDelivr CDN 載入，PDF、Word、Excel 等元件在第一次用到時才下載。
- 檔案轉換時整份讀進記憶體，因此程式本身沒有大小限制，實際上限取決於裝置記憶體（手機通常低於電腦）。
- 轉換期間會使用 Wake Lock 保持螢幕不熄滅，並在離開頁面前跳出提醒，降低手機中途中斷的機率。
- 多個檔案依序處理，完成一個就釋放暫存，減少記憶體壓力。
- 提供 `manifest.json`，可在手機上「加入主畫面」當成 App 使用。

## 功能

- **影片**：mp4、webm、mkv、mov、avi、flv、wmv、mpg、ogv、m4v、gif
- **音訊**：mp3、wav、ogg、flac、m4a、aac、wma、aiff、ac3（也能從影片擷取音訊）
- **圖片**：png、jpg、webp、avif、bmp、tiff、tga、ico（可讀取 svg）
- **PDF**：圖片或文件轉 PDF；PDF 轉圖片（每頁一張，多頁自動打包 zip）；PDF 轉 txt
- **文件**：docx、md、html、txt 之間轉成 txt、html、doc、PDF
- **表格與資料**：xlsx、xls、ods、csv、tsv、json、xml、yaml 互轉
- **其他**：任何檔案轉成 zip 或 base64
- 支援多檔批次、拖曳上傳、深色模式與手機版面

## 部署

1. 將 `index.html`、`manifest.json` 與 `icons/` 資料夾放到 repo 根目錄。
2. 到 Settings → Pages，選擇要發佈的分支。
3. 開啟 Pages 網址即可使用。

目錄結構：

```
index.html
manifest.json
icons/
  icon-192.png
  icon-512.png
```
