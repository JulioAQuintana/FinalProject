# NFL SuperBowl Champion 

## Overview

 **Offense wins games, but defense wins championships**
 
We would like to put this to the test. The NFL provides a wide range of statistics for each player, which we will aggregated for each team in the league. Statistics will then be categorized into those pertaining to "Defensive Stats", "Offensive Stats", and "Special Teams". Since there are a lot of statistics within each of those categories, Principal Component Analysis will be used to extract the principal components of each category. We will then look to train a classifier (Random Forest) that can accurately predict the likelihood that a team is champion, with the intention of analyzing the feature importance of principal components representing "Defensive Stats", "Offensive Stats", and "Special Teams", respectively. 

### Reason why we selected this topic

As NFL fans we would like to have a better understanding of which stats can be relevant for our teams to turn out as champions (or at lest make it to Playoffs), and we think this tool could help to bringing some light on the matter.

### Data Source

After some searching we determined that the best source available is a Kaggle dataset that is broken down unto 19 csv's containing player basic information, career stats and game logs with a variety of statistical metrics. 

Before deciding on Kaggle we considered some other sites, but they specialize on betting and have already done some of the work we want to do for our project, and we would prefer to do it on our own to make sure the results are not biased or have taken into consideration the wrong variables.

### Questions we hope to answer

The main quiestion we hope to answer is: Which of the teams' (offense, defense or special) performance can be determinant on winning the superbowl?

Though we can consider some other questions that we should answer in order to achieve our main goal, such as:
- Which stats, of any team, weight the most for a team to be champion?
- Is there a ratio between offense and defense that implies a higher chance to be SuperBowl champion?
- Wich stats, though widely recognized, may not be significant on wether a team will or not be champion?

## Communication Protocols

The members of the team have provided the following information contact:

Carlos Alvarado:
- Cellphone 8110220336
- e-mail   carlosalvaradogo@gmail.com
 
Mario González:
- Cellphone 5566774418
- e-mail   mariogf92@gmail.com
    
Julio Quintana:
- Cellphone 8441209369
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

In "unique_teams.ipynb" in the "Data_Preparation" directory of this repository, we detail the creation of "unique_teams.csv" (which si found on "Resources"). The file "unique_teams.csv" was then used to create the "unique_teams" table in our database.

It holds all unique team names found on the dataset downloaded from kaggle.

## Machine Learning Model

### Overview of the model

Currently, the model connects to the three tables in our Database using SQLAlchemy. The input for the model, or the features, are the dummy statistics generated as explained above. On the other hand, the target variable is a binary outcome column that was randomly generated and named "Champion". For this segment of the project we omitted the dimensionality reduction aspect of the process, that is, since we are working with dummy data with significantly less columns than the real data, we did not employ Principal Component Analysis (however we will do so when work with the real data). Finally, using Scikit Learn, we trained a classifier (Random Forest) and used it to predict our target variable "Champion". When working will the real data, a further step will be needed, that of determining the feature importance, but since at this stage we are working with dummy features, this step was omitted. 

For more details of the model, see "ML_Model_FP.ipynb" found on this repository. 
