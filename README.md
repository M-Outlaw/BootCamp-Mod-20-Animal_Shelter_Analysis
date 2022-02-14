## Machine Learning Model
1. After initial preprocessing, some additional processing was required to prepare the features. All features in our dataset are categorical. Therefore, we did not scale the data, since all features will be on the same scale already (0,1).
- All existing binary features were recoded to 0,1 (sex, spay/neuter, mixed breed, breed restrictions, prior encounters).
- All categorical features with more than two values were dummy coded. (Encounter type, condition at intake, age category, month, breed type, size and color)

2. Two alternate outcomes were generated from the existing data. 
- The 5 primary categories for outcome were consolidated into 3: Adoption/Transfer, Return to Owner, and Death.
- A binary outcome was generated based on length of stay, splitting the data at the 75th percentile. 

3. Data was split into training and testing sets using the default 75/25 split with scikit learn.

4. Because all data are binary, we did not amploy any scaler. 

5. Two models were tested with the 5 category outcome: Random Forest, and Gradient Boosting Classifier.
Both models had accuracy scores of ~0.6. 

      Although we had anticipated a distinction between adoptions and transfer, examining the confusion matrix, both models had particular issues with classifying transfers as adoptions. 

      Considering that our multiclass outcome is challenging for modeling, we decided to consolidate the five outcomes into three, combining adoptions with transfers, and euthanasia with died. 

6. Accuracy scores for the three class outcome improved to 0.76 and 0.78, respectively, but recall values for the rare outcome (Death) were very low. 

7. We then did oversampling with SMOTE (scikit-learn), and then repeated both models. Overall accuracy scores were slightly lower, but recall for the rare outcome did increase. 

8. For real world application, the models predicting outcome would need to be significantly better than we found. At this point we changed our focus to length of stay at the shelter. 

9. We chose to define prolonged stay at the shelter as 13 days and longer. This cut point leads to the longest 25% of encounters being defined as cases. It is also a logical cutpoint based on the distribution of length of stay, marking the beginning of the very long tail of the distribution. 

10. As before, we tested Random Forest and Gradient Boosting Classifier, but also added Naive Bayes. Because all of our data are binary, we have used the Bernoulli application.

11. We again used SMOTE and SMOTEEN to test whether resampling could improve performance of the model. While recall for the minority class is improved, it comes at great cost to other performance metrics, and overall substantially reduced accuracy.

12. If deployed, the goal of the model predicting long stays is to provide shelter staff with insight into which dogs are at risk of lengthy stays, so that interventions designed to move dogs out of the shelter faster can be focused on the dogs most likely to benefit.

13. We have chosen the Naive Bayes model, without resampling, as our final model. While the overall accuracy is slightly lower than Random Forest or Gradient Boosting Classifier, the Naive Bayes model results in the highest precision for the majority class (0.79), and highest recall for the minority class (0.39). These are the two metrics which we prioritize.
High precision for the majority class provides confidence that those encounters predicted to be of normal length will be. Interventions can then be focused on dogs at risk. 
A higher recall for the majority class indicates that the prediction is capturing more of the true cases. The recall with Naive Bayes was relatively low, at 0.39, but this was an improvement over the other two models.

The model has provided a reduced number of at risk encounters, allowing more efficient use of resources on interventions.
An additional attractive element of Naive Bayes in this application is its relative ease of interpretation.
      
