# 興嘉國小網頁書籤管理 V2.1

可部署至 GitHub Pages 的前台，加上 Google Apps Script（GAS）與 Google 試算表資料庫的完整書籤管理系統。

## 已完成設定

| 項目 | 值 |
| --- | --- |
| Google 試算表 ID | `1ohUUpKQu06rj3Ocot58Noa9XLubZtbYbT2Q_PkqcNpI` |
| 工作表名稱 | `工作表1` |
| 前端 API URL | `https://script.google.com/macros/s/AKfycbzDWo9MRqc06jF-yZ9ouRkznMWs41DUMwNQfUgc-4DTJ2d7EftUYpMjEEdhdlCXWzP5/exec` |

## 部署步驟

1. 開啟 [Apps Script](https://script.google.com/)，建立專案，貼上 `Code.gs` 的完整內容。
2. 在專案設定的「指令碼屬性」新增 `ADMIN_KEY`，其值就是管理者登入密碼。**不可**把密碼寫入 `index.html`。
3. 部署為「網頁應用程式」：執行身分選擇自己，存取權限選擇所有人；首次部署須完成 Google 授權。
4. 本專案的 `index.html` 已填入指定的 `/exec` URL。如您重新部署並產生新網址，請只更新 `index.html` 中的 `API_URL`。
5. 將 `index.html` **及完整的 `assets` 資料夾**一起上傳到 GitHub 儲存庫根目錄，並在 GitHub Pages 設定從該分支根目錄發布。請保留資料夾名稱小寫的 `assets`；不可只上傳 `index.html`。校徽另有內嵌備援圖示，若圖片檔暫時無法讀取仍會顯示。

## 試算表結構

後端首次讀取時會建立或檢查 `工作表1` 的標題列：

`id | category | name | url | createdAt | updatedAt`

請不要調整標題列欄位順序。每筆資料的 `id` 為系統自動產生，前台顯示的「編號」依當前篩選與搜尋結果排序。

## API 合約與相容性檢查

| 操作 | 請求 | 回應 |
| --- | --- | --- |
| 讀取 | `GET ?action=list` | `{ ok, items: Bookmark[] }` |
| 驗證登入 | `POST { action:'verify', adminKey }` | `{ ok }` |
| 新增 | `POST { action:'create', adminKey, category, name, url }` | `{ ok, item }` |
| 編修 | `POST { action:'update', adminKey, id, category, name, url }` | `{ ok, item }` |
| 刪除 | `POST { action:'delete', adminKey, id }` | `{ ok, item }` |

`Bookmark` 欄位固定為 `id, category, name, url, createdAt, updatedAt`；前後端已依此同一契約製作。管理密碼僅會在管理者登入後隨單次管理請求傳至後端驗證，不會保存至瀏覽器或原始碼。

## 專案內容

```text
bookmark-manager-v2/
├── index.html
├── Code.gs
├── README.md
└── assets/
    ├── logo.png
    ├── school.jpg
    └── students.png
```
