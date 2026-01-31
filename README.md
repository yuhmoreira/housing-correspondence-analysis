# 🏠 Housing Market Correspondence Analysis (CA & MCA)

Statistical exploration of a housing dataset using **Correspondence Analysis (CA)** and **Multiple Correspondence Analysis (MCA)** to uncover hidden patterns between **price**, **area**, and **amenities** such as **air conditioning**, **hot water heating**, **furnishing status**, and others.

The project combines:
- Chi-Square tests + Cramér’s V for **feature selection**
- **Jenks Natural Breaks** for optimal binning of continuous variables
- 2D and 3D **perceptual maps** (including animation) to visualize categorical associations

---

## 📊 Main Analyses

### 1. Chi-Square Tests & Cramér’s V

Before running CA/MCA, we:
- Test independence between the target variable (e.g. `price_bin`) and each categorical feature
- Use **p-value** to check statistical significance
- Use **Cramér’s V** to measure **strength of association**

This avoids including variables that are either:
- Not associated with price at all, or
- Statistically significant but with **negligible effect size**

> **Key idea:** p-value tells us *“is there an association?”*  
> Cramér’s V tells us *“how strong is it?”*

---

### 2. Binning with Jenks Natural Breaks

Continuous variables like **price** and **area** are transformed into categories using **Jenks Natural Breaks** (`jenkspy`), which:
- Minimizes variance **within** groups
- Maximizes variance **between** groups
- Finds **natural groupings** in skewed real‑world distributions

This produces bins such as:

- `price_bin`: `low`, `medium`, `high`, `very_high`  
- `area_bin`: `small`, `medium`, `large`, `very_large`

These bins are more faithful to the data than equal-width or equal-frequency cuts and are ideal for CA/MCA.

---

### 3. Simple Correspondence Analysis (CA)

We first run **CA** using only:

- `price_bin`  
- `area_bin`

Results:
- The first two dimensions explain **≈ 100% of inertia**, so a 2D map is almost a perfect summary.
- Clear gradient:  
  - `low` ↔ `small`  
  - `high` / `very_high` ↔ `large` / `very_large`

This confirms the intuitive but now statistically supported idea: **larger houses are generally more expensive**, but the very top price tier starts to show its own distinct behaviour.

---

### 4. Multiple Correspondence Analysis (MCA)

Next, we add a third variable:

- `airconditioning` (`yes` / `no`)

With **MCA**, we now analyse the joint structure of:
- `price_bin`
- `area_bin`
- `airconditioning`

2D MCA map:
- The first two dimensions explain a **smaller share of inertia** (e.g. ~44%), because the information is spread across more dimensions.
- We begin to see that **air conditioning** is strongly aligned with **high price** and **larger areas**, while `low` + `small` tend to be associated with `no` air conditioning.

3D MCA map (with rotation):
- By using **three dimensions**, we recover more of the lost inertia and see that:
  - `very_large` and `very_high` form particularly distinctive profiles.
  - The “standard” segment (`low`, `small`, `no`, `medium`) lies close to the “floor” of the space,
    while premium categories “lift off” into different directions.

---

## 🧪 Methods & Libraries

- **Python**: data handling and analysis
- **pandas / numpy**: data manipulation
- **scipy.stats**: chi-square tests
- **Cramér’s V**: custom calculation from chi-square
- **jenkspy**: Jenks Natural Breaks binning
- **prince**: CA and MCA implementation
- **seaborn / matplotlib**: 2D and 3D visualizations (including animation)

---

## 📁 Project Structure

```text
.
├── data/
│   └── Housing.csv                  # Original dataset (download from Kaggle)
├── notebooks/
│   ├── Correspondence_Analysis_Housing.ipynb
├── outputs/
│   ├── ca_2d_perceptual_map.png
│   ├── mca_2d_perceptual_map.png
│   └── mca_3d_perceptual_map.gif
├── requirements.txt
├── README.md
└── LICENSE
