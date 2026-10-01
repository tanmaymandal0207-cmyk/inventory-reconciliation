# Anonymisation

The workbooks come from real operations. Before publishing, every file was anonymised and then checked three separate ways. Reconciliation quantities are untouched; only identifying and commercial data is masked.

## What was masked

| Data | Treatment |
|---|---|
| Party names on challans (dealers, individuals, "delivery at" parties) | Pseudonym `Dealer-###`, consistent across all years and sheets |
| Contact persons (`ATTN: Mr …`, names in remarks) | `Contact-###` / `Contact` |
| Transporters (`Through: …`) | `Transporter-###` |
| Street addresses, PIN codes | `Address line (redacted)`, `-XXXXXX` |
| GSTIN, TIN / VAT numbers | `GSTIN-MASKED`, `TIN-MASKED` |
| Phone numbers (text, numeric cells, `tel:` hyperlinks) | `XXXXXXXXXX` / `PHONE-MASKED`; hyperlinks removed |
| E-mail addresses, vehicle registration numbers, bank account / branch | masked |
| Challan `Price` and `Amount` on line items (including continuation pages) | replaced by `1` (`-1` for credits, `0` stays `0`): no price information remains, but formulas that check whether a line is priced or free still behave exactly as before |
| Price thresholds used by formulas | Two Recon-14 formulas identify a product variant by price (compressor oil 2 L at `= 330`, SUPERKING at `>= 4000`). For just those lines the masked price is set to the threshold, so the formulas give the same result. The thresholds are already visible in the formulas. |
| Cached results of formulas on the challan sheets | removed, so no pre-masking amounts remain inside the file; they are recalculated when the workbook opens |
| Prices quoted in text (`Rs. <amount>`) | `Rs.[x]` |
| Company name and its part-code prefix | `Company-A`; part codes carry the neutral prefix `PC-` / `PCP-`, applied everywhere (cells, formulas, pivot caches) so joins still line up |
| Workbook authors, comment authors, company metadata | `Analyst` / blank |
| Page headers / footers with the company address | `Company-A` |
| Links to other workbooks on a local drive | removed; the last calculated values are kept |

The original-to-pseudonym mapping is kept privately for traceability and is **not** in this repository.

## What was *not* changed

- Part names and descriptions (car models, sizes, part families)
- All quantities: cartons, pieces per carton, totals, cancellations, stock
- Challan numbers and dates
- Every formula, table, pivot table, comment and the Audit / Findings / Data Sources sheets

## Checks run on every published file

1. **Leak scan.** Every XML part of the workbook is scanned: cells, shared strings, formula literals, comments, pivot caches, drawings, hyperlinks and metadata. It looks for GSTIN / TIN / phone / e-mail / vehicle / bank / price patterns, the company name and code prefix, author names, local paths and **every original party string**. Result: 0 hits on all 12 workbooks.
2. **Structure diff** against the original. Sheet names and order, sheet dimensions, table names, ranges, columns and header cells, pivot cache fields and defined names must all be identical.
3. **Recalculation diff.** The original and anonymised workbooks were both fully recalculated in LibreOffice, and every numeric cell was compared. The only differences are challan `Price` / `Amount` and the formulas built on them (qty × price, totals). Recon, Part Summary, Pivot Data, Master and Audit figures are identical.
4. **Challan cell audit.** Every changed number on a challan sheet is either a masked price/amount under a PRICE/AMOUNT header, a masked ID, or a formula whose cached result was removed; no line-item price or amount survives.

## Opening the files

The files are set to recalculate fully when opened. Tools that read the file without calculating it (e.g. pandas) will see the challan-sheet helper formulas as blank; every other sheet keeps its saved values. Excel may take a few seconds on the larger years (Recon-13, Recon-17, Recon-18) and will ask to save on close. That prompt is only the recalculation, not a change.
