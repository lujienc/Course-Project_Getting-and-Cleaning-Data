# Course Project for "Getting and Cleaning Data"
### Following tasks are completed by the R scripts enclosed

1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement. 
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names. 
5. Creates a second, independent tidy data set with the average of each variable for each activity and each subject.

### A tidy dataset "tidy_data.txt" is generated using the R scripts.
### A codebook for "tidy_data.txt" can be found in the repo: CodeBook.md.

### Explanations on R scripts of "ruan_analysis.R"
Deatiled R scripts can be found in "run_analysis.R" in the repo.
In additon to the "base" package, "dplyr" and "Hmisc" packages are used for related functions.
There are six sections in the R scripts:

##### Section 1 (Feature names)
This section completes three tasks:

1. Read 561 feature names from a txt file.
2. Ensure 561 unique feature names using the "make.names()".
3. Double-check and ensure that there are 561 unique feature names.

##### Section 2 (Training set)
This section completes six tasks:

1. Read subject IDs from a text file for the training set.
2. Read activity IDs from a text file for the training set.
3. Read training data from a text file.
4. Assign names to all 563 variables: "subjectid" + "activityid" + 561 features
5. Combine the three datasets as the training dataset.
6. Genreate a new indicator in the training dataset: "trainortest" with assinged value of 1

##### Section 3 (Testing set)
This section completes six tasks:

1. Read subject IDs from a text file for the testing set.
2. Read activity IDs from a text file for the testing set.
3. Read testing data from a text file.
4. Assign names to all 563 variables: "subjectid" + "activityid" + 561 features
5. Combine the three datasets as the training dataset.
6. Genreate a new indicator in the training dataset: "trainortest" with assinged value of 2

##### Section 4 (Combined dataset)
This section completes five tasks:

1. Append training and testing datasets.
2. Sort combined dataset using "subjectid" and "activityid".
3. Select variables with names containing "mean" but without ("angle" or "Freq").
4. Select variables with names containig "std".
5. Generate a new dataset including variales with names containing "mean" or "std".

##### Section 5 (Varialbe names)
This section completes two tasks:

1. Make "activityid" a factor and name its values: six activities.
2. Make "trainortest" a factor and name its values: indicator of training vs. testing data.

##### Section 6 (New tidy data)
This section completes three tasks:

1. Average each varialbe for each subject and each activity.
2. Clean variable names: drop dots, ensure lowercases, drop redundant information
3. Export the new tiday dataset as a txt file with 69 variables: "subjectid" + "activityid" + "trainortest" + 66 mean features

##### The new dataset is tidy because in the dataset:
1. Each variable forms a column
2. Each observation forms a row
3. Each type of observational unit forms a table
