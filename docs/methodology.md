# Methodology

How eleven years of delivery challans and stock ledgers were turned into one audited, part-level reconciliation.

## Sources, per year

| Source | Sheet(s) | Shape as found |
|---|---|---|
| Delivery challans | `CHALLAN` (and `GST` from 2017) | One printed challan per block: header (`To,` party, date, challan #), then line items `S.No · Description · Part No · CTN · PCS/CTN · QTY · Price · Amount`. 350–1,150 challans a year, typed by hand. |
| Stock ledger | `Part 1` … `Part 13` | One product family per sheet (condensers, cooling coils, hoses …), running stock per part. |
| Year reconciliation | `Recon` / `Rec` | Per part: challan quantity vs ledger stock, gap, total available. |

## Pipeline

```
CHALLAN blocks ──► Pivot Data ──► Part Summary ──┐
   (1 row per line)   (per part: total /         │
                       cancelled / net)          ├──► Recon (per year) ──► Master (all years) ──► Audit
Stock ledger (Part n) ───────────────────────────┘      challan vs stock     2,425 parts × year     tie-outs +
                                                                              blocks                findings
```

### 1. Flatten challans → `Pivot Data`

Each challan line becomes one row: `Source Row · Date · Challan No · Part Name · Part Number · D (cartons) · E (pcs per carton) · Total Qty · Basis · Cancelled`.

- **Quantity basis.** `Total Qty = D × E` when both are present. When a line only has a quantity (no carton / pcs split), the quantity column is used and `Basis` records which rule applied. The rule is the same every year (see findings `Fxx-…` under *Qty basis*).
- **Traceability.** `Source Row` points back to the exact challan row, so every pivot figure can be drilled to the printed challan.
- **Validation.** Line counts in `Pivot Data` are tied to the challan sheet every year (`CHALLAN lines Δ = 0` in the Year Register).

### 2. Cancellations

A challan counts as cancelled only with evidence: the stock ledger's cancelled list and/or a *CANCELLED* stamp in the challan block. `Recon_Setup` holds the authoritative list; `Cancel Evidence` (2016+) records which source confirmed it. Cancelled lines stay in `Pivot Data`, flagged, and are excluded from net quantity.

### 3. `Part Summary`

Per part: `Total Qty`, `Cancelled Qty`, `Net Qty (excl. Cancelled)`. This is the challan side of the reconciliation.

### 4. Year reconciliation (`Recon` / `Rec`)

Challan (sold) vs ledger stock, per part, with `Gap = Challan − Stock`. Challan lines are matched to ledger rows by:

| Key type | Example | Used for |
|---|---|---|
| Code core | `86064`, `E420` | Most parts: part number stripped of prefix / suffix noise |
| Description | `VALVE PIN R12` | Consumables with no code |
| Wildcard pattern | `14x18x20*` | Hose fittings sold in size families |

Rows whose key is ambiguous are **not** force-matched. They are listed in the year's audit sheet for owner review.

### 5. Master (`Master.xlsx`)

- **Grain:** one row per part (`Section · Group · Item · Part Code`), 2,425 rows.
- **Year blocks:** `Total Available · Challan · Stock · Difference` for each year 2009–2019.
- **Matching into the master is section-scoped.** Part codes were reused across product families over the decade, so a code match is accepted only inside the same master section. A year row with no safe match is added as a *year-only* row rather than merged into the wrong part.
- `Difference` is always recomputed (`Challan − Stock`). Typed gap values in the sources are not trusted.

### 6. Audit (`Master.xlsx` → `Audit`)

- **Year Register.** For every year, source vs master for row count, challan qty, stock qty, total available and challan lines, each with a Δ column. All eleven years tie (Δ = 0).
- **Findings Log.** 116 findings, each with a reference (`F13-03`), area, affected rows, treatment and status (`Info` / `Open` / `Closed`). Nothing was silently fixed. Every adjustment, exclusion or override has a finding.
- **Data Sources.** Provenance for every loaded range: source workbook, sheet, row count, what changed after retrieval.

## Validation checklist (run after every load)

1. `Pivot Data` line count = challan line count (Δ 0).
2. `Part Summary` net total = `Pivot Data` total − cancelled.
3. Recon rows carried to master = recon rows in source − blank/subtotal rows (Δ 0).
4. Challan / Stock / Total Available sums: source = master (Δ 0).
5. No part code matched across master sections.
6. Every override, exclusion or manual fill has a finding reference.
