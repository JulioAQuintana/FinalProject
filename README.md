# NFL Super Bowl Champion 

## Overview

 **Offense wins games, but defense wins championships**
 
We would like to put this to the test. The NFL provides a wide range of statistics for each player, which we will aggregate for each team in the league. Statistics will then be categorized into those pertaining to "Defensive Stats", "Offensive Stats", and "Special Teams". Since there are a lot of statistics within each of those categories we will examine which are really valuable by applying EDA, and then Principal Component Analysis will be used to extract the principal components of each category. We will then train a classifier (Random Forest) that can accurately predict the likelihood of a team being the champion, with the intention of analyzing the feature importance of principal components representing "Defensive Stats", "Offensive Stats", and "Special Teams", respectively. 

### Reason why we selected this topic

As NFL fans, we would like to have a better understanding of which stats can be relevant for our teams to turn out as champions, and we think this tool could help to bring some light on the matter.

### Data Source

After some searching we determined that the best source available is a Kaggle dataset (https://www.kaggle.com/kendallgillies/nflstatistics) that is broken down unto 19 csv's containing players' basic information, career stats and game logs with a variety of statistical metrics. 

Before deciding on Kaggle we considered some other sites, but they mostly specialize on betting and have already done some of the work we want to do for our project, and we would prefer to do it on our own to make sure the results are not biased or have taken into consideration the wrong variables.

### Questions we hope to answer

The main quiestion we hope to answer is: Is it possible to predict the Super Bowl champion considering the available statistics?

We can consider some other questions that we should answer in order to achieve our main goal, such as:
- Which set of stats (Offense, Defense or Special Teams) weights the most for a team to be champion?
- Which specific stats weight the most for a team to be champion?

## Glosary

- Tackle: Stoping the advance of the opponent carrier (player in posession of the ball).
- Sack: Capturing the Quarterback while he is still in posession of the ball.
- Rush: Advancing the ball by running (no pass or kick is involved in the play).
- Pass: Attempt to advance the ball by throwing it to a teammate. 
- Reception: Recieving the pass thrown by the Quarterback.
- Field-goal: Football kick that requires a player to kick the ball into the opposing team's goal post, it's worth three points.
- Extra-Point: Football kick that follows a touchdown and requires a player to kick the ball into the opposing team's goal post, it's worth one point.
- Down: A play or attempt. Every time a team gets the posession of the ball it has four downs (attempts) to advance the ball ten yards from the position where the first down starts.

## Database

Our database is hosted in PostgreSQL and we uploaded to it the cleaned datasets in the "Career stats" category. For further information on the process of cleaning the data please refer to the files in the folder "Data_Preparation" in this project, which contains the files used for preparing each dataset for analysis and later use for the model, as well as the file "NFLDB_script" with the queries to create the tables in our database .  

## Exploratory Data Analysis

In order to ensure that we train our model with the right data we must first analyze it and see if all our variables are relevant or if some of them might be duplicating information.

### Merging clean datasets

In previous steps we cleaned our original datasets to homologate names and years criterion, as well as to drop some clearly unnecessary data, and in this step we merge them into a new dataframe according to the team and year.

### Selecting period to consider in the model

Once we have our merged dataframe we create a new column holding only the year, to see if there is any tendency common to all teams related to a chronological feature.

![safeties_def](https://user-images.githubusercontent.com/89816213/157457714-a1b110c1-0eb9-4e30-88af-e8d4eb6ff5db.PNG)

![sacks_def](https://user-images.githubusercontent.com/89816213/157457706-cc3b02f8-cc56-4df3-9e49-1328cc78ff58.PNG)  

![Total_tackles](https://user-images.githubusercontent.com/89816213/157457735-03d1c44b-32dd-4cfd-bccc-47d75093069d.PNG)

While processing the information we realized  that many variables hold no information for the years shown, therefore our next logical step is to see which variables are available through the years, so we can determine a period of study holding data for all relevant variables.

We can see that not all statistics were registered for all the years in the dataset. Actually, one of the most important variables ("total_tackles") is considered only after the year 2000. According to this, our study has to consider only data from 2001 to 2016.

![stats_years](https://user-images.githubusercontent.com/89816213/157457993-2bf833a3-827f-45d7-983c-ecd0003c174b.PNG)

### Redundancies and correlated data

Once we had chosen our period of study, we looked for any kind of duplicity among the variables we considered, and found that there were many variables that either referred to the same stat or worked better when considered as a ratio. The criterion taken to solve this redundancies
are explained in detail in the code used for the EDA (stat_analysis_by_year.ipynb).

### Variables created for our study

Some of the variables we dropped might be relevant if one is trying to measure the performance of a single player, but are not relevant when aggregated to the whole team's performance. On the other hand, these variables still offered valuable information, they just needed to be used in the correct way. With that in mind we created the following variables:

- Field goal success rate (fg_success_rate): Rate of successful field goal attempts (fgs_made_kick) divided by the total field goals attempted (fgs_attempted_kick).
- Extra point success rate (extra_success_rate): Rate of successful extra point attempts (fgs_made_kick) divided by the total extra points attempted (fgs_attempted_kick).
- Pass success rate (pass_success_rate): Number of passes completed (passes_completed_pass) divided by the total passes attempted (passes_attempted_pass).
- Average yards per pass (avg_yards_pass): Total receiving yards (recieving_yards_rec) divided by the total passes completed (receptions_rec).
- Average yards per rush (avg_yards_rush): Total rushing yards (rushing_yards_rush) divided by the total rushing attempts (rushing_attempted_rush).
- Total Kicks blocked (kicks_blocked): Kicks (field goal) attempted and blocked by the defense, plus extra points attempted (extra_points_blocked_kick) and blocked by the defense.
- Passing attempts to rushing attempts ratio: Total passes attempted (passes_attempted_pass) divided by the total rushing attempts (rushing attempted rush).

### Final dataset 

After performing the EDA process and getting to know which variables we want to feed the model with, we created a new dataset with these variables and added a new one to show which teams were champions in which years, to do so we imported the Super Bowl champions dataset (superbowl.csv) and created a new column holding "1" for all team_years, and then performed a left join with "0" in all empty values. This file is saved as "sb_champion_stats" in the "Resources" folder.

## Machine Learning Model

### Source

Our model connects with the tables in our PostrgreSQL Database using SQLAlchemy. The input for the model is the "sb_champion_stats" file created int the last step of the EDA process. 

### Libraries

We used Pandas, Numpy and PyPlot to perform some further analysis of our data, given the results from Scikit-learn and Imb-learn imports, which are used to create and evaluate Machine Learning models.

### Adressing target variable imbalance

Our objective is to predict the Super Bowl champions (target variable) indicated as "1" in the "Champion" column of our dataset and, as we know, there is only one champion for each year, but we have 31 teams that are not champions ("0") for that same period! If we pretend to create random testing and training tests, most of the times they will not have a champion in their set and the model would not learn what stats are involved in "creating a champion".

This problem is adressed by using random over and under sampling, as well as by using SMOTE oversampling and SMOTEEN. The specifics for each method are shown in the "ML_Model_first_iteration.ipynb" file, as well as in the subsequent iterations (second and third).

### Principal Component Analysis

We expected to answer which variables were the most relevant to predict the Super Bowl champion, and in such endeavour decided to perform PCA to determine also which set of variables is more important. The results are as follows:

![Variance](https://user-images.githubusercontent.com/89816213/156940011-95c24bab-671a-4c2d-87e7-39140f0c9d40.PNG)

![Cumulative](https://user-images.githubusercontent.com/89816213/156940014-999b9ea2-37b4-4556-9667-8f6526678f06.PNG)

This results show that we would need at least 5 variables to predict the Super Bowl champion, and that  means they could be scattered among any of the categories. The only task left is to focus on predicting the SuperBowl champion.

### Supervised learning Models

Since we are working with labeled data and we have a clear target it's logical to use supervised learning models. In the "ML_Model" files we show the process and results for the following methods:
- Logistic Regression
- Random forest Classifier
- Balanced Random Forest Classifier (this specific method balances the observations by itself, so it doesn't need previous balancing)

### First iteration (refer to Model/ML_model_first_iteration.ipynb)

During this iteration all models were unable to predict the champions accurately, no matter which balancing technique was used. It's also relevant to point that eventhough some models did manage to have "acceptable" perfromance they did so by affecting the predictions of "not champion" observations.

In this model we performed a study of the feature importance and compared the p-values to see which variables might be obstaculizing our model rather than helping it learn. From this analysis we decided to exclude from our study "avg_yards_rush", "avg_yards_pass", "ints_def" and "total_tackles_def".

### Second iteration (refer to Model/ML_model_second_iteration.ipynb)

The process for the second iteration was pretty much the same as in the first iteration, with the only difference that we dropped the variables that, according to our analysisis performed in the previous iteration, were not relevant for the model. Strange enough, but the models actually performed even worse than in the previous iteration!

This, of course, tells us that even if the variables dropped seemed not to be relevant on their own, there is some correlation between them and a team's success. With that in mind we ran a new iteration.

### Third iteration (refer to Model/ML_model_third_iteration.ipynb)

For this iteration we decided to re-run the process without dropping the variables we eliminated in the second iteration and try some other ways to improve the performance of our model.

An investigation on how some programers have solved this problem showed that many of them improved their models by increasing the thresholds of their models. This, simply explained, means that the model will not just "vote" for a result given that it has a probability higher than 50% of being true, but will only do so if the probability is over a given condition (we ran the model with thresholds from 50% to 90%)

The best result we obtained came from a model that performed logistic regression using naive random oversampling with a 75% decision threshold.
 

### Final Model (ML_Model_FP)

Our Model has the following characteristics:
- Conected to PostgreSQL via SQAlchemy to access our datasets (tables)
- Imports data from "sb_champion_stats" (created in the EDA process)
- Scales data through StandardScaler, imported from Scikit Learn 
- Uses Scikit Learn's imbalanced RandomOverSampler to address imbalance between target variables
- Uses Scikit Learn's LogisticRegression to asimilate data to a result cnsidering the available variables (labeled data) 
- Has a Threshold of 0.75, which makes the model more selective when considering a candidate as "champion"

The results given by our model are:
- Accuracy Score = 91.4% 
- Precision (not champion) = 97%
- Precision (champion) = 22%
- Recall (not champion) = 95%
- Recall (champion) = 33%

This results tell us that our model can predict a champion one of every 5 to 4 times it is tried and if given a set of champions it would correctly predict 1 in 3. Seems a bit low, but considering the variety of variables and size of the imbalance it actually gives a pretty useful view of the possible winners of the SuperBowl.

## Conclusions

Our model does not answer the questions in the way we expected, but actually gives a new perspective on how we can understand them:

- Can the Super Bowl champion be predicted using the available information? No, but the possible winners can be narrowed down to a small number of possibilities
- Which set of stats (Offense, Defense or Special Teams) weights the most for a team to be champion? Our PCA shows that there must be smaller categories to differentiate a teams performance and, as one might not be relevant enough, We can tell that the whole team wins championships, it's not a matter of one strong team (deffense, ofense or special teams).
- Which specific stats weight the most for a team to be champion? Passing seems to be one of the most important characteristics to consider, but an inverse correlation with passing/runing plays tells us that actually it's a matter f balancing the trust a team has in their Quarterback and the ability of their runners.

## Dashboard

Using public Tableau platform, we created a web dashboard for end user that shows information about NFL statistic variables, also in the story section you can find data classified for defense and offense statistics by champion status used in the machine learning model and in same way the dashboard shows Non relevant variables dropped for model. Please see reference in the following link:

[NFL Champions Analysis](https://public.tableau.com/app/profile/julio.quintana1006/viz/FinalProject_NFL_Champions_Dash/NFLChampionsAnalysis_1#1)

Additionally we created a Google Slides presentation with the Overview of the project, to see it refer to the following link: [Presentation](https://docs.google.com/presentation/d/1eM06rsP77x76NgBpDXDDxPqoE1Rq0Wlk9VHd3mIImzc/edit?usp=sharing)
