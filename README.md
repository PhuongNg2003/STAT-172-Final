---
title: "Meals on Wheels Expansion Recommendation"
author: "Phuong Nguyen, Matt Colbert, and Aria Fisher"
date: "12/08/2024"
output: html_document
---

# Introduction
This repository will show you how to reproduce the results that support the recommendation on where in Iowa that Wesley Life should expand into based on whether an individual has food anxiety in the past year (FSWROUTY), whether they receive SNAP (FSSTAMPVALC), and whether they have enough food and the kinds of food they wanted to eat in the past year (FSFOODS). We will follow the description of how to run for FSWROUTY variable, and repeat the steps for the other 2 variables (FSSTAMPVALC & FSFOODS)

# Requirements
To install the required R packages, run the following code in R:

```r
install.packages(c("tidyverse", "caret", "ggthemes", 
  "glmnet", "haven", "knitr", 
  "logistf", "sf", "pROC", 
  "randomForest", "RColorBrewer", 
  "rpart", "rpart.plot", "reshape2"))
```

# Data

We use four different data files. 
```/data/cps_00006.csv``` contains all the Current Population Survey Food Security Supplement survey data from the Department of Agriculture. 
```/data/tl_2023_19_puma20/``` folder contains the shapefile and other relevant information used to create the maps of Iowa divided by PUMA. 
```/data/total_iowa_seniors_by_puma.csv``` contains the number of seniors and each PUMA and was provided to us. 
```/data/spm_pu_2022.sas7bdat``` is the data we use to predict.

We also save our predictions in the ```/data/fswrouty_prediction.csv```, ```/data/fsstamp_prediction.csv```, ```/data/fsfoods_prediction.csv```, ```/data/single_senior_household.csv```
to make the reproduction of the ```code/combining_predictions.R``` quicker.

# Reproduce 

### Clean data
We created ```code/clean_acs.R``` (data we use to predict) and ```code/clean_cps.R``` (Current Population Survey data) with the goal of cleaning our x and y variables before using them

## Short-cut Approach
As we mentioned above, for a quicker reproduction of the result, you can run  ```code/combining_predictions.R``` to get the heat map of areas in Iowa that rank from most important (lowest number) to least important (highest number).

## Other Approach
However, in order to understand the process of choosing the model and the individual y-variable prediction, we can run the y-variable files separately.

### Y-variables
We need to run these ```code/FSWROUTY_variable.R ```, ```code/fsstamp_analysis.R```, ```code/fsfoods_analysis.R``` files for each y-variable. 
All these codes have the same format
* Clean y-variable and transform them into binary
* Split data into training and testing data
* Adding interaction and squared terms (this only happens in ```code/fsstamp_analysis.R``` and ```code/fsfoods_analysis.R```)
* Trying different regressions including MLE, Firth's penalized likelihood, Random Forest, Decision Tree, Lasso and Ridge
* Compare all these AUC from different regressions
* Make a prediction on the ```r clean_acs.R``` data
* Upload the Iowa PUMA shape map to create a map with proportions of people in Iowa regarding the chosen y-variable
* Upload the senior data and run prediction in this population
* Present the final predictions using the heat map to highlight different areas
* For the FSWROUTY variable, there are a few extra codes at the end to focus on Single-Senior household prediction

After running the 3 y-variable files, you can run ```code/combining_predictions.R```, using source of the different y-variable. This file helps compile all predictions from the 3 files above to rank areas that Wesley Life should look into with the lower number meaning it's important to take immediate action and the high number indicating the less importance. 

## Extra 
You can run ```code/visualizations_and_general_analysis.R``` to see more visualization work, specifically on the y-variable overlap and senior population percentage. 
You can also look into ```code/cluster_model_testing.R``` for some extra work regarding x-variables clusters, which was found to be not working well afterward. 


