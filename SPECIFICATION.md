# CPOS Specification

**Collectibles Portfolio Operating System**

Version 3.0

This document is the complete implementation standard for CPOS.

Any implementation — prompt, database, API, MCP server, or web UI — must conform to this specification.

The design documents in [design/](design/) are the authoritative source of truth. This specification implements them and must never contradict them.

---

## 1. Scope

CPOS manages collectible investment portfolios (trading cards, TCG, and comparable graded collectibles).

CPOS behaves as a professional Portfolio Manager, not a pricing engine.

CPOS provides analysis, recommendations, and market intelligence. Final investment decisions always belong to the user.

---

## 2. Invariants

Every conforming implementation MUST enforce these invariants at all times:

1. The Master Inventory is the Single Source of Truth (SSOT).
2. Inventory integrity is always more important than valuation accuracy.
3. The inventory never changes automatically. Only the user may add, remove, rename, merge, replace, or change the quantity of an item.
4. The AI may only update market-related information.
5. Reports never modify the inventory.
6. Historical market information never overwrites inventory records.
7. Recommendations must include reasoning.
8. Uncertainty must be disclosed, never hidden.

---

## 3. Architecture

CPOS consists of six logical modules, executed in order:

| # | Module | Responsibility |
|---|---|---|
| 1 | Initialization | Convert a raw collection into a confirmed Master Inventory |
| 2 | Master Inventory | Persist and protect the SSOT |
| 3 | Market Intelligence | Evidence-based valuation of every item |
| 4 | Portfolio Management | Scoring, classification, allocation, and risk |
| 5 | Daily Report | Produce the 收藏投資組合日報 |
| 6 | Continuous Improvement | Improve methods without touching the inventory |

Each module must be independently maintainable.

---

## 4. Data Model

### 4.1 Item Record

Every inventory item carries the following fields.

**Owner** defines who is allowed to change the field: `Human` (user only) or `AI` (market analysis).

| Field | Owner | Description |
|---|---|---|
| Item | Human | Full item name |
| Player / Character / Subject | Human | Subject of the item |
| Year | Human | Release year |
| Product / Set | Human | Product line or set |
| Card Number | Human | Card number within the set |
| Parallel / Insert | Human | Parallel or insert designation |
| Serial Number | Human | Serial numbering (e.g., /99) |
| Autograph / Memorabilia | Human | Auto or memorabilia attributes |
| Grading Company | Human | PSA / BGS / CGC / Raw |
| Grade | Human | Numeric grade |
| Category | Human | See §4.2 |
| Quantity | Human | Number of copies owned |
| Source | AI | Source of the valuation evidence |
| Last Sold | AI | Most recent relevant sale (price and date) |
| FMV | AI | Fair Market Value |
| Suggested List Price | AI | Recommended listing price |
| Confidence | AI | High / Medium / Low |
| Confidence Reason | AI | e.g., Low Population, Only 2 Recent Sales, Large Bid / Ask Spread |
| 7 Day Trend | AI | Short-term trend |
| 30 Day Trend | AI | Medium-term trend |
| 90 Day Trend | AI | Long-term trend |
| Population (PSA / BGS / CGC) | AI | Graded population, when available |
| New Sales Alert | AI | Flag for new sales since last report |
| Opportunity Score | AI | 1–100, see §6.1 |
| Tier | AI | S / A / B / C, see §6.2 |
| Action | AI | Hold / Buy / Sell / Watch |
| Investment Thesis | AI | Required for S and A tiers, see §6.3 |

### 4.2 Categories

Basketball, Baseball, Soccer, Pokémon, MTG, Marvel, Entertainment, Other.

---

## 5. Module Specifications

### 5.1 Initialization

Trigger: no confirmed Master Inventory exists.

Workflow:

1. Request the collection list. Accepted input formats: plain text, spreadsheet, photos, screenshots, platform inventory export, mixed format.
2. Convert the input into a structured inventory, extracting or inferring the Human-owned fields of §4.1.
3. Mark any uncertain value as **Needs Confirmation**. Silent guessing is prohibited.
4. Output a `## Master Inventory Draft` table.
5. Request explicit user confirmation. The draft is NOT the SSOT.
6. On confirmation, lock the list as the **Master Inventory (SSOT)**.
7. Values embedded in uploaded images or screenshots (including historical Fantastic Collection values) are historical context only and must never be used as current market value.
8. After confirmation, produce the first Daily Report.

### 5.2 Master Inventory

The SSOT rules of §2 apply.

Prohibited without explicit user instruction: rename, remove, replace, merge, rebuild, or otherwise modify inventory items.

The inventory is persistent. Market data changes; inventory does not.

### 5.3 Market Intelligence

Valuation priority (highest first):

1. Recent Sold Listings
2. Current Live Fantastic Collection Market Values
3. Active Listings
4. Comparable Sales
5. PSA / BGS / CGC Population Reports
6. Current Player Performance
7. Overall Market Trend

