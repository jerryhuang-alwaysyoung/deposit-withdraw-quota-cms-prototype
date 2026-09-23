# Deposit Withdraw Quota Management — CMS Prototype

XREX Exchange 後台 `[Admin] Deposit Withdraw Quota Management` 的靜態原型，供 PM / 工程 / 風控 review 用。

**本期範圍：KGI + FEIB 兩條通路。** 遠銀（FEIB）已於 2026-09-21 解除 hold 並納入本版。

對應 PRD：`deposit-withdraw-quota-PRD.md` v2.8。

## Pages

| 入口 | 說明 |
| --- | --- |
| `site/index.html` | 主原型（單一檔案，可直接以瀏覽器開啟，或透過 GitHub Pages 瀏覽） |

## 原型可操作項目

- **Add whitelist（引導式流程）** — 輸入 `UID` 後系統帶出該用戶的 `Account Type`、`KYC Level` 與已開啟的 custodian／幣別；選定 custodian + currency 後**依上限矩陣自動預填** 6 個額度欄位，OP 可再調整
- **上限卡控** — 依 **Account Type × KYC level × Custodian × Currency × 交易方向** 取值。個人戶與企業戶為兩套數值（尊榮的 KGI×TWD 入金單月：個人戶 10000000、企業戶 300000000）
- **輸入檢核** — `Whole numbers only.`（小數點）／`Numbers only.`（文字符號）／`Below basic limit.`（低於基本額度）／`Exceeds maximum limit.`（超過上限）
- **四態 Status** — `ACTIVATE` / `PENDING ACTIVATION` / `ACTIVATION FAILED` / `DELETED`。後兩者僅 FEIB 通路會產生
- **FEIB 非同步流程** — 建立後為 `PENDING ACTIVATION`，以 topbar 的「模擬 18:00 批次」推進至生效或失敗；失敗且達自動補送上限者，Action 欄出現 `Resend`
- **Delete 閘控** — 僅在該用戶 KYC level 已降為 `Basic` 時可用，否則為 disabled 並顯示停用原因
- **篩選** — ID 搜尋（類別 + 關鍵字）、Account Type、KYC Level、Currency、Custodian、Status，以及「時間欄位 + 區間」
- **Activity Log** — 標題右側 icon 開啟右側 drawer，含 `Field / From / To` 差異表
- **權限切換** — 右上角可切換 `view` / `edit`

## 原型專用控制項（非產品功能）

topbar 右側區塊在正式版整區移除：

| 控制項 | 用途 |
| --- | --- |
| 模擬結果 | 正常 / KGI API 失敗 / FEIB 批次失敗 |
| 模擬 18:00 批次 | 推進 FEIB 的 `PENDING ACTIVATION` 資料 |
| Demo UIDs | 示範帳號名冊，含「尚可新增」欄位；點任一列直接開啟 Add whitelist 並帶入該 UID |

## 已知的刻意設定

- **Crypto/Bitcheck quota whitelist tab 完全不動**，僅靜態呈現現況（無 UID 欄、無 Activated Time、無上限檢核）
- 示範資料 `88001 / DNGR8801` 為**刻意的不一致**：KYC 已降為 `Basic` 但額度仍是大型企業級距，用於示範 Delete 解鎖與 PRD 中記載的殘留風險
- 額度上限矩陣採**官方額度總表**數值；系統現行 `custodian period_limits` 低於此，DB 值隨本版一併更新（PRD `FR-OPS01-11`）

## 免責

本原型為產品討論用的靜態頁面，資料皆為示範資料，不代表任何已實作的控制或已核定的額度。
