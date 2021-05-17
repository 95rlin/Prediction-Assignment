# Prediction-Assignment
### Introduction

Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways

Six young health participants were asked to perform one set of 10 repetitions of the Unilateral Dumbbell Biceps Curl in five different fashions: 
1. exactly according to the specification (Class A), 
2. throwing the elbows to the front (Class B)
3. lifting the dumbbell only halfway (Class C) 
4. lowering the dumbbell only halfway (Class D)
5. throwing the hips to the front (Class E)

### Preparing Datasets
The datasets are imported. All of the non-numerical data (blank spaces, "NA", "#DIV/0!") is set to NA. 
```
library(caret)
library(randomForest)

train_csv <- read.csv(file = "week4_proj/pml-training.csv", header = TRUE, na.strings=c("", "NA", "#DIV/0!"))
test_csv <- read.csv(file = "week4_proj/pml-testing.csv", header = TRUE, na.strings=c("", "NA", "#DIV/0!")) 
```
The datasets are further cleaned by removing the useless data in the first 7 columns. Columns with NA are also removed.
```
train_csv <- train_csv[ , -c(1:7)] # remove first 7 columns of useless data
test_csv <- test_csv[ , -c(1:7)] # remove first 7 columns of useless data
clean_train_csv <- train_csv[, colSums(is.na(train_csv)) ==0]] # remove columns with NA
clean_test_csv <- test_csv[,colSums(is.na(test_csv)) == 0] # remove columns with NA
```
Clean_train_csv is partitioned 70% for training and 30% for testing. The seed is set to 999. 
```
set.seed(999)
inTrain <- createDataPartition(y=clean_train_csv$classe, p=0.7, list=FALSE) #
training <- clean_train_csv[inTrain,]
testing <- clean_train_csv[-inTrain,]
```
### Model (Random Forest)
I decided to use Random Forest as the model because of its accuracy. However, I had to limit the number of trees generated in the model because of Random Forests's slow speed.

```
modFit <- train(classe ~., data=clean_train_csv, method="rf", ntree=5)

Random Forest 

19622 samples
   52 predictor
    5 classes: 'A', 'B', 'C', 'D', 'E' 

No pre-processing
Resampling: Bootstrapped (25 reps) 
Summary of sample sizes: 19622, 19622, 19622, 19622, 19622, 19622, ... 
Resampling results across tuning parameters:

  mtry  Accuracy   Kappa    
   2    0.9610778  0.9507256
  27    0.9809803  0.9759303
  52    0.9732050  0.9660856

Accuracy was used to select the optimal model using the largest value.
The final value used for the model was mtry = 27.
```


