# One Label, Different Air

**Does one AQI category hide different pollution mixes across cities?**

ANLY 530 Machine Learning, Harrisburg University. KA1 conference-paper project (individual). Author: Rama Aneesh Tumuluri.

## Research questions

- **RQ1.** Do cities that share an AQI category differ in their mix of pollutant sub-indices (CO, ozone, NO2, PM2.5)?
- **RQ2.** How sensitive are the pollution profiles to scaling, clustering algorithm and random seed?

The study is unsupervised (clustering). There is no target label. AQI Category is used only after fitting, to describe the clusters.

## Data

- **Dataset:** Global Air Pollution Dataset, Kaggle (Al Muzdadid, n.d.): https://www.kaggle.com/datasets/hasibalmuzdadid/global-air-pollution-dataset
- **Source:** per the Kaggle page, scraped from elichens.com and lightly preprocessed by hand. The licence is not clearly stated.
- **The CSV is not included in this repository.** Download it from Kaggle and save it as:

```
data/global_air_pollution_dataset.csv
```

- **Unit:** one city (23,463 cities, 175 countries), measured once (cross-sectional).
- **Clustering inputs:** `CO AQI Value`, `Ozone AQI Value`, `NO2 AQI Value`, `PM2.5 AQI Value`.

## Repository structure

```
.
├── README.md
├── requirements.txt
├── CONTRIBUTION.md
├── .gitignore
├── data/                      # put the Kaggle CSV here (not tracked by git)
└── ANLY530_Air_Pollution_Profiles_Combined.ipynb
```

## How to reproduce

```bash
git clone https://github.com/rama17aneesh/Anly-air-pollution-profiles.git
cd Anly-air-pollution-profiles
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
# download the CSV into data/ (see above), then:
jupyter notebook ANLY530_Air_Pollution_Profiles_Combined.ipynb
```

Tested with Python 3.10+. Each notebook runs in a few minutes on a laptop or a free Kaggle CPU session. All randomness uses seed 42 unless a stability test varies it. Notebook 01 also runs on Kaggle: https://www.kaggle.com/code/ramatum/week-10

## Exploratory results (Week 10)

| Check | Result |
|---|---|
| Elbow (WCSS) | bend at k = 3 |
| Silhouette (1,500-city sample) | k = 2: 0.620, k = 3: 0.615 |
| Ward vs k-means (same sample, k = 3) | ARI 0.705 |
| Stability across seeds 0 to 4 | ARI ≥ 0.996 |
| Stability on 80% subsamples | ARI ≥ 0.976 |
| Robust scaling / no scaling vs standard | ARI 0.921 / 0.525 |

Three exploratory profiles (k = 3): a lower PM2.5-led majority (86%), high PM2.5 with NO2 and CO (7.5%), and high PM2.5 with ozone (6.5%). Among Moderate cities, those in the NO2 profile have about seven times the NO2 sub-index of other Moderate cities.

## k = 2 vs k = 3 and cluster-wise stability (notebook 02)

| Check | Result |
|---|---|
| k = 2 "high pollution" cluster | silhouette 0.02 (mixes NO2-led and ozone-led cities) |
| k = 3 NO2-led / ozone-led clusters | silhouette 0.24 / 0.33 |
| Moderate cities separated as NO2-heavy | k = 2: 156, k = 3: 725 |
| Jaccard, seeds / bootstrap / 80% subsamples (worst run, all clusters) | ≥ 0.90 |
| NO2-led profile under robust / min-max / no scaling | 0.85 / 0.32 / 0.19 |
| Rows where overall AQI exceeds all four sub-indices | 141 (126 India), a sign of unrecorded PM10 or SO2 |

Cluster-wise stability follows Hennig (2007). The NO2-led profile is repeatable but depends on spread-based scaling (standard or robust).

These are pollution profiles, not health-risk groups. The data has no health outcomes.

## Limitations

One reading per city at an unknown time, different local times across continents, undocumented collection method, no PM10 or SO2, sub-indices capped at 500, cities only (urban vs rural unknown).

## AI Use

I used Claude, an AI assistant, as a text and coding assistant.
