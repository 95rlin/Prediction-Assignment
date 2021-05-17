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
The accuracy of Random Forest can also be improved by implementing a k-fold cross validation. A 2nd modfit is generated with 10 folds in a grid search.
```
modFit <- train(classe ~., data=clean_train_csv, method="rf", ntree=5)

trControl <- trainControl(method = "cv", number=10, search="grid")
modFit2 <- train(classe ~., data=clean_train_csv, trControl = trControl, method="rf", ntree=5)
```
### Validation
I use the fitted model in Modfit and Modfit2 to test the prediction accuracy with the testing set. 
To calculate the accuracy, I divided each prediction's number of correct guesses over the total number of test cases (testing's number of rows).
It was found that modFit2 has better accuracy at 99.98% compared to modFit's 99.81%. Although both models are probably accurate enough to use. 
```
pred <- predict(modFit, testing)
> table(pred,testing$classe)
pred    A    B    C    D    E
   A 1674    0    0    0    0
   B    0 1139    2    0    1
   C    0    0 1024    2    1
   D    0    0    0  962    0
   E    0    0    0    0 1080
pred2 <- predict(modFit2, testing)
> table(pred2,testing$classe)
pred2    A    B    C    D    E
    A 1674    1    0    0    0
    B    0 1138    0    0    0
    C    0    0 1026    0    0
    D    0    0    0  964    0
    E    0    0    0    0 1082

accuracy1 <- sum(pred==testing$classe)/(nrow(testing))
> accuracy1
[1] 0.9981308
accuracy2 <- sum(pred2==testing$classe)/(nrow(testing))
> accuracy2
[1] 0.9998301
```
### Prediction
Since modFit2 is more accurate, I use this model to predict the classe's in the test set (clean_test_csv).

```
results <- predict(modFit2, newdata=clean_test_csv)
> results
 [1] B A B A A E D B A A B C B A E E A B B B
Levels: A B C D E
```

