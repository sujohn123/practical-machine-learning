---
title: "Practical Machine Learning final assignment"
author: "sujohn"
date: "10 April 2016"
output: html_document
---
<h2>Summary</h2>
Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement - a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it.

<h2>Objective</h2>
In this assignment i have build a prediction model to predict the manner in which the above participants did the exercise which is the 'classe' variable.
The training data used for this project is available here:

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv

The test data used for the project is available here:

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv

<h3>Loading and cleaning data</h3>

Here, we are importing the datasets and then removing the columns which consits of NA values totally.

```{r}
library(caret)
setwd("f:/r")
traindata<-read.csv("pml-training.csv",, na.strings=c("NA", "#DIV/0!"))
testdata<-read.csv("pml-testing.csv", na.strings=c("NA", "#DIV/0!"))

##removing values with na columns
traindata1<-traindata[, apply(traindata, 2, function(x) !any(is.na(x)))]
testdata1<-testdata[, apply(testdata, 2, function(x) !any(is.na(x)))]

newtraindata1<-traindata1[,-c(1:8,52)]
newtestdata1<-testdata1[,-c(1:8,52)]

```

<h3>Data partition and model building</h3>
Here we are dividing 70% of the train dataset to train the model and 30% as test for validation.

We are aloo building random forest model from the 70% dataset that we have obtained and then applyingt the model in test dataset.

```{r}
library(caret)
library(randomForest)
t<-createDataPartition(y=newtraindata1$classe, p=0.70,list=F)
train1<-newtraindata1[t,] 
test1<-newtraindata1[-t,]

fitness<-randomForest(train1$classe~.,method="rf",data=train1)

cf<-predict(fitness, newdata=test1)
confusionMatrix(cf, test1$classe)

```
From the confusion matrix we can say that the accuracy level is pretty high i.e above 99%. Hence, the model is performing pretty good.

<h3>Predicting the final test dataset</h3>
Lets apply the model to predict our actual test dataset.

```{r}
finaltest<-predict(fitness,newdata=newtestdata1)                            
finaltest

```






