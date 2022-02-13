![NFL](https://user-images.githubusercontent.com/89816213/153754753-6ccc76f6-c5b4-4d93-915b-6bc92151f314.png)

# NFL SuperBowl Champion 

## Overview

A common saying regarding the NFL is that the offensive wins games, but the defensive wins championships; this, off course, can be tested using a machine learning model, since the NFL presents a wide range of statistics for each player and they can be divided into 'Deffensive stats", "Offensive stats" and "Special Teams stats". The stats of each player can be added to a "Team total" and later be analyzed by the ML model to recieve feedback on which stats have the higher impact on wether a team turned out to be the SuperBowl champion of the year.

### Reason why we selected this topic

![Plabook](https://user-images.githubusercontent.com/89816213/153755450-3ee415d0-0b55-41c6-996b-3b83e08cf617.png)

As NFL fans we would like to have a better understanding of which stats can be relevant for our teams to turn out as champions (or at lest make it to Playoffs), and we think this tool could help to bringing some light on the matter.

### Data Source

![Source_Kaggle](https://user-images.githubusercontent.com/89816213/153754678-8a75519d-4684-49e6-98c8-df6a3184a327.PNG)

After some scrapping we determined that the best source available is Kaggel (as shown in image above) since it has a pretty complete database and contains a variety of metrics that is not too narrow nor very wide (some sites show quite useless stats). 

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
- ![Communication_WA](https://user-images.githubusercontent.com/89816213/153756886-0ca35645-5229-4014-9b92-bf0577c09e99.PNG)

- Slack (with help of our TA)
- ![Communication_Slack](https://user-images.githubusercontent.com/89816213/153756906-6be5804f-383d-4067-9c92-da2ef09e5141.PNG)

- Teams meetings
- ![Communication_Teams](https://user-images.githubusercontent.com/89816213/153756924-fb376591-3323-44db-b124-de7a6627de97.PNG)

## GitHub

One of our team members created a GitHub repository named "FinalProject", in which we have uploaded our advances, as shown in the images below:

![GitHub_main](https://user-images.githubusercontent.com/89816213/153757064-37f708a8-9509-4100-a742-44fcf1c4ccf9.PNG)

- Each team membar has created a branch in the repository, in order to be able to advance and test ideas without affecting the main branch:
-![Branches](https://user-images.githubusercontent.com/89816213/153754694-6b71ae49-1f8b-4c9b-b0ba-e6e8293552a6.PNG)

- Each team member has made several commits to the repository:
-![Commits](https://user-images.githubusercontent.com/89816213/153754684-f0bda6e8-640b-4ddf-9a90-43cbcbe183b2.PNG)

## Database

Provisional database in SQL-Posgres contains three tables correponding to:

### dummy table

In "create_dummmy_dataset.ipynb" in the "Data_Preparation" directory of this repository, we detail the creation of "dummy.csv" (which is found on "Resources").The file "dummy.csv" was then used to create the "dummy" table in our database.

Contains randomly generated statistics to be used as dummy data (formatted as the real data) for the first segment.

### superbowl table

Contains the team name of all superbowl champions for the period of analysis.

### unique_teams table

In "unique_teams.ipynb" in the "Data_Preparation" directory of this repository, we detail the creation of "unique_teams.csv" (which si found on "Resources"). The file "unique_teams.csv" was then used to create the "unique_teams" table in our database.

It holds all unique team names found on the datasets downloaded from kaggle.

## Machine Learning Model

### Data Preparation

In order to start testing our model we have cleansed our data in order to have an easier aproach on it.
- First we imported our dependiencies and sources:
- ![Data_Preparation_1](https://user-images.githubusercontent.com/89816213/153757758-3a03e959-ae45-4265-b252-20457443cb88.PNG)

-Then we found the unique values for the teams variable (we want to show the tams overall performance) and created a new CSV file, which was then added to our database:
-![Data_Preparation_2](https://user-images.githubusercontent.com/89816213/153757849-a02a7738-58ce-466d-bae1-aa2faad038ed.PNG)

- In the next step we merged our dataframes into a new one: 
- ![Data_Preparation_3](https://user-images.githubusercontent.com/89816213/153757993-dd79b665-c8e9-426b-b524-dc76ed88cb6b.PNG)

- Then we created a random series to fill the dataframe and changed our axis to a combination of team-year, so that we could drop those columns and avoid duplicating them:
- ![Data_Preparation_4](https://user-images.githubusercontent.com/89816213/153758093-d523b7d2-2fc9-49e9-a883-8b5e0232f19f.PNG)



