Measurment of movements in differents activities of volunteers
==============================

Description of  experiment
-------------------------
Experiment consistes in registering variales of monitoring different mesurements of movement of volunteers in different activities.

General description of variables
--------------------------------
Variables included are related to:
1. Triaxial acceleration from the accelerometer
2. Triaxial Angular velocity from the gyroscope. 
3. A 561-feature vector with time and frequency domain variables. 
4. Its activity label. 
5. An identifier of the subject who carried out the e

Names of variables has prefix:
* tBodyAcc
* tGravityAcc
* tBodyAccJerk
* tBodyGyro
* tBodyGyroJerk
* tBodyAccMag
* tGravityAccMag
* tBodyAccJerkMag
* tBodyGyroMag
* tBodyGyroJerkMag
* fBodyAcc
* fBodyAccJerk
* fBodyGyro
* fBodyAccMag
* fBodyAccJerkMag
* fBodyGyroMag
* fBodyGyroJerkMag

The variables estimated form signals are self-desctiptive in name and are added to names as suffix:

* mean(): Mean value
* std(): Standard deviation
* mad(): Median absolute deviation 
* max(): Largest value in array
* min(): Smallest value in array
* sma(): Signal magnitude area
* energy(): Energy measure. 
* iqr(): Interquartile range 
* entropy(): Signal entropy
* arCoeff(): Autorregresion coefficients with Burg
* correlation(): correlation coefficient between two signals
* maxInds(): index of the frequency component with largest magnitude
* meanFreq(): Weighted average of the frequency components to obtain a mean frequency
* skewness(): skewness of the frequency domain signal 
* kurtosis(): kurtosis of the frequency domain signal 
* bandsEnergy(): Energy of a frequency interval within the 64 bins of the FFT of each window.
* angle(): Angle between to vectors.

Units

train/Inertial Signals/body_gyro_x_train.txt: The angular velocity vector measured by the gyroscope for each window sample. The units are radians/second.


The dataset is cncentrated on the following files:
---------------------------------------
* 'features.txt': List of all features.
* 'activity_labels.txt': Links the class labels with their activity name.
* 'train/X_train.txt': Training set.
* 'train/y_train.txt': Training labels.
* 'test/X_test.txt': Test set.
* 'test/y_test.txt': Test labels.
