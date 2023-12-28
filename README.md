# manufacturing-process-optimisation-academic
## PROJECT: Defect & Cost Optimisation of a Manufacturing Process Using a Large Dataset
#### Project Context

This academic project requires analysis of a large dataset containing input variables and 2 output variables - a defect variable and a cost variable - of a new 4-step manufacturing process (each step has 4 input variables - Temperature, Vibration, Arc Gap and Pressure - as well as the aformentioned 2 output variables).

The numbers of defects & the costs of production are deemed too high. The defect numbers & the costs are also volatile - and seen to be uncontrollable. The new production line is seen as a 'black box'. 

The owner of the manufacturing process wants to make sense of the large dataset & to control/reduce the probabilities of defects & the costs of production going forward.

The CSV files are:
- CML_2023-09-11#7-machine_params_and_cost.csv
- CML_2023-09-11#6-Ion_Diffusion_Defects.csv
- CML_2023-09-11#5-crystalisation_Defects.csv
- CML_2023-09-11#4-contamination_defects.csv
- CML_2023-09-11#3-burnishing_Defects.csv

The objectives are:
1. From the existing dataset, identify the ‘optimal' set(s) of M1-M4 (M1=Step 1, M2=Step 2, M3=Step 3, M4=Step 4) input variables that will meet particular defect
probability & cost requirements
2. Predict the defect probabilities & costs of a new set of M1-M4 input variables

#### Analyses

1. Cleaning & Merging the Data

We look at the 5 data files at a high level & clean them - remedying missing data (NaNs) & 0s (where there should NOT be 0s). We then merge all data files into 1 dataset called merged_df.

Part of the merge process is to create a column of 1s & 0s for each of the 4 files (contamination, crystallisation, ion diffusion, burnishing) to indicate defect (1) vs no defect (0) for all 2,000 data points. We do this by creating a new column of 1s in the file indicating a defect, merging it with merged_df & replacing the NaNs with 0s.

2. Reviewing the Data Visually (Scanning the Data)

Now that we have a cleaned-and-merged dataset, we review & gain a ‘bird’s eye view’ of the data. We review each set of input variables (M1-M4) & ALL current/later-step defect/cost variables (we include later-step outcome variables for M1, M2 & M3 each since we may find a cross-step correlation such as the M1 input variables predicting the ion diffusion defect) in a scatter matrix & a heat map.

3. Linear/Polynomial Regressions (Supervised ML): m1_cost, m2_cost, m3_cost, m4_cost

Multivariate Regressions

We apply a linear regression to each set of M1-M4 input variables – and further apply 2nd/3rd-degree polynomial regressions if/where the linear regression appears to show a meaningful correlation. The outcome variables are the cost variables (m1_cost, m2_cost, m3_cost, m4_cost). For each set of input variables from M1 to M4, we attempt to identify correlations vis-à-vis the direct cost variable (for example, the M1 input variables vs m1_cost) as well as any later-step cost variable (for example, the M1 input variables vs m2_cost, m3_cost or m4_cost).

For each of our multivariate linear & 2nd/3rd-degree polynomial regressions, we move through the following steps:
(1) Run the regression using 80:20 split training & test datasets
(2) Derive the regression fit using R2 & RMSE

In addition, for each of the multivariate 2nd-degree polynomial regressions, we:
(3) Attempt to find the minimum/optimum point provided that the regression fit is sufficiently strong
(4) Apply ridge regularisation to ‘correct’ for overfitting (where necessary)

Single-Input-Variable Regressions

Using linear & 2nd/3rd-degree polynomial regressions (following the steps described above), we analyse the 3 (potential) single-input-variable correlations identified from the heat map: B1 vs m1_cost, B3 vs m3_cost, T3 vs m3_cost. In addition, we analyse each input variable vis-à-vis its direct cost variable in the same step of the process but only after segregating the input variable data into 2 clusters based on visual reviews of scatter plots.

4. Logistic Regressions (Supervised ML): defects - contamination, crystallisation, ion diffusion, burnishing

5. 


