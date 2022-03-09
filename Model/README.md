# Machine Learning Model (Iterations)

For our Machine Learning Model our approach was to run various attempts and choose the one that performed the best for our final model. Given so, we made three iterations before choosing our final model. Here we describe the iterations previously referred and the PCA performed to answer the question "Which set of variables (Defensive or Offensive) wights the most to determie the Super Bowl champion?"

## Principal Component Analysis (PCA)

As stated before, one of our main questions to answer through this analysis is which set of variables weights the most to predict the Super Bowl champion, and to find out we used Scikit Learn's PCA for the offensive features, getting the following results:

![variance](https://user-images.githubusercontent.com/89816213/157539290-7b120f91-af9b-4787-a934-f35bc30c6ec2.PNG)

This image shows that one of the variables has a relevance of 30% but from there on the variables keep loosing weght, and that means that the defensive variables weight even less, and no set can explain the result by itself.

![cumulative](https://user-images.githubusercontent.com/89816213/157539297-981f2553-bc1e-442d-901a-a81ac2b033ab.PNG)

This graph shows that we would need over 5 variables to predict the result, therefore there is no possibility to aseverate that this specific set of variables or any other can be used, isolated, to predict the Super Bowl champioon.

## ML Model Iterations

All the iterations and final model import the data from our PostgreSQL database, using the "sb_champion_stats" dataset, and use Scikit Learn's StandardScaler to scale the data to import it to the model.

### First iteration

The first evident problem to sove with our data is the fact that we have one winner per year and 31 "non-champions", which heavily imbalances the possible sets to train and test our model. The solutions considered were:
- Naive Random Oversampling: Increases the number of "champion" observations by replicating aleatory wxisting champions characteristics
- Naive Random Undersampling: Creates training and testing sets with  a low number of "non-champions" to balance the probablity of finding each result
- SMOTE Oversampling: Increases the number of "champion" observations by creating synthetic observations with characteristics similar to those of the "champions"
- SMOTEENN: Combines Over and Under sampling and Edited Nearest Neighbours

Considering the fact that we are working with labeled data and have a clearly binary (champion or not champion) target, we must work with supervsed learning models, for which we considered:
- Logistic Regression
- Random Forest Classifier
- Balanced Random Forest Classifier (this method doesn't require previous balancing)

The models showed that they were severely affected by the imbalance in the dataset, since the models were either highly precise and didn't choose any champion or were too sensible and found many champions that were really non champions. Therefore we evaluated the features' importance to consider dropping irrelevant data and having a more specific model. Our criteron was to drop all the variables that wer not found relevant in at least one of the models when using p_values.

![feature_importance](https://user-images.githubusercontent.com/89816213/157543949-00abe7bb-5d3d-4ba7-a660-f35913f2aa49.PNG)

### Second iteration

This iteration took in consideration the same parameters and rationale of the firs iteration, with the difference that we woeked with a dataframe from which we dropped the "irrelevant" variables.

For our surprise, this model performed even worse than the first one, which led us to understand that though some variables may seem irrelevant they might actually interact in a way that we didn't percieve. This led us to try a third iteration.

### Third iteration

For our third iteration we chose to re-run the model with the original dataset, but found that a solution for our model might be increasing the threshold for our model to vote a champion.

This prooved to be a good way to deal with higher sensibility (getting more "champions") but having a highly demanding parameter for the model to make the choice. 

The ideal model used Random Oversampling, Logistic Reression and the ideal threshold for our model turned out to be 0.75 (we tried from 0.5 to 0.9), and has a reasonable 0.22% of precission and a sensitivity of 33% for the "champion" variable, while having an overall 91% accuracy score. This are the parameteres we used for our final Model.




