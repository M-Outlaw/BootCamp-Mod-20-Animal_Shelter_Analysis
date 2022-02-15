# BootCamp-Mod-20-Animal_Shelter_Analysis

## Project Outline
We will be analyzing animal shelter intake and outake data from Austin, TX with the initial goal to predict the outcome for a dog who enters the shelter.

## Team Members
- [Michelle Outlaw](https://github.com/M-Outlaw)
- [RoseAnne Amimo](https://github.com/01ramimo)
- [Anna Wiste](https://github.com/annawiste)
- [Jordan Thomas](https://github.com/Jthomas856)

## Tools and Techniques
We plan to use the following for the project:
- Pandas for data cleaning
- SQL for our database
- Python (SciKitLearn- Machine Learing Classifier Model
- Tableau for dashboard presentation

## Anticipated Challenges
We anticipate that there will be some challenges with cleaning and transforming the data to make it useable for our model. While we do have a large dataset, it is still limited to one city in the US so we do not have a representation of the entire country. Despite this we feel the data is strong enough to make meaningful predictions. Through the evolution of the project it is likely that our initial question that we are asking of the data might change, thus changing the direction of our project. Finally there will be some challenges in ensuring we interpret and present the data in a way that provides value to our identified stakeholders.

## Dataset
The dataset we plan to use includes ~76000 encounters involving dogs at the Austin Animal Center (Austin, TX) from 2013 through the present. 
Data is publicly available through the Austin open data portal. Data is available on [intakes](https://data.austintexas.gov/Health-and-Community-Services/Austin-Animal-Center-Intakes/wter-evkm) and [outcomes](https://data.austintexas.gov/Health-and-Community-Services/Austin-Animal-Center-Outcomes/9t4d-g238). 
Possible outcomes include:
- adoption
- return to owner
- euthanasia
- transfer

Intake fields include:
- date of encounter
- type of encounter (stray, owner surrender, public assist, etc)
- sex 
- age
- breed
- color

Additional features can be extracted/estimated from these fields, including:
- spay/neuter status
- size
- breed type
- breed restriction status
- length of stay
- prior encounters

## Data Cleaning
- The biggest component of the data cleaning was to merge the intake and outcome files into one dataframe, once both files were filtered for only dogs.
  * Merging the files required ordering and filtering to make sure that the intake date lined up with the correct outgoing date since many of the dogs went through the shelter multiple times
- Other Main Components of Cleaning:
  * Dog Age
    - Consolidating case differences for different string inputs
    - Converting all strings to numbers where values given as days, weeks, or months is converted to a fraction of a year
  * Dog Breed
    - Removing notations of Mix to be able to compare the breeds
    - Consolidating when the same breed is listed twice
  * Dog color
    - Alphabetizing the color column to match when the dog is the same color but in a different order, such as black/white and white/black since those dogs have the same coloring
    - Reducing the categories down from 383 to 17
  * Breed Group and Size
    - Mapping each breed to the breed’s group and size according to the American Kennel Club records
  * Austin Breed Restrictions
    - Mapping each breed to whether it is on the list of which breeds are not allowed when renting in Austin.

## Database Integration
#### •	Connect Pandas and SQL

Created a new database and used the built-in “to_sql ()” method in Pandas to create tables for Austin Animal Center (AAC) data (AAC_Intakes, AAC_Outcomes, BreedInfo, BreedRestrictionAustin, and CleanedData).

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

<h3 align="center"><b>Naive Bayes Model</b></h3>
<p align="center"><img src="https://github.com/M-Outlaw/BootCamp-Mod-20-Animal_Shelter_Analysis/blob/main/Graphics/NB_Confusion_Matrix.png" width="283" height="150"/>&nbsp;<img src="https://github.com/M-Outlaw/BootCamp-Mod-20-Animal_Shelter_Analysis/blob/main/Graphics/NB_Classification_Report.png" width="459" height="223"/></p>

## Presentation/Dashboard

- [Presentation slides](https://docs.google.com/presentation/d/1OiE5D7VYmm6KsXCUUHtG2Hv3Lr9HjXpQY_gYK4ltiK4/edit?usp=sharing)

The dashboard is intended to be used to supplement the machine learning model to further analyze length of stay. The user is able to filter by the dog's sex, age group, breed type, size and color to see a detailed analysis of median length of stay. This allows the user to better interpret the machine learning model and see trends from the data to make decisions on how to better allocate resources. For example, upon intake the shelter runs the dog description through the model and gets an outcome that the dog is likely to be a prolonged stay, the shelter can use the dashboard to see a length of stay analysis based on past intakes of similar dogs.

- [Dashboard](https://public.tableau.com/app/profile/jordan.thomas5085/viz/LengthofStayDashboard_16447681694690/LOSDashboard?publish=yes)

## Challenges

## Future Directions
- Look at the change that occured with Covid/ Compare pre and post Covid
