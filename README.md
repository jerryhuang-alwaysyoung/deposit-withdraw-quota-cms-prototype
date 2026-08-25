# Deposit Withdraw Quota Management — CMS Prototype

XREX Exchange 後台 `[Admin] Deposit Withdraw Quota Management` 的靜態原型，供 PM / 工程 / 風控 review 用。

**本期範圍：KGI only。** 遠銀（FEIB）通路與其大型企業額度已 put on hold，規則保留於 PRD，未刪除。

## Pages

| 入口 | 說明 |
| --- | --- |
| `site/index.html` | 主原型（單一檔案，可直接以瀏覽器開啟，或透過 GitHub Pages 瀏覽） |

## 原型可操作項目

- **Add whitelist** — 完整建立流程，含必填檢核、依幣別控管的額度上限檢核、重複設定檢核（10116）。按下 Create 會立即呼叫 KGI API：成功則資料置頂寫入列表且狀態為 `ACTIVATE`；失敗則不建立資料、右上角跳出錯誤訊息（可用 topbar 的「模擬 KGI API 失敗」開關切換）
- **Activity Log** — 標題右側 icon 開啟右側 drawer，含 `Field / From / To` 差異表
- **Status 篩選** — `ACTIVATE` / `DELETED`
- **權限切換** — 右上角可切換 `view` / `edit`，觀察異動按鈕的顯示差異
- **模擬 KGI API 失敗** — 右上角開關，用於展示同步呼叫失敗時的行為（原型專用，非產品功能）

## Deployment

`main` 分支有異動時，GitHub Actions 會自動將 `site/` 部署至 GitHub Pages。

## Notes

示範資料為靜態 demo data，不代表 production 真實資料。狀態機、權限與稽核行為尚待風控、合規、工程確認。

本 repo 僅收錄可分享的靜態原型。PRD、額度核定依據與通知信草稿等內部文件不隨此 repo 發布，請洽 PM 取得。
