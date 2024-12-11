---
title: "Meals on Wheels Expansion Recommendation"
author: "Phuong Nguyen, Matt Colbert, and Aria Fisher"
date: "12/08/2024"
output: html_document
---

# Introduction
The project identifies areas in Iowa for Wesley Life's potential expansion based on food security metrics. The analysis uses food security metrics to highlight regions with the greatest need, focusing on three key variables:  
- **Food anxiety (FSWROUTY)**  
- **SNAP receipt (FSSTAMPVALC)**  
- **Food adequacy (FSFOODS)**  

# Requirements
To install the required R packages, run the following code in R:

```r
install.packages(c("tidyverse", "caret", "ggthemes", 
  "glmnet", "haven", "knitr", 
  "logistf", "sf", "pROC", 
  "randomForest", "RColorBrewer", 
  "rpart", "rpart.plot", "reshape2"))
```

# Data Sources and Preparation
Sources and Prediction data can be found under data folder

## Sources
The analysis relies on the following data files:
* ```/data/cps_00006.csv``` contains all the Current Population Survey Food Security Supplement survey data from the Department of Agriculture. 
* ```/data/total_iowa_seniors_by_puma.csv``` contains the number of seniors and each PUMA and was provided to us. 
* ```/data/spm_pu_2022.sas7bdat``` is the data we use to predict.
* ```/data/tl_2023_19_puma20/``` folder contains the shapefile and other relevant information used to create the maps of Iowa divided by PUMA. 

## Prediction data
Predictions are saved in the following files for quick access:
* ```/data/fswrouty_prediction.csv```
* ```/data/fsstamp_prediction.csv```
* ```/data/fsfoods_prediction.csv```
* ```/data/single_senior_household.csv```
These predictions are used in ```code/combining_predictions.R``` to generate the Iowa heat map with food security prediction.

## Clean data
Data preparation is handled in the following scripts:
```code/clean_acs.R```: Cleans prediction data 
```code/clean_cps.R```: Cleans survey data 

# Reproduction Guide 

## Short-cut Approach
Run  ```code/combining_predictions.R``` to quickly generate a heat map ranking Iowa regions from most important (low rank) to least important (high rank) for expansion consideration.

## Detailed Approach
To understand the full modeling process and prediction for each variable, follow these steps:

### Individual Y-variable Analysis
Run the following scripts for each y-variable:
* ```code/FSWROUTY_variable.R```
* ```code/fsstamp_analysis.R```
* ```code/fsfoods_analysis.R```

### Common Workflow for Each Script:
* Clean y-variable and transform them into binary
* Split data into training and testing data
* Adding interaction and squared terms (in ```code/fsstamp_analysis.R``` and ```code/fsfoods_analysis.R```)
* Test different regressions including MLE, Firth's penalized likelihood, Random Forest, Decision Tree, Lasso, and Ridge
* Compare model using AUC metrics
* Make a prediction on the ```r clean_acs.R``` data
* Generate a map showing proportions for the target y-variable across Iowa PUMA regions
* Upload the senior data and run prediction in this population
* Present the final predictions using the heat map to highlight different areas
* For the FSWROUTY variable, include additional analysis focusing on single-senior households.

After running the 3 y-variable files, you can run ```code/combining_predictions.R```, using source of the different y-variable. This file helps compile all predictions from the 3 files above to rank Iowa regions based on predicted need, with lower numbers indicating higher priority.

## Additional Analysis
### Visualization
```code/visualizations_and_general_analysis.R``` shows visualizations of the y-variable overlap and senior population percentage. 
### Clusters
```code/cluster_model_testing.R``` shows building prediction models using the x-variables clusters approach, which, then, was found to not be an effective approach. 


