# RT-IoT2022 - Anomaly Detection on IoT Network Traffic

The goal is to detect malicious traffic in an IoT network using **anomaly detection**. The models are trained **only on normal traffic** and must recognize as anomalous everything that deviates from it. Three models are compared:

- **One-Class SVM**
- **Isolation Forest**
- **Autoencoder**

## Dataset

The project uses the **RT-IoT2022** dataset (UCI Machine Learning Repository):
https://archive.ics.uci.edu/dataset/942/rt-iot2022

- 123117 network flows, 83 input features, 12 traffic classes
- **Normal traffic** (3 classes): `MQTT_Publish`, `Thing_Speak`, `Wipro_bulb`
- **Anomalous traffic** (9 attack classes): `DOS_SYN_Hping`, `ARP_poisoning`, `DDOS_Slowloris`, `Metasploit_Brute_Force_SSH` and five NMAP scans.

The dataset is **not included** in this repository. Download it and place the CSV file here:

```
data/raw/RT_IOT2022.csv
```

## Repository structure

```
RT-IoT2022-Anomaly-Detection/
├── data/
│   ├── raw/                 
│   └── processed/           
├── notebooks/
│   ├── 01_EDA.ipynb
│   ├── 02_preprocessing.ipynb
│   ├── 03_one_class_svm.ipynb
│   ├── 04_isolation_forest.ipynb
│   ├── 05_autoencoder.ipynb
│   └── 06_model_comparison.ipynb
├── results/
│   ├── tables/              
│   ├── figures/             
│   └── models/               
├── requirements.txt
└── README.md
```

## How to run

Run the notebooks **in order** from the notebooks folder:

| # | Notebook | What it does |
|---|---|---|
| 01 | `01_EDA.ipynb` | Exploratory analysis of the dataset |
| 02 | `02_preprocessing.ipynb` | Cleaning, data split, encoding and scaling. Saves the processed sets in `data/processed/` |
| 03 | `03_one_class_svm.ipynb` | One-Class SVM: tuning of `nu` and `gamma`, test evaluation |
| 04 | `04_isolation_forest.ipynb` | Isolation Forest: tuning of `contamination` and `n_estimators`, test evaluation |
| 05 | `05_autoencoder.ipynb` | Autoencoder: threshold and latent space analysis, test evaluation |
| 06 | `06_model_comparison.ipynb` | Final comparison of the three models |

Notebook 02 must be run before notebooks 03-05, and notebooks 03-05 must be run before notebook 06, which reads their results from `results/tables/`.

All random operations use a fixed seed (`42`), so the results are reproducible.

## Methodology

**Preprocessing**

- The index column and 5195 duplicated rows are removed **before** the split.
- Categorical features (`proto`, `service`) are one-hot encoded and numerical features are standardized. Encoder and scaler are fitted **only on the training set**, to avoid data leakage.
- Each sample has 92 features after preprocessing