# FPL Player Clustering Analysis

Unsupervised segmentation of Fantasy Premier League (FPL) players into performance-based
groups using PCA, K-Means, and hierarchical clustering.

## Contents

- `sports_clustering_analysis.ipynb` — the full analysis notebook
- `fpl_data.csv` — the dataset (see source below)

## Data Source

[Fantasy Premier League - Dataset](https://www.kaggle.com/datasets/ritviyer/fantasy-premier-league-dataset)
on Kaggle, by Ritvi Iyer. Refer to the Kaggle page for the dataset's license and terms of use
before redistributing it.

**Shape:** 476 players × 13 columns

| Column | Type | Description |
|---|---|---|
| `Player_Name` | text | Player's name |
| `Club` | text | Premier League club (17 unique clubs in this snapshot) |
| `Position` | text | `Goalkeeper`, `Defender`, `Midfielder`, or `Forward` |
| `Goals_Scored` | int | Goals scored in the season |
| `Assists` | int | Assists in the season |
| `Total_Points` | int | Total FPL points earned |
| `Minutes` | int | Minutes played |
| `Goals_Conceded` | int | Goals conceded while on the pitch |
| `Creativity` | float | FPL creativity index |
| `Influence` | float | FPL influence index |
| `Threat` | int | FPL threat index |
| `Bonus` | int | Bonus points earned |
| `Clean_Sheets` | int | Clean sheets |

Position breakdown: 195 midfielders, 172 defenders, 64 forwards, 45 goalkeepers.

## Method

1. **Preprocessing** — numeric performance columns are standardized (`StandardScaler`).
2. **Dimensionality reduction** — PCA reduces the standardized features to 3 components
   for clustering and visualization.
3. **Choosing k** — inertia (elbow method) and silhouette score are computed for
   k = 2–10 on the PCA-reduced data.
4. **K-Means clustering** — players are grouped using K-Means at the chosen k.
5. **Hierarchical clustering** — agglomerative clustering (Ward linkage) is run as a
   cross-check, including a dendrogram and a comparison against the K-Means labels.
6. **Profiling** — cluster sizes, feature means, and top differentiating stats per
   cluster are used to name and interpret each group (e.g. "Attacking Dynamo",
   "Workhorse Defender").

## Requirements

```
pandas
numpy
matplotlib
seaborn
scikit-learn
scipy
```

## Running

Open `sports_clustering_analysis.ipynb` in Jupyter (or JupyterLab / VS Code) with
`fpl_data.csv` in the same directory, and run all cells top to bottom.

## Notes on Choosing k

Silhouette score is slightly higher at k=2 (~0.52) than k=3 (~0.51), but the difference
is small and silhouette scores are close across k=2–6. k=3 was used because:
- inertia drops sharply from k=2 to k=3 and then flattens (a clear elbow at 3), and
- the resulting groups map onto distinct, football-meaningful roles (attackers,
  high-minute defenders, defensively-influential players) rather than the coarser
  "attackers vs. everyone else" split k=2 tends to produce.

This is a judgment call, not a hard rule — the notebook's "Choosing the Number of
Clusters" section has everything needed to re-evaluate with a different k.
