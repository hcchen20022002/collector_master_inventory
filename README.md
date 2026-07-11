# Collector Master Inventory

用 AI 管理收藏品（運動卡、Pokémon、MTG、Marvel 等）投資組合的提示詞（Prompt）。

將個人收藏清單（Master Inventory v2.2）交給具備網路搜尋能力的 AI（如 ChatGPT、Claude），由 AI 查詢即時市場行情、追蹤價格變化，並產出一份中文的「收藏投資組合日報」。

## 免責聲明 / Disclaimer

本專案為免費開源軟體，僅供資訊與教育用途，**並非**財務或投資建議，亦不構成任何顧問或受託關係。所有估值與建議皆為盡力而為的估計，可能有誤；收藏品屬高波動、低流動性之另類資產。投資決策與其風險概由使用者自行承擔。本軟體依「現狀（AS IS）」提供，不附任何明示或默示擔保，詳見 [LICENSE](./LICENSE)。

This project is free and open-source software provided for informational and educational purposes only. It is **not** financial or investment advice and does not create any advisory or fiduciary relationship. All valuations and recommendations are best-effort estimates and may be inaccurate. You are solely responsible for your own investment decisions. The software is provided "AS IS", without warranty of any kind; see [LICENSE](./LICENSE).

## Repo 結構

```
prompt/
└── master_inventory.md   # 主提示詞（Master Inventory v2.2）
```

## 使用方式

1. 開啟 `prompt/master_inventory.md`。
2. 若已有整理好的清單，貼到 `(Paste your inventory here.)` 的位置；若還沒有，可留空，由 AI 引導完成初次建檔（見下方「初次建檔流程」）。
3. 將整份內容貼給支援即時搜尋的 AI，AI 會依規則查價並產出日報。

## 提示詞設計重點

### 初次建檔流程（First-Time Use）

若尚未建立 Master Inventory v2.2，AI 會引導完成建檔：

1. 請使用者提供收藏清單——支援純文字、試算表、照片、截圖、平台匯出檔或混合格式。
2. 將提供的內容轉換為 Master Inventory v2.2，盡可能擷取每個品項的：名稱、球員/角色、年份、系列、卡號、平行/特卡、限量編號、簽名/實物卡、鑑定公司、評級、類別、數量。
3. 資訊不確定時明確標註「Needs Confirmation」，不可默默猜測。
4. 先輸出「Master Inventory v2.2 Draft」結構化表格，經使用者確認後才鎖定為正式 SSOT。
5. 鎖定後即不得重新命名、刪除、合併或替換任何品項；圖片中的舊平台價格只能當作歷史脈絡，不可作為現值。
6. 確認完成後，才產出第一份收藏投資組合日報。

### 清單即 SSOT（Single Source of Truth）

Master Inventory v2.2 是唯一權威基準：除非使用者明確指示，AI 不得重新命名、刪除、替換或修改清單中的任何項目。

### 市場估值規則

估值來源依優先順序排列：

1. 近期成交紀錄（Recent Sold Listings）
2. 即時 Fantastic Collection 市場價格（僅可作為參考之一，不可作為唯一來源）
3. 目前在售價格（Active Listings）
4. 可比銷售（Comparable Sales）
5. PSA / BGS / CGC 鑑定量（Population Reports）
6. 球員近期表現
7. 整體市場趨勢

同時明確要求忽略：截圖中過時的歷史價格、不合理的開價、明顯的市場操縱。

### 每張卡片維護的欄位

- **基本資料**：品項、類別、鑑定公司、評級
- **市場資料**：來源、最近成交價、公允市值（FMV）、建議上架價
- **信心度**：High / Medium / Low，並附理由（如低鑑定量、成交筆數少、買賣價差大）
- **趨勢**：7 日 / 30 日 / 90 日
- **鑑定量**：PSA / BGS / CGC
- **狀態**：新成交警示、操作建議（Hold / Buy / Sell / Watch）

### 評分與分級

- **Opportunity Score（1–100）**：綜合 FMV、成交動能、流動性、稀缺度、長期潛力等因素。90+ 強力買進、80–89 買進、65–79 持有、50–64 觀察、50 以下考慮出售。
- **卡片分級**：S（核心持有）、A（高成長）、B（持有觀察）、C（建議出售）。S 與 A 級卡片需維護投資論點（Investment Thesis）。
- **Portfolio Risk Score（1–10）**：評估流動性、新秀集中度、老卡曝險、分散度、市場波動、下檔保護。

### 日報輸出格式

日報（`# 收藏投資組合日報`）包含：

- 今日整體估值（流動價值 / 公允市值 / 建議總上架價）
- 資產配置（依類別：籃球、棒球、足球、Pokémon、MTG、Marvel、娛樂、其他）
- Portfolio Risk Score
- S / A / B / C 各級持倉明細表
- 今日 Top 10 持倉（依 FMV）與 Top 10 Opportunity Score
- 今日最值得加碼、今日最可能被低估
- Watch List（接近買入區 / 接近賣出區 / 最大漲跌幅 / 新市場機會）
- 與前次報告差異（只列出有意義的變化，不重複未變動的卡片）

### 更新原則

只在發生有意義的變化時更新卡片（新成交、新高/新低、FMV 變動、信心度變動、重大球員或市場消息），避免每天重新估算全部卡片；準確性優先於速度。
