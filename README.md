# Inventory Reconciliation 2009–2019

Eleven years of hand-typed delivery challans and stock ledgers, rebuilt into a traceable part-level reconciliation: challan lines flattened and validated, cancellations evidenced, each year's challan-vs-stock reconciliation tied out, and every year consolidated into a single master with an audit register and findings log.

**Stack:** Excel 365 · structured tables · pivot tables · formula-driven reconciliation · Git

## Results

All eleven years tie to the master with **Δ 0** on row count, challan quantity, stock quantity, total available and challan lines.

| Year | Recon rows | Challan qty | Stock qty | Challan lines loaded | Findings (open) | Tie-out |
|---|---:|---:|---:|---:|---:|---|
| [2009](docs/years/2009.md) | 246 | 109,320 | 71,814 | 1,911 | 13 (4) | ✅ Ties |
| [2010](docs/years/2010.md) | 353 | 133,929.35 | 134,228.18 | 2,670 | 14 (3) | ✅ Ties |
| [2011](docs/years/2011.md) | 444 | 342,970 | 340,798 | 2,499 | 15 (5) | ✅ Ties |
| [2012](docs/years/2012.md) | 496 | 193,317 | 197,723 | 4,031 | 10 (4) | ✅ Ties |
| [2013](docs/years/2013.md) | 1,012 | 163,107 | 165,664 | 4,531 | 10 (5) | ✅ Ties |
| [2014](docs/years/2014.md) | 1,132 | 206,744 | 181,798 | 5,416 | 11 (6) | ✅ Ties |
| [2015](docs/years/2015.md) | 916 | 167,028 | 164,589 | 7,325 | 10 (5) | ✅ Ties |
| [2016](docs/years/2016.md) | 1,307 | 117,951 | 121,899 | 6,138 | 12 (5) | ✅ Ties |
| [2017](docs/years/2017.md) | 1,355 | 199,242 | 192,742 | 11,830 | 11 (4) | ✅ Ties |
| [2018](docs/years/2018.md) | 1,656 | 178,241 | 194,336 | 11,300 | 5 (3) | ✅ Ties |
| [2019](docs/years/2019.md) | 1,260 | 84,334 | 65,853 | 5,709 | 5 (0) | ✅ Ties |

116 findings logged (see [audit findings](docs/audit-findings.md)); items marked *Open* need an owner decision and were **not** silently adjusted.

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
| 2009–2019 | [`Master.xlsx`](workbooks/Master.xlsx) | [findings](docs/audit-findings.md) | ✅ published |

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
