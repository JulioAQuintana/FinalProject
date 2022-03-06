# Data Preparation

## Contents

This folder contains the following files:
- create_dummy_dataset.ipynb
- unique_teams.ipynb
- clean_defensive.ipynb
- clean_field_goal_kickers.ipynb
- clean_fumbles.ipynb
- clean_passing.ipynb
- clean_receiving.ipynb
- clean_rushing.ipynb
- clean_superbowl.ipynb
- NFLDB_script

## Languages

The languages used in the files contained in this folder are:
- Python: Used for the cleaning process and dummy data creation codes
- SQL: Used to create the tables to contain the datasets in PostgreSQL (NFLDB_script)

## Libraries

For the data cleaning and creation of dummy data we used the following libraries in Jupyter Notebook:
- Pandas
- NumPy
- FuncTools

## Unique labels

Some of the NFL teams have changed their name through the years or have moved from one city to another, therefore we made a list containing unique teams names to check if there was no duplicity (e.g. if a team moved and somehow it appeared for both cities for any season).
The process involved importing all of our original datasets (described in Resources) and keeping only the "Team" column for each of them. Then we concatenated this columns with NumPy to create a single column containing all of the teams' names. Finally, using Pandas we selected the unique values and exported this dataset to a CSV file ("unique_teams.csv").  

## Dummy Data

In "create_dummy_dataset.ipynb" the reader can find the code with which, from a non clean dataset, we created a dummy dataset used to make the first tests of our model.

It was created from "Career_Stats_Defensive.csv" because it is the largest dataset in our resources and that led us to think it should have all of the teams and years combinations. The process involved importing our 


## Data cleaning

All the datasets we worked with went through a cleaning process, for efficiency we will only describe the cleaning process of the "defensive" dataset, emphasizing that all the other datasets went through a similar process.

### Unnecessary data

Our datasets contian data referred to the name or position of the players. While this information may be relevant to understand individual performance, we do not need it to evaluate the team's performance, therefore this variables are dropped fromm our dataset.

### Data Types

Given that we will use our data for a ML Model we need all or data to be "integer" type, and the next step is to drop or convert all data that is not an integer. 

### Creating unique identifiers and ordering columns

As we intend to evaluate the performance of each team on a given year, we can add our statistics for a new variable called "team_year" holding our needed identifiers; and once we have it we can reorder our columns and export a new csv file that will be added to our database.




