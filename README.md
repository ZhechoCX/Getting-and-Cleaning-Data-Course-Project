# Getting-and-Cleaning-Data-Course-Project

## Peer-graded Assignment: Getting and Cleaning Data Course Project

## Project Instructions
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set.

Review criteria
less 
The submitted data set is tidy. 

The Github repo contains the required scripts.

GitHub contains a code book that modifies and updates the available codebooks with the data to indicate all the variables and summaries calculated, along with units, and any other relevant information.

The README that explains the analysis files is clear and understandable.

The work submitted for this project is the work of the student who submitted it.

## The Task
Getting and Cleaning Data Course Project
less 
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.

One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:

<http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones>

Here are the data for the project:

 <https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip>  

1. You should create one R script called run_analysis.R that does the following. 

2. Merges the training and the test sets to create one data set.

3. Extracts only the measurements on the mean and standard deviation for each measurement. 

4. Uses descriptive activity names to name the activities in the data set. Appropriately labels the data set with descriptive variable names. 

5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

## How to complete this project
1. Download the data using the link and save it on your local drive. Unzip the files and you'll have a folder UCI HAR Dataset with all the files required.
2. Open the run_analysis.R and save it in the parent directory of UCI HAR Dataset and set that as a working directory with the setwd() function in RStudio.
3. Run the "run_analysis.R" script and that will generate a new tidy data file called tidy_data.txt

## Dependencies
I have used the data.table and dplyr packages.

## License and Acknowledgements
Use of this dataset in publications must be acknowledged by referencing the following publication [1] 

[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012

This dataset is distributed AS-IS and no responsibility implied or explicit can be addressed to the authors or their institutions for its use or misuse. Any commercial use is prohibited.

Jorge L. Reyes-Ortiz, Alessandro Ghio, Luca Oneto, Davide Anguita. November 2012.
