Causal Machine Learning for Ventilator-Associated Pneumonia (VAP) Prevention

This repository contains the R scripts used for the analyses and figures presented in the article:
"Looking beyond average effects: can causal machine learning inform VAP prevention?"

1. Data Availability & Access Policy :
The data used to develop the algorithms were derived from the prospective, observational POS-VAP study and are not available publicly due to institutional and privacy restrictions. However, data access for secondary analyses can be requested directly from the POS-VAP scientific coordinators:
•	B. François
•	M. E. A. de Kraker
•	C. H. van Werkhoven

3. Statistical Analysis Plan (SAP) :
The code and methodology implemented in these pipelines strictly follow the pre-published Statistical Analysis Plan available via OSF:
Fromage, Y., Padilla, A. C. H., Vaidie, J., Sayadi, H., de Kraker, M., Woillard, J., & François, B. (2026, August 4). Statistical Analysis Plan - Causal Machine Learning for Ventilator Associated Pneumonia. https://doi.org/10.17605/OSF.IO/J8SR7

5. Script Execution Order & Workflow :
To reproduce the complete analytical pipeline, scripts must be executed sequentially.

A. Core Preventive Interventions (ATE & CATE Estimation)
For each of the 5 preventive measures (sedation vacation, oral care, head of bed elevation, deep venous thromboprophylaxis, and ulcer prophylaxis), follow this execution order:
1.	Mortality_sedation_CLEAN-VAP_GitHub.R (evaluating 28-day mortality)
2.	VAPFD28_sedation_CLEAN-VAP_GitHub.R (evaluating ventilator-associated pneumonia-free days)
3.	VFD28_sedation_CLEAN-VAP_GitHub.R (evaluating ventilator-free days)
Note: To switch between preventive measures, modify only the target_treatment variable parameter within the script corresponding to the prevention bundle you wish to test.

B. SODD Analysis
After running the main interventions, execute the SODD scripts:
•	SODD_GitHub.R

C. Visualization and Sensitivity Analyses
•	Forest Plots: All main manuscript and supplementary forest plots were generated using:
o	Plot_CLEAN_VAP_GitHub.R
•	Sensitivity Analyses (E-value): E-value robustness assessments were performed using:
o	E_value_CLEAN_VAP_GitHub.R

5. Software Requirements & Prerequisites :
All analyses were performed using R (version 4.3.1).
Required Packages by Domain
•	For Average Treatment Effect (ATE) Estimation: dplyr, ggplot2, caret (for one-hot encoding via dummyVars), tmle, SuperLearner, bartCause, bcf, grf, DoubleML, mlr3 (with mlr3learners for Ranger), AIPW, WeightIt.
•	For Conditional Average Treatment Effect (CATE) Estimation: grf, ranger, dbarts, bartCause, rlearner, glmnet, pracma.
Methodological Specifications
•	Causal Forest: Implemented using the grf package with tune.parameters = "all" for automatic hyperparameter optimization. Cross-fitted propensity scores (W.hat) were provided explicitly to ensure orthogonality and reduce bias from nuisance parameter estimation.
•	T-Learner: Built using ranger (Random Forest) separately for each treatment arm. Hyperparameters (mtry, min.node.size, num.trees) were tuned via 5-fold cross-validation within the derivation cohort.
•	X-Learner (X-BART): Implemented using dbarts for the BART nuisance models and bartCause for effect estimation. Kaplan-Meier differences were estimated for each arm