Rules:

- Live Fantastic Collection values MAY be one reference, never the sole source.
- Ignore: historical platform values embedded in images, unrealistic asking prices, obvious market manipulation.
- If no reliable sales exist: estimate FMV from comparable sales and reduce Confidence.
- Every valuation must cite its evidence (Source, Last Sold).

Confidence levels:

| Level | Condition (typical) |
|---|---|
| High | Multiple recent sales, tight bid/ask spread |
| Medium | Few sales, or moderate spread |
| Low | No recent sales, comparable-based estimate, thin market |

A Confidence Reason is mandatory whenever Confidence is not High.

### 5.4 Portfolio Management

See §6 for scoring and classification algorithms.

Portfolio-level outputs:

- **Portfolio Summary**: Liquid Value, Fair Market Value, Suggested Total List Value.
- **Portfolio Allocation**: FMV, Percentage of Portfolio, and Trend per category (§4.2).
- **Portfolio Risk Score**: 1–10, evaluating Liquidity, Prospect Concentration, Vintage Exposure, Diversification, Market Volatility, Downside Protection. The score must be explained.

Change detection — an item is updated only on meaningful change:

- New Sale, New High, New Low, FMV Change, Confidence Change, Significant Player News, Significant Market News.

Full daily re-estimation of every item is prohibited.

### 5.5 Daily Report

The report format is specified in §7 and templated in [templates/DAILY-REPORT-TEMPLATE.md](templates/DAILY-REPORT-TEMPLATE.md).

### 5.6 Continuous Improvement

Valuation methods, report quality, market intelligence, and the investment framework may improve over time — without changing the inventory and remaining backward compatible whenever possible.

Every important recommendation must answer: Why? What changed? What is the expected outcome? What are the risks?

---

## 6. Algorithms

### 6.1 Opportunity Score

Range: 1–100.

Inputs: Current FMV, Recent Sales Momentum, Liquidity, Scarcity, Population, Long-term Upside, Player Trajectory, Overall Market Sentiment, Downside Risk.

Interpretation:

| Score | Meaning |
|---|---|
| 90–100 | Strong Buy |
| 80–89 | Buy |
| 65–79 | Hold |
| 50–64 | Watch |
| < 50 | Consider Selling |

### 6.2 Portfolio Classification

Inputs: FMV, Liquidity, Growth Potential, Downside Risk, Overall Asset Quality.

| Tier | Meaning |
|---|---|
| S | Core Holdings |
| A | High Growth |
| B | Hold / Watch |
| C | Sell / Rotate |

### 6.3 Investment Thesis

Required for every S and A tier item. Optional for B and C.

A thesis is a short list of long-term reasons for holding the asset (e.g., Franchise upside, SSP, Rookie Year, Strong Liquidity, Low Population).

The thesis must remain stable unless the investment case materially changes.

---

## 7. Daily Report Specification

Title: `# 收藏投資組合日報`

Sections, in order:

| # | Section | Content |
|---|---|---|
| 1 | 今日整體估值 | Liquid Value, Fair Market Value, Suggested List Value |
| 2 | Portfolio Allocation | Per §5.4 |
| 3 | Portfolio Risk Score | Score 1–10 with explanation |
| 4 | 核心持有（S級） | Tier table (§7.1) |
| 5 | 高成長主力（A級） | Tier table (§7.1) |
| 6 | 持有觀察（B級） | Tier table, Investment Thesis optional |
| 7 | 建議出售區（C級） | Tier table, Investment Thesis optional |
| 8 | 今日 Top10 持倉 | Ranked directly by FMV |
| 9 | Top10 Opportunity Score | Ranked directly by Opportunity Score |
| 10 | 今日最值得加碼 | Why, Target Buy Range, Investment Thesis, Risk |
| 11 | 今日最可能被低估 | Why, Comparable Sales, Long-term Potential |
| 12 | Watch List | Approaching Buy Zone / Approaching Sell Zone / Biggest Gainers / Biggest Losers / New Market Opportunities |
| 13 | 與前次報告差異 | Meaningful changes only (§7.2) |

### 7.1 Tier Table Columns

Item, Source, Last Sold, FMV, Suggested List Price, Confidence, Confidence Reason, 7D, 30D, 90D, Population, New Sales Alert, Opportunity Score, Action, Investment Thesis.

### 7.2 Change Markers

▲ New Sale, ▲ FMV Up, ▼ FMV Down, ▲ Confidence Up, ▼ Confidence Down, ▲ New High, ▼ New Low.

Unchanged items must never be repeated.

---

## 8. Versioning

CPOS follows the roadmap in [design/ROADMAP.md](design/ROADMAP.md).

This specification describes v3.0.

Future versions (Cost Basis, Portfolio Memory, Performance Analysis, MCP Server, etc.) must extend this specification without breaking the invariants of §2.
