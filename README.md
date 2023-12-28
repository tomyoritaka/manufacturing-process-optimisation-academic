# manufacturing-process-optimisation-academic
## PROJECT: Defect & Cost Optimisation of a Manufacturing Process Using a Large Dataset
#### Project Context

This academic project requires analysis of a large dataset containing 4 input variables and 2 output variables (a defect variable and a cost variable) for a new 4-step manufacturing process (each step has 4 input variables - Temperature, Vibration, Arc Gap and Pressure - as well as the aformentioned 2 output variables).

The numbers of defects & the costs of production are deemed too high. The defects & the costs are also volatile - and seen to be uncontrollable. The new production line is seen as a 'black box'. 

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

#### 1. Cleaning & Merging the Data

We look at the 5 data files at a high level & clean them - remedying missing data (NaNs) & 0s (where there should NOT be 0s). We then merge all data files into 1 dataset called merged_df.

Part of the merge process is to create a column of 1s & 0s for each of the 4 files (contamination, crystallisation, ion diffusion, burnishing) to indicate defect (1) vs no defect (0) for all 2,000 data points. We do this by creating a new column of 1s in the file indicating a defect, merging it with merged_df & replacing the NaNs with 0s.

#### 2. Reviewing the Data Visually (Scanning the Data)

Now that we have a cleaned-and-merged dataset, we review & gain a ‘bird’s eye view’ of the data. We review each set of input variables (M1-M4) & ALL current/later-step defect/cost variables (we include later-step outcome variables for M1, M2 & M3 each since we may find a cross-step correlation such as the M1 input variables predicting the ion diffusion defect) in a scatter matrix & a heat map.

#### 3. Linear/Polynomial Regressions (Supervised ML): m1_cost, m2_cost, m3_cost, m4_cost

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

#### 4. Logistic Regressions (Supervised ML): defects - contamination, crystallisation, ion diffusion, burnishing

We now turn our attention to the defect variables (conta_d, crys_d, i_diff_d, burn_d) – each as the outcome variable of a multivariate logistic regression. As in the previous section, for each set of M1-M4 input variables, we attempt to identify correlations vis-à-vis the direct defect variable as well as any later-step defect variable. 

For each of our multivariate logistic regressions, we move through the following steps: 
(1) Run the regression using 80:20 split training & test datasets
(2) Estimate the predicted defect probability of each set of input variables (test dataset)
(3) Derive the regression fit using the ‘how often did we get the outcome right’ score as well as the confusion matrix & the following values: Accuracy, Misclassification Rate, False Positive Rate, Precision

#### 5. Clustering (Unsupervised ML): m1_cost, m2_cost, m3_cost, m4_cost

We apply clustering to identify & segregate distinct sets of data points & apply algorithms/models accordingly in the hope of teasing out stronger correlations and/or identifying lower-cost clusters.  We analyse each set of M1-M4 input variables vis-à-vis their direct cost variable (for example, the M1 input variables vs m1_cost), but not any of the cross-step variables. 

For each of our clustering analyses, we move through the following steps:
(1)	Determine the number of clusters (k) via the silhouette method or the elbow method; in our analysis, we use k=2
(2)	Cluster the data assuming k clusters & using K-means
(3) Plot the clustering result for each input variable/cost variable pair & review visually to identify meaningful clusters

#### 6. Gaussian Mixture Models: m1_cost, m2_cost, m3_cost, m4_cost

We next turn our attention to Gaussian Mixture Models in the hope of identifying more than 1 sub-distribution of m1_cost, m2_cost, m3_cost and/or m4_cost.

For each of our Gaussian Mixture Models, we move through the following steps:
(1) Assume the number of sub-distributions; for our analysis, we assume 2 sub-distributions given the results of our initial scatter plots & the subsequent clustering analyses
(2) Apply a Gaussian Mixture Model to the cost data & derive the estimated means/other related statistics
(3) Compare the 2 sub-distributions in a histogram as well as the 2 means – and determine if 2 sub-distributions exist for each cost variable

#### 7. Decision Trees/Classifications (Supervised ML): defects - contamination, crystallisation, ion diffusion, burnishing / m1_cost, m2_cost, m3_cost, m4_cost

In the final section of our analysis, we apply basic decision trees & Random Forest Classifications to both the defect variables & the cost variables. We convert the costs into categorical variables by assigning a value of 0 if the cost is in the lowest 25th percentile (otherwise, 1). We analyse each set of input variables - M1-M4 – vis-à-vis their direct defect or cost variable (for example, the M1 input variables vs conta_d).

For each decision tree/Random Forrest Classifier analysis, we move through the following steps:
(1) Construct a basic decision tree using each set of input variables (M1-M4) & their direct defect or cost variable
(2) ‘Prune’ the decision tree by using ‘ccp_alpha=’ to ‘correct’ for potential overfitting
(3) Run a Random Forest Classifier using 80:20 split training & test datasets
(4) Estimate the predicted defect probability or the probability of the cost being in the lowest 25th percentile for each set of input variables (test dataset)
(5) Derive the classification fit using the ‘how often did we get the outcome right’ score as well as the confusion matrix & the following values: Accuracy, Misclassification Rate, False Positive Rate, Precision
(6) Compare the performance of the Random Forest Classifier with that of the basic decision tree

