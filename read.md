# **Dietary Minimum Days Estimation**

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites/Installation](#prerequisitesinstallation)
- [Project Structure](#project-structure)
- [Usage](#usage)
- [Report and presentation](#report-and-presentation)

## Introduction
The aim of this work is to identify the minimum number of days generally required to achieve reliable estimates of the nutritional intake of a subject over a week. The accuracy of nutritional features estimations is assessed using two metrics: the mean intake over different days and the within-subject standard deviation (W-std) in intake between days. These metrics are computed for different number of days combinations (1, 2, 3, ..., 7), for each subject and nutritional features, and compared with a reference period (entire week, 7 days), using the Intraclass Correlation Coefficient (ICC), to evaluate the reliability depending on the number and combinations of days sampled. Additional methods including Principal Component Analysis (PCA), Bland-Altman, and the within, between subject variation coefficient ratio ($CVw/CVb$) analysis were performed to compare, confirm, and strengthen the results

## Prerequisites / Installation
- Python 3.11+
- Jupyter Notebook
- Python Package:
    - pandas
    - numpy
    - matplotlib
    - seaborn
    - scipy
    - statsmodels
    - tqdm
    - pingouin
    - scikit-learn

1. Clone the repository:
   `git clone https://github.com/your-username/your-repository.git`
2. Create conda environement:
`conda env create -f environement.yml`
`conda activate Dietary_Minimum_Days_Estimation_env`


## Project Structure
```
Dietary_Minimum_Days_Estimation/
│
├── Bland_Altman/                  # Contains Bland-Altman analysis results
├── data_raw/                      # Raw, untreated data
├── data_set/                      # Processed raw data, prepared for analysis
├── data_var_coef_analysis/        # Contains Coefficient of Variation ratio analysis results and related useful files
├── Food group/                    # pkl files for nutritional features groups
├── ICC_one_week/                  # Contains ICC (Intraclass Correlation Coefficient) analysis results (pkl files)
├── table/                         # Contains summarized ICC results used for various figures
│
├── notebooks/
│   ├── category.ipynb             # Script to categorize nutritional features and produce demographic feature distribution table
│   ├── one_week_ICC.ipynb         # Script for ICC and Bland-Altman analysis
│   ├── PCA.ipynb                  # Script for Principal Component Analysis (PCA)
│   ├── prep_data.ipynb            # Script for raw data cleaning and preparation
│   ├── table.ipynb                # Script for formatting ICC results into tables (non-consecutive days combination approach)
│   ├── var_coef.ipynb             # Script for Coefficient of Variation ratio analysis
│   └── week_comp.ipynb            # Script for comparing weeks in terms of mean intake using ICC
│
├── environment.yml                # Conda environment configuration for reproducibility
├── README.md                      # Main project documentation
```
## Usage
All the analysis are done using the prepared data set [data_for_analysis.csv](#data_for_analysis.csv) in the folder Data/data_set/
This dataset comprising 453 distinct subjects, each providing dietary intake data representing one complete consecutive week. The 37 nutritional features listed below were analyzed: 
![alt text](image.png)




### **ICC, Bland-Altan and LMM Analysis** 
**[ICC_Bland_LMM.ipynb notebook](#ICC_Bland_LMM.ipynb)**

**1) LMM**

```python
def compute_LMM(data, features = features, save = False, covid = False):
def plot_LMM_heatmap(p_values, coef_values):
```
These function allows to plot (Figure 2 in the report) a heatmap presenting the coefficients for each day of the week, treated as a categorical fixed effect in a LMM, for each nutritional feature. Additionally, it includes the p-value results that test the null hypothesis that the coefficients for each day are equal to zero. The LMM was fitted using the subjects as random effect and BMI, gender, age group, and day of the week as categorical fixed effects.
They must be called this way: 
```python
stat = compute_LMM(full_weeks, save = True)
plot_LMM_heatmap(stat[0], stat[1])
```

**2) ICC consecutive days approach**


```python
def ICC_mean_consec(data = full_weeks, features = features, criterion = criterion_mean, single_day = False):
```
This function calculates the ICC scores (using the mean across the combined days) for 1 to 7 consecutive days combination for each nutritional features

To get the ICC scores (mean for consecutive day comb.) it must be called this way:
```python
icc_mean_single_day = ICC_mean_consec(single_day=True)
icc_mean_consec = ICC_mean_consec()
```

```python
def wstd_consec(data = full_weeks,features = features):
```
This function compute the w-std using LMM which will be used to compute the ICC
```python
def ICC_wstd_consec(wstd_values, nb_day = 7, features = features, data = full_weeks):
```
This function calculates the ICC scores (using the W-std across the combined days) for 1 to 7 consecutive days combination for each nutritional features

To get the ICC scores (W-std for consecutive day comb.) they must be called this way:
```python
wstd_week_consec = wstd_consec()
icc_wstd_consec = ICC_wstd_consec(wstd_values=wstd_week_consec)
```

```python
def plot_ICC(icc_scores, features = features, nb_day = 7, add_curve_uncons = False, var = False):
```
This function plots (Figure 4 and 5 in the report) the ICC scores calculated by comparing the mean or w-std intake over a combination
 of 1 to 7 consecutive days with the mean or w-std intake over the reference period (whole week), for each
 nutritional features

```python
def plot_single_day_ICC(icc_score_single_day):
```
This function plots (Figure 3 in the report)the averaged ICC scores over all features for each individual days of the week

To get the plot they must be called this way using the results of the previous functions:

```python
plot_ICC(icc_mean_consec)
plot_single_day_ICC(icc_mean_single_day)
plot_ICC(icc_wstd_consec, var=True)
```

All the ICC scores are saved in the pkl file format in the ICC_Results/consecuive/ folders

**3) ICC unconsecutive days approach**
```python
def convert_days(comb):
def all_combinations(nb_days, numb_comb=0):
```
Function to get all possible 2-7 days combination

```python
def ICC_mean_uncons(nb_day, day_combination = day_comb_mean, full_weeks = full_weeks, features = features):
```
This function calculates the ICC scores (using the mean across the combined days) for all possible 1 to 7 days combination for each nutritional features, for exemple to get the ICC scores for all possible 2 days combination, it must be called this way: 
```python
icc_mean_uncons_2 = ICC_mean_uncons(2) # ICC scores for all 2 days combination possible
```

```python
def wstd_comb(numb_day,day_permut=day_comb_var ,df = full_weeks, feat = features):
def ICC_wstd_uncons(wstd_values, criterion = criterion_wstd, nb_day = 7, features = features, data = full_weeks):
```
These functions calculates the W-std and the ICC scores (using the W-std across the combined days) for all possible 1 to 7 days combination for each nutritional features, for exemple to get the ICC scores for all possible 2 days combination, it must be called this way: 
```python
wstd_2 = wstd_comb(2)
icc_score_wstd_2 = ICC_wstd_uncons(wstd2)
```
All the ICC scores are saved in the pkl file format in the ICC_Results/unconsecuive/ folders

**4) Bland-Altmann**
The diferents function creta Bland-Altmann metrics table saved in Bland_Altman_Results/ folder


### **PCA** analysis 
**[PCA.ipynb](#PCA.ipynb)**

The notebook contains function to compute and plot the PCA results.

### **Variation Coefficient Ratio Analysis** analysis 
**[Var_Coef_Ratio.ipynb](#Var_Ratio_Coef.ipynb)**
The notebook contains function to compute the Variation Coefficient Ration Analysis results.

## Report and presentation
- ADD pdf file