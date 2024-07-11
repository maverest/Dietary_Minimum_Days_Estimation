# Dietary_Minimum_Days_Estimation

### **ICC, Bland-Altan and LMM** analysis 
**[ICC_Bland_LMM.ipynb notebook](#ICC_Bland_LMM.ipynb)**

**1) LMM**

`def compute_LMM(data, features = features, save = False, covid = False):`

`def plot_LMM_heatmap(p_values, coef_values):`

These function allows to plot (Figure 2 in the report) a heatmap presenting the coefficients for each day of the week, treated as a categorical fixed effect in a LMM, for each nutritional feature. Additionally, it includes the p-value results that test the null hypothesis that the coefficients for each day are equal to zero. The LMM was fitted using the subjects as random effect and BMI, gender, age group, and day of the week as categorical fixed effects.

They must be called this way: 

`stat = compute_LMM(full_weeks, save = True)
plot_LMM_heatmap(stat[0], stat[1])`

**2) ICC**
`def ICC_mean_consec(data = full_weeks, features = features, criterion = criterion_mean, single_day = False):`
This function calculates the ICC scores (using the mean across the combined days) for 1 to 7 consecutive days combination for each nutritional features
To get the ICC scores (mean for consecutive day comb.) it must be called this way
`icc_mean_single_day = ICC_mean_consec(single_day=True)`
`icc_mean_consec = ICC_mean_consec()`

`def wstd_consec(data = full_weeks,features = features):`
This function compute the w-std using LMM which will be used to compute the ICC
`def ICC_wstd_consec(wstd_values, nb_day = 7, features = features, data = full_weeks):`
This function calculates the ICC scores (using the W-std across the combined days) for 1 to 7 consecutive days combination for each nutritional features
To get the ICC scores (W-std for consecutive day comb.) they must be called this way
`wstd_week_consec = wstd_consec()`
`icc_wstd_consec = ICC_wstd_consec(wstd_values=wstd_week_consec)`

`def plot_ICC(icc_scores, features = features, nb_day = 7, add_curve_uncons = False, var = False):`
This function plots (Figure 4 and 5 in the report) the ICC scores calculated by comparing the mean or w-std intake over a combination
 of 1 to 7 consecutive days with the mean or w-std intake over the reference period (whole week), for each
 nutritional features.
`def plot_single_day_ICC(icc_score_single_day):`
This function plots (Figure 3 in the report)the averaged ICC scores over all features for each individual days of the week

To get the plot they must be called this way using the results of the previous functions
`plot_ICC(icc_mean_consec)`
`plot_single_day_ICC(icc_mean_single_day)`
`plot_ICC(icc_wstd_consec, var=True)`

All the ICC scores a saved in the pkl file format in the ICC_Results/ folder
