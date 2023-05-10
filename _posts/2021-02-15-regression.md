---
title: "Regression model"
excerpt: "regression model test"

categories:
  - R
tags:
  - Analysis
last_modified_at: 2021-02-15
---

## **0. library import** ##
```R
library(openxlsx)
```

## **1. 데이터 import** ##
```R
#test 0.2 , train 0.8
Test<-read.xlsx('gene.CNV.mean.xlsx','Test')
Train<-read.xlsx('gene.CNV.mean.xlsx','Train')
```

## **2. regression 모델 만들기** ##
```R
model<-glm(max.CN~Tumor.fraction+reads.ratio,data=Train)
summary(model)
```
```R
Call:
glm(formula = max.CN ~ Tumor.fraction + reads.ratio, data = Train)
Deviance Residuals: 
    Min       1Q   Median       3Q      Max  
-63.651  -20.070    2.896   14.639   52.076  
Coefficients:
               Estimate Std. Error t value Pr(>|t|)    
(Intercept)       26.35       3.02   8.728 2.74e-13 ***
Tumor.fraction -3954.59     638.86  -6.190 2.34e-08 ***
reads.ratio      149.00      19.25   7.738 2.46e-11 ***
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
(Dispersion parameter for gaussian family taken to be 463.8893)
    Null deviance: 72887  on 83  degrees of freedom
Residual deviance: 37575  on 81  degrees of freedom
AIC: 759.06
Number of Fisher Scoring iterations: 2
```


## **3. 모델 이용해 예측값** ##
```R
predict(model,Test,interval='prediction')
```
```R
        1         2         3         4         5         6         7         8         9 
 48.30034  51.62229 251.03720  26.35346  37.12539  37.46735 134.81769  26.35346  34.14540 
       10        11        12 
 66.52223 146.73764  26.35346 
```
