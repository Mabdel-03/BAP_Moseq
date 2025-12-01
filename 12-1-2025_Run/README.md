# 12-1-2025 MoSeq Run

## Overview

This directory contains the MoSeq pipeline runs and analysis code for the December 2025 data processing run.

## Structure

```
12-1-2025_Run/
├── All/
│   └── code/Processing/          # MoSeq pipeline notebooks for all subjects
├── Moms/
│   └── code/Processing/          # MoSeq pipeline notebooks for Moms
├── Offsprings/
│   ├── code/
│   │   ├── Processing/           # MoSeq pipeline notebooks
│   │   └── Analysis/             # Analysis notebooks (paths updated for new data)
│   ├── data/                     # Model output from model-006-4641589
│   │   ├── fingerprints/         # Full and robust fingerprint summaries
│   │   ├── analysis_output/      # Output from analysis notebooks
│   │   ├── moseq_df.csv          # Main MoSeq dataframe
│   │   ├── stats_df.csv          # Statistics dataframe
│   │   └── *_syllable_counts.csv # Syllable counts by group
│   └── figs/                     # Figure outputs
└── README.md
```

## Pipeline Status

| Population | Processing | Model | Analysis |
|------------|------------|-------|----------|
| All        | In progress | Not run | Not started |
| Moms       | In progress | Not run | Not started |
| Offsprings | Complete | model-006-4641589 | Ready to run |

## Data Source

- **Offsprings model**: `/om/scratch/Mon/mabdel03/Moseq/Run_Offsprings/moseq_data/models/model-006-4641589`
- **Processing notebooks**: Copied from `/om/scratch/Mon/mabdel03/Moseq/Run_*/moseq_data/`

## Analysis Notebooks

The analysis notebooks in `Offsprings/code/Analysis/` have been adapted from the 3-27-2025 run with updated file paths:
- `Analyze_Moseq.ipynb` - Main MoSeq analysis
- `Offsprings_Fingerprint_Classifiers.ipynb` - Fingerprint-based classification
- `Female_Offsprings_Classifiers_vMahmoud.ipynb` - Female offspring analysis
- `Male_Offsprings_Classifiers_vMahmoud.ipynb` - Male offspring analysis
- `ID_Reassignment.ipynb` - ID reassignment utilities

## Syllable Usage Analysis Results

### Statistical Methods

Two statistical approaches were used to compare syllable usage between experimental conditions and their controls:

1. **Uncorrected (Mann-Whitney U)**: Raw p-values < 0.05 marked as significant
2. **FDR Corrected (Kruskal-Wallis)**: Benjamini-Hochberg corrected p-values ≤ 0.05 marked as significant

### Males: Syllable Usage Comparisons

#### EE vs. Ctrl-EE/SI
| Syllable | Raw p-value | FDR-corrected p-value |
|----------|-------------|----------------------|
| 6 | 0.0104 | 0.1232 |
| 8 | 0.0374 | 0.1412 |
| 13 | 0.0370 | 0.1412 |
| 15 | 0.0104 | 0.1232 |
| 19 | 0.0250 | 0.1306 |
| 21 | 0.0163 | 0.1232 |
| 23 | 0.0163 | 0.1232 |
| 27 | 0.0104 | 0.1232 |
| 32 | 0.0250 | 0.1306 |
| 34 | 0.0163 | 0.1232 |
| 35 | 0.0374 | 0.1412 |
| 42 | 0.0163 | 0.1232 |
| 44 | 0.0039 | 0.1232 |
| 48 | 0.0250 | 0.1306 |
| 56 | 0.0250 | 0.1306 |
| 59 | 0.0374 | 0.1412 |
| 61 | 0.0104 | 0.1232 |
| 62 | 0.0370 | 0.1412 |

**Summary**: 18 syllables significant (uncorrected), 0 syllables significant (FDR-corrected)

