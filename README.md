# 💰 Analyzing African Debt Trends and Financial Crises (2000–2024)

## 🧠 Project Overview

This project examines the relationship between government debt issuance and financial crises across 13 African countries from 2000 to 2014. By integrating historical crisis data with contemporary debt records, we analyze patterns and correlations between debt accumulation and various types of financial crises including banking, currency, inflation, and systemic crises.

---

## 🎯 Objectives

- **Identify debt issuance patterns** across countries and years (2000-2014)
- **Correlate debt trends** with crisis indicators including inflation rates and sovereign defaults
- **Analyze the relationship** between debt levels and crisis occurrences
- **Visualize patterns** through interactive Tableau dashboards
- **Provide insights** into African debt sustainability and crisis vulnerability

---

## 📦 Data Sources

### 1. African Crises Dataset (1870-2014)
- **Source**: Kaggle / Reinhart & Rogoff inspired dataset
- **Coverage**: 13 African countries over 144 years
- **Key Variables**: 
  - Systemic crisis indicators
  - Banking, currency, and inflation crisis flags
  - Sovereign debt defaults
  - Annual inflation rates (CPI)
- **Format**: CSV
- **Records**: 1,059 country-year observations

### 2. African Debt Database (2000-2024)
- **Source**: Kiel Institute for the World Economy
- **Coverage**: 54 African countries, quarterly data over 24 years
- **Key Variables**:
  - Debt issuance amounts (USD millions)
  - Creditor information (multilateral, bilateral)
  - Debt instruments and terms
  - Interest rates and maturity periods
- **Format**: CSV (converted from Excel)
- **Records**: 10,000+ debt transactions

### 3. Supplementary Data
- **World Bank API**: GDP and inflation indicators (for validation)
- **IMF Databases**: Economic context cross-referencing

---

## 🧰 Tech Stack

| Component | Technology |
|-----------|------------|
| **Data Processing** | Python (Pandas, NumPy) |
| **Database** | MySQL Workbench |
| **Visualization** | Tableau Desktop |
| **Environment** | Jupyter Notebook |
| **Version Control** | Git/GitHub |
| **Languages** | Python, SQL |

---

## 📊 Countries Analyzed

The analysis focuses on **13 African countries** present in both datasets:

