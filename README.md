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

We also save our predictions in the 
```r /data/fswrouty_prediction.csv
/data/fsstamp_prediction.csv
/data/fsfoods_prediction.csv
/data/single_senior_household.csv
``` to make reproduction of the ```r code/combining_predictions.R``` quicker, but there are also comments at the top of the file if you would prefer to source just that file.
