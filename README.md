# 🔋 Global EV Adoption Behavior 2026 — Power BI Dashboard

> An end-to-end data analytics project exploring electric vehicle adoption likelihood across 50,000 global consumers — built with Python, Power BI, and DAX.

---

## 📌 Project status

| Page | Title | Status |
|------|-------|--------|
| Page 1 | Executive Summary | ✅ Complete |
| Page 2 | Demographics & Behavior | ✅ Complete |

---

## 📂 Repository structure

```
global-ev-adoption-powerbi/
│
├── data/
│   ├── global_ev_adoption_behavior_2026.csv   # Raw dataset (50,000 rows)
│   └── ev_adoption_cleaned.csv                # Python-cleaned dataset (ready for Power BI)
│
├── scripts/
│   └── ev_data_cleaning.py                    # Full Python cleaning script
│
├── dashboard/
│   └── EV_Adoption_Dashboard.pbix             # Power BI dashboard file
│
├── docs/
│   └── EV_Dashboard_Complete_Guide.pdf        # Step-by-step build guide (all 5 phases)
│
└── README.md
```

---

## 📊 Dataset overview

| Property | Value |
|----------|-------|
| Source | Synthetic behavioral dataset — 2026 |
| Rows | 50,000 consumer records |
| Columns | 23 features |
| Target variable | `ev_adoption_likelihood` (Low / Medium / High) |
| Known issues | Negative fuel expenses, ~500 nulls in 2 columns |

### Column categories

| Category | Columns |
|----------|---------|
| Demographics | `age`, `annual_income`, `education_level`, `city_type` |
| Travel behavior | `daily_commute_km`, `weekly_travel_distance_km`, `current_vehicle_type`, `vehicle_age_years` |
| Infrastructure | `charging_station_accessibility`, `nearest_charging_station_km`, `home_charging_available` |
| Financials | `fuel_expense_per_month`, `electricity_cost_per_kwh`, `monthly_charging_cost`, `monthly_energy_consumption_kwh` |
| Attitudes | `environmental_awareness_score`, `government_incentive_awareness`, `technology_affinity_score`, `range_anxiety_score`, `battery_replacement_concern`, `ev_knowledge_score` |
| Experience | `previous_ev_experience` |

---

## 🐍 Python data cleaning

Run the cleaning script before loading data into Power BI:

```bash
pip install pandas numpy
python scripts/ev_data_cleaning.py
```

### What the script fixes

| Step | Issue | Fix applied |
|------|-------|-------------|
| 1 | Negative `fuel_expense_per_month` values | Replaced with `NaN`, filled with median |
| 2 | ~500 nulls in `charging_station_accessibility` | Filled with column median (5.9) |
| 3 | ~500 nulls in `ev_knowledge_score` | Filled with column median (7.0) |
| 4 | Binary 0/1 columns | Converted to `Yes` / `No` text |
| 5 | No age segmentation | Added `Age_Group` column (21–29, 30–39, etc.) |
| 6 | No income segmentation | Added `Income_Segment` column (Low / Middle / Upper Middle / High) |
| 7 | No savings column | Added `Potential_Monthly_Savings` = fuel cost − charging cost |
| 8 | Alphabetical chart sorting | Added `Adoption_Sort_Order` (Low=1, Medium=2, High=3) |

### Output

The script exports `ev_adoption_cleaned.csv` with 29 columns (23 original + 6 derived). Load this file into Power BI — all cleaning is already done.

---

## 📈 Dashboard pages

### ✅ Page 1 — Executive Summary

**Purpose:** High-level KPIs for stakeholders to understand the adoption landscape at a glance.

**Visuals:**
- 4 KPI Cards — Total Respondents · % High Adoption · Home Charging % · Avg Annual Income
- Donut chart — Adoption breakdown (High 59.3% / Medium 24.2% / Low 16.5%)
- Clustered bar — Adoption likelihood by city type
- Clustered bar — Adoption likelihood by education level
- Smart Narrative — Auto-generated text summary

**Slicers:** City type (dropdown) · Education level (tile) · Vehicle type (dropdown) · Age range (slider)

**Key finding:** Urban residents show 63% High adoption vs 47% for Rural — a 16-point gap driven by infrastructure access.

---

