---
title       : Exercise Correctness Prediction
subtitle    : Predicting Exercise Mistake Type
author      : akrylove
job         : student
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---
### Objective
Find a regression model that predicts whether a dumbbell lifting exercise was performed correctly or which of the four mistake types was encountered.

### Analysis
1. Load the data set and split it in into training and testing sets. 
2. Compress the training set and select features *.
3. Build a Random Forest model with the training set using the selected features.



```r
inTrain <- createDataPartition(y=data$classe, p=0.6, list=FALSE)
training <- data[inTrain,]; testing <- data[-inTrain,]
```






```r
model <- randomForest(classe ~ ., data=training, proximity=TRUE
                      , na.action=na.omit, keep.forest=TRUE, ntree=100)
```

For compression and feature selection details, refer to http://akrylove.github.io/machine-learning/CourseProject-Final.html.

---
### Model Evaluation
- Low (6%) out of sample error rate - good accuracy.
- Confusion matrix with 3% to 10% misclassification rate - not overfitted.

```
## 
## Call:
##  randomForest(formula = classe ~ ., data = training, proximity = TRUE,      keep.forest = TRUE, ntree = 100, na.action = na.omit) 
##                Type of random forest: classification
##                      Number of trees: 100
## No. of variables tried at each split: 4
## 
##         OOB estimate of  error rate: 6.84%
## Confusion matrix:
##      A    B    C    D    E class.error
## A 3192   33   60   58    5     0.04659
## B  103 2050   87   18   21     0.10048
## C   46   71 1904   28    5     0.07303
## D   51   15   98 1754   12     0.09119
## E    5   38   24   28 2070     0.04388
```

---

Run the model on the test set and evaluate its accuracy.

```r
pred <- predict(model,testing); testing$predRight <- pred==testing$classe
table(pred,testing$classe)
```

```
##     
## pred    A    B    C    D    E
##    A 2156   48   32   37    3
##    B   16 1401   42    2   30
##    C   20   54 1285   54   14
##    D   40    7    7 1182   16
##    E    0    8    2   11 1379
```

<b>Try it yourself at http://akrylove.shinyapps.io/dataprod/ </b>

---
### Conclusion
Using a Random Forest method on 16 acceleration-related variables, we can create a model that has over 90% accuracy rate of detecting whether a dumbbell was lifted correctly and, if it was not, what type of five known mistakes was made.  The model is not intelligent enough to catch mistakes of types other than the five captured in the data set. Neither can the model determine whether more than one mistake was made (for example, accellerating both belt and arm too fast).  We also do not know how well the model scales beyond the original set of 19,000 observations.  However, for data sets under 20,000 observations where each observation has zero or one exercise mistakes, the model is 94% accurate predicting whether the exercise was performed correctly or which of the four known mistake types occured.

### References
http://groupware.les.inf.puc-rio.br/har
https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv
http://akrylove.github.io/machine-learning/CourseProject-Final
http://akrylove.shinyapps.io/dataprod/


