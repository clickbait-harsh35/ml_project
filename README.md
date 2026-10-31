# TB Case-Notification Gaps: Profiling States/UTs for the TB Elimination Programme

**Author:** Harsh Kumar (Roll No. 24020570056)
**Theme:** Health | SDG 3 (Good Health and Well-being), SDG 10 (Reduced Inequalities)
**Methods:** Clustering (k-means, hierarchical, PCA) + Regression (OLS, Ridge)

## Aim
India carries the world's largest tuberculosis burden, and missed or late notification keeps transmission going. This case study (1) compares each State/UT's notified TB cases with its target and treatment initiation, (2) clusters States by notification performance, and (3) tests whether notification rates relate to rural health-infrastructure indicators.

## Data
| File | Content | Source |
|---|---|---|
| `RS_Session_257_AU_279_1.csv` | State/UT-wise TB notification, target and treatment initiation, 2021 and Jan-May 2022 | Ministry of Health and Family Welfare (Ni-kshay) via OGD Platform India |
| `population_census2011.csv` | Census 2011 population by State/UT (denominator for rates per lakh) | Census of India |
| `infrastructure.csv` | Rural Sub-Centres, PHCs, CHCs and PHC doctors in position, per lakh rural population | Rural Health Statistics 2021-22, MoHFW |

## Method
- **Measures per State/UT:** notification rate per lakh (2022 annualised x12/5, because it covers only five months), percentage of target achieved, and percentage of net patients initiated on treatment.
- **Outliers handled separately:** Delhi, Chandigarh (referral hubs that notify patients from neighbouring States) and Lakshadweep (20 cases, very small population).
- **Clustering:** standardised features, k-means and Ward hierarchical clustering, k chosen using SSE (elbow) and silhouette score, PCA to 2-D for plotting.
- **Regression:** 2021 notification rate on four infrastructure indicators (SC, PHC, CHC and PHC doctors per lakh rural population), with OLS and Ridge, evaluated by leave-one-out R² and RMSE against a predict-the-mean baseline.

## Key results
**Clusters (k = 3, 33 States/UTs; silhouette 0.29 for k-means, 0.25 for Ward, adjusted Rand index 0.72):**

| Cluster | Rate per lakh (2021) | Target achieved (2022) | Treatment initiated | States |
|---|---|---|---|---|
| Low notifiers | 110 | 81% | 95% | 14, including Assam, Bihar, Karnataka, Tamil Nadu, West Bengal, Kerala |
| Steady performers | 184 | 95% | 97% | 11, including Andhra Pradesh, Gujarat, Madhya Pradesh, Odisha, Telangana |
| High notifiers, weaker follow-through | 222 | 88% | 93% | 8: Uttar Pradesh, Rajasthan, Maharashtra, Haryana, Punjab, Uttarakhand, Arunachal Pradesh, Puducherry |

**Regression:** rural health infrastructure did **not** predict notification rate. Leave-one-out R² was between -0.13 and 0.01 (RMSE about 57-61 against a baseline of 57.2), so the models do no better than predicting the average. States notifying far less than their infrastructure would suggest (J&K, Tripura, West Bengal, Kerala, Manipur, Karnataka) are candidates for case-finding attention; this is a hypothesis, not a conclusion.

## Limitations
- Notification rate is not incidence; a low rate may reflect fewer cases or more missed cases.
- 2022 covers only January-May; there are only two periods.
- Silhouette scores are low, so the clusters describe the data but are not sharply separated.
- Rates use Census 2011 populations, so absolute levels are overstated (rankings are unaffected).
- Some rural populations are small or reclassified (Kerala, Goa, Sikkim, Mizoram), which inflates per-lakh infrastructure figures.
- State-level (ecological) analysis with 33 observations; the outcome covers all cases but predictors describe rural facilities only.

## How to run
```bash
pip install -r requirements.txt
jupyter notebook TB_notification_clustering.ipynb
```
Keep the CSV files in the same folder as the notebook (or update the file paths).

## Files
- `TB_notification_clustering.ipynb`: full analysis with outputs
- `state_clusters.csv`: features and cluster label per State/UT (-1 = excluded special case)
- `fig_k_selection.png`, `fig_pca_clusters.png`, `fig_dendrogram.png`: figures

## Licence and sources
Data: Government Open Data Licence - India (GODL-India). Sources: Ni-kshay via OGD Platform India; Rural Health Statistics 2021-22 (MoHFW); Census of India 2011.
