# Demo Dataset Creator

> A skill for Claude that generates realistic, demo-ready datasets for any sub-industry,
> tailored to a specific end-user persona and presentation purpose.

**Designed and Developed by Logeshkumar Sivakumar**
**Contact: elogu2001@outlook.com**
**© 2026 Logeshkumar Sivakumar. All rights reserved.**

---

## What This Skill Does

Demo Dataset Creator instructs Claude to produce a single, self-contained Python script
that generates a full set of CSV files and documentation when run. No data is generated
inside the chat. The script is the only deliverable shown in conversation.

The datasets produced are built for product demos, proof-of-concept evaluations, client
workshops, tool showcases, and hackathons. They are deliberately realistic - not toy data.

### What Makes This Different From Generic Sample Data

| Capability | Description |
|---|---|
| Company research | If you name a real company, Claude researches its business model and geography, then models the dataset after it - without using the company name anywhere |
| Dynamic tables | No fixed table set is always generated. Every table is justified by the use case and end-user persona |
| Global geography specificity | No broad regional label (APAC, Europe, MENA, LATAM) is ever a leaf node. Every region is decomposed into sub-regions, countries, and cities with authentic local names |
| Country-level name pools | Customer and employee names are drawn from country-specific pools matching local ethnicity and naming conventions - across 30+ countries |
| Product size weightage | Demand distributions differ by product category and region. A medium T-shirt outsells an XXL in the US; XS outsells L in East Asia |
| Regional product affinity | Products are weighted by regional preference. Junk food sells more in North America; instant noodles sell more in SE Asia; alcohol is zero in Middle East |
| Zero reconciliation | Every connected value pair (order header vs lines, inventory in vs out, gross vs net pay, returns vs sales) reconciles to zero |
| Macro event engine | Real-world disruptions (supply chain crisis, inflation surge, COVID impact) are applied as time-bounded multipliers across geographies and categories |
| Seasonal accuracy | Seasonality is hemisphere-aware. Australian summer is December, not June. Diwali affects South Asia and its diaspora. Ramadan follows the lunar calendar |
| Discount rotation | Max 15-25% of SKUs are discounted in any week. Discounts rotate by category and align with seasonal events |
| Dataset size tiers | Small (20K-100K rows), Medium (100K-500K), Large (500K-2M) - selected at intake |

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
- `/demo-dataset-creator` (direct invocation)

### Intake Questions

Before generating anything, Claude asks four questions in a single grouped message:

1. **Use case** - What story does the data need to tell? (Sales performance, HR analytics, supply chain, marketing ROI, etc.)
2. **Sub-industry** - Which specific vertical? (Grocery retail vs warehouse club vs fashion vs electronics, etc.)
3. **Dataset size** - Small (S), Medium (M), or Large (L)?
4. **Audience** - Who will be in the room? (CFO, Sales Director, Operations Manager, BI Developer, etc.)

If you name a real company and provide a detailed description, Claude extracts most of this
from your input and only asks for what is genuinely missing.

### Example Prompt

```
Create a dataset for a retail business like Costco - warehouse concept across NA,
Australia, and Asia. Products should vary by region. Include online sales with logistics,
carrier tracking, returns and refunds, and a membership programme with Gold, Executive,
and Business tiers. Also include warehouse employees by department, IT hubs as a separate
cost centre, and seasonal products (spring flowers, winter sports) for relevant countries.
Dataset size: Large. Audience: VP of Operations.
```

Claude will research the warehouse club retail model, design the schema, apply global
geography specificity, encode regional product preferences and size weights, build a
discount rotation calendar, embed macro events, and output a complete Python script.

### What You Get

After answering the intake questions, Claude produces:

- **`generate_[industry]_dataset.py`** - the complete Python script, shown as a
  downloadable artifact in chat. Run it with `python generate_[industry]_dataset.py`.

The script itself generates (in an `output_[industry]_[date]/` folder):

