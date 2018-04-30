This CodeBook describes the variable names and steps carried out to analyse the data gathered from: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

The original database contains information regarding the use of smarthpones to understand the activities from 30 individuals. This data was acquired with Samsung smarthphones. These datasets were originally split in two: a "training" dataset and a "test" dataset. This was carried out by the authors to develop predictive models based on the training dataset. These models were tested on the "testing" dataset. 
The steps to produce the submitted dataset were:

*opening the relevant files
*identifying which parts of the data files corresponded with each other; that is:
  *X_train.txt: contained the measurement for each position in the training dataset. This is in gravitational units.
  *y_train.txt: Contained the activity labels for each measurement (originally a factor variable). The possibilities for this variable were: "Walking", "Walking Upstairs", "Walking Downstairs", "Sitting", "Standing", "Laying"
  *X_test.txt: Contained the measurements for each position in the test dataset. This is in gravitational units.
  *y_test.txt: Contained the activity labels for each measurement (originally a factor variable). The possibilities for this variable were: "Walking", "Walking Upstairs", "Walking Downstairs", "Sitting", "Standing", "Laying"
  *subject_train.txt: Contains the information on the subjects who performed each activity in the training setup. This is a factor variable, numbered 1-30
  *subject_test.txt: Contains the information on the subjects who performed each activity in the testing setup. This is a factor variable, the possible values were: 2, 4, 9, 10, 12, 13, 18, 20, 24
*Merging of the corresponding activity information with the corresponding activity labels
*Matching these data with the subject who carried out the activity
*Merging the training and the testing datasets
*Appropriately labeling the activities (originally coded as numeric variables)
*calculating the mean measurement for each activity for each subject
*Producing a final "tidy" dataset, with the following variables:
  *activitylabel: The name of the activity which registered the data on the corresponing columns
  *subjectnumber: Number of the subject who performed the activity
  *meanaverage: The average of the mean of each measurement for each activity and each subject
  *meansd: The average of the Standard Deviation of each measurement for each activity and each subject

The actual R script is included in this repository as "CodeBook.R"
