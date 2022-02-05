## Machine Learning Model
1. After initial preprocessing, some additional processing was required to prepare the features. All features in our dataset are categorical. Therefore, we did not scale the data, since all features will be on the same scale already (0,1).
- All existing binary features were recoded to 0,1 (sex, spay/neuter, mixed breed, breed restrictions, prior encounters).
- All categorical features with more than two values were dummy coded. (Encounter type, condition at intake, age category, month, breed type, size and color)

2. Two alternate outcomes were generated from the existing data. 
- The 5 primary categories for outcome were consolidated into 3: Adoption/Transfer, Return to Owner, and Death.
- A binary outcome was generated based on length of stay, splitting the data at the 75th percentile. 

3. Data was split into training and testing sets using the default 75/25 split with scikit learn.

4. Two models were tested with the 5 category outcome: Random Forest, and Gradient Boosting Classifier.
Both models had accuracy scores of ~0.6. 
Although we had anticipated a distinction between adoptions and transfer, examining the confusion matrix, both models had particular issues with classifying transfers as adoptions. 
Considering that our multiclass outcome is challenging for modeling, we decided to consolidate the five outcomes into three, combining adoptions with transfers, and euthanasia with died. 
Accuracy scores improved to 0.76 and o.78, respectively, but recall values for the rare outcome (Death) was very low. 
Therefore, we did oversampling with SMOTE, and then repeated both models. 
Overall accuracy scores were slightly lower, but recall for the rare outcome increase slightly. 
