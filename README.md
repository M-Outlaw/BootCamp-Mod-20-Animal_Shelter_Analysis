# BootCamp-Mod-20-Final_Project

### Data Cleaning
- Intake and Outcome csv files from the Austin Animal Center, which is the largest no-kill animal shelter in the U.S. and it is located in Austin, Tx.
- The intake file contained 135,708 animal records and was read into the intake_df dataframe.
- The outcome file contained 135,974 animal records and was read into the outcome_df dataframe.
- We wanted to only look at dog records. 
  * Therefore, the intake_df and outcome_df dataframes was filtered for only dogs.
  * This reduced the intake_df dataframe to 76,356 dogs.
  * This reduced the outcome_df dataframe to 76,374 dogs.
    - Interesting note is that the number of dogs in the intake file did not match the outcome file.
 
#### Dog Age
- The first step was to convert the Age upon Intake column from a string value to an integer.
  * Not only was the column a string value but it contained different units from number of days to number of years.
  * Therefore, after spliting the string into the number and the unit of time, the value was converted a fraction of a year if it was less than a year, so that all of the values had the same units and only the float value was left in the column.
- Age was then binned to reduce the number of different values and the fact that most people prefer to adopt puppies or young dogs.
  * Puppies: dogs 0 to 1 year old
  * Young: dogs 1 to 3 years old
  * Adult: dogs 3 to 7 years old
  * Senior: dogs over 7 years old

#### Male/Female 
-Then, the male and female column was combined with whether the dog had been spayed or neutered or still intact.
  * This was converted into a true Male/Female column and an Intactness column.
  * Since we did not really care whether the dog was spayed or neutered, because the only difference is that spayed refers to a female and neutered refers to a male, this column was converted to just intact or altered.

#### Merging the income_df and outcome_df dataframes
- Merging the dataframes took a little more effort than just merging by index.
  * The databases did not contain the same number of rows.
  * Also, some dogs went through the shelter more than once.
- The first step in merging was convert the DateTime columns in both databases to be in year, month, then day format so that the dates could be sorted from earliest to latest.
- Then, the dataframes were merged by Animal ID
  * The dogs that only entered and exited the shelter once are automatically perfectly matched up.
  * The dogs that entered and exited the shelter more than once created extra lines when merged.
    -- When merged, a row for every combination of entered time and outgoing time possible is created.
- To line up the outgoing times to the correct entering times:
  * First, the merged dataframe was filtered to remove any rows where the outgoing times are before the entering times since that is not possible.
  * Then, a Drop column was added to mark which rows should be dropped and which should not be dropped.
    -- Going row by row, if the animal Id was the same as the animal Id of the row before and the entering time is the same as the row before, only keep the row with the earliest outgoing time.
  * With all of the rows labeled, drop any row that has Yes in the Drop column.

#### Dog Breeds
- First, any breed that is classified as a Mix or has two different breeds listed is labeled as a mix and any that only has one breed listed is labeled as pure breed.
- The breed strings were split up to remove any instance of mix since we already created the mix column.
- Any time the breed is labeled twice, we removed one of the instances.
- Once the breeds were seperated, a csv of breed information that uses the American Kennel Club information and provides the group the breed belongs to and the size of the dog was then mapped to each row based on the breeds.

#### Color of the Dog
- 
  
  
  
  
  
