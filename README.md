# gettingandcleaning
submission for final project
Repo contains:
Codebook: Includes data processing steps and variable information
Complete Data processing script with comments for each part
______________________________________
Script:
#Setting Working Directory
run_analysis.R <- traindata <- setwd("/Users/jomr/Desktop/Coursera/Cleaningdata/Proyecto final/UCI HAR Dataset")
#opening file that contains data labels
datalabels <- read.table("activity_labels.txt")
#opening and merging all the test data
testlabelsscript <- read.table("/Users/jomr/Desktop/Coursera/Cleaningdata/Proyecto final/UCI HAR Dataset/test/y_test.txt")
testsetscript <- read.table("/Users/jomr/Desktop/Coursera/Cleaningdata/Proyecto final/UCI HAR Dataset/test/X_test.txt")
testsubjectsscript <- read.table("/Users/jomr/Desktop/Coursera/Cleaningdata/Proyecto final/UCI HAR Dataset/test/subject_test.txt")
testdfscript <- cbind(testsubjectsscript, testlabelsscript, testsetscript)
#naming columns
colnames (testdfscript) <- c ("subjectnumber", "activitylabel", paste("Measurement", c (1:561), sep=""))
#converting to tbl_df
testdfscript <- tbl_df(testdfscript)
#opening and merging all the training data
trainlabelsscript <- read.table("/Users/jomr/Desktop/Coursera/Cleaningdata/Proyecto final/UCI HAR Dataset/train/y_train.txt")
trainsetscript <- read.table("/Users/jomr/Desktop/Coursera/Cleaningdata/Proyecto final/UCI HAR Dataset/train/X_train.txt")
trainsubjectsscript <- read.table ("/Users/jomr/Desktop/Coursera/Cleaningdata/Proyecto final/UCI HAR Dataset/train/subject_train.txt")
traindfscript <- cbind(trainsubjectsscript, trainlabelsscript, trainsetscript)
#naming columns
colnames (traindfscript) <- c ("subjectnumber", "activitylabel", paste("Measurement", c (1:561), sep=""))
#converting to tbl_df
traindfscript <- tbl_df(traindfscript)
#adding an origin database identifier column
testdfscript <- mutate(testdfscript, origindatabase ="test")
traindfscript <- mutate(traindfscript, origindatabase ="training")
#merging train_df and test_df
complete <- bind_rows(testdfscript, traindfscript)
#creating database with mean and sd as for each observation
mean <- mutate(complete, MeasurementMean = rowMeans(select(complete, starts_with("Measurement")), na.rm = TRUE))
sd<- mean %>%
      rowwise() %>%
      mutate(stdev = sd(c(Measurement1:Measurement561, na.rm=TRUE)))
#selecting only subject, activity, mean and sd
database <- select(sd, origindatabase, subjectnumber, activitylabel, MeasurementMean, stdev)
#labeling the activities
database$activitylabel <- factor(database$activitylabel, labels=c("Walking", "Walking Upstairs", "Walking Downstairs", "Sitting", "Standing", "Laying"))
#producing final db
database <- group_by(database, activitylabel, subjectnumber)
tidydb <- dplyr::summarise(database, meanaverage=mean(MeasurementMean), meansd= mean(stdev))
#exporting final db
write.table(tidydb, file="course_submission.csv")
write.table(tidydb, file="course_submission.txt", sep="\t")
