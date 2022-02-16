# Additional Notes on Model Development

## Encounter Outcome

Code is in OutcomeModeling.ipynb

- Our first model addressed classification of the outcome of the encounter, starting with 5 primary categories. 
  
- Two models were tested with the 5 category outcome: Random Forest, and Gradient Boosting Classifier.
Both models had accuracy scores of ~0.6. 

      Although we had anticipated a distinction between adoptions and transfer, examining the confusion matrix, both models had particular issues with classifying transfers as adoptions. 

      Considering that our multiclass outcome is challenging for modeling, we decided to consolidate the five outcomes into three, combining adoptions with transfers, and euthanasia with died. 

- The 5 primary categories for outcome were then consolidated into 3: Adoption/Transfer, Return to Owner, and Death.
 
- Accuracy scores for the three class outcome improved to 0.76 and 0.78, respectively, but recall values for the rare outcome (Death) were very low. 

- We then did oversampling with SMOTE (scikit-learn), and then repeated both models. Overall accuracy scores were slightly lower, but recall for the rare outcome did increase. 

- For real world application, the models predicting outcome would need to be significantly better than we found. At this point we changed our focus to length of stay at the shelter. 