- All dimension and fact CSV files (DimDate, DimGeography, DimProduct, FactSalesTransaction, etc.)
- `[industry]_dataset_guide_public.md` — audience-facing documentation (KPIs, table inventory, relationship overview)
- `[industry]_dataset_guide_internal.md` — builder documentation (schema details, macro events applied, reconciliation rules, generation parameters)

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

---

## Output File Naming

| File | Description |
|---|---|
| `generate_[industry]_dataset.py` | The Python script (shown in chat) |
| `DimDate.csv` | Date dimension - always present |
| `DimGeography.csv` | Geography hierarchy - always present |
| `DimProduct.csv` | Product dimension - present when use case involves products |
| `DimCustomer.csv` | Customer dimension - present when use case involves customers |
| `FactSalesTransaction.csv` | Sales fact - present for sales/revenue use cases |
| `FactOrder.csv` / `FactOrderLine.csv` | Order header and lines - present for fulfilment use cases |
| `FactInventorySnapshot.csv` | Inventory flow - present for operations/supply chain use cases |
| `FactPayroll.csv` | Payroll - present for HR use cases |
| `FactReturn.csv` | Returns and refunds -   present when sales or fulfilment tables are included |
| `[industry]_dataset_guide_public.md` | Audience-facing documentation |
| `[industry]_dataset_guide_internal.md` | Builder/developer documentation |

---

## Validation Checks Run by Every Script

The script runs the following checks before printing the final summary:

| Check | What It Validates |
|---|---|
| FK integrity | Every FK in every fact table exists in its corresponding dimension |
| Date coverage | All fact dates fall within DimDate |
| Header/line reconciliation | FactOrder.OrderAmount equals sum of FactOrderLine.LineAmount per order |
| Inventory flow reconciliation | ClosingStock = Opening + In - Out for every snapshot row |
| Payroll reconciliation | GrossAmount = NetPay + all deductions for every payroll row |
| Size weight diversity | Size distributions differ across regions (not the same global average) |
| Regional affinity applied | Category volumes vary meaningfully across regions |
| Geography specificity | No broad regional label used as a leaf node in DimGeography |
| Anonymisation | Real company name does not appear in any output file |

Failures appear as warnings in the summary - they do not stop the script.

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

---

## Geography Coverage

The skill includes country-level name pools for 30+ countries across all major regions:

North America (USA, Canada, Mexico), Latin America (Brazil, Argentina, Colombia),
Western Europe (UK, Germany, France, Spain, Italy, Netherlands, Sweden),
Eastern Europe (Poland, Russia), Middle East (UAE, Saudi Arabia, Israel, Egypt),
Sub-Saharan Africa (Nigeria, Kenya, South Africa), East Asia (China, Japan, South Korea),
Southeast Asia (Indonesia, Malaysia, Philippines, Thailand, Vietnam), South Asia (India
North/South, Bangladesh), Oceania (Australia, New Zealand).

Multi-ethnic country distributions are applied for Singapore, Malaysia, South Africa,
and Trinidad where relevant.

---

## Attribution and Copyright

```
Demo Dataset Creator
Designed and Developed by: Logeshkumar Sivakumar
Contact: elogu2001@outlook.com

© 2026 Logeshkumar Sivakumar. All rights reserved.

This skill, its schema design, realism engine, geographic and demographic modelling,
seasonal event framework, and documentation are the original intellectual work of
Logeshkumar Sivakumar. Unauthorised reproduction, redistribution, or commercial use
without explicit written permission is prohibited.
```

Every Python script and documentation file generated by this skill includes the above
attribution block automatically.

---

## Related Skills

| Skill | Description |
|---|---|
| `enterprise-pbix-model-builder` | Builds a fully modelled Power BI semantic model from an enterprise dataset. Produces BIM, DAX measures, and documentation. Can be chained after enterprise-planning-dataset-creator. |

---

## Changelog

| Version | Date | Changes |
|---|---|---|
| 1.0 | May 2026 | Initial release - global geography, size weights, regional affinity, zero reconciliation, company research, dynamic table selection |

---

*© 2026 Logeshkumar Sivakumar. All rights reserved.*
*elogu2001@outlook.com*
