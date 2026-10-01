# Data dictionary

Column names vary slightly by year (e.g. `Challan No` / `Challan Number` / `Challan #`). The canonical name is listed first.

## `Pivot Data` (each Recon workbook)

| Column | Type | Meaning |
|---|---|---|
| `Book` | text | 2017+: which challan book the line came from (`CHALLAN` or `GST`) |
| `Source Row` | int | Row of the line in the challan sheet (drill-back key) |
| `Date` | text `dd.mm.yy` | Challan date as printed |
| `Challan No` | text | Challan number, e.g. `GD/001/14-15`, `2018-001` |
| `Part Name` | text | Description as written on the challan |
| `Part Number` | text | Part code as written (anonymised prefix `PC-` / `PCP-`) |
| `D` / `Ctn` | number | Cartons or units |
| `E` / `PCs / ctn` | number | Pieces per carton |
| `Total Qty` | number | `D × E`, or the quantity column when D/E are absent |
| `Basis` | text | Which quantity rule produced `Total Qty` |
| `Cancelled` | flag | `-` = active; otherwise the challan is cancelled |
| `Cancel Evidence` | text | 2016+: source that confirmed the cancellation |

## `Part Summary`

| Column | Meaning |
|---|---|
| `Part Name`, `Part Number` | Part as written on challans |
| `Total Qty` | Sum of `Total Qty` over all lines |
| `Cancelled Qty` | Sum over cancelled lines |
| `Net Qty (excl. Cancelled)` | `Total Qty − Cancelled Qty` |

## `Recon` / `Rec`

Laid out in side-by-side bands, one per product family:

| Column | Meaning |
|---|---|
| `Name`, `Code` | Ledger part name and code |
| `Match Key` | Key used to pull the challan quantity (code core / description / pattern) |
| `Challan` | Quantity despatched per challans |
| `Stock` | Quantity per stock ledger |
| `Gap` | `Challan − Stock` |
| `Total Available` | Opening + receipts per ledger |

## `Master.xlsx` → `Master` (table `MasterInventory`)

| Column | Meaning |
|---|---|
| `Section` | Master section (product family), e.g. `Part 1 – Condensers` |
| `Group` | Sub-family |
| `Item` | Part name |
| `Part Code` | Canonical part code |
| `Total Available YYYY` | From that year's Recon |
| `Challan YYYY` | From that year's Recon |
| `Stock YYYY` | From that year's Recon |
| `Difference YYYY` | Recomputed `Challan − Stock` |

## `Master.xlsx` → `Audit`

**Year Register** (one row per year): `Recon rows in source`, `Rows not carried (blank / subtotal)`, `Rows carried to master`, `Row check Δ`, then `master / source / Δ` triples for Challan, Stock, Total Available and challan lines, `Filled existing rows`, `Added as year-only rows`, `Status`.

**Findings Log** (table `AuditFindings`):

| Column | Meaning |
|---|---|
| `Year`, `Ref` | e.g. `2013`, `F13-03` |
| `Area` | Source, Matching, Cancellations, Units, Qty basis, … |
| `Finding` | What was found, with row / challan references |
| `Rows` | Rows affected |
| `Treatment` | Uniform rule · Carried as found · Excluded · Override · Needs owner review … |
| `Status` | `Info` (documented) · `Open` (owner decision needed) · `Closed` |

## Anonymised values you will see

| Value | Replaces |
|---|---|
| `Dealer-###`, `Contact-###`, `Transporter-###` | Party, contact person, transporter. The same party has the same code in every year. |
| `Address line (redacted)` | Street address lines |
| `GSTIN-MASKED`, `TIN-MASKED`, `PHONE-MASKED`, `XXXXXXXXXX` | Tax IDs, phone numbers |
| `1` (or `-1` / `0`) in challan `Price` / `Amount` | Commercial prices (only the sign is kept) |
| `Company-A`, `PC-` / `PCP-` part-code prefix | Company name and its part-code prefix |
