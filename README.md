# 📊 Clinical Trial Demographic Summary Using R

## 🚀 Project Overview

This project demonstrates clinical trial data manipulation, demographic
tabulation, and percentage derivation using R.

Using a CDISC ADaM ADSL dataset, I generated a demographic summary table
(Sex by Treatment Group) for the Efficacy population. The workflow
mirrors real-world clinical programming tasks performed in regulatory
reporting environments.

------------------------------------------------------------------------

## 📂 Dataset Information

-   Dataset: ADSL (Subject-Level Analysis Dataset)
-   Standard: CDISC ADaM
-   Source: PHUSE CDISC Pilot Data
-   Format: SAS Transport File (.xpt)

Population Used: EFFFL == "Y" (Efficacy Population)

------------------------------------------------------------------------

## 🛠 Tools & Technologies

-   R
-   tidyverse (dplyr, tidyr)
-   rio (SAS file import)
-   GitHub for version control

------------------------------------------------------------------------

## 🔍 Key Steps Performed

### 1️⃣ Import SAS Dataset

``` r
library(tidyverse)
library(rio)

adsl <- import("adsl.xpt")
```

### 2️⃣ Subset Efficacy Population

``` r
adsl_eff <- adsl %>%
  filter(EFFFL == "Y")
```

### 3️⃣ Calculate Total Subjects per Treatment (Big N)

``` r
big_N <- adsl_eff %>%
  group_by(TRT01AN, TRT01A) %>%
  count(name = "N")
```

### 4️⃣ Calculate Counts by Sex (Small n)

``` r
small_n <- adsl_eff %>%
  group_by(TRT01AN, TRT01A, SEX) %>%
  count(name = "n")
```

### 5️⃣ Merge and Derive Percentages

``` r
final_table <- small_n %>%
  left_join(big_N, by = c("TRT01AN", "TRT01A")) %>%
  mutate(
    perc = round((n/N)*100, 1),
    npct = paste0(n, " (", format(perc, nsmall = 1), ")")
  )
```

------------------------------------------------------------------------

## 📊 Final Output (Sex by Treatment)

  Treatment              Sex      n    N    n (%)
  ---------------------- -------- ---- ---- -----------
  Placebo                Female   46   79   46 (58.2)
  Placebo                Male     33   79   33 (41.8)
  Xanomeline Low Dose    Female   47   81   47 (58.0)
  Xanomeline Low Dose    Male     34   81   34 (42.0)
  Xanomeline High Dose   Female   35   74   35 (47.3)
  Xanomeline High Dose   Male     39   74   39 (52.7)

------------------------------------------------------------------------

## 📈 Skills Demonstrated

✔ Clinical Trial Data Handling (ADaM Structure)\
✔ Population Flag Logic (EFFFL, ITTFL, SAFFL)\
✔ Grouped Summarization\
✔ Big N / Small n Derivation\
✔ Percentage Formatting\
✔ Regulatory-Style Table Preparation\
✔ Data Wrangling with dplyr\
✔ Clean and Reproducible Workflow

------------------------------------------------------------------------

## 🔬 Industry Relevance

This workflow replicates tasks commonly performed in:

-   Demographic Tables (Baseline Characteristics)
-   Safety Summary Tables
-   Regulatory submission reporting
-   CDISC ADaM analysis workflows

------------------------------------------------------------------------

## 👤 Author

Gopinathan M\
M.Sc. Statistics\
Clinical Data \| Biostatistics \| R Programming

Email: gopinathan.stat@gmail.com
