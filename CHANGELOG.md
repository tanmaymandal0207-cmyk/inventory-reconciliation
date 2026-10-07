# Changelog

## v1.0 — 11 Oct 2026

First complete release: 2009–2019.

### Added
- `Recon-09.xlsx` … `Recon-19.xlsx`: one reconciliation workbook per year (challans flattened to `Pivot Data`, cancellations evidenced, `Part Summary`, challan-vs-stock `Recon`).
- `Master.xlsx`: 2,425 parts × year blocks, with the `Audit` sheet (Year Register tie-outs, 116-item Findings Log) and the `Data Sources` provenance log.
- Docs: methodology, data dictionary, anonymisation, per-year notes, audit findings.

### Changed
- Recon-12, Recon-13, Recon-14: removed empty text boxes and blank placeholder images left over from copy-pasting challan blocks (4,100+ objects). No data or formulas changed.

### Fixed
- Recon-11 notes and the 2009 notes: the word "unique" had been masked as a dealer pseudonym (one dealer is named "Unique"). The ordinary word is now kept; the dealer is still masked.

### Known open items
- Findings marked **Open** in the Findings Log need an owner decision; they are reported, not adjusted.
- 2019: the master block was reloaded from the final Recon-19 (findings F19-01 to F19-05; F19-05 records the owner's decision to merge 13 clutch-plate / bearing rows into their Part 7 – Coils rows).