#### SI vs. Ctrl-EE/SI
| Syllable | Raw p-value | FDR-corrected p-value |
|----------|-------------|----------------------|
| 1 | 0.0321 | 0.1456 |
| 6 | 0.0043 | 0.0753 |
| 12 | 0.0101 | 0.0765 |
| 18 | 0.0321 | 0.1456 |
| 22 | 0.0027 | 0.0753 |
| 25 | 0.0321 | 0.1456 |
| 27 | 0.0321 | 0.1456 |
| 30 | 0.0101 | 0.0765 |
| 31 | 0.0101 | 0.0765 |
| 38 | 0.0066 | 0.0753 |
| 39 | 0.0321 | 0.1456 |
| 43 | 0.0066 | 0.0753 |
| 46 | 0.0066 | 0.0753 |
| 49 | 0.0455 | 0.1820 |
| 52 | 0.0319 | 0.1456 |
| 54 | 0.0027 | 0.0753 |
| 56 | 0.0455 | 0.1820 |

**Summary**: 17 syllables significant (uncorrected), 0 syllables significant (FDR-corrected)

#### LNB vs. Ctrl-LNB
| Syllable | Raw p-value | FDR-corrected p-value |
|----------|-------------|----------------------|
| 3 | 0.0127 | 0.5878 |
| 8 | 0.0476 | 0.6141 |
| 20 | 0.0181 | 0.5878 |
| 31 | 0.0350 | 0.6141 |

**Summary**: 4 syllables significant (uncorrected), 0 syllables significant (FDR-corrected)

---

### Females: Syllable Usage Comparisons

#### EE vs. Ctrl-EE/SI
| Syllable | Raw p-value | FDR-corrected p-value |
|----------|-------------|----------------------|
| 2 | 0.0201 | 0.1309 |
| 12 | 0.0098 | 0.1309 |
| 14 | 0.0201 | 0.1309 |
| 15 | 0.0142 | 0.1309 |
| 23 | 0.0045 | 0.1309 |
| 24 | 0.0282 | 0.1666 |
| 27 | 0.0201 | 0.1309 |
| 33 | 0.0201 | 0.1309 |
| 51 | 0.0098 | 0.1309 |
| 59 | 0.0098 | 0.1309 |
| 61 | 0.0201 | 0.1309 |

**Summary**: 11 syllables significant (uncorrected), 0 syllables significant (FDR-corrected)

#### SI vs. Ctrl-EE/SI
| Syllable | Raw p-value | FDR-corrected p-value |
|----------|-------------|----------------------|
| 1 | 0.0045 | 0.0977 |
| 2 | 0.0389 | 0.2297 |
| 10 | 0.0045 | 0.0977 |
| 13 | 0.0282 | 0.2036 |
| 17 | 0.0282 | 0.2036 |
| 22 | 0.0389 | 0.2297 |
| 26 | 0.0098 | 0.1596 |
| 29 | 0.0019 | 0.0977 |
| 31 | 0.0282 | 0.2036 |
| 32 | 0.0282 | 0.2036 |
| 33 | 0.0201 | 0.2036 |

**Summary**: 11 syllables significant (uncorrected), 0 syllables significant (FDR-corrected)

#### LNB vs. Ctrl-LNB
| Syllable | Raw p-value | FDR-corrected p-value |
|----------|-------------|----------------------|
| 6 | 0.0304 | 0.4859 |
| 38 | 0.0167 | 0.4859 |
| 53 | 0.0304 | 0.4859 |
| 60 | 0.0070 | 0.4489 |

**Summary**: 4 syllables significant (uncorrected), 0 syllables significant (FDR-corrected)

---

### Key Findings

- **No syllables survive FDR correction** in any comparison, indicating that while many syllables show nominally significant differences (p < 0.05), these do not remain significant after correcting for multiple comparisons.
- **Males show more uncorrected significant syllables** than females in EE and SI comparisons.
- **LNB comparisons** show fewer significant syllables in both sexes compared to EE and SI comparisons.

## Notes

- Raw session data (depth.dat, rgb.mp4, etc.) is not included in this directory
- Session data remains in the original Run_* directories

## Large Files (Not in Repository)

The following large files are excluded from the repository due to GitHub's 100MB file limit:

| File | Size | Source Location |
|------|------|-----------------|
| `Offsprings/data/moseq_df.csv.gz` | ~227MB | `/om/scratch/Mon/mabdel03/Moseq/Run_Offsprings/moseq_data/models/model-006-4641589/moseq_df.csv` |

To obtain these files, copy from the source location and compress with gzip:
```bash
cp /om/scratch/Mon/mabdel03/Moseq/Run_Offsprings/moseq_data/models/model-006-4641589/moseq_df.csv \
   Offsprings/data/moseq_df.csv
gzip Offsprings/data/moseq_df.csv
```

