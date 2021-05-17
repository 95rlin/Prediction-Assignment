# Prediction-Assignment
### Introduction

Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways

Six young health participants were asked to perform one set of 10 repetitions of the Unilateral Dumbbell Biceps Curl in five different fashions: 
1. exactly according to the specification (Class A), 
2. throwing the elbows to the front (Class B)
3. lifting the dumbbell only halfway (Class C) 
4. lowering the dumbbell only halfway (Class D)
5. throwing the hips to the front (Class E)

### Clean training and testing set


```
train_csv <- read.csv(file = "week4_proj/pml-training.csv", header = TRUE, na.strings=c("", "NA", "#DIV/0!"))  # set all missing data to NA
test_csv <- read.csv(file = "week4_proj/pml-testing.csv", header = TRUE, na.strings=c("", "NA", "#DIV/0!")) # set all missing data to NA
```
