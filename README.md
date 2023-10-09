# ModelEvaluation
 Model evaluation is used in machine learning to evaluate the performance of a model and compare different models in order to choose the best performing one. Examples include precision, recall, area under curve, F1 score.
For the model car dataset was used  first I used only these columns:Make, Model, Year, Engine HP, Engine Cylinders, Transmission Type, Vehicle Style, highway MPG, city mpg, MSRP then I replaced lowercase the column names and replace spaces with underscores, filled the missing values with 0 and made the price binary (1 if above the average, 0 otherwise) - this will be our target variable above_average.
I Split the data into 3 parts: train/validation/test with 60%/20%/20% distribution. Use train_test_split function for that with random_state=1
ROC AUC could also be used to evaluate feature importance of numerical variables.
For each numerical variable, use it as score and compute AUC with the above_average variable, Using the training dataset for that
If your AUC is < 0.5, invert this variable by putting "-" in front (e.g. -df_train['engine_hp'])
AUC can go below 0.5 if the variable is negatively correlated with the target variable. I can change the direction of the correlation by negating this variable - then negative correlation becomes positive. numerical variable that has the highest AUC Is engine_hp.
Training the model i applied one hot encoding using DictVectorizer and train the logistic regression with these parameters: LogisticRegression(solver='liblinear', C=1.0, max_iter=1000) AUC of this model on the validation dataset is 0.979. compute precision and recall for the model. I evaluated the model on all thresholds from 0.0 to 1.0 with step 0.01 For each threshold, I compute precision and recall , threshold precision and recall curves intersect at 0.48.
F1 score:  precision and recall are conflicting - when one grows, the other goes down. That's why they are often combined into the F1 score - a metrics that takes into account both
This is the formula for computing
F_1 = 2 \cdot \cfrac{P \cdot R}{P + R}
Where p is precision and R is recall.
computing F1 for all thresholds from 0.0 to 1.0 with increment 0.01, threshold F1 is maximal at 0.52.
Using  KFold class from Scikit-Learn to evaluate our model on 5 different folds: I Iterated over different folds of df_full_train, split the data into train and validation, trained the model on train with these parameters: LogisticRegression(solver='liblinear', C=1.0, max_iter=1000) Used AUC to evaluate the model on validation, standard devidation of the scores across different folds is 0.003.
Hyper parameter tunning: use 5-Fold cross-validation to find the best parameter C, Iterated over the following C values: [0.01, 0.1, 0.5, 10] Initialized KFold with the same parameters as previously Using these parametes for the model: LogisticRegression(solver='liblinear', C=C, max_iter=1000) I computed the mean score as well as the std (round the mean and std to 3 decimal digits). The C that leads to the best mean score is 10.
