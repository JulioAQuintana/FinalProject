# NFL Super Bowl Champion 

## Overview

 **Offense wins games, but defense wins championships**
 
We would like to put this to the test. The NFL provides a wide range of statistics for each player, which we will aggregate for each team in the league. Statistics will then be categorized into those pertaining to "Defensive Stats", "Offensive Stats", and "Special Teams". Since there are a lot of statistics within each of those categories we will examine which are really valuable by applying EDA, and then Principal Component Analysis will be used to extract the principal components of each category. We will then train a classifier (Random Forest) that can accurately predict the likelihood that a team is champion, with the intention of analyzing the feature importance of principal components representing "Defensive Stats", "Offensive Stats", and "Special Teams", respectively. 

### Reason why we selected this topic

As NFL fans, we would like to have a better understanding of which stats can be relevant for our teams to turn out as champions (or at lest make it to Playoffs), and we think this tool could help to bringing some light on the matter.

### Data Source


After some searching we determined that the best source available is a Kaggle dataset (https://www.kaggle.com/kendallgillies/nflstatistics) that is broken down unto 19 csv's containing players' basic information, career stats and game logs with a variety of statistical metrics. 

Before deciding on Kaggle we considered some other sites, but they specialize on betting and have already done some of the work we want to do for our project, and we would prefer to do it on our own to make sure the results are not biased or have taken into consideration the wrong variables.

### Questions we hope to answer

The main quiestion we hope to answer is: Which of the teams' (offense, defense or special) performance can be determinant on winning the superbowl?

We can consider some other questions that we should answer in order to achieve our main goal, such as:
- Which stats, of any team, weight the most for a team to be champion?
- Is there a ratio between offense and defense that implies a higher chance to be SuperBowl champion?
- Which stats, though widely recognized, may not be significant on whether a team will or not be champion?

## Glosary

- Tackle: Stoping the advance of the opponent carrier (player in posession of the ball).
- Sack: Capturing the Quarterback while he is still in posession of the ball.
- Rush: Advancing the ball by running (no pass or kick is involved in the play).
- Pass: Attempt to advance the ball by throwing it to a teammate. 
- Reception: Recieving the pass thrown by the Quarterback.
- Field-goal: Football kick that requires a player to kick the ball into the opposing team's goal post, it's worth three points.
- Extra-Point: Football kick that follows a touchdown and requires a player to kick the ball into the opposing team's goal post, it's worth one point.
- Down: A play or attempt. Every time a team gets the posession of the ball it has four downs (attempts) to advance the ball ten yards from the position where the first down starts.

## Communication Protocols

The members of the team have provided the following information contact:

Carlos Alvarado:
- e-mail   carlosalvaradogo@gmail.com
 
Mario González:
- e-mail   mariogf92@gmail.com
    
Julio Quintana:
- e-mail   quintanajulio@johndeere.com

We have also established the following channels:

- WhatsApp Group

![Communication_WA](https://user-images.githubusercontent.com/89816213/153756886-0ca35645-5229-4014-9b92-bf0577c09e99.PNG)

- Slack (with help of our TA)

![Communication_Slack](https://user-images.githubusercontent.com/89816213/153756906-6be5804f-383d-4067-9c92-da2ef09e5141.PNG)

- Teams meetings

![Communication_Teams](https://user-images.githubusercontent.com/89816213/153756924-fb376591-3323-44db-b124-de7a6627de97.PNG)

## GitHub

One of our team members created a GitHub repository named "FinalProject", in which we have uploaded our advances, as shown in the images below:

![GitHub_main](https://user-images.githubusercontent.com/89816213/153757064-37f708a8-9509-4100-a742-44fcf1c4ccf9.PNG)

- Each team membar has created a branch in the repository, in order to be able to advance and test ideas without affecting the main branch:

![Branches](https://user-images.githubusercontent.com/89816213/153754694-6b71ae49-1f8b-4c9b-b0ba-e6e8293552a6.PNG)

- Each team member has made several commits to the repository:

![Commits](https://user-images.githubusercontent.com/89816213/153754684-f0bda6e8-640b-4ddf-9a90-43cbcbe183b2.PNG)

## Database

Provisional database in SQL-Posgres contains three tables correponding to:

### dummy table


In "create_dummmy_dataset.ipynb" in the "Data_Preparation" directory of this repository, we detail the creation of "dummy.csv" (which is found on "Resources").The file "dummy.csv" was then used to create the "dummy" table in our database.

Contains randomly generated statistics to be used as dummy data (formatted as the real data) for the first segment.

### superbowl table

Contains the team name of all superbowl champions for the period of analysis.

### unique_teams table

In "unique_teams.ipynb" in the "Data_Preparation" directory of this repository, we detail the creation of "unique_teams.csv" (which is found on "Resources"). The file "unique_teams.csv" was then used to create the "unique_teams" table in our database.

It holds all unique team names found on the dataset downloaded from kaggle.

## Exploratory Data Analysis

In order to ensure that we train our model with the right data we must first analyze it and see if all our variables are relevant and if some of them might be duplicating information.

### Merging clean datasets

In previous steps we cleaned our original datasets to homologate names and years criterion, as well as to drop some clearly unnecessary data, and in this step we merge them into a new dataframe according to the team and year.

![original_df](https://user-images.githubusercontent.com/89816213/154853938-ec600501-1e9a-4f60-bfa1-9f8b4556606e.PNG)

### Selecting period to consider in the model

Once we have our merged dataframe we create a new column holding only the year, to see if there is any tendency common to all teams related to a chronological feature.
![orig_years_table](https://user-images.githubusercontent.com/89816213/154854422-69a273c7-9b6a-45d5-8286-d25f44aefbdb.PNG)

In the previous image we can see that many variables hold no information for the years shown, therefore our next logical step is to see which variables are available through the years, so we can determine a period of study holding data for all relevant variables.

![oirg_def_tackles](https://user-images.githubusercontent.com/89816213/154854619-336e02a0-5c5a-48ef-9369-c9b71bdf8dfc.PNG)
![orig_safeties_years](https://user-images.githubusercontent.com/89816213/154854615-cf5af7be-ee28-4880-931d-6d3c3550e982.PNG)
![orig_fgs_kicks](https://user-images.githubusercontent.com/89816213/154854616-ae97fb5b-3d43-4089-8dfb-bad52f72c9fd.PNG)

We can see that not all statistics were registered for all the years in the dataset. Actually, one of the most important variables ("total_tackles") is considered only after the year 2000. According to this, our study has to consider only data from 2001 to 2016.

![reducing_years_dataframe](https://user-images.githubusercontent.com/89816213/154854815-43fa14c3-b2f6-4ee3-860a-7aea0e4f60c8.PNG)

### Redundancies and correlated data

Once we have chosen our period of study, we must check if there is any kind of duplicity among the variables we consider:

![redundant](https://user-images.githubusercontent.com/89816213/154855860-0fa8cf4b-d0b0-4188-9602-caaa8fb17651.PNG)

The criterion taken to solve this redundancies is explained in detail in the code used for the EDA (stat_analysis_by_year.ipynb).

### Variables created for our study

Some of the variables we dropped might be relevant if one is trying to measure the performance of a single player, but are not relevant when aggregated to the whole team's performance. On the other hand, these variables still offered valuable information, they just needed to be "applied" in the correct way. With that in mind we created the following variables:

- Field goal success rate (fg_success_rate): Rate of successful field goal attempts (fgs_made_kick) divided by the total field goals attempted (fgs_attempted_kick).
- Extra point success rate (extra_success_rate): Rate of successful extra point attempts (fgs_made_kick) divided by the total extra points attempted (fgs_attempted_kick).
- Pass success rate (pass_success_rate): Number of passes completed (passes_completed_pass) divided by the total passes attempted (passes_attempted_pass).
- Average yards per pass (avg_yards_pass): Total receiving yards (recieving_yards_rec) divided by the total passes completed (receptions_rec).
- Average yards per rush (avg_yards_rush): Total rushing yards (rushing_yards_rush) divided by the total rushing attempts (rushing_attempted_rush).
- Total Kicks blocked (kicks_blocked): Kicks (field goal) attempted and blocked by the defense, plus extra points attempted (extra_points_blocked_kick) and blocked by the defense.
- Passing attempts to rushing attempts ratio: Total passes attempted (passes_attempted_pass) divided by the total rushing attempts (rushing attempted rush).

### Final dataset 

After performing the EDA process and getting to know which variables we want to feed the model with, we can create a new dataset with these variables and adding a new one to show which teams were champions in which years, to do so we import the Super Bowl champions dataset (superbowl.csv) and create a new column holding "1" for all team_years, and then perform a left join with "0" in all empty values. This file is saved as "sb_champion_stats".

## Dashboard

Using the "sb_champion_stats" dataset we creted charts for all variables spreading data considering the champion for each year. From this process we identified some variables in our dataset that could cause trouble in the model. From this visualizations we created a story showing the main variables for offensive and defensive categories, as well as the ones we decided to drop.

To see the story refer to: https://public.tableau.com/app/profile/julio.quintana1006/viz/FinalProject_NFL_Champions_Dash/NFLChampionsAnalysis_1

## Machine Learning Model

### Source

Our model connects with the tables in our PostrgreSQL Database using SQLAlchemy. The input for the model is the file created int the last step of the EDA process. 

### Libraries

We used Pandas, Numpy and PyPlot to perform some further analysis of our data, given the results from Scikit-learn and Imb-learn imports.

### Adressing target variable imbalance

Our objective is to predict the Super Bowl champions (that is our target variable) indicated as "1" in the "Champion" column and, as we know, there is only one champion for each year but we have 31 teams that are not champions "0" for that same period! If we pretend to create random testing and training tests, most of the times they will not have a champion in their set and the model would not learn what stats are involved in "creating a champion".

This problem is adressed by using random over and under sampling, as well as by using SMOTE oversampling and SMOTEEN. The specifics for each method are shown in the "ML_Model_first_iteration.ipynb" file, as well as in the subsequent iterations (second and third).

### Supervised learning Models

Since we are working with labeled data and we have a clear target it's logical to use supervised learning models. In the "ML_Model" files we show the process and results for the following methods:
- Logistic Regression
- Random forest Classifier
- Balanced Random Forest Classifier (this specific method balances the observations and does not need previous balancing)


 
