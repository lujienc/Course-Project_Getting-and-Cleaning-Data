# Codebook for "tidy_data.txt"
## "tidy_data.txt" is generated by "run_analysis.R"

#### Procedure of getting "tidy_data"
1. Combined information from training and testing data sets covering 30 volunteers engaging 6 activities respectively.
2. Altogether 561 unique features were in the original datasets.
3. Selected information reagrding the mean and standard deviation of each measurement.
4. Measurements with only related information on mean were dropped (e.g., "angle" and "Freq" measurements).
5. All selected measurements were averaged for each acitity of each volunteer.
6. There are 69 varialbes in the dataset: "subjectid" + "activityid" + "trainortest" + 63 averaged features.
7. There are 180 observations in the dataset: 6 activites for each of the 30 volunteers.
 
#### Variable information
##### Subject, activity, and data source indicators
* "subjectid": an integer varialbe, raning from 1 to 30, IDs of each volunteer.
* "activityid": a factor variables, with 6 levels for walking, walking_upstairs, walking_downstairs, sitting, standing, laying.
* "trainortest": a factor varialbes, with 2 levels for whehter the information was collected from training or testing data.

##### Measures
######The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals. These time domain signals were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals using another low pass Butterworth filter with a corner frequency of 0.3 Hz.
* tbodyaccx/y/z
* tgravityaccx/y/z
* tbodygyrox/y/z

###### Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals. Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm. 
* tbodyaccjerkx/y/z
* tbodygyrojerkx/y/z
* tbodyaccmag
* tgravityaccmag
* tbodyaccjerkmag
* tbodygyromag
* tbodygyrojerkmag

###### Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing
* fbodyaccx/y/z
* fbodyaccjerkx/y/z
* fbodygyrox/y/z
* fbodyaccmag
* fbodyaccjerkmag
* fbodygyromag
* fbodygyrojerkmag

###### Then the means and standard deviations of the aforementioned feautres are (1) selected, (2) averaged for each activity of each volunteer, and (3) saved in "tidy_data.txt".
* All measures are numeric variables.
* Altogether there are 33 mean-related measures and 33 standard-deviation-related measures.
* Measures with names containing "mean" are the mean of the aforementined features.
* Measures with names containing "std" are the standard devaition of the aforementioned features.
* Measures with names starting with "t" are time domain measures.
* Measures with names starting with "f" are Fast Fourier Transform (FFT) transformed measures.
* Measures with names containing "bodyacc" are body accelerometer signal measures.
* Measures with names containing "gravityacc" are gravity accelerometer signal measures.
* Measures with names containing "gyro" are gyroscope signal measures.
* Measures with names containing "jerk" are Jerk signal measures.
* Measures with names containing "mag" are magnitude measures of dimensional signals using the the Euclidean norm.
* Measures with names containing "x", "y", and "z" are used to differentiate between signals along the 3 axials. 
