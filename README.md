# Inventory Reconciliation 2009–2019

Eleven years of hand-typed delivery challans and stock ledgers, rebuilt into a traceable part-level reconciliation: challan lines flattened and validated, cancellations evidenced, each year's challan-vs-stock reconciliation tied out, and every year consolidated into a single master with an audit register and findings log.

**Stack:** Excel 365 · structured tables · pivot tables · formula-driven reconciliation · Git

## Contents

| Year | Workbook | Notes | Status |
|---|---|---|---|
| 2009 | [`Recon-09.xlsx`](workbooks/Recon-09.xlsx) | [notes](docs/years/2009.md) | ✅ published |
| 2010 | [`Recon-10.xlsx`](workbooks/Recon-10.xlsx) | [notes](docs/years/2010.md) | ✅ published |
| 2011 | [`Recon-11.xlsx`](workbooks/Recon-11.xlsx) | [notes](docs/years/2011.md) | ✅ published |
| 2012 | [`Recon-12.xlsx`](workbooks/Recon-12.xlsx) | [notes](docs/years/2012.md) | ✅ published |
| 2013 | [`Recon-13.xlsx`](workbooks/Recon-13.xlsx) | [notes](docs/years/2013.md) | ✅ published |
| 2014 | [`Recon-14.xlsx`](workbooks/Recon-14.xlsx) | [notes](docs/years/2014.md) | ✅ published |
| 2015 | [`Recon-15.xlsx`](workbooks/Recon-15.xlsx) | [notes](docs/years/2015.md) | ✅ published |
| 2016 | [`Recon-16.xlsx`](workbooks/Recon-16.xlsx) | [notes](docs/years/2016.md) | ✅ published |
| 2017 | [`Recon-17.xlsx`](workbooks/Recon-17.xlsx) | [notes](docs/years/2017.md) | ✅ published |
| 2018 | [`Recon-18.xlsx`](workbooks/Recon-18.xlsx) | [notes](docs/years/2018.md) | ✅ published |
| 2019 | [`Recon-19.xlsx`](workbooks/Recon-19.xlsx) | [notes](docs/years/2019.md) | ✅ published |
| 2009–2019 | `Master.xlsx` | — | ⏳ coming |

## How the reconciliation works

1. **Flatten challans.** Every challan line becomes one row in `Pivot Data`, with a drill-back `Source Row`, a quantity rule (`D × E` or quantity-only) and a cancellation flag.
2. **Evidence cancellations.** A challan counts as cancelled only when the ledger's list or the challan stamp says so.
3. **Summarise per part.** `Part Summary` gives total, cancelled and net quantity.
4. **Reconcile the year.** `Recon` compares challan quantity with ledger stock per part, matching on code cores, descriptions or size patterns, and never force-matches ambiguous rows.
5. **Consolidate.** `Master.xlsx` holds 2,425 parts × one block per year. Matching is limited to the same product section because part codes were reused across families, and unmatched rows are kept as year-only rows.
6. **Audit.** The Year Register ties source to master for every year. The Findings Log records every rule, exclusion and open question.

Details: [methodology](docs/methodology.md) · [data dictionary](docs/data-dictionary.md)

## Data privacy

These are real operational records, anonymised before publication: party names, contacts, addresses, tax IDs, phone numbers and prices are masked; quantities, dates, challan numbers and all formulas are unchanged. Each file passed a leak scan, a structure diff and a full recalculation diff against the original. See [anonymisation](docs/anonymisation.md).

## Repository layout

```
workbooks/   Recon-09.xlsx … Recon-19.xlsx, Master.xlsx
docs/        methodology, data dictionary, anonymisation, per-year notes, audit findings
```