1. 🇩🇿 Algeria
2. 🇦🇴 Angola
3. 🇨🇫 Central African Republic
4. 🇨🇮 Ivory Coast (Côte d'Ivoire)
5. 🇪🇬 Egypt
6. 🇰🇪 Kenya
7. 🇲🇺 Mauritius
8. 🇲🇦 Morocco
9. 🇳🇬 Nigeria
10. 🇿🇦 South Africa
11. 🇹🇳 Tunisia
12. 🇿🇲 Zambia
13. 🇿🇼 Zimbabwe

**Time Period**: 2000-2014 (14-year overlap)

---

## 🔄 ETL Process

### Extract
- Loaded African Crises Dataset (CSV)
- Loaded African Debt Database (CSV, converted from XLSX)
- Verified data integrity and structure

### Transform

#### Crises Dataset Cleaning:
- Filtered data to 2000-2014 time period
- Selected 8 relevant columns from 14 original
- Converted `banking_crisis` from text to binary
- Dropped redundant identifiers and historical columns

#### Debt Dataset Cleaning:
- Aggregated quarterly data to annual totals
- Selected 5 key columns from 25 original
- Removed rows with missing debt amounts
- Calculated total debt issued per country-year

#### Data Integration:
- Standardized country names (fixed "Ivory Coast" vs "Côte d'Ivoire")
- Identified 13 common countries
- Merged datasets on Country + Year
- Inner join resulted in 195 country-year observations

### Load
- Exported cleaned data to `debt_crisis_merged_final.csv`
- Imported into Tableau for visualization
- Created correlation matrices and trend analyses

---

## 📈 Dataset Schema

### Final Merged Dataset
**File**: `debt_crisis_merged_final.csv`  
**Records**: 195 country-year observations  
**Columns**: 9

| Column Name | Type | Description |
|-------------|------|-------------|
| `Country` | String | Country name |
| `year` | Float | Year (2000-2014) |
| `total_debt_issued_musd` | Float | Total debt issued (million USD) |
| `systemic_crisis` | Integer | Systemic crisis indicator (0/1) |
| `currency_crises` | Integer | Currency crisis count |
| `inflation_crises` | Integer | Inflation crisis indicator (0/1) |
| `inflation_annual_cpi` | Float | Annual inflation rate (%) |
| `sovereign_external_debt_default` | Integer | External debt default (0/1) |
| `banking_crisis_binary` | Integer | Banking crisis indicator (0/1) |

---

## 🔍 Key Findings & Insights

### Crisis Patterns
- **Most Crisis-Prone**: Zimbabwe (hyperinflation of 21,989,695% in 2008)
- **Recent Systemic Crisis**: Nigeria (2009-2014 with banking crisis)
- **Stable Economies**: Morocco, Tunisia, Mauritius (no major crises)

### Debt Patterns
- **Highest Total Debt**: Egypt ($188B by 2014, from $59M in 2000)
- **Post-Crisis Borrowing**: Angola increased debt after crisis period ended
- **Crisis-Period Behavior**: Zimbabwe reduced borrowing during hyperinflation

### Correlations
- **External debt defaults** often coincide with currency and inflation crises
- **Systemic crises** show correlation with sustained high debt issuance
- **Banking crises** tend to follow periods of rapid debt accumulation


## 🚀 Getting Started

### Prerequisites
```bash
# Python packages
pandas>=1.5.0
numpy>=1.23.0
jupyter>=1.0.0

# Database
MySQL 8.0+

# Visualization
Tableau Desktop 2023.1+
```

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/paulgikonyo/Gen-Data-Capstone-Project.git
cd Gen-Data-Capstone-Project
```

2. **Install Python dependencies**
```bash
pip install -r requirements.txt
```

3. **Load data into MySQL** (optional)
```bash
mysql -u username -p database_name < sql/load_data.sql
```

4. **Run Jupyter notebooks**
```bash
jupyter notebook
main.ipynb
```

5. **Open Tableau workbook**
- Navigate to `tableau/` folder
- Open `.twbx` file in Tableau Desktop

---

## 📊 Analysis Workflow

### Step 1: Data Extraction
```python
import pandas as pd

# Load datasets
crises_df = pd.read_csv('data/raw/african_crises.csv')
debt_df = pd.read_csv('data/raw/african_debt_database.csv')
```

### Step 2: Data Cleaning
```python
# Filter crises data for 2000-2014
crises_clean = crises_df[
    (crises_df['year'] >= 2000) & 
    (crises_df['year'] <= 2014)
][['country', 'year', 'systemic_crisis', 'banking_crisis', 
   'currency_crises', 'inflation_crises', 'inflation_annual_cpi',
   'sovereign_external_debt_default']]

# Aggregate debt data to annual
debt_annual = debt_df.groupby(['Country', 'year']).agg({
    'Amount_musd': 'sum'
}).reset_index()
```

### Step 3: Data Integration
```python
# Merge datasets
merged_df = pd.merge(
    debt_annual,
    crises_clean,
    left_on=['Country', 'year'],
    right_on=['country', 'year'],
    how='inner'
)
```

### Step 4: Analysis & Visualization
- Trend visualization in Tableau
- Statistical testing for relationships

---

## 📈 Visualizations

The Tableau dashboard includes:

1. **Debt Trends Over Time** - Line charts showing debt issuance by country
2. **Crisis Heatmap** - Geographic and temporal distribution of crises
3. **Correlation Matrix** - Relationships between debt and crisis types
4. **Country Profiles** - Detailed analysis for each of the 13 countries
5. **Crisis Timeline** - Interactive timeline of major financial events
6. **Debt vs Inflation** - Scatter plots examining the relationship

---

## 🎓 Methodology

### Data Quality
- **Missing Value Treatment**: Dropped records with missing debt amounts
- **Outlier Detection**: Identified and documented extreme cases (e.g., Zimbabwe hyperinflation)
- **Data Validation**: Cross-referenced with World Bank and IMF sources

### Statistical Methods
- **Correlation Analysis**: Pearson correlation between debt and crisis indicators
- **Trend Analysis**: Time series analysis of debt accumulation
- **Comparative Analysis**: Country-level comparisons across regions

### Limitations
- **Sample Size**: Only 13 countries due to dataset overlap
- **Time Period**: Limited to 2000-2014 (debt data extends to 2024)
- **Causality**: Correlation does not imply causation
- **Data Gaps**: Some country-years missing in both datasets

---

## 🔮 Future Enhancements

- [ ] Extend analysis to 2015-2024 using additional crisis indicators
- [ ] Include all 54 countries from debt database
- [ ] Add GDP data for debt-to-GDP ratio analysis
- [ ] Implement predictive models for crisis forecasting
- [ ] Create interactive web dashboard using Plotly/Dash
- [ ] Add regional analysis (North, East, West, Central, Southern Africa)
- [ ] Incorporate COVID-19 pandemic impact (2020-2021)

---

## 👨‍💻 Author

**Paul Gikonyo**  
📍 Nairobi, Kenya  
📧 paulgikonyo100@gmail.com  
💼 Data Analyst | Economic Research Enthusiast  
🔗 [GitHub](https://github.com/paulgikonyo) | 

---

## 📝 License

This project is licensed under the MIT License 

---

## 🙏 Acknowledgments

- **Kiel Institute for the World Economy** - African Debt Database
- **Reinhart & Rogoff** - Inspiration for crisis dataset methodology
- **Kaggle Community** - African Crises Dataset
- **World Bank & IMF** - Supplementary economic data
- **Zindua School** - Data Analytics Program support

---

## 📚 References

1. Reinhart, C. M., & Rogoff, K. S. (2009). *This Time is Different: Eight Centuries of Financial Folly*
2. African Development Bank. (2023). *African Economic Outlook*
3. Kiel Institute. (2024). *African Debt Database Documentation*
4. IMF. (2023). *Regional Economic Outlook: Sub-Saharan Africa*

---

## 📞 Contact & Support

For questions, suggestions, or collaboration opportunities:

- **Email**: paulgikonyo100@gmail.com

---

**Last Updated**: November 2024  
**Version**: 1.0.0  
**Status**: ✅ Complete - Ready for Visualization