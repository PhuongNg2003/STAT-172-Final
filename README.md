---
title: "Meals on Wheels Expansion Recommendation"
author: "Phuong Nguyen"
date: "12/08/2024"
output: html_document
---

## Introduction
This repository will show you how to reproduce the results that support the recommendation on where in Iowa that Wesley Life should expand into based on whether an individual has food anxiety in the past year (FSWROUTY), whether they receive SNAP (FSSTAMPVALC), and whether they have enough food and the kinds of food they wanted to eat in the past year (FSFOODS). We will follow the description of how to run for FSWROUTY variable, and repeat the steps for the other 2 variables (FSSTAMPVALC & FSFOODS)

## Requirements
To install the required R packages, run the following code in R:

```r
install.packages(c("tidyverse", "caret", "ggthemes", 
  "glmnet", "haven", "knitr", 
  "logistf", "sf", "pROC", 
  "randomForest", "RColorBrewer", 
  "rpart", "rpart.plot", "reshape2"))
```

## Data

We use four different data files. 
```r /data/cps_00006.csv``` contains all the Current Population Survey Food Security Supplement survey data from the Department of Agriculture. 
```r /data/tl_2023_19_puma20/``` folder contains the shapefile and other relevant information used to create the maps of Iowa divided by PUMA. 
```r /data/total_iowa_seniors_by_puma.csv``` contains the number of seniors and each PUMA and was provided to us. 
```r /data/spm_pu_2022.sas7bdat``` is the data we use to predict.

We also save our predictions in the ```r /data/fswrouty_prediction.csv```, ```r /data/fsstamp_prediction.csv```, ```r /data/fsfoods_prediction.csv```, ```r /data/single_senior_household.csv```
to make the reproduction of the ```r code/combining_predictions.R``` quicker, but there are also comments at the top of the file if you would prefer to source just that file.

## Reproduce 

# Clean data
We created ```r code/clean_acs.R``` (data we use to predict) and ```r code/clean_cps.R``` (Current Population Survey data) with the goal of cleaning our x and y variables before using them

We, then, need to run the files in order to create a smooth follow and have enough data. Thus, we will need to run all the y-variable analysis files first

# Y-variables
We need to run these ```r code/FSWROUTY_variable.R ```, ```r code/fsstamp_analysis.R```, ```r code/fsfoods_analysis.R``` files for each y-variable. 
All these codes have the same format
* clean y-variable and transform them into binary
* split data into training and testing data
* adding interaction and squared terms (this only happens in ```r code/fsstamp_analysis.R``` and ```r code/fsfoods_analysis.R```)
* trying different regressions including MLE, Firth's penalized likelihood, Random Forest, Decision Tree, Lasso and Ridge
* compare all these AUC from different regressions
* make a prediction on the ```r clean_acs.R``` data
* upload the Iowa PUMA shape map to create a map with proportions of people in Iowa regarding the chosen y-variable
* upload the senior data and run prediction in this population
* present the final predictions using the heat map to highlight different areas
* For the FSWROUTY variable, there are a few extra codes at the end to focus on Single-Senior household prediction

After running the 3 y-variable files, you can run ```r code/combining_predictions.R```



