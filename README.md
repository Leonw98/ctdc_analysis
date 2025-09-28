# 📊 CTDC Human Trafficking Analysis & Power BI Dashboard

This project combines **Python‑based data auditing and reshaping** with **Power BI modelling and visualisation** to explore the *Counter‑Trafficking Data Collaborative (CTDC)* dataset. The aim is to create a transparent, reproducible workflow that highlights both the **scale of exploitation** and the **gaps in data coverage**, while also surfacing gender‑based differences in exploitation pathways.

---

## 1. 🎯 Project Objectives
- Audit the CTDC dataset for **missingness and completeness**.  
- Reshape the data into a **long format** suitable for BI tools.  
- Integrate **UN population data** to normalise case counts per 100k population.  
- Build **Power BI dashboards** that balance absolute counts, per‑capita rates, and coverage indicators.  
- Provide **insights by gender, age, region, and exploitation type**.  
- Recommend **governance and standardisation steps** for future data collection.

---

## 2. 📂 Repository Structure
```
ctdc_analysis/
│
├── data/
│   ├── raw/                # Original CTDC + UN population files
│   └── outputs/            # Cleaned population lookup, reshaped tables
│
├── outputs/                # Audit summaries, CSVs, styled Excel, plots
│
├── power_bi/               # Power BI dashboards (.pbix) and screenshots
│
├── ctdc_analysis.ipynb     # Python notebook for audit + reshaping
├── README.md               # This document
```

---

## 3. 🧹 Data Audit (Python)
- **Initial load**: ~97k rows × 64 columns. Added a stable `CaseID`.  
- **Missingness audit**:  
  - Dropped boilerplate “Terms of Use” column.  
  - Computed missing counts/percentages for all fields.  
  - Exported styled Excel + CSV summaries.  
- **Group completeness**: Defined logical subgroups (Demographics, Control, Exploitation, Recruiter, Abduction).  
  - Found **>85% missingness** in many control/exploitation fields.  
  - Demographics relatively complete.  

📸 See:  
- `outputs/missing_summary_styled.xlsx`  
- `outputs/group_missing_summary.png`

---

## 4. 📈 Completeness Trends
- Tracked subgroup completeness by **source and country**.  
- Key findings:  
  - **Early years**: exploitation types fairly strong, control data absent.  
  - **Mid‑2010s**: improved labour exploitation reporting.  
  - **Recent years**: collapse in completeness across most groups.  
  - Country coverage uneven (e.g. Egypt, India, Kenya strong; Lebanon, Belarus patchy).  

📸 See:  
- `outputs/completion_trends.png`  
- `outputs/raw_vs_normalised.png`

---

## 5. 🔄 Reshaping for BI
- Problem: wide 0/1 columns not BI‑friendly.  
- Solution: reshape to **long format** with demographics repeated.  
- Steps:  
  - Fill blanks in demographics.  
  - Melt wide table into long format.  
  - Keep only rows where flag = 1.  
  - Map `SubCategory → ParentCategory`.  

Outputs:  
- `outputs/ctdc_long_with_demographics.csv`  
- `outputs/case_abduction_flags.csv`

---

## 6. 🌍 Population Integration
- Loaded UN population dataset (`WPP2024_GEN_F01_DEMOGRAPHIC_INDICATORS_COMPACT.xlsx`).  
- Extracted **Total Population (thousands)**.  
- Scaled by ×1,000 for true counts.  
- Exported cleaned lookup:  
  - `data/outputs/cleaned_population_lookup.csv`

---

## 7. 📐 Power BI Modelling
### Core Measures
```DAX
TotalPopulation =
SUM('cleaned_population_lookup'[PopulationCount]) * 1000

CasesPer100k =
DIVIDE(DISTINCTCOUNT(ctdc[Case No]), [TotalPopulation]) * 100000

PersonsPer100k =
DIVIDE(SUM(ctdc[Persons Identified]), [TotalPopulation]) * 100000

AbductionsPer100k =
DIVIDE(SUM('case_abduction_flags'[isAbduction]), [TotalPopulation]) * 100000
```

### Validation
- Built sanity‑check tables (`Country | Case Count | Population | CasesPer100k`).  
- Corrected inflated rates (e.g. Montenegro 31,200 → 153 per 100k).  

---

## 8. 📊 Dashboard Insights (Gender Filters)

- **Female filter**
  - Sexual exploitation dominates female cases.  
  - Recruitment often via **intimate partners or family members**.  
  - Control types lean heavily on **psychological abuse, threats, and restriction of movement**.  
  - Broader age spread, including older age brackets.  

- **Male filter**
  - Labour exploitation dominates male cases.  
  - Recruitment more **community‑based** (friends, family), with less emphasis on intimate partners.  
  - Control types show more **direct coercion** (physical abuse, economic abuse, threats).  
  - Age distribution concentrated in **working‑age brackets (22–46)**, with notable presence of adolescents in forced labour/criminality.  

- **Not Known filter**
  - Smaller group, but important to track as it highlights **data quality gaps**.  
  - Often under‑specified in both exploitation type and control methods.  
  - Should be flagged in dashboards as “data incomplete” rather than ignored.  

📸 Screenshots in:  
- `power_bi/`

---

## 9. 🔑 Key Takeaways
- **Normalisation matters**: per‑100k rates reveal vulnerabilities in small states.  
- **Gendered patterns**: females → sexual exploitation, psychological control; males → labour exploitation, physical coercion.  
- **Recruitment**: often by trusted individuals, not strangers.  
- **Coverage gaps**: labour exploitation under‑reported, sexual exploitation uneven.  
- **Transparency**: dashboards show counts + rates + coverage %.  

---

## 10. 🚀 Next Steps

- **Data Governance & Oversight**
  - Establish a **project group or steering committee** to oversee CTDC data collation methods.  
  - Define a shared **ideology and methodology** for data capture, ensuring consistency across sources.  
  - Set **clear goals for data completeness** (e.g. minimum % coverage for control type, exploitation type, recruiter relation).  

- **Standardisation**
  - Agree on **controlled vocabularies** for exploitation types, control methods, and recruiter relations.  
  - Standardise demographic categories (age bands, gender identities) to reduce fragmentation.  
  - Document and publish a **data dictionary** for all contributors.  

- **Quality Monitoring**
  - Build **completeness dashboards** that track missingness by source, country, and gender.  
  - Flag sources consistently under‑reporting certain fields.  
  - Provide **feedback loops** to data providers to improve future submissions.  

- **Strategic Use**
  - Pair **absolute counts** with **per‑capita rates** and **coverage indicators** in all reporting.  
  - Use gender‑split dashboards to highlight **different exploitation pathways**.  
  - Encourage stakeholders to interpret findings with both **scale** and **data quality** in mind.  

