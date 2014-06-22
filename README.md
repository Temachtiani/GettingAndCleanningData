GettingAndCleaningData
====================================

# ANNOTATIONS:
## Place file "UCI HAR Dataset" on root level in order to find the right path to data.
## Output is saved as tab separated text format


run_analysis <- function(){
  
  # LIBRARIES
  library(data.table);
  library(stringr)
  
  # ESTABLISH PATHS
  path.data <- paste0(getwd(), "/UCI HAR Dataset/");
  
  # GET DATA
  data.test.X <- as.data.table(read.table(paste0(path.data, "test/X_test.txt")));
  data.test.Y <- as.data.table(read.table(paste0(path.data, "test/y_test.txt")));
  data.test.S <- as.data.table(read.table(paste0(path.data, "test/subject_test.txt")));
  data.train.X <- as.data.table(read.table(paste0(path.data, "train/X_train.txt")));
  data.train.Y <- as.data.table(read.table(paste0(path.data, "train/y_train.txt")));
  data.train.S <- as.data.table(read.table(paste0(path.data, "train/subject_train.txt")));
  
  data.F <- as.data.table(read.table(paste0(path.data, "features.txt")));
  data.A <- as.data.table(read.table(paste0(path.data, "activity_labels.txt")));
  

  # LOOK FOR INDEX ON VARIABLES NAMES WITH "MEAN" AND "STD"
  pos.mean <- as.vector(regexpr("mean", data.F$V2));
  pos.mean <- which(pos.mean > 0);
  pos.meanFreq <- as.vector(regexpr("meanFreq", data.F$V2));
  pos.meanFreq <- which(pos.meanFreq > 0);
  pos.mean <- setdiff(pos.mean, pos.meanFreq);
  pos.std <- as.vector(regexpr("std", data.F$V2));
  pos.std <- which(pos.std > 0);
  idx <- sort(c(pos.mean, pos.std));
  
  # SET DESCRIPTIVE NAMES TO DATA
  setnames(data.F, 1:2, c("VAR", "DESC"));
  setnames(data.A, 1:2, c("ACT", "DESC"));
  
  # SET DECRIPTIVE VARIABLE NAMES
  data.F.desc <- str_replace_all(as.character(data.F$DESC), "[[:punct:]]", "");
  setnames(data.test.X, 1:ncol(data.test.X), as.character(data.F.desc));
  setnames(data.train.X, 1:ncol(data.train.X), as.character(data.F.desc));
  
  # PRINT DIMENSION OF DATA TABLES
  print(dim(data.test.X));
  print(dim(data.test.Y));
  print(dim(data.train.X));
  print(dim(data.train.Y));

  # CREATE DESCRIPTIVE NAMES FOR ACTIVITY AND TYPE (TEST OR TRAIN)
  setkey(data.A, ACT);
  # FOR TEST
  setnames(data.test.Y, 1, "ACT");
  setkey(data.test.Y, ACT);  
  DescActTest <- merge(data.test.Y, data.A);
  IdTest <- cbind(TYPE=rep("TEST", nrow(data.test.X)), DescActTest, data.test.S);
  setnames(IdTest, 4, "SUBJ");
  # FOR TRAIN
  setnames(data.train.Y, 1, "ACT");
  setkey(data.train.Y, ACT);  
  DescActTrain <- merge(data.train.Y, data.A);
  IdTrain <- cbind(TYPE=rep("TRAIN", nrow(data.train.X)), DescActTrain, data.train.S);
  setnames(IdTrain, 4, "SUBJ");
  
  # SELECT VARIABLES WITH MEAN AND STD; ADD ID
  # FOR TEST
  TestMeanStd <- data.test.X[, idx, with=FALSE];
  TestMeanStd <- cbind(IdTest, TestMeanStd);  
  # FOR TRAIN
  TrainMeanStd <- data.train.X[, idx, with=FALSE];
  TrainMeanStd <- cbind(IdTrain, TrainMeanStd);
  
  # JOIN TEST AND TRAIN IN ONE TIDY TABLE
  TestTrain <- rbind(TestMeanStd, TrainMeanStd);
  print(head(TestTrain[, 1:5, with=FALSE]));
  print(tail(TestTrain[, 1:5, with=FALSE]));
  
  
  # GET MEANS OF FEATURES GROUPED BY ACTIVITY AND SUBJECT
  TestTrainMean <- TestTrain[, list(mean(tBodyAccmeanX), mean(tBodyAccmeanY), mean(tBodyAccmeanZ), mean(tBodyAccstdX), mean(tBodyAccstdY), mean(tBodyAccstdZ), mean(tGravityAccmeanX), mean(tGravityAccmeanY), mean(tGravityAccmeanZ), mean(tGravityAccstdX), mean(tGravityAccstdY), mean(tGravityAccstdZ), mean(tBodyAccJerkmeanX), mean(tBodyAccJerkmeanY), mean(tBodyAccJerkmeanZ), mean(tBodyAccJerkstdX), mean(tBodyAccJerkstdY), mean(tBodyAccJerkstdZ), mean(tBodyGyromeanX), mean(tBodyGyromeanY), mean(tBodyGyromeanZ), mean(tBodyGyrostdX), mean(tBodyGyrostdY), mean(tBodyGyrostdZ), mean(tBodyGyroJerkmeanX), mean(tBodyGyroJerkmeanY), mean(tBodyGyroJerkmeanZ), mean(tBodyGyroJerkstdX), mean(tBodyGyroJerkstdY), mean(tBodyGyroJerkstdZ), mean(tBodyAccMagmean), mean(tBodyAccMagstd), mean(tGravityAccMagmean), mean(tGravityAccMagstd), mean(tBodyAccJerkMagmean), mean(tBodyAccJerkMagstd), mean(tBodyGyroMagmean), mean(tBodyGyroMagstd), mean(tBodyGyroJerkMagmean), mean(tBodyGyroJerkMagstd), mean(fBodyAccmeanX), mean(fBodyAccmeanY), mean(fBodyAccmeanZ), mean(fBodyAccstdX), mean(fBodyAccstdY), mean(fBodyAccstdZ), mean(fBodyAccJerkmeanX), mean(fBodyAccJerkmeanY), mean(fBodyAccJerkmeanZ), mean(fBodyAccJerkstdX), mean(fBodyAccJerkstdY), mean(fBodyAccJerkstdZ), mean(fBodyGyromeanX), mean(fBodyGyromeanY), mean(fBodyGyromeanZ), mean(fBodyGyrostdX), mean(fBodyGyrostdY), mean(fBodyGyrostdZ), mean(fBodyAccMagmean), mean(fBodyAccMagstd), mean(fBodyBodyAccJerkMagmean), mean(fBodyBodyAccJerkMagstd), mean(fBodyBodyGyroMagmean), mean(fBodyBodyGyroMagstd), mean(fBodyBodyGyroJerkMagmean), mean(fBodyBodyGyroJerkMagstd)), by=list(DESC, SUBJ)];
                                                                  
  setnames(TestTrainMean, 3:ncol(TestTrainMean), paste0("M", data.F.desc[idx]));
  print(head(TestTrainMean));
  print(dim(TestTrainMean));
  print(dim(TestTrain));

  # write.table(TestTrainMean, paste0(path.data, "TestTrainMean.txt"), sep="\t", row.names=FALSE);


}
