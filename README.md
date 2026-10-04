# PRABASI: A Multi-Decadal Dataset of Socio-Economic Disparities and Interstate Migration in India


This repository contains the anonymized version of the PRABASI dataset prepared for submission to ACM-AI Letters.

PRABASI is a multi-decadal dataset integrating interstate migration observations with state-level socio-economic, demographic, health, education, and related indicators for India across multiple Census periods.

The dataset is intended to support computational and machine-learning-based analysis of interstate migration patterns and socio-economic disparities.


## Repository Contents

The structure of this repository is as follows:

```text
PRABASI/
├── README.md
├── DATA_CARD.md
├── CITATION.cff
│
├── code/
│   └── distance_calculator.py
│   └── data_loader.py
│   └── feature_engineering.py
│   └── temporal_holdout.py
│   └── spatial_holdout.py
│
├── data/
│   └── PRABASI.csv
│
├── metadata/
│   ├── schema.csv
│   ├── state_codes.csv
│   └── source_provenance.csv
│
└── documentation/
    ├── methodology.md
    └── limitations.md
    └── state_name_correction.csv

```
