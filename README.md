GettingAndCleaningData
====================================

Description of script run_analysis.R
======================================

About source dataset
-------------------------------------
* Place file "UCI HAR Dataset" on root level in order to find the right path to data.
* Output is saved as tab separated text format

Preparing initial dataset
--------------------------------------
* Load labels activities, feature names and data for train and test data.
* Build ID for each data set (train and test) with a column variable TYPE (TRAIN, TEST), column ACT with activity code and descriptive name od activities (DESC).
* Eliminate regular expresions from names o fetures ("-" and "()").
* Change names of trainX and testX variables to names without regular expresions.
* Merge ID and data for each data set: train and test.
* Find indexes (columns) with variables that has strings "mean()" or "std()".
* Select columns that corresponds to indexes found in the step before for train and test datasets.
* Add column TYPE with "TRAIN" or "TEST" in correspondance with each dataset.
* Merge Train and Test datasets in one dataset named TestTrain.
* Calculate grouped mean of variables according to Activity and Subject category, which is names TestTrainMean. This is the tidy final dataset.

Criterions used to assign descriptive names of activities and variables and construction of final tidy dataset.
-------------------------------------------------------
* As activity names given in activity_labels.txt file are self-descroptive, the file was loaded as data.fame to R script and asigned in a new column named DESC in correspondance to ACT column with key for activities.
* Variables names ara also self-descripitve, but it contains regular expresions caracters in features.txt. In order to assign valid names, characters like "-" and "()" were removed and the remain is assigned as variables names.
* Performing a search on variable names, the comuns or data selected corresponds to all that include the string "mean()" or "std()".
* Variables with string "meanFreq()" are not included on tidy data set.
* The average for all filter variables were calculated grouping features activity-subject categories.
* The source dataset include a variable that identifies types of data (TRAIN or TEST). This categories are not used to group to calculate means of columns.
