
---

## 🛠️ Technologies Used

| Tool / Library   | Version  | Purpose                                        |
|------------------|----------|------------------------------------------------|
| **Python**       | 3.x      | Core programming language                      |
| **Pandas**       | Latest   | Data loading, cleaning, feature engineering    |
| **NumPy**        | Latest   | Numerical operations and array handling        |
| **Matplotlib**   | Latest   | Bar charts, box plots, radar/spider charts     |
| **Seaborn**      | Latest   | Statistical visualizations (box plots)         |
| **Google Colab** | —        | Cloud-based notebook execution environment     |
| **Google Drive** | —        | Dataset storage and mounting                   |
| **openpyxl**     | Latest   | Reading `.xlsx` Excel files                    |

---

## 🔬 Methodology

### Step 1 — Data Loading & Inspection
- Loaded the Excel file using `pd.read_excel()` with `parse_dates=['Date']`
- Inspected shape, dtypes, null counts, and first 10 rows
- Generated descriptive statistics for all numerical columns

### Step 2 — Data Cleaning
- Identified missing values across PM2.5, PM10, NOx, NH3, CO, AQI columns
- Applied **median imputation per city** to preserve city-specific distributions
- Validated AQIBucket labels for consistency

### Step 3 — Feature Engineering
Extracted time-based features from the `Date` column:

| New Column   | Description                              |
|--------------|------------------------------------------|
| `Year`       | Calendar year of measurement             |
| `Month`      | Month number (1–12)                      |
| `MonthName`  | Month name (Jan, Feb, … Dec)             |
| `Season`     | Summer / Autumn / Winter / Spring        |

Season mapping:
- **Summer** — March, April, May, June
- **Monsoon/Autumn** — July, August, September, October
- **Winter** — November, December, January, February

### Step 4 — City-wise AQI Summary
Computed `mean`, `median`, `max`, `min`, and `std deviation` of AQI grouped by city.

### Step 5 — Pollutant Correlation Analysis
Calculated **Pearson correlation coefficients** between each pollutant and AQI to identify the strongest drivers of air quality degradation.

### Step 6 — Dominant Pollutant Identification
Normalised all pollutant columns per city to determine which pollutant contributes most relative to its typical range.

### Step 7 — Seasonal AQI Analysis
Grouped data by `Season` to compute mean, median, min, and max AQI — revealing how air quality fluctuates across seasons.

### Step 8 — Radar (Spider) Chart
Built a multi-axis radar chart using normalised pollutant values to visually compare the pollutant fingerprint of all 5 cities simultaneously.

---

## 📈 Analysis & Results

### 🏙️ City-wise AQI Summary

| City        | Mean AQI | Median AQI | Max AQI | Min AQI | Std Dev |
|-------------|----------|------------|---------|---------|---------|
| **Delhi**   | 316.60   | 333.0      | 415.0   | 233.0   | 71.54   |
| **Gurugram**| 261.00   | 244.0      | 442.0   | 138.0   | 101.94  |
| **Kolkata** | 164.60   | 148.0      | 226.0   | 94.0    | 54.83   |
| **Hyderabad**| 146.83  | 138.0      | 252.0   | 37.0    | 84.19   |
| **Bengaluru**| 68.50   | 67.5       | 91.0    | 49.0    | 16.66   |

> Delhi and Gurugram consistently fall in the **Poor to Very Poor** AQI range. Bengaluru remains the **cleanest city** in this dataset with minimal variance.

### 🔗 Pollutant Correlation with AQI

| Pollutant | Pearson r | Strength          |
|-----------|-----------|-------------------|
| PM10      | **0.79**  | Strong positive   |
| PM2.5     | 0.74      | Strong positive   |
| NO2       | 0.68      | Moderate positive |
| NOx       | 0.65      | Moderate positive |
| NO        | 0.61      | Moderate positive |
| CO        | 0.57      | Moderate positive |
| SO2       | 0.49      | Moderate positive |
| NH3       | 0.38      | Weak positive     |
| O3        | 0.22      | Weak positive     |

> **PM10** is the single strongest predictor of AQI — a 1-unit increase in PM10 is more associated with AQI rise than any other pollutant.

### 📅 Seasonal AQI Patterns

| Season   | Mean AQI | Observations                               |
|----------|----------|--------------------------------------------|
| Winter   | ~280     | Highest — temperature inversion traps pollutants near surface |
| Autumn   | ~210     | Moderate — post-monsoon dry conditions increase dust |
| Summer   | ~145     | Moderate — wind disperses pollutants       |
| Spring   | ~110     | Lowest — improved air circulation          |

---

## 🔑 Key Findings

1. 🏙️ **Delhi is the most polluted city** — with a mean AQI of **316.6**, it consistently falls in the *Very Poor* range, posing serious health risks.
2. 🌿 **Bengaluru has the cleanest air** — mean AQI of **68.5**, mostly in the *Good–Satisfactory* range with very low variance (std = 16.66).
3. 💨 **PM10 is the dominant AQI driver** — highest Pearson correlation (r = 0.79), followed closely by PM2.5 (r = 0.74).
4. ⚠️ **51.9% of sampled days** recorded unhealthy AQI values (Poor / Very Poor / Severe).
5. ✅ Only **29.6% of days** fell in the healthy range (Good / Satisfactory).
6. ❄️ **Winter months are the worst** — due to temperature inversions that trap pollutants near the ground.
7. 📍 **Gurugram recorded the highest single AQI reading** of **442** (Severe), even exceeding Delhi's maximum.
8. 🏭 **PM2.5 and PM10 exceed WHO limits** across most sampled cities — a direct public health concern.
9. 🌱 **Dominant pollutants vary by city**:
   - Delhi → PM2.5
   - Gurugram → PM10
   - Kolkata → NO2
   - Hyderabad → O3
   - Bengaluru → NH3

---

## 📊 Visualizations

| # | Chart | Description |
|---|-------|-------------|
| 1 | **Mean AQI Bar Chart** | Colour-coded bar chart showing city-level mean AQI with Poor (200) and Moderate (100) threshold lines |
| 2 | **AQI Box Plot** | Distribution spread (IQR, outliers) per city using Seaborn |
| 3 | **Pollutant Radar Chart** | Spider/radar chart comparing normalised multi-pollutant profiles across all 5 cities |
| 4 | **Seasonal AQI Table** | Summary statistics of AQI broken down by season |
| 5 | **Pollutant Correlation Heatmap** | Pearson r values between all pollutants and AQI |

### AQI Colour Coding (used throughout visualizations)

| AQI Bucket   | Hex Colour | Visual |
|--------------|------------|--------|
| Good         | `#2ecc71`  | 🟢     |
| Satisfactory | `#a8e063`  | 🟡     |
| Moderate     | `#f39c12`  | 🟠     |
| Poor         | `#e67e22`  | 🔴     |
| Very Poor    | `#e74c3c`  | 🟣     |
| Severe       | `#8e44ad`  | ⚫     |

---

## 🎨 AQI Classification

As defined by the **Central Pollution Control Board, India**:

| AQI Range   | Category      | Health Impact                                              |
|-------------|---------------|------------------------------------------------------------|
| 0 – 50      | **Good**       | Minimal impact                                             |
| 51 – 100    | **Satisfactory**| Minor breathing discomfort for sensitive people           |
| 101 – 200   | **Moderate**   | Discomfort for people with lung/heart disease              |
| 201 – 300   | **Poor**       | Breathing discomfort for most people                       |
| 301 – 400   | **Very Poor**  | Respiratory illness on prolonged exposure                  |
| 401+        | **Severe**     | Affects healthy people; seriously impacts those with disease|

---

