---
title: "regression model"

categories:
  - R
tags:
  - Analysis
last_modified_at: 2021-02-15
---
library(openxlsx)
library(ggplot2)

#데이터 import
Test<-read.xlsx('gene.CNV.mean.xlsx','Test')
Train<-read.xlsx('gene.CNV.mean.xlsx','Train')


#regression 모델 만들기

model<-glm(max.CN~Tumor.fraction+reads.ratio,data=Train)
summary(model)

_Call:
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
Number of Fisher Scoring iterations: 2_



#모델 이용해 예측값
predict(model,Test,interval='prediction')



