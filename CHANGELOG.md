# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-07-31

### Added
- Initial repository for the Fertilizer Shortage (FS) index.
- `config/config.json` documenting the exact boolean search query, language, and
  reporting interval used to build the index.
- `config/sources.csv` listing the media sources with coverage windows and
  importance weights, and `config/sources1.csv` with a high-importance source
  subset used for robustness.
- `data/raw/num_hits.csv` with the raw monthly article counts (matching articles
  and total articles) per source.
- `data/processed/index.csv` and `data/processed/index.xlsx` with the computed
  index (four series), covering 1977-01 through 2025-11.
- `README.md` with the methodology, a reproducible `config/` → `data/raw/` →
  `data/processed/` pipeline, and a per-file data dictionary.
- `CITATION.cff` with machine-readable citation metadata for the companion paper,
  published in *Food Policy*, Volume 133, 2025, 102835
  (https://doi.org/10.1016/j.foodpol.2025.102835).
- `LICENSE` — Creative Commons Attribution 4.0 International (CC BY 4.0).
- `.gitattributes` for cross-platform LF normalization.
