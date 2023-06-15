# Recipe-Rating-Prediction-Model
This is the final project for DSC80 at UCSD

## Framing the Problem

My prediction problem is: Predict total fat of the recipes.

The type of this problem is regression.

The response variable is 'total fat' and the reason that I choose it is that I would like to know if 
total fat has any relationship between other numerical columns and categorical columns.

The metric I am using to evaluate my model is accuracy. The reason that I choose it rather than other 
metrics is that it doesn't matter if getting too many false positives or false negative. Therefore, 
precision and recall score are not meaningful for my model and the only thing I care about is whether 
the result I predict is correct or not.

## Baseline Model

There are three features in my baseline model: submitted(year), total fat, sugar. submitted is 
categorical(nominal), and the other two are numerical(quantitative). Therefore, I one-hot-encoded 
submitted features and leave the other two as-is.

The performance of my baseline model(Linear Regression) seams to be not good. The reason is the 
following:

1. The accuracy for the train data is around 40.07% and the accuracy for the test data is about 35.35%. These data mean that the model I create cannot predict the total fat accurately and to make the model more convincing, the accuracy should at least higher than 80%.

2. The RMSE of my model is about 1873.31, which is way much higher than I expected. The result indicates that the performance of this model is relatively poor. Also, the prediction of this model is significantly different from the true values.

3. The R^2 of my model is 0.3894. The result means that the performance of this model is worse and does not explain a lot of variance in the dependent variable. In other words, the independent variables are not strongly related to the dependent variables.

According to the reason I listed above, I should do more feature engineering and choose more features 
to my final model.

## Final Model

There are three new features added in my final model: scale calories, saturated fat and carbohydrates as numerical. I think these three are good features because we found that the relationship between total fat and these three features are possitvely coorelated, which means that the more the total fat is, the more these features are.

<iframe src="./data/fat_and_calories_scatter.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="./data/fat_and_carbohydrates_scatter.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="./data/fat_and_saturated fat_scatter.html" width=800 height=600 frameBorder=0></iframe>

I chose DecisionTreeRegressor and hyperparameters (find the best of max_dealth=13) in this model. For this model, the trained accuracy is about 96.95%, testing accuracy is about 91.40%, RMSE is about 158.83, and R^2 is about 0.956. The reason that I chose DecisionTreeRegressor instead of LinearRegression is that DecisionTreeRegressor can handle more complicated interaction in the data and can perform non-linear data, which LinearRegression cannot perform. In conclusion, my final model did increase the accuracy, decrease RMSE and make R^2 close to 1. Therefore, I think my model has relatively good ability to predict total fat accurately.

## Fairness Analysis

To test the fairness of our model, we analyze the RMSE for two groups: the year before 2013 and after 2013. I found out that the RMSE for year before 2013 is roughly 185.31 and that for year after 2013 is roughly 338.66. The difference is not small between two groups but to check if it is true we have to do the permutation test to verify it. Here is the null hypothesis and alternative hypothesis:

null hypothesis: the regression's RMSE for year before 2013 and after 2013 are roughly the same.
alternative hypothesis: the regression's RMSE for year before 2013 and after 2013 are not roughly the same.

<iframe src="./data/permutation.html" width=800 height=600 frameBorder=0></iframe>

the samples are selected at random. We set the significance level: 5%. After I perform the test 1000 times, I got the p-value: 0.951, which is > 0.05. Therefore, we fail to reject the null hypothesis that the regression's RMSE for year before 2013 and after 2013 are roughly the same. In conclusion, my model is fair.