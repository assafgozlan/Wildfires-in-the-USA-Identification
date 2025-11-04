# 🌲 Wildfires in the USA — Cause Identification
**Applied Competitive Lab – Final Project, Hebrew University of Jerusalem**

Predicting the **cause of wildfires** across the United States using temporal, geographical, and meteorological data.  
This project demonstrates a **complete data science workflow** — from data cleaning and preprocessing  
to feature engineering, model selection, Bayesian optimization, and evaluation.

---

## 🎯 Project Overview
Wildfires cause devastating damage to the environment, economy, and public safety.  
This project aims to **predict the cause of each wildfire incident** (e.g., lightning, campfire, debris burning, powerline)  
based on spatial, temporal, and meteorological attributes.  

By integrating the official U.S. wildfire database with **weather** and **parks** datasets,  
we explored how human and environmental factors combine to shape fire ignition patterns.

---

## 🧠 Key Insights
- Wildfire causes exhibit **strong spatial–temporal patterns**: human-caused fires peak during weekends and summer months.  
- **Feature aggregation** (regional/yearly statistics) and **domain priors** substantially improved prediction accuracy.  
- **Random Forest** achieved the best performance–interpretability trade-off among tested models.  
- Adding **contextual priors** (probability of cause per region-year) improved generalization and reduced confusion.

---

## 📊 Data Sources
The project integrates three main data sources:

| Source | Description |
|---------|-------------|
| **FPA-FOD (Core Dataset)** | U.S. Fire Program Analysis – Fire Occurrence Database (`FPA_FOD_20170508.sqlite`). |
| **Weather Data** | Hourly/daily meteorological data: temperature, precipitation, wind speed. |
| **Parks Data** | Number of national parks per state (proxy for human–nature interaction). |

All preprocessed data files can be downloaded from  
👉 [Google Drive – Wildfire Project Data](https://drive.google.com/drive/folders/1ZY3o190w7mjXHIr2n_QXgOj1iv4hnlqP?usp=sharing)

| File | Description |
|------|-------------|
| `met_data_clean.pkl` | Cleaned weather measurements. |
| `stations_df_clean.pkl` | Metadata of ~300 U.S. weather stations. |
| `states_parks.csv` | Number of national parks per U.S. state. |
| `units_try.csv` | Example for `NWCG_UnitIDActive_20170109` structure. |

---

## ⚙️ Methodology and Workflow

### 1️⃣ Exploratory Data Analysis (EDA)
- Visualized fire cause distribution by **state**, **month**, **weekday**, and **year**.  
- Generated **geographical heatmaps** showing the spatial concentration of different causes.  
- Explored correlations between weather, human activity, and fire frequency.

### 2️⃣ Data Cleaning & Integration
- Merged wildfire, weather, and parks data by geographic coordinates and timestamps.  
- Interpolated missing weather data and removed incomplete stations.  
- Normalized state names and unit codes for consistency.

### 3️⃣ Feature Engineering
New derived and aggregated features included:
- `fire_size_mean`: Average fire size (acres) per region-year.  
- `fire_gacc_count`: Fire frequency per geographic coordination center (GACC).  
- `fire_unit_count`: Fire frequency per reporting unit.  
- `gcc_mean_year_temp`: Mean yearly temperature per region.  
- `prior_cause_prob`: Probability distribution of causes per `(unit, year)`.

Encoding:
- **Ordinal encoding** for serial categories (e.g., `SOURCE_SYSTEM_TYPE`).  
- **One-hot encoding** for nominal ones (`state`, `agency`, `owner`).

### 4️⃣ Feature Selection
- Removed highly correlated or redundant variables such as `Agency`, `Department`, `Unit_Type`, `Fire_Year`, `Discovery_Date`.  
- Verified through correlation heatmaps and PCA analysis.

### 5️⃣ Model Selection and Training
Models tested:
| Model | Type | Performance | Notes |
|--------|------|-------------|-------|
| Logistic Regression | Linear | ~40% accuracy | Poor for non-linear patterns |
| XGBoost | Tree-based | ~65% accuracy | Good, but slower |
| **Random Forest** | Ensemble | **67–70% accuracy** | **Best trade-off** |

Optimization:
- Bayesian Hyperparameter Optimization for:  
  `n_estimators`, `min_samples_leaf`, `min_samples_split`.  

Evaluation metrics:  
- Accuracy  
- Confusion Matrix  
- Feature Importance  

### 6️⃣ Evaluation & Validation
- Balanced training/test performance confirmed **no overfitting**.  
- Random Forest demonstrated strong generalization and stability.  
- Visualization of confusion matrices highlighted distinct human vs. natural fire patterns.

---

## 📈 Results

| Metric | Value |
|--------|--------|
| **Accuracy (Random Forest)** | **≈ 0.69** |
| **Training Time** | < 1 minute |
| **Overfitting** | None detected |
| **Top Features** | Region-year priors, mean yearly temperature, number of fires per unit |

**Findings:**
- Natural causes dominate hot and dry months.  
- Human causes correlate with populated areas and weekends.  
- Weather and prior features provided the strongest predictive signals.  

Visual results and plots are available in:
📄 [`Applied Competitive Lab Final Project.pdf`](./Applied%20Competitive%20Lab%20Final%20Project.pdf)

---

## 🧰 Technologies Used
- **Programming:** Python 3.10  
- **Libraries:** `Pandas`, `NumPy`, `Matplotlib`, `Seaborn`, `scikit-learn`  
- **Environment:** Jupyter Notebook  
- **Version Control:** Git & GitHub  

---

## 📁 Repository Structure
```text
├── project_190223.ipynb              # Main notebook – full ML pipeline
├── meteoStatTry.ipynb                # Weather data integration & EDA
├── cause_stats.py                    # Script to compute prior features
├── requirements.txt                  # Dependencies
├── Applied Competitive Lab Final Project.pdf   # Final report (EDA + results)
└── README.md                         # This file
