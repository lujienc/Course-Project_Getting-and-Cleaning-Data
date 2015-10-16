# Course Project for "Getting and Cleaning Data"
### Following tasks are completed by the R codes enclosed
1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement. 
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names. 
5. Creates a second, independent tidy data set with the average of each variable for each activity and each subject.

### Explanations on R scripts
Deatiled R scripts can be found in "run_analysis.R" in the repo.
In additon the "base" package, "dplyr" and "Hmisc" packages are used for related functions.
There are six compoents in the R scripts:

##### Section 1
This section completes three tasks:
1. Read 561 feature names from a txt file.
2. Ensure 561 unique feature names using the "make.names()".
3. Double-check and ensure that there are 561 unique feature names.

##### Section 2
This section completes five tasks:
1. Read subject IDs from a text file for the training set.
2. Read activity IDs from a text file for the training set.
3. Read training data from a text file.
4. Assign names to all 563 variables: "subjectid" + "activityid" + 561 features
5. Combine the three datasets as the training dataset.
6. Genreate a new indicator in the training dataset: "trainortest" with assinged value of 1


4. Read subject labels form a txt file for the training set
5. Read activity labels from a txt file for the training set
6. Read training data from a txt file
7. Rename all varialbes
8. Merge the three data sets for the training set
tsubid <- read.table("./train/subject_train.txt", header = FALSE)
tactid <- read.table("./train/y_train.txt", header = FALSE)
trainr <- read.table("./train/X_train.txt", header = FALSE)

names(tsubid) <- "subjectid"
names(tactid) <- "activityid"
names(trainr) <- vnames

trainm <- cbind(tsubid, tactid, trainr)
trainm <- mutate(trainm, trainortest=1)

9. Read subject labels form a txt file for the testing set
10. Read activity labels from a txt file for the testing set
11. Read training data from a txt file
12. Rename all varialbes
13. Merge the three data sets for the testting set
xsubid <- read.table("./test/subject_test.txt", header = FALSE)
xactid <- read.table("./test/y_test.txt", header = FALSE)
xtestr <- read.table("./test/X_test.txt", header = FALSE)

names(xsubid) <- "subjectid"
names(xactid) <- "activityid"
names(xtestr) <- vnames

xtestm <- cbind(xsubid, xactid, xtestr)
xtestm <- mutate(xtestm, trainortest=2)

14. Append training and testing data sets
15. Sort appended data set using Subject and Accitivity IDs
16. Select measures on the mean and standard deviation for each measurement
17. Select measures with both information on mean and standard deviation
18. Varialbes containing "angle" & "Freq" have no corresponding infomration on standard deviation, thus dropped
txfull <- rbind(trainm, xtestm)
txselect1 <- select(txfull, activityid, subjectid, trainortest, grep("mean", names(txfull), ignore.case = TRUE))
txselect1 <- select(txselect1, -grep("angle", names(txselect1), ignore.case = TRUE))
txselect1 <- select(txselect1, -grep("Freq", names(txselect1), ignore.case = TRUE))
txselect2 <- select(txfull, grep("std", names(txfull), ignore.case = TRUE))
txselect <- cbind(txselect1, txselect2)

19. Name activities
20. Name data sources: training vs. testing sets
actname <- read.table("activity_labels.txt", header = FALSE)
txselect$activityid <- factor(txselect$activityid, levels = c(1,2,3,4,5,6), labels = actname$V2)
txselect$trainortest <- factor(txselect$trainortest, levels = c(1, 2), labels = c("Trainingset", "Testingset"))

21. Average of each variable for each activity and each subject
22. Clean varialbe names: drop dots, all in lowercases, drop redundant information
23. Export a txt file for the new tidy data set 
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
