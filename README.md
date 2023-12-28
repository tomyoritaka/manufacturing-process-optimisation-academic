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

#### Analyses & Results

1. Cleaning & Merging the Data

We look at the 5 data files at a high level & clean them - remedying missing data (NaNs) & 0s (where there should NOT be 0s). We then merge all data files into 1 dataset called merged_df.

Part of the merge process is to create a column of 1s & 0s for each of the 4 files (contamination, crystallisation, ion diffusion, burnishing) to indicate defect (1) vs no defect (0) for all 2,000 data points. We do this by creating a new column of 1s in the file indicating a defect, merging it with merged_df & replacing the NaNs with 0s.

2. Reviewing the Data Visually (Scanning the Data)

Now that we have a cleaned-and-merged dataset, we review & gain a ‘bird’s eye view’ of the data. We review each set of input variables (M1-M4) & ALL current/later-step defect/cost variables (we include later-step outcome variables for M1, M2 & M3 each since we may find a cross-step correlation such as the M1 input variables predicting the ion diffusion defect) in a scatter matrix & a heat map.

3. Linear/Polynomial Regressions (Supervised ML): m1_cost, m2_cost, m3_cost, m4_cost



4. 


