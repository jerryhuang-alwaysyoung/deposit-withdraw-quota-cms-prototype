# Deposit Withdraw Quota Management — CMS Prototype

XREX Exchange 後台 `[Admin] Deposit Withdraw Quota Management` 的靜態原型，供 PM / 工程 / 風控 review 用。

對應 PRD：`deposit-withdraw-quota-PRD.md` v3.9。

## 兩種模式

原型右上角可切換，兩者資料集獨立，切換會重置示範資料。

| | 新版（僅尊榮）— 預設 | 原版（含大型企業） |
| --- | --- | --- |
| KYC 等級 | Basic / Premium | Basic / Premium / Enterprise |
| 額度欄位 | 唯讀，確認後送出 | 可編輯 |
| 數值檢核 | 無 | 上限／下界／格式共四則訊息 |

大型企業額度已於 2026-10-01 自本版移除（需額外開發資源建置該 KYC level）。原版模式保留供對照。

## Pages

| 入口 | 說明 |
| --- | --- |
| `site/index.html` | 主原型（單一檔案，可直接以瀏覽器開啟或透過 GitHub Pages 瀏覽） |

## 原型可操作項目

- **Add whitelist（引導式）** — 輸入 `UID` 帶出 `Account Type`、`KYC Level` 與已開啟的 custodian／幣別；選定 custodian + currency 後依矩陣帶出 6 個額度值
- **上限矩陣** — 依 Account Type × KYC level × Custodian × Currency × 交易方向取值；個人戶與企業戶為兩套數值
- **五態 Status** — `ACTIVATE` / `PENDING ACTIVATION` / `PENDING DELETION` / `UPDATE FAILED` / `DELETED`，後三者僅 FEIB
- **FEIB 週五批次** — 新增與刪除皆為非同步；失敗即轉 `UPDATE FAILED`（待重送，非終止），重送成功依異動方向轉 `ACTIVATE` / `DELETED`
- **Slack 告警** — 批次失敗時產生 `alert-fiat-feib-prod` 訊息（含 `failCount` 與固定 `retryPolicy`），存於 `window.slackLog`
- **Delete** — 僅在該用戶 KYC level 已降為 `Basic` 時可用；依通路分流，並顯示等級變更確認文案
- **篩選** — ID 搜尋（類別 + 關鍵字）、Account Type、KYC Level、Currency、Custodian、Status、時間欄位 + 區間、Reset
- **Activity Log** — 標題右側 icon 開啟右側 drawer，含 `Field / From / To` 差異表
- **權限切換** — `view` / `edit`

## 原型專用控制項（非產品功能）

topbar 右側區塊在正式版整區移除：

| 控制項 | 用途 |
| --- | --- |
| 模式 | 新版（僅尊榮）／原版（含大型企業） |
| 模擬結果 | 正常／KGI API 失敗／FEIB 批次失敗 |
| 模擬週五批次 | 推進 FEIB 的待處理與待重送資料 |
| Demo UIDs | 示範帳號名冊，含「尚可新增」欄位；點任一列直接開啟 Add whitelist |

## 已知的刻意設定

- **Crypto/Bitcheck tab 完全不動**，僅靜態呈現現況
- `88001 / DNGR8801` 為**刻意的不一致**：KYC 已降為 `Basic` 但額度仍為高等級，用於示範 Delete 解鎖與 PRD 記載的殘留風險
- 額度上限採**官方額度總表**數值；系統現行 `custodian period_limits` 低於此，DB 值隨本版一併更新（PRD `FR-OPS01-12`）
- CMS 不提供 retry／`Resend`；OP 的重送介入點在 Airflow

## 免責

本原型為產品討論用的靜態頁面，資料皆為示範資料，不代表任何已實作的控制或已核定的額度。
