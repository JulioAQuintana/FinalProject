# Exploratory Data Analysis (EDA)

The file contained in this folder holds the code with which we performed the EDA process, in order to understand our data and choose the right Model to solve the questions presented for our project.

### Sources

The files used for the EDA process are the "clean" datasets:
- defensive
- kickers
- fumbles
- passing
- receiving
- rushing

## EDA Process

### Period of analysis

The first step we took to explore our data was merging the different datasets, which had the same "team_year" column. and from there we added our variables.

Once added we used the describe function to see the relevant statistics for our variables, and realizad some of them had an unusual bahaivour, which was explained by the fact that they don't exist for the whole period considered in the datasets. From this observation we decided to anlyze our data by year, and graphed the count of values for each year and variable; this showed that some important variables, such as total takles, were not take in acount 2001.

![stats_years](https://user-images.githubusercontent.com/89816213/157533119-97e77c3d-b8ef-4b10-97c5-bc3772e2c8eb.PNG)

This determined that our analysisi would only consider from 2001 to the most recent season in the dataset.

### Redundancies

THe next step was to check for redundancies between our variables, for this we used the "corr" (correlation) function, which showed that we had a high number of correlated variables and, to solve this issue we took one of this options:
- Adding data into one unique variable and dropping redundancies
- Creating ratios between our variables and dropping redundancies
- Keeping variables (in case the variables were correlated but there was no clear way to convert them to a unique variable)

### Creating new dataset

Using our EDA process as a step for further cleaning of our data proved to be a time saving step, so we decided to create the datastet to be used in our model using the insights from the EDA process and taking advantage of the code for the advance already done.

