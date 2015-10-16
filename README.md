# Course Project for "Getting and Cleaning Data"
### Following tasks are completed by the R codes enclosed
1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement. 
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names. 
5. Creates a second, independent tidy data set with the average of each variable for each activity and each subject.

### R codes
setwd("D:/Data Science Certificate/Course 3_Getting and Cleaning Data/Project")
library(dplyr)
library(Hmisc)

## Read variable names from a txt file
## Force unique column names
## Make sure there are 561 unique names for all documented features
vlabel <- read.table("features.txt", header = FALSE)
vnames <- make.names(vlabel$V2, unique = TRUE)
length(unique(vnames))

## Read subject labels form a txt file for the training set
## Read activity labels from a txt file for the training set
## Read training data from a txt file
## Rename all varialbes
## Merge the three data sets for the training set
tsubid <- read.table("./train/subject_train.txt", header = FALSE)
tactid <- read.table("./train/y_train.txt", header = FALSE)
trainr <- read.table("./train/X_train.txt", header = FALSE)

names(tsubid) <- "subjectid"
names(tactid) <- "activityid"
names(trainr) <- vnames

trainm <- cbind(tsubid, tactid, trainr)
trainm <- mutate(trainm, trainortest=1)

## Read subject labels form a txt file for the testing set
## Read activity labels from a txt file for the testing set
## Read training data from a txt file
## Rename all varialbes
## Merge the three data sets for the testting set
xsubid <- read.table("./test/subject_test.txt", header = FALSE)
xactid <- read.table("./test/y_test.txt", header = FALSE)
xtestr <- read.table("./test/X_test.txt", header = FALSE)

names(xsubid) <- "subjectid"
names(xactid) <- "activityid"
names(xtestr) <- vnames

xtestm <- cbind(xsubid, xactid, xtestr)
xtestm <- mutate(xtestm, trainortest=2)

## Append training and testing data sets
## Sort appended data set using Subject and Accitivity IDs
## Select measures on the mean and standard deviation for each measurement
## Select measures with both information on mean and standard deviation
## Varialbes containing "angle" & "Freq" have no corresponding infomration on standard deviation, thus dropped
txfull <- rbind(trainm, xtestm)
txselect1 <- select(txfull, activityid, subjectid, trainortest, grep("mean", names(txfull), ignore.case = TRUE))
txselect1 <- select(txselect1, -grep("angle", names(txselect1), ignore.case = TRUE))
txselect1 <- select(txselect1, -grep("Freq", names(txselect1), ignore.case = TRUE))
txselect2 <- select(txfull, grep("std", names(txfull), ignore.case = TRUE))
txselect <- cbind(txselect1, txselect2)

## Name activities
## Lable some key variables
actname <- read.table("activity_labels.txt", header = FALSE)
txselect$activityid <- factor(txselect$activityid, levels = c(1,2,3,4,5,6), labels = actname$V2)
txselect$trainortest <- factor(txselect$trainortest, levels = c(1, 2), labels = c("Trainingset", "Testingset"))

## Average of each variable for each activity and each subject
## Clean varialbe names: drop dots, all in lowercases, drop redundant information
## Export a txt file for the new tidy data set 
txselect$trainortest <- as.numeric(txselect$trainortest)
tidynew <- txselect %>%
        arrange(subjectid, activityid) %>%
        group_by(subjectid, activityid) %>%
        summarize_each(funs(mean))
tidynew$trainortest <- factor(tidynew$trainortest, levels = c(1, 2), labels = c("Trainingset", "Testingset"))

cnames <- names(tidynew)
cnames <- tolower(gsub(".", "", cnames, fixed = TRUE))
cnames <- sub("fbodybody", "fbody", cnames, fixed = TRUE)
names(tidynew) <- cnames

write.table(tidynew, file = "tidy_data.txt", row.names = FALSE)
