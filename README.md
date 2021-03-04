# Wine-Quality-Prediction-KNN-Model
Quality of wine is predicted for a grocery retailer 

## Scenario
In the example, I am the data scientist tasked with creating a model to predict the quality level of wine for a grocery retailer. My audience receiving my work is intended to be a non-technical group of buyers at the retailer. The variables presented in the data set are the same information that the retailer has (except for the quality) to decide on what wines to sell in their stores. The dataset is a sample of wines sold at the grocery retailer and wine quality has been measured after it has been purchased by the retailer and placed on the shelves. The quality can only be determined after the wine has been obtained and tested by the retailer. The retailer wants to predict quality based on current data without further expensive inventory testing. 

Thus, I will elaborate on my process to create a predictive model to alleviate this expense. 

## Data
The dataset for this scenario contained 1,600 rows, or data points, with 11 explanatory variables and 1 integer response variable, quality. Quality ranged from 3 to 8 with the lower end of the scale representing low quality wine and higher quality wines at the opposite end of the spectrum. The 11 explanatory variables were all numeric with a variety of ranges. The 11 explanatory variables included the following: fixed acidity, volatile acidity, citric acid, residual sugar, chlorides, free sulfur dioxide, total sulfur dioxide, density, pH, sulphates, and alcohol.

## Data Resampling
![wines data resampling](https://user-images.githubusercontent.com/46107551/109912278-b553ff00-7c79-11eb-8db9-4012763e62cd.png)

For this analysis, my first thought was to categorize our wine quality scores into categories of either high, low, or med quality. I categorized the quality because I bet even the savviest of wine consumers can’t taste the difference between a 4 or 5 quality wine or even a 7 or 8 quality. Quality scores of 3 or 4 were given a low category, scores of 5 or 6 received a med category, and the highest scores of 7 or 8 were labeled as high. There were no other quality scores in our dataset. When we first look at our dataset it becomes apparent that we need to do some resampling of it. 

The chart on the left shows that our original data contains a very high proportion of med quality wines. If we use this data for our model, by default if our model returns med it will automatically be correct about 82% of the time. We can resample our dataset to include all high and low quality wines then a random 20% of our med wines. The resulting chart on the right shows our oversampled data is more balanced and we now can begin to see what really is driving the difference in wine quality. It is oversampled because we took 100% of all wines that were not med quality.

## Model Type
![wines model type](https://user-images.githubusercontent.com/46107551/109912373-e59b9d80-7c79-11eb-99c8-3901e8d161b2.png)

I chose to make a K-Nearest Neighbor model (KNN) for this analysis. A KNN model predicts a wine’s quality based on the wine qualities around it. For instance, suppose that the analysis determined that alcohol and volatile acidity were the two most important factors in determining wine quality. Let’s also suppose that based on testing, 5 nearest neighbors gave us the best predictive accuracy. When a new wine comes into question, we plot it on the chart using alcohol and volatile acidity as our axis and count the quality categories of the 5 nearest data points. In this example, the X on the chart represents the new wine and the circle shows its 5 nearest neighbors with 4 of them being high quality and 1 as med quality. By majority vote, the KNN model would classify the new wine X as high quality. For the analysis portion, I will determine what are the critical factors in determining wine quality and test how many neighbors should be counted to give the highest accuracy to correctly classify a new wine. 

 ## Analysis of Explanatory Variables
The first step in my analysis was to look at the correlations of the explanatory variables and the original wine quality scores. No single variable was highly correlated with the wine quality, meaning there was no obvious first choice for any variable to be included in the model. 
 
I continued my analysis by performing a chi squared test of the explanatory variables grouped into the wine quality categories. The test assumes that for each variable that the proportions of the observed values would be equal among all 3 quality categories. Variables that show to be significantly disproportional among the quality categories gives an indication that the variable might contribute to determining the differences in wine quality. The chi squared test showed that all variables except for free and total sulfur dioxide were disproportionate among the 3 quality categories. Thus, all variables besides sulfur dioxide were further analyzed. 

I considered again our original wine quality scores from 3 to 8 in a linear model. The goal of the linear model is to minimize the difference in predicted wine quality and actual wine quality scores. I made this model in a forward stepwise procedure. A linear model was made one by one with just 1 variable at a time. The model that predicted best with only 1 variable would then have just 1 other variable added to it. Then the model with just 2 variables would be considered and the process would be repeated in this way adding 1 additional variable at a time until the best linear model was produced. From here I then worked to reduce the best model to see if removing any variable would significantly affect its predictive ability. The logic being that if a model could predict nearly as well as the best model from the step wise procedure with fewer variables, that the simpler model with fewer variables would be preferable. 
The process lead me to predictors with that produced the best possible chance of accurately predicting the wine quality scores. The best predictors of wine quality scores in a linear model were used as inputs in the KNN model. The final set of variables used in the KNN model were the following: volatile acidity, alcohol, sulphate, pH, and chlorides. 

## Final Model & Results
The final component needed for the KNN model was how many neighboring data points should be counted to produce the most accurate wine quality category prediction. The oversampled data set was randomly split into a training and a hold out dataset. The training data set contained 80% of the oversampled data and the hold out dataset contained the remaining 20%. 

Testing within the training dataset lead to counting 15 of a data point’s nearest neighbors to predict quality category (low, med, high) most accurately. Our KNN model with volatile acidity, alcohol, sulphate, pH, and chlorides as inputs and takes a majority vote of wine quality categories of the 15 nearest datapoints to predict a new wine’s quality category. 

This model was now applied to the hold out dataset to see how well it could predict the wine quality categories of wines that were not used in testing. Our model predicted the wine quality categories of the wines in the hold out set with 66% accuracy! If we break this down further, the model correctly predicted a high or med quality with approximately 76% accuracy and a low quality category with only 16% accuracy. 

## How to Improve
Breaking down the accuracy of the model above shows that the model performs worst when dealing with a low quality of wine. This is the result of our oversampled dataset only containing 63 low quality wines out of 554 total wines. If we want to improve the model an easy first step would be to collect more data on low quality wines. 

If we ask wine experts what makes a wine a good or bad, we will likely find that our data collected is leaving many important factors out. In our dataset we have no information on the type of wine produced (such as a red or white), the wine’s age, what type of grapes were used, or where the wine came from (like California, Italy, France, or elsewhere), etc. If the goal is to be able to identify the best wines for our retailer’s shelves without making investments in inventory for testing and by using a cost effective predictive model, collecting more data on our wines should be the next step. 
