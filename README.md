# CPOS

**Collectibles Portfolio Operating System**

Version 3.0

CPOS is an AI-powered operating system for managing collectible investment portfolios.

The goal is not simply to estimate prices.

The goal is to help collectors and investors maximize long-term portfolio value through:

- Market Intelligence
- Portfolio Management
- Risk Analysis
- Capital Allocation
- Daily Monitoring

CPOS treats collectibles as investment assets instead of inventory records.

CPOS behaves as a professional Portfolio Manager rather than a pricing engine.

---

## Disclaimer

This project is free and open-source software provided for informational and educational purposes only. It is **not** financial or investment advice and does not create any advisory or fiduciary relationship. Collectibles are volatile, illiquid, and unregulated assets. All valuations and recommendations are best-effort estimates and may be inaccurate. You are solely responsible for your own investment decisions. The software is provided "AS IS", without warranty of any kind; see [LICENSE](LICENSE).

---

## Architecture

CPOS consists of six logical modules:

```
Initialization
      ↓
Master Inventory
      ↓
Market Intelligence
      ↓
Portfolio Management
      ↓
Daily Report
      ↓
Continuous Improvement
```

The Master Inventory is the Single Source of Truth (SSOT).

- Portfolio reports never modify the inventory.
- Inventory changes must be explicitly requested by the user.
- The human owns the inventory. The AI owns the analysis.

See [design/ARCHITECTURE.md](design/ARCHITECTURE.md) and [design/00-PRINCIPLES.md](design/00-PRINCIPLES.md) for details.

---

## Quick Start

1. Open [TASK-EDITION.md](TASK-EDITION.md).
2. Copy the operational prompt into a ChatGPT Task (or any AI assistant with live web search).
3. Provide your collection in any format — text, spreadsheet, photos, screenshots, or platform exports.
4. Confirm the generated Master Inventory draft.
5. Receive your daily 收藏投資組合日報 (Portfolio Daily Report).

---

## Repository Layout

| Path | Description |
|---|---|
| [TASK-EDITION.md](TASK-EDITION.md) | The operational ChatGPT prompt |
| [SPECIFICATION.md](SPECIFICATION.md) | The complete implementation standard |
| [CHANGELOG.md](CHANGELOG.md) | Version history |
| [templates/](templates/) | Master Inventory and Daily Report templates |
| [examples/](examples/) | Example inventory and example report |
| [design/](design/) | Authoritative design documents |

The design documents are the authoritative source of truth for implementation.

Implementation must follow these documents instead of redesigning the architecture.

---

## Core Principles

CPOS follows 16 principles defined in [design/00-PRINCIPLES.md](design/00-PRINCIPLES.md). The most important:

- **Human Judgment Prevails** — CPOS assists human judgment. It never replaces it.
- **Inventory First** — Inventory integrity is always more important than valuation accuracy. The inventory never changes automatically.
- **Portfolio, Not Pricing** — Investment quality is more important than today's price.
- **Evidence over Speculation** — All valuation must be supported by evidence.
- **Only Meaningful Changes Matter** — Reports never repeat unchanged information.
- **Transparency over Precision** — Never imply certainty where none exists.

---

## Terminology

| Term | Meaning |
|---|---|
| CPOS | Collectibles Portfolio Operating System |
| Master Inventory | Single Source of Truth (SSOT) |
| FMV | Fair Market Value |
| Liquid Value | Estimated immediate liquidation value |
| Suggested List Price | Recommended listing price |
| Opportunity Score | Overall investment attractiveness (1–100) |
| Investment Thesis | Long-term reason for holding the asset |
| Portfolio Manager | The AI responsible for investment analysis |
| Task Edition | The operational ChatGPT prompt |
| Specification | The complete implementation standard |

Full glossary: [design/TERMINOLOGY.md](design/TERMINOLOGY.md)

---

## Roadmap

| Version | Scope |
|---|---|
| v3.0 | Portfolio Manager, Master Inventory, Opportunity Score, Daily Report, Initialization, Inventory Maintenance |
| v3.1 | Cost Basis, Purchase Date, Portfolio Memory, Report Metadata |
| v4.0 | Performance Analysis, Expected Return, Exit Strategy, Capital Allocation Optimizer |
| v5.0 | MCP Server, Database, API, Web UI |

Details: [design/ROADMAP.md](design/ROADMAP.md)

---

## License

See [LICENSE](LICENSE).
