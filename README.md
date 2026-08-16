# Demo Dataset Creator

> A skill for Claude that generates realistic, demo-ready datasets for any sub-industry,
> tailored to a specific end-user persona and presentation purpose.

[![License: Proprietary](https://img.shields.io/badge/License-Proprietary-red.svg)](LICENSE)
[![Built for Claude](https://img.shields.io/badge/Built%20for-Claude%20AI-orange.svg)](https://claude.ai)
[![Maintainer](https://img.shields.io/badge/Maintainer-Logeshkumar%20Sivakumar-green.svg)](https://www.linkedin.com/in/logeshkumar-sivakumar/)

**Copyright (c) 2026 Logeshkumar Sivakumar (elogu2001@outlook.com). All rights reserved.**

---

## What This Skill Does

Demo Dataset Creator instructs Claude to produce a single, self-contained Python script
that generates a full set of CSV files and documentation when run. No data is generated
inside the chat. The script is the only deliverable shown in conversation.

The datasets produced are built for product demos, proof-of-concept evaluations, client
workshops, tool showcases, and hackathons. They are deliberately realistic, not toy data.

### Two Operating Modes

**Standard Mode** is triggered by general dataset requests. Produces realistic transactional
datasets with macro events, seasonal patterns, geographic specificity, and zero
reconciliation on connected value pairs. Asks four intake questions before generating.

**Integrated Planning Mode** is triggered by keywords such as integrated planning,
planning dataset, write-back, or scenario planning. Enforces 15
additional rules including star schema format, integrated ledger derivation, employee-grain
headcount, BIM sourceColumn validation, and 10 mandatory validation checks at max diff
$0.00. Asks five clarification questions before generating anything.

### What Makes This Different From Generic Sample Data

| Capability | Description |
|---|---|
| Company research | If you name a real company, Claude researches its business model and geography, then models the dataset after it without using the company name anywhere |
| Dynamic tables | No fixed table set is always generated. Every table is justified by the use case and end-user persona |
| Global geography specificity | No broad regional label (APAC, Europe, MENA, LATAM) is ever a leaf node. Every region is decomposed into sub-regions, countries, and cities with authentic local names |
| Country-level name pools | Customer and employee names are drawn from country-specific pools matching local ethnicity and naming conventions across 30+ countries |
| Product size weightage | Demand distributions differ by product category and region. A medium T-shirt outsells an XXL in the US; XS outsells L in East Asia |
| Regional product affinity | Products are weighted by regional preference. Junk food sells more in North America; instant noodles sell more in SE Asia; alcohol is zero in Middle East |
| Zero reconciliation | Every connected value pair (order header vs lines, inventory in vs out, gross vs net pay, returns vs sales) reconciles to zero. In Integrated Planning mode, reconciliation is verified to exact $0.00 by automated checks |
| Mixed budget/actual variance | Realistic mix of favourable and adverse variance across income and expense accounts. At least 20% of all rows show adverse variance. Variance bands widen during macro shock periods |
| Correct variance direction (V3) | Budget derived FROM Actual: `Budget = Actual / (1 + bias)`. Income accounts use positive bias (favourable = Actual beats Budget), cost accounts use negative bias (favourable = Actual underspends Budget) |
| DimAccount sort keys | Five sort key columns on every DimAccount (Level1SortKey, Level2SortKey, Level3SortKey, AccountSortKey, HierarchySortKey) for correct P&L display order in Power BI |
| Integrated ledger | When DimAccount is present, FactFinancial derives from upstream facts. Revenue from FactARR, NRR from FactSalesActivity, payroll and COGS from FactHeadcount via DimDeptAccountBridge. Independent generation is last resort only |
| Employee-grain headcount | FactHeadcount is at employee grain (one row per active employee per month per scenario) when DimEmployee is present. Budget and Forecast use probabilistic inclusion via OVERHIRE_MULTIPLIER |
| Star schema enforcement | Long-format facts (one measure per row with ScenarioKey), boolean classifiers become dimension FKs, bridge tables carry only keys and split ratios, no unexpected object dtype columns |
| Planning scenarios | Every integrated planning dataset includes exactly three planning scenarios, each traceable step by step from a single changed input through the derivation chain to an EBITDA delta |
| BIM sourceColumn validation | Relationship columns in BIM files use sourceColumn values (camelCase), not display names. A post-build check catches silent relationship failures |
| 10 mandatory validation checks | Integrated planning datasets run all 10 checks: star schema (x3), variance mix, headcount reconciliation, ARR waterfall, payroll reconciliation, COGS reconciliation, revenue reconciliation, NRR reconciliation. Any warning stops delivery |
| Macro event engine | Real-world disruptions (supply chain crisis, inflation surge, COVID impact) are applied as time-bounded multipliers across geographies and categories |
| Seasonal accuracy | Seasonality is hemisphere-aware. Australian summer is December, not June. Diwali affects South Asia and its diaspora. Ramadan follows the lunar calendar |
| Discount rotation | Max 15-25% of SKUs are discounted in any week. Discounts rotate by category and align with seasonal events |
| CSV output rules | First row is always the column header. No comment rows, copyright rows, or blank rows prepended. CSVs use UTF-8-sig encoding for Excel and Power BI compatibility on Windows |
| Dataset size tiers | Small (20K-100K rows), Medium (100K-500K), Large (500K-2M), selected at intake |

---

## Installation

This skill uses the Claude Skills system. To install it:

1. Copy `SKILL.md` to your Claude skills directory:
```
/mnt/skills/user/demo-dataset-creator/SKILL.md
```

2. The skill is auto-detected when Claude starts. No restart required.
3. Trigger it in any conversation with one of the phrases below.

---

## How to Use

### Trigger Phrases

Claude will activate this skill when it detects any of the following:

- `"create a demo dataset for [industry]"`
- `"generate sample data for a [use case] demo"`
- `"build a POC dataset for [company or scenario]"`
- `"I need test data for a [tool] demo"`
- `"create a dataset like [real company name]"`
- `"data for a presentation to [job role]"`
- `"integrated planning dataset"`
- `"scenario planning dataset"`
- `"scenario planning dataset"`
- `"EBITDA planning"`
- `/demo-dataset-creator` (direct invocation)

### Intake Questions

**Standard Mode** asks four questions in a single grouped message before generating anything:

1. **Use case** - What story does the data need to tell? (Sales performance, HR analytics, supply chain, marketing ROI, etc.)
2. **Sub-industry** - Which specific vertical? (Grocery retail vs warehouse club vs fashion vs electronics, etc.)
3. **Dataset size** - Small (S), Medium (M), or Large (L)?
4. **Audience** - Who will be in the room? (CFO, Sales Director, Operations Manager, BI Developer, etc.)

**integrated planning Mode** asks five clarification questions:

1. **Dataset identity** - What industry, fictional company profile, revenue scale, and geographic footprint?
2. **Fact table scope** - Which upstream fact tables are needed? (Sales/Transactions, ARR/Subscriptions, Inventory, Purchase Orders, Headcount)
3. **Planning scenarios** - What three planning scenarios should be built?
4. **Chart of accounts depth** - How many leaf accounts in DimAccount? (Minimum 20 recommended)
5. **Deliverables** - Dataset only, or also BIM file, report design document, and/or voiceover script?

If you name a real company and provide a detailed description, Claude extracts most of this
from your input and only asks for what is genuinely missing.

### Example Prompt (Standard Mode)

```
Create a dataset for a warehouse club retail business operating across NA,
Australia, and Asia. Products should vary by region. Include online sales with logistics,
carrier tracking, returns and refunds, and a membership programme with Gold, Executive,
and Business tiers. Also include warehouse employees by department, IT hubs as a separate
cost centre, and seasonal products (spring flowers, winter sports) for relevant countries.
Dataset size: Large. Audience: VP of Operations.
```

### Example Prompt (Integrated Planning Mode)

```
Create an integrated planning dataset for a B2B SaaS company. Include ARR
waterfall, headcount planning with department-level cost allocation, and a full P&L with
budget vs actual variance. Dataset size: Medium. Audience: CFO.
```

Claude will research the business model, design the schema, apply global geography
specificity, encode regional product preferences and size weights, build a discount
rotation calendar, embed macro events, and output a complete Python script.

### What You Get

After answering the intake questions, Claude produces:

- **`generate_[industry]_dataset.py`** - the complete Python script, shown as a
  downloadable artifact in chat. Run it with `python generate_[industry]_dataset.py`.

The script itself generates (in an `output_[industry]_[date]/` folder):

- All dimension and fact CSV files (DimDate, DimGeography, DimProduct, FactSalesTransaction, etc.)
- In Integrated Planning mode: DimScenario, DimVersion, DimEntity, DimDeptAccountBridge, and other planning dimensions
- `[industry]_dataset_guide_public.md` - audience-facing documentation (KPIs, table inventory, relationship overview)
- `[industry]_dataset_guide_internal.md` - builder documentation (schema details, macro events applied, reconciliation rules, generation parameters)

### Script Requirements

The generated script requires only two non-standard Python libraries:

```bash
pip install pandas numpy
```

Python 3.8 or later. No database connection required. All output is flat CSV files.

---

## Realism Engines Built Into Every Script

### Macro Event Engine

Sourced from trusted references (IMF, World Bank, McKinsey, Reuters, Euromonitor).
Applied as time-bounded multipliers with ramp-up and recovery curves.

```python
MACRO_EVENTS = {
    "Supply Chain Disruption 2021-22": {
        "start": datetime.date(2021, 6, 1),
        "end":   datetime.date(2022, 9, 30),
        "peak":  datetime.date(2022, 1, 1),
        "affected_columns": ["SalesAmount", "LeadTimeDays", "UnitCost"],
        "multiplier_at_peak": 1.35,
        "affected_geographies": [],  # all
        "affected_categories":  ["Electronics", "Appliances"],
    },
    # ... more events per use case
}
```

### Size Weight Engine

```python
PRODUCT_SIZE_WEIGHTS = {
    ("Apparel", "North America"): {"XS": 0.05, "S": 0.15, "M": 0.32, "L": 0.28, "XL": 0.13, "XXL": 0.07},
    ("Apparel", "East Asia"):     {"XS": 0.18, "S": 0.35, "M": 0.30, "L": 0.12, "XL": 0.04, "XXL": 0.01},
    # ... per category x region
}
```

### Regional Product Affinity Engine

```python
REGIONAL_PRODUCT_AFFINITY = {
    ("Junk Food / Snacks", "North America"): 1.8,   # high demand
    ("Junk Food / Snacks", "East Asia"):     0.7,   # below baseline
    ("Alcohol", "Middle East"):              0.0,   # not sold
    ("Instant Noodles", "SE Asia - Maritime"): 1.8,
    # ... per category x region
}
```

### Zero Reconciliation

Every connected value pair is derived, not independently generated:

- `FactOrder.OrderAmount` is always the sum of its `FactOrderLine.LineAmount` rows
- `FactInventorySnapshot.ClosingStock` = Opening + StockIn - StockOut - Wastage
- `FactPayroll.NetPay` = GrossAmount - all deductions (never generated separately)
- `FactReturn` rows are sampled from real `FactSalesTransaction` rows (no orphan returns)
- In Integrated Planning mode: `FactFinancial` amounts for revenue, payroll, and COGS accounts are derived from upstream fact tables via lookup and bridge, not independently generated

### Budget vs Actual Variance Engine (V3)

Budget and Forecast are always derived FROM Actual, never the other way around.
Each account type has a bias and noise profile in `VARIANCE_PROFILE`. The bias
sign convention ensures the correct variance direction:

- Income accounts: positive bias means Budget < Actual (favourable)
- Cost accounts: negative bias means Budget > Actual (favourable = underspend)
- At least 20% of all rows show adverse variance, enforced by validation check
- Variance noise bands widen during macro shock periods via `shock_multiplier`

---

## Output File Naming

| File | Description |
|---|---|
| `generate_[industry]_dataset.py` | The Python script (shown in chat) |
| `DimDate.csv` | Date dimension - always present |
| `DimGeography.csv` | Geography hierarchy - always present |
| `DimScenario.csv` | Scenario dimension (Actual, Budget, Forecast) - present in financial and planning use cases |
| `DimVersion.csv` | Version dimension (Annual Budget, Quarterly Reforecast) - present in Integrated Planning mode |
| `DimEntity.csv` | Consolidation entity dimension - present in Integrated Planning mode |
| `DimProduct.csv` | Product dimension - present when use case involves products |
| `DimCustomer.csv` | Customer dimension - present when use case involves customers |
| `DimEmployee.csv` | Employee dimension - present when use case involves HR or headcount |
| `DimAccount.csv` | Chart of accounts with sort keys - present for financial P&L use cases |
| `DimDeptAccountBridge.csv` | Department-to-account allocation bridge - present when both DimAccount and DimDepartment exist |
| `FactSalesTransaction.csv` | Sales fact - present for sales/revenue use cases |
| `FactOrder.csv` / `FactOrderLine.csv` | Order header and lines - present for fulfilment use cases |
| `FactARR.csv` | ARR waterfall movements - present for SaaS/subscription use cases |
| `FactHeadcount.csv` | Headcount at employee grain - present for HR and planning use cases |
| `FactFinancial.csv` | P&L financials (long format) - present for financial reporting and planning use cases |
| `FactInventorySnapshot.csv` | Inventory flow - present for operations/supply chain use cases |
| `FactReturn.csv` | Returns and refunds - present when sales or fulfilment tables are included |
| `[industry]_dataset_guide_public.md` | Audience-facing documentation |
| `[industry]_dataset_guide_internal.md` | Builder/developer documentation |

---

## Validation Checks Run by Every Script

### Standard Mode Validation (non-financial use cases)

The script runs these checks before printing the final summary. Failures appear as
warnings; they do not stop the script.

| Check | What It Validates |
|---|---|
| FK integrity | Every FK in every fact table exists in its corresponding dimension |
| Date coverage | All fact dates fall within DimDate |
| Star schema dtype | No unexpected `object` dtype columns in fact tables |
| Header/line reconciliation | FactOrder.OrderAmount equals sum of FactOrderLine.LineAmount per order |
| Inventory flow reconciliation | ClosingStock = Opening + In - Out for every snapshot row |
| Payroll reconciliation | GrossAmount = NetPay + all deductions for every payroll row |
| Size weight diversity | Size distributions differ across regions (not the same global average) |
| Regional affinity applied | Category volumes vary meaningfully across regions |
| Geography specificity | No broad regional label used as a leaf node in DimGeography |
| Anonymisation | Real company name does not appear in any output file |
| DimAccount sort keys | All five sort key columns present and HierarchySortKey unique per leaf (when DimAccount exists) |

### Integrated Planning Mode: 10 Mandatory Validation Checks

For integrated planning datasets, all 10 checks must pass. Any warning stops the
dataset from being delivered.

| # | Check | Assertion |
|---|---|---|
| 1 | Star schema (all facts) | No `object` dtype columns in any fact table |
| 2 | Star schema (FactHeadcount) | No `object` dtype in FactHeadcount specifically |
| 3 | Star schema (FactARR) | No `object` dtype in FactARR specifically |
| 4 | Variance mix | Adverse variance rows >= 20% of total |
| 5 | Headcount reconciliation | FactHeadcount Actuals distinct EmployeeKeys == DimEmployee active count, diff = 0 |
| 6 | ARR waterfall | ClosingARR == OpeningARR + NewARR + ExpansionARR + ContractionARR + ChurnARR, zero mismatches |
| 7 | Payroll reconciliation | FactFinancial payroll amounts == SUM(FactHeadcount x bridge x SplitPct), max diff $0.00 |
| 8 | COGS (SDC) reconciliation | FactFinancial SDC amounts == SUM(FactHeadcount x COGS bridge x SplitPct) for bridge GeoKeys, max diff $0.00 |
| 9 | Revenue (RR) reconciliation | FactFinancial RR amounts == FactARR.ClosingARR grouped by RevenueAccountCode, max diff $0.00 |
| 10 | NRR reconciliation | FactFinancial NRR amounts == FactSalesActivity.Revenue grouped by RevenueAccountCode with 40/60 IMPL split, max diff $0.00 |

Checks 7-10 use a shared `check_recon()` helper that recomputes expected values
independently from scratch, not from the lookup dictionaries used during generation.

---

## Dataset Size Tiers

| Tier | Label | Total Rows (approx) | Best For |
|---|---|---|---|
| S | Small | 20K to 100K | 5 to 10 minute walkthroughs, desktop demos |
| M | Medium | 100K to 500K | Full POC, client workshops, tool evaluations |
| L | Large | 500K to 2M | Stress testing, BI performance demos, enterprise pitches |

---

## Supported Use Cases

The skill dynamically selects tables for any of the following (and combinations):

- Sales and revenue performance
- Customer segmentation and behaviour
- Operations and fulfilment tracking
- HR and workforce analytics
- Supply chain and inventory management
- Marketing campaign performance
- Financial management reporting (P&L, budget vs actuals)
- Logistics and delivery tracking
- Membership and loyalty programmes
- Multi-department workforce (including IT hubs as separate cost centres)
- SaaS/subscription revenue (ARR waterfall, NRR, customer cohorts)
- Integrated planning (revenue + headcount + P&L in a closed ledger via Integrated Planning mode)

---

## Geography Coverage

The skill includes country-level name pools for 30+ countries across all major regions:

North America (USA, Canada, Mexico), Latin America (Brazil, Argentina, Colombia),
Western Europe (UK, Germany, France, Spain, Italy, Netherlands, Sweden),
Eastern Europe (Poland, Russia), Middle East (UAE, Saudi Arabia, Israel, Egypt),
Sub-Saharan Africa (Nigeria, Kenya, South Africa), East Asia (China, Japan, South Korea),
Southeast Asia (Indonesia, Malaysia, Philippines, Thailand, Vietnam), South Asia (India
North/South, Bangladesh), Oceania (Australia, New Zealand).

Multi-ethnic city distributions are applied for Singapore, Kuala Lumpur, Toronto, London,
New York, Johannesburg, and other cosmopolitan cities where relevant.

---

## Attribution and Copyright

```
Demo Dataset Creator
Designed and Developed by: Logeshkumar Sivakumar
Contact: elogu2001@outlook.com

Copyright (c) 2026 Logeshkumar Sivakumar. All rights reserved.

This skill, its schema design, realism engine, geographic and demographic modelling,
seasonal event framework, integrated ledger derivation rules, and documentation are
the original intellectual work of Logeshkumar Sivakumar. Unauthorised reproduction,
redistribution, or commercial use without explicit written permission is prohibited.
```

Every Python script and documentation file generated by this skill includes the above
attribution block automatically.

---

## Related Skills

| Skill | Description |
|---|---|
| [`enterprise-pbix-model-builder`](https://github.com/Logeshkumar-10/pbix-model-creator) | Builds a fully modelled Power BI semantic model from an enterprise dataset. Produces BIM, DAX measures, and documentation. Can be chained after demo-dataset-creator. |

---

## Changelog

| Version | Date | Changes |
|---|---|---|
| 2.0 | June 2026 | Integrated Planning mode with 15 additional rules. Three planning sheet structures (Revenue/ARR, Headcount, P&L Master). Planning scenario design (Rule 12): three scenarios with step-by-step EBITDA traceability. Planning sheet structure reference (Rule 13). CSV output rules for planning mode (Rule 14): no comment rows, integer surrogate keys, YYYYMMDD DateKey format. Five clarification questions for Integrated Planning mode (Rule 15). BIM sourceColumn validation (Rule 11). |
| 1.4 | June 2026 | Integrated ledger rules (Rules 7-11): FactFinancial built last, derives from FactARR (revenue), FactSalesActivity (NRR), FactHeadcount via bridge (payroll + COGS). Mandatory build order: FactARR, FactSalesActivity, FactHeadcount, FactFinancial. INDEPENDENT_BASE restricted to accounts with no upstream source. Bridge-covered accounts skip guard prevents default-amount rows. 10-check validate_all() replacing 4-check run_all_checks() for integrated planning datasets. |
| 1.3 | June 2026 | Rule 6: Employee-grain FactHeadcount. Probabilistic Budget/Forecast inclusion via OVERHIRE_MULTIPLIER. |
| 1.2 | June 2026 | Star schema rules; DimEmployee level columns with sort keys; four mandatory validation checks; CSV output rule; documentation schema data-type tables; UTF-8 encoding rule. |
| 1.1 | June 2026 | Fixed inverted variance direction. Added DimAccount sort key columns. |
| 1.0 | May 2026 | Initial release. |

---

## License

This software is proprietary. See [LICENSE](LICENSE) file for full terms.

No usage, copying, modification, or distribution rights are granted by viewing this
repository or its source code.

Copyright (c) 2026 Logeshkumar Sivakumar (elogu2001@outlook.com). All rights reserved.
