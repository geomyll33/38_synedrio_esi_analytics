# Multivariate Water-Network Anomaly Detection — EYDAP Open Data

Αναπαραγώγιμο pipeline ανάλυσης για την εργασία **«Ανίχνευση Ανωμαλιών στο Αστικό
Σύστημα Υδροδότησης με Χρήση Πολυμεταβλητών Χρονοσειρών»**, Γεώργιος Μυλλής &
Αλκιβιάδης Τσιμπίρης, 38ο Πανελλήνιο & 4ο Διεθνές Συνέδριο Στατιστικής (ΕΣΙ 2026).

Reproducible analysis pipeline for the paper *"Anomaly Detection in the Urban
Water Supply System Using Multivariate Time Series"*, submitted to the 38th
Panhellenic & 4th International Conference of the Greek Statistical Institute
(ESI 2026).

## Τι κάνει / What it does

Ενοποιεί επτά ετερογενή ανοικτά σύνολα δεδομένων της ΕΥΔΑΠ (κατανάλωση, νέες
συνδέσεις, αποθέματα ταμιευτήρων, παραγωγή πόσιμου νερού, διακοπές υδροδότησης,
χλώριο, θολότητα) σε ενιαίο μηνιαίο πίνακα, και εφαρμόζει:

- Ανάλυση συσχετίσεων Spearman
- Αποσύνθεση χρονοσειρών STL + τυπικούς ελέγχους στασιμότητας (ADF, KPSS)
- Πολυμεταβλητή ανίχνευση ανωμαλιών (Isolation Forest, απόσταση Mahalanobis) σε
  10 επιλεγμένες μεταβλητές (μέσω διαγνωστικού ελέγχου Spearman, ρ>0.85 → αφαίρεση
  πλεονασμού)
- Συγκριτική ομαδοποίηση χρονοσειρών με 4 μεθόδους (k-means, ιεραρχική/Ward,
  DTW, PCA/feature-based), σε επίπεδο μηνών και ταχυδρομικών κωδίκων
- Δείκτες συμφωνίας μεθόδων ομαδοποίησης (Adjusted Rand Index, Normalized
  Mutual Information)

Integrates seven heterogeneous EYDAP open datasets into a unified monthly
table and applies correlation analysis, STL decomposition with formal
stationarity testing (ADF, KPSS), multivariate anomaly detection (Isolation
Forest, Mahalanobis distance) on 10 variables selected via Spearman-based
redundancy screening, and comparative time-series clustering (four methods,
two analysis levels), with quantified agreement metrics (ARI, NMI) between
clustering methods.

## Βασικές παράμετροι / Key parameters

| Στοιχείο | Τιμή |
|---|---|
| Μεταβλητές ανίχνευσης ανωμαλιών | 10 (επιλεγμένες μέσω Spearman, ρ>0.85 → αφαίρεση πλεονασμού) |
| Isolation Forest | `n_estimators=100, contamination=0.1, random_state=42` |
| Mahalanobis | Δειγματικός πίνακας συνδιακύμανσης, κατώφλι **DM > 4** (≈ 90ό εκατοστημόριο του χ²(df=10), τυποποιημένος κανόνας) |
| Ομαδοποίηση μηνών | k=2 (silhouette 0.360) |
| Ομαδοποίηση ΤΚ | k=2 (silhouette 0.239) |

## Δομή repository / Repository structure

```
.
├── data/                    # 7 πρωτογενή ανοικτά σύνολα δεδομένων ΕΥΔΑΠ (raw inputs)
├── notebooks/
│   └── 38_synedrio_esi_analytics_FINAL.ipynb   # πλήρες, εκτελέσιμο pipeline
├── results/                 # έξοδοι του notebook (CSV)
├── figures/                 # γραφήματα (STL από το notebook· τα υπόλοιπα είναι
│                             #  συμπληρωματικά γραφήματα του άρθρου, βλ. παρακάτω)
├── requirements.txt
├── LICENSE
└── README.md
```

## Πώς να το τρέξεις / How to run

```bash
git clone https://github.com/geomyll33/38_synedrio_esi_analytics.git
cd 38_synedrio_esi_analytics
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python -m ipykernel install --user --name python3
jupyter notebook notebooks/38_synedrio_esi_analytics_FINAL.ipynb
```

Τρέξτε τα κελιά με τη σειρά («Run All»). Το notebook διαβάζει τα δεδομένα από
`../data/` και γράφει αποτελέσματα στον φάκελο `../results/` και τα γραφήματα
STL στον φάκελο `../figures/`.

Run all cells in order ("Run All"). The notebook reads data from `../data/`
and writes results to `../results/` and STL figures to `../figures/`.

## Γραφήματα / Figures

Τα `figures/stl_*.png` παράγονται απευθείας από το notebook. Τα υπόλοιπα
γραφήματα στον φάκελο (`pca_monthly_anomalies.png`, `profile_cluster_month_kmeans.png`,
`regime_radar_summary.png`) είναι συμπληρωματικά γραφήματα που χρησιμοποιούνται
στο άρθρο, χτισμένα πάνω στις εξόδους του notebook (`results/monthly_results.csv`).

The `figures/stl_*.png` files are produced directly by the notebook. The
remaining figures are supplementary paper figures built on top of the
notebook's outputs (`results/monthly_results.csv`).

## Δεδομένα / Data

Πηγή: [Ανοικτά Δεδομένα ΕΥΔΑΠ](https://opendata.eydap.gr/) (Ιανουάριος 2023 –
Ιανουάριος 2026). Source: EYDAP Open Data portal.

## Αναφορά / Citation

Εάν χρησιμοποιήσετε αυτό το πλαίσιο, παρακαλούμε αναφέρετε:

Myllis, G. & Tsimpiris, A. (2026). *Ανίχνευση Ανωμαλιών στο Αστικό Σύστημα
Υδροδότησης με Χρήση Πολυμεταβλητών Χρονοσειρών*. 38ο Πανελλήνιο & 4ο Διεθνές
Συνέδριο Στατιστικής (ΕΣΙ 2026), Διεθνές Πανεπιστήμιο Ελλάδος.

## Άδεια χρήσης / License

MIT — βλ. [LICENSE](LICENSE).
