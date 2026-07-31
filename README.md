# A Fertilizer Shortage (FS) Index

The Fertilizer Shortage (FS) index measures the intensity of reported disruptions to global fertilizer supply through comprehensive media analysis. Building on the search-based methodology of Baker, Bloom, and Davis (2016), the index tracks how frequently the news jointly discusses fertilizers and supply shortages, providing a high-frequency signal of stress in global fertilizer markets.

**Reference**: Morão (2025)  
Hugo Morão,  
The economic consequences of fertilizer supply shocks,  
Food Policy,  
Volume 133, 2025, 102835,  
https://doi.org/10.1016/j.foodpol.2025.102835.  

## Economic Intuition Behind the Index
Fertilizers—nitrogen, phosphate, and potash—are essential inputs to global agriculture, and their supply is concentrated among a handful of producing regions and traded through long, energy-intensive supply chains. Shortages, scarcity, and logistical bottlenecks in this segment raise input costs for farmers, feed into food prices, and propagate into broader economic uncertainty. By quantifying media coverage of fertilizer supply disruptions, the FS index provides an early, quantitative indicator of stress in fertilizer markets.

## Construction Methodology
The index follows the Baker et al. (2016) methodology for search-based indices, measuring the frequency of news articles that jointly reference two semantic categories:

1. **Fertilizer (F)** — the fertilizer industry and its main products, identified through an industry code and terms such as *fertilizer* / *fertiliser*, *potash*, and *ammonia*.

2. **Shortage (S)** — supply-disruption language such as *shortage*, *scarcity*, *backlog*, *disruption*, and *bottleneck* (including the relevant supply/shortage subject codes).

An article is counted only when it contains at least one term from each category and has a word count exceeding 99 but not surpassing 2000 words. Articles that are summaries of news, corporate digests, calendars of events, rankings-based content, routine general and market/financial news, news agency materials, letters, headline-only content, personal announcements, contract tenders, obituaries, and TV/Radio listings are excluded to ensure that only relevant articles are selected. The exact boolean query used to retrieve articles is stored in `config/config.json`.

## Coverage and Data Quality
The index is monthly and begins in January 1977. It draws on an extensive network of English-language media sources across major economies, including the United States, United Kingdom, Canada, Australia, and Asia-Pacific, Middle Eastern, and European markets. The full list of sources, with each source's coverage window and importance weight, is provided in `config/sources.csv`. A smaller, high-importance subset used for robustness is provided in `config/sources1.csv`.

## Repository Structure
```
fs_world/
├── config/
│   ├── config.json      # search definition: boolean query, languages, interval
│   ├── sources.csv      # media sources with coverage windows and importance weights
│   └── sources1.csv     # alternative high-importance source subset (robustness)
├── data/
│   ├── raw/
│   │   └── num_hits.csv # raw monthly article counts per source (index numerator and denominator)
│   └── processed/
│       ├── index.csv    # computed FS index (long form, four series)
│       └── index.xlsx   # computed FS index (spreadsheet)
├── CITATION.cff         # machine-readable citation metadata
├── CHANGELOG.md         # record of notable changes
├── LICENSE              # license terms (CC BY 4.0)
└── README.md
```

## Reproducing the Index
The index is built in three stages, moving left to right through the folders above:

1. **Define the search** — `config/config.json` holds the exact boolean query (fertilizer and shortage terms, the word-count filter, and excluded subject codes) together with the target language and reporting interval. `config/sources.csv` lists the media sources to query and the coverage window over which each is available.
2. **Collect raw counts** — running the query in `config.json` against each source in `sources.csv` (via the news database, e.g. Factiva) yields `data/raw/num_hits.csv`: for every source and month, the number of matching articles (`num_hits`) and the total number of articles published (`total_articles`).
3. **Compute the index** — the raw counts are scaled by each source's total article volume, aggregated across sources (optionally weighted by `importance`), and standardized to produce `data/processed/index.csv`. The result is provided in both CSV and XLSX form and covers 1977-01 through the latest available month.

### Data Dictionary
**`config/config.json`**
| Field | Description |
| --- | --- |
| `type` | Index type (`index`). |
| `languages` | Languages of the retrieved articles. |
| `interval` | Reporting frequency of the index (`Monthly`). |
| `search_text` | The full boolean query: a fertilizer-industry code / fertilizer terms combined with supply-shortage terms, the word-count filter (`wc>99 AND wc<2000`), and excluded subject codes (`NOT ns=...`). |

**`config/sources.csv`** and **`config/sources1.csv`**
| Column | Description |
| --- | --- |
| `name` | Source (publication) name. |
| `fry`, `frm`, `frd` | Coverage start — from year, month, day. |
| `toy`, `tom`, `tod` | Coverage end — to year, month, day. |
| `importance` | Relative weight of the source used in the weighted index series. |

**`data/raw/num_hits.csv`**
| Column | Description |
| --- | --- |
| `source` | Source name (matches `sources.csv`). |
| `year`, `month`, `day` | Period of the count (`day` empty for monthly aggregates). |
| `num_hits` | Number of articles matching the FS query in that source-period. |
| `total_articles` | Total number of articles published by the source in that period (scaling denominator). |
| `error` | Retrieval error message, if any (empty on success). |
| `num_tries` | Number of retrieval attempts made. |

**`data/processed/index.csv`**
| Column | Description |
| --- | --- |
| `year`, `month` | Period of the observation. |
| `unstandardized` | Aggregate scaled article frequency, equal source weighting. |
| `unstandardized weighted` | Aggregate scaled article frequency, weighted by source `importance`. |
| `standardized` | Standardized (mean/standard-deviation normalized) FS index, equal weighting. |
| `standardized weighted` | Standardized FS index, weighted by source `importance`. |

## How to Cite
If you use this index or dataset, please cite the companion paper:

> Hugo Morão, "The economic consequences of fertilizer supply shocks," *Food Policy*, Volume 133, 2025, 102835. https://doi.org/10.1016/j.foodpol.2025.102835

A machine-readable citation is available in `CITATION.cff`, and GitHub's
"Cite this repository" button will generate APA and BibTeX entries from it.

## License
The data and documentation in this repository are released under the
[Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).
You are free to share and adapt the material, including for commercial purposes,
provided you give appropriate credit by citing the paper above.
