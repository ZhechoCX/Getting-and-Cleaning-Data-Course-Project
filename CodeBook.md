title: "CodeBook.md"
author: "ZD"
date: "5/11/2022"
output: md_document
---
# Getting and Cleaning Data Course Project

## Submission Requirements
You will be required to submit: 
1) a tidy data set as described below, 
2) a link to a Github repository with your script for performing the analysis, 
3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. 
You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.

## Data
The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:

<http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones> 

Here are the data for the project:

<https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip>

## Tasks
You should create one R script called run_analysis.R that does the following. 

1. Merges the training and the test sets to create one data set.

2. Extracts only the measurements on the mean and standard deviation for each measurement. 

3. Uses descriptive activity names to name the activities in the data set

4. Appropriately labels the data set with descriptive variable names. 

5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.


## My Course Project
### Prepare for work
### Download files
```{r}
fileURL <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
```

### Set working directory

```{r}
setwd("~/Desktop/Coursera/Course_3")
```

### download files
```{r}
download.file(fileURL, destfile = "~/Desktop/Coursera/Course_3/Dataset.zip", method = "curl")
```
### unzip files

```{r}
unzip(zipfile = "~/Desktop/Coursera/Course_3/Dataset.zip", exdir = "~/Desktop/Coursera/Course_3")
```
### check
```{r}
list.files("~/Desktop/Coursera/Course_3")
```
### get the path to files
```{r}
data_folder <- file.path("~/Desktop/Coursera/Course_3", "UCI HAR Dataset")
```
### see files in data folder
```{r}
list.files(data_folder)
list.files("~/Desktop/Coursera/Course_3/UCI HAR Dataset/train")
list.files("~/Desktop/Coursera/Course_3/UCI HAR Dataset/test")
```
## 1. Merge the training and the test sets to create one data set.
### Load data.table to read data
```{}
library(data.table)
```
### Read the data in the test and training sets
```{}
subjectID_train <- read.table(file.path(data_folder, "train", "subject_train.txt"))
x_data_train <- read.table(file.path(data_folder, "train", "X_train.txt"))
y_activity_train <- read.table(file.path(data_folder, "train", "y_train.txt"))

subjectID_test <- read.table(file.path(data_folder, "test", "subject_test.txt"))
x_data_test <- read.table(file.path(data_folder, "test", "X_test.txt"))
y_activity_test <- read.table(file.path(data_folder, "test", "y_test.txt"))
```
### Combine the data sets by rows
```{}
subjectIDs <- rbind(subjectID_train, subjectID_test)
xData <- rbind(x_data_train, x_data_test)
y_activity <- rbind(y_activity_train, y_activity_test)
```
###Check data
```{}
head(subjectIDs)
head(xData)
head(y_activity)
```
### Need to provide labels to the columns
```{}
colnames(subjectIDs) <- "SubjectID"
```
### Before naming the xData variables with the features names, we need to get those from features.txt
```{}
features <- read.table(file.path(data_folder, "features.txt"), head=FALSE)
```
### Look at the data
```{}
head(features)
```
### Labels are in V2
```{}
colnames(xData) <- features$V2
```
### Look up xData
```{}
str(xData)
```
### Label the Activity Column
```{}
colnames(y_activity) <- "Activity_ID"
```
### now combine all into one table
```{}
All_Data <- cbind(subjectIDs, y_activity, xData)
```
### Look at the structure of the data
```{}
str(All_Data)
```
### We have 10299 obs. of  563 variables.
###########
## 2. Extracts only the measurements on the mean and standard deviation for each measurement.
# We already have the list of all features from above (i.e.)
```{}
features <- read.table(file.path(data_folder, "features.txt"), head=FALSE)
```
### Look at the data
```{}
head(features)
```
### Extract the features we want (mean and std)
```{}
Interested_feature_names <- features$V2[grep("mean\\(\\)|std\\(\\)", features$V2)]
```
### Now filter the data to include only the features we want i.e. mean and std
```{}
interested_columns <- c(as.character(Interested_feature_names), "SubjectID", "Activity_ID" )
Interested_Data <- subset(All_Data, select = interested_columns)
str(Interested_Data)
```
## 3. Uses descriptive activity names to name the activities in the data set
### Get the activity labels
```{}
activity_lables <- read.table(file.path(data_folder, "activity_labels.txt"))
```
### Look at the data
```{}
str(activity_lables)
```
### Update the column names
```{}
names(activity_lables) <- c("Activity_ID", "Activity_Name")
```
### Now we can merge the activity labels data set with our subset data file
```{}
Labeled_Activity_Data <- merge(Interested_Data, activity_lables, by = "Activity_ID")
str(Labeled_Activity_Data)
```
## 4. Appropriately labels the data set with descriptive variable names. 
```{}
names(Labeled_Activity_Data)
```
#### Here is what the Readme.txt says:
####"From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details".
#### Then the 'features_info.txt' says: 
####"The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ. These time domain signals (prefix 't' to denote time)..."
#### "Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals)"
### So "t" is for time, "f" is for frequency, "Acc" is for  accelerometer, "Gyro" is for "gyroscope"

### Let's substitute the abbreviations with the full descriptive names
```{}
names(Labeled_Activity_Data) <- gsub("^t", "time", names(Labeled_Activity_Data))
names(Labeled_Activity_Data) <- gsub("^f", "frequency", names(Labeled_Activity_Data))
names(Labeled_Activity_Data) <- gsub("Acc", "Accelerometer", names(Labeled_Activity_Data))
names(Labeled_Activity_Data) <- gsub("Gyro", "Gyroscope", names(Labeled_Activity_Data))

names(Labeled_Activity_Data)
```
#### when we look at the re-labelled variable names, we see the abbreviations of Jerk and Mag. 
#### From the below we see that "Jerk" is ok as as, and "Mag" means "magnitude" ("Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag))

```{}
names(Labeled_Activity_Data) <- gsub("Mag", "Magnitude", names(Labeled_Activity_Data))

names(Labeled_Activity_Data)
```
## 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and subject.

### let's load the dplyr package to make the manipulation easier 
```{}
library(dplyr)
```
### create a new tidy data set with the average of each variable for each activity and each subject

```{}
Tidy_Data <-aggregate(. ~ SubjectID + Activity_Name, Labeled_Activity_Data, mean)
Tidy_Data <- Tidy_Data[order(Tidy_Data$SubjectID, Tidy_Data$Activity_Name),]
write.table(Tidy_Data, file = "tidy_data.txt", row.names = FALSE)
```
### Check the end result
```{}
Result <- read.table("~/Desktop/Coursera/Course_3/tidy_data.txt")
head(Result)
str(Result)
```