### ✅ Page 2 — Demographics & Behavior

**Purpose:** Understand which demographic profiles are most likely to adopt EVs and why.

**Visuals:**
- Column chart — Age group distribution by adoption likelihood
- Scatter plot — Annual income vs EV knowledge score (colored by adoption)
- Stacked bar — Vehicle type by adoption likelihood
- Matrix table — Avg commute, income, fuel expense by education level (with conditional formatting)
- Bookmark toggle — Switch between chart view and table view

**Slicers:** Adoption likelihood (tile) · City type (dropdown) · Income segment (tile)

**Key finding:** 30–39 age group shows highest concentration of High adopters. PhD-educated respondents earn 2.2× more than High School graduates and show stronger adoption intent.

---

## 🧮 DAX measures

All measures are stored in a dedicated `_Measures` table inside the Power BI model.

```dax
% High Adoption =
DIVIDE(
    COUNTROWS(FILTER('ev_adoption_cleaned', 'ev_adoption_cleaned'[ev_adoption_likelihood] = "High")),
    COUNTROWS('ev_adoption_cleaned')
)

Home Charging % =
DIVIDE(
    COUNTROWS(FILTER('ev_adoption_cleaned', 'ev_adoption_cleaned'[home_charging_available] = 1)),
    COUNTROWS('ev_adoption_cleaned')
)

Avg Monthly Savings =
AVERAGE('ev_adoption_cleaned'[fuel_expense_per_month])
    - AVERAGE('ev_adoption_cleaned'[monthly_charging_cost])

Age_Group =
SWITCH(TRUE(),
    'ev_adoption_cleaned'[age] < 30, "21-29",
    'ev_adoption_cleaned'[age] < 40, "30-39",
    'ev_adoption_cleaned'[age] < 50, "40-49",
    'ev_adoption_cleaned'[age] < 60, "50-59",
    "60+"
)
```

---

## ✅ Data validation reference

After loading into Power BI, verify these distributions match:

| Column | Expected |
|--------|----------|
| `ev_adoption_likelihood` = High | 59.3% |
| `ev_adoption_likelihood` = Medium | 24.2% |
| `ev_adoption_likelihood` = Low | 16.5% |
| `city_type` = Urban | 45.3% |
| `city_type` = Suburban | 34.7% |
| `city_type` = Rural | 20.0% |
| `home_charging_available` = Yes | ~65% |
| `previous_ev_experience` = Yes | ~20% |
| Total rows after cleaning | 50,000 |

---

## 🛠️ Tools used

| Tool | Purpose |
|------|---------|
| Python 3.x | Data cleaning and feature engineering |
| pandas / numpy | Data manipulation |
| Power BI Desktop | Dashboard development |
| DAX | Custom measures and calculated columns |
| Power Query (M) | Data transformation (if using raw CSV) |

---

## 💡 Key business insights (Pages 1 & 2)

1. **59.3% of respondents** are High EV adoption candidates — a large addressable market.
2. **Urban residents** adopt at 16 percentage points higher than Rural — infrastructure drives adoption.
3. **30–39 age group** is the primary High adoption segment — prime target for EV marketing.
4. **Higher income + higher EV knowledge** strongly predicts High adoption likelihood (visible in scatter plot).
5. **PhD and Master's educated respondents** show the strongest adoption intent and highest average income.
6. **Sedan owners** are the largest single vehicle type group and show strong High adoption lean.

---

## 🚀 How to run this project

1. Clone this repository
   ```bash
   git clone https://github.com/your-username/global-ev-adoption-powerbi.git
   cd global-ev-adoption-powerbi
   ```

2. Install Python dependencies
   ```bash
   pip install pandas numpy
   ```

3. Run the cleaning script
   ```bash
   python scripts/ev_data_cleaning.py
   ```

4. Open Power BI Desktop and load `data/ev_adoption_cleaned.csv`

5. Open `dashboard/EV_Adoption_Dashboard.pbix` to view the dashboard

---

## 📬 Contact

Built by — **[Aniket Patil]**
- LinkedIn: [https://linkedin.com/in/aniket-patil-10b503330]
- Portfolio: [(https://github.com/aniketrpatil0708)]

---

## 📄 License

This project is for educational and portfolio purposes. Dataset is synthetic.
