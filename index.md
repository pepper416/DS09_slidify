---
title : Predict the probability of being admitted by graduate school
subtitle : Logistic Regress
author : pepper416
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : standalone # {standalone, draft}
---

## Data Description
A researcher is interested in how variables, such as GRE (Graduate Record Exam scores),
        GPA (grade point average) and prestige of the undergraduate institution, effect admission
        into graduate school. The response variable, admit/don't admit, is a binary variable.

The dataset can be downloaded from [http://www.ats.ucla.edu/stat/data/binary.csv](http://www.ats.ucla.edu/stat/data/binary.csv)

```r
mydata <- read.csv("http://www.ats.ucla.edu/stat/data/binary.csv")
head(mydata)
```

```
##   admit gre  gpa rank
## 1     0 380 3.61    3
## 2     1 660 3.67    3
## 3     1 800 4.00    1
## 4     1 640 3.19    4
## 5     0 520 2.93    4
## 6     1 760 3.00    2
```

--- 
  
## Logistic regression model
Since the response variable `admit` is binary, we can fit a logistic regress model to predict the probability of being admitted by graduate school.

```r
mydata$rank <- factor(mydata$rank)
mylogit <- glm(admit ~ gre + gpa + rank, data = mydata, family = "binomial")
summary(mylogit)$coefficients
```

```
##                 Estimate  Std. Error   z value     Pr(>|z|)
## (Intercept) -3.989979073 1.139950936 -3.500132 0.0004650273
## gre          0.002264426 0.001093998  2.069864 0.0384651284
## gpa          0.804037549 0.331819298  2.423119 0.0153878974
## rank2       -0.675442928 0.316489661 -2.134171 0.0328288188
## rank3       -1.340203916 0.345306418 -3.881202 0.0001039415
## rank4       -1.551463677 0.417831633 -3.713131 0.0002047107
```


---  

## Prediction


```r
GRE = 1600
GPA = 4
Rank = 1
pred = predict(mylogit,newdata=list(gre=GRE,gpa=GPA,rank=as.factor(Rank)),
                interval=("confidence"),type = "link", se = TRUE)
PredictedProb <- plogis(pred$fit)
```

Based on our model, if a student has GRE = 1600, GPA= 4, and graduates from the university with prestige rank 1, the predicted probability of being admitted is 0.95.


--- 

### Box plot of GRE score vs GPA


```r
# assign the GPA into different groups
mydata$gpa_group = cut(mydata$gpa, breaks = c(2,2.5,3,3.5,4))
# make a boxplot of GRE by GPA
boxplot(gre~gpa_group, mydata, main = "boxplot of GRE vs GPA",
        xlab = "GPA", ylab = "GRE", col = "grey")
```

![plot of chunk unnamed-chunk-4](assets/fig/unnamed-chunk-4.png) 
