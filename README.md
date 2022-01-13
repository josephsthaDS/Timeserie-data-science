1. Title: Activity Recognition from a Single Chest-Mounted Accelerometer
	Updated Nov, 2013 , P. Casale, email: plcasale@ieee.org
	
2. Abstract: The dataset collects data from a wearable accelerometer mounted on the chest. Uncalibrated Accelerometer Data are collected from 15 participants performing 7 activities. The dataset is intended for Activity Recognition research purposes. It provides challenges for identification and authentication of people using motion patterns.

3. Relevant Information:
   --- The dataset collects data from a wearable accelerometer mounted on the chest
   --- Sampling frequency of the accelerometer: 52 Hz
   --- Accelerometer Data are Uncalibrated
   --- Number of Participants: 15
   --- Number of Activities: 7
   --- Data Format: CSV

4. Dataset Information
   --- Data are separated by participant
   --- Each file contains the following information
       ---- sequential number, x acceleration, y acceleration, z acceleration, label 
   --- Labels are codified by numbers
       --- 1: Working at Computer
       --- 2: Standing Up, Walking and Going up\down stairs
       --- 3: Standing
       --- 4: Walking
       --- 5: Going Up\Down Stairs
       --- 6: Walking and Talking with Someone
       --- 7: Talking while Standing
       
5. Reference Papers

   --- Casale, P. Pujol, O. and Radeva, P. 
       "BeaStreamer-v0.1: a new platform for Multi-Sensors Data Acquisition in Wearable Computing Applications", 
       CVCRD09, ISBN: 978-84-937261-1-9, 2009
       available on https://www.researchgate.net/publication/257132489_BeaStreamer-v0.1_a_new_platform_for_Multi-Sensors_Data_Acquisition_in_Wearable_Computing_Applications?ev=prf_pub

   --- Casale, P. Pujol, O. and Radeva, P. 
       "Human activity recognition from accelerometer data using a wearable device", 
       IbPRIA'11, 289-296, Springer-Verlag, 2011
       available on https://www.researchgate.net/publication/221258784_Human_Activity_Recognition_from_Accelerometer_Data_Using_a_Wearable_Device?ev=prf_pub
       
  --- Casale, P. Pujol, O. and Radeva, P. 
       "Personalization and user verification in wearable systems using biometric walking patterns"
       Personal and Ubiquitous Computing, 16(5), 563-580, 2012
       available on https://www.researchgate.net/publication/227192676_Personalization_and_user_verification_in_wearable_systems_using_biometric_walking_patterns?ev=prf_pub
       
       **Solution Summary

To solve the time series features classification problem, I mainly explored data distribution with subjects and found it to be balanced. On the other hand, data was not evenly distributed across classes. I further checked spectrograms to identify unique time-frequency characteristics. Some unique characteristics I observed from spectrograms was that walking, going up/down, and walking and talking has repeating overtones in higher frequency bands. 

For feature generation, given the time constraints for completing this assignment, I computed mainly rolling time series and rolling Fourier transform features for each component. In addition to that, I added sequential information by making use of lagged time series. It should be noted that the time series were shifted for each subject and activity pair separately and rows with no lagged values were removed. I used Random Forest as the machine algorithm to train the model. For validation, I used two approaches. One was the “normal approach” where I used train and test split where all participants can be in the train and test data. The drawback of this type of validation is that the model isn’t being validated based on unseen data, it is just being trained to identify unseen instances of available data/subjects. The second approach is where we split each subject as training and testing data called the “personalized approach”. Here we try to learn the model’s ability to learn individual behavior patterns. Here we can understand the model’s ability to learn individual behaviors. One approach I didn’t try was leaving one subject out and treating the rest of the data as training data for validation. This will check our model’s ability to see unseen behavior. However, we run the risk of not learning enough from the data. For example, a 15-20 person might run up the stairs or a 200-300 lbs person might have slower motions while climbing the stairs. 

For evaluation metrics, I first randomly undersample every class except the minority class. Once the class distribution is balanced, I use accuracy. The main reason we chose accuracy is that it doesn’t penalize false positives or false negatives, but maintains a balanced approach. An example where false positive might not be a problem is filtering out graphic violent content on a social media platform. Filtering out normal content accidently is less harmful than letting graphic violent content through. I obtained 85% accuracy with Random Forest with 8-fold crossvalidation with little variance in the accuracy scores for the normal approach. While for the personalized approach where the model is trained on each subject separately, the accuracy is above 70% for nearly all subjects. However, this gives a good idea about the model’s ability to learn the person’s behavior. 

Using the “normal approach” we got a 67% testing accuracy on the test data. From the confusion matrix, we found that 4 classes are well classified, namely, Working at Computer, Walking, Going Up/Down Stairs, Talking while Standing. We can improve the accuracy mainly by adding more wavelet based features. In the trained model “minimum of rolling 15 sample (0.28 sec) feature” and “maximum of rolling 15 sample (0.28 sec) feature” have the highest feature importance. To move forward, I would train the model with 52 samples or 104 samples to add more sequential information. In such a case, diving the time series data to segments might also reduce the amount of rows to be trained and increase model speed and performance. I would also try other algorithms like RNN, XGBoost Classifier, Naïve Bayes Classifier and LSTM to improve classification accuracy. RNN and LSTM are particulary designed for sequential data.

If the intention to apply this model is to more naturalistic setting, more naturalistic data would be required. Also, it is also important to include more data from subjects representative of different age groups, weight, height, gender and country would give us the ability to make more generalizable predictions to be made on new subjects. Finally, care should be taken to get calibrated measurements from sesnors with the same sensitivity. In the case of sensors with unequal sensitivities, we can apply corrections or scaling to compute the features.
