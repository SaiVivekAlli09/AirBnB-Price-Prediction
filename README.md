# AirBnB-Price-Prediction

Airbnb is a home-sharing platform that allows homeowners and renters (‘hosts’) to put their properties (‘listings’) online, so that guests can pay to stay in them. Hosts are expected to set their own prices for their listings. Although Airbnb and other sites provide some general guidance, there are currently no free and accurate services which help hosts price their properties using a wide range of data points. Airbnb pricing is important to get right particularly in big cities like New York where there is lots of competition and even small differences in prices can make a big difference. It is also difficult thing to do correctly – price too high and no one will book. Price too low and you will be missing out on a lot of potential income.

This project aims to solve this problem, by using regressor models to predict the price for properties across major cities in the United States. By examining a large data set of past home rentals and finding patterns and statistical relationships between house’s characteristics and its price including patterns that might not be evident to a human who’s looking at the data. We have explored the preparation and cleaning of Airbnb data and conducted some exploratory data analysis. With this the users can know what features of an Airbnb listing are most important as well as how prices are fluctuating based on amenities, location etc.

## Introduction

Unlike hotels, which have their own pricing system, Airbnb prices are usually determined by the hosts empirically. It poses challenges for the new hosts, as well as for existing hosts with new listings, to determine the prices reasonably high yet without losing popularity. On the consumers’ side, though they can compare the price across other similar listings, it is still valuable for them to know whether the current price is worthy and if it is a good time to book the rooms. This project is used to determine and predict the price of Airbnb listings in major US cities like Boston, Chicago, Washington DC, Los Angeles, New York, San Francisco.
The price for Airbnb renting depends on multiple factors, and we divide the input type into 3 categories, including continuous, categorical, set (amenities) features. We have extracted more than 60 features from the dataset. Here we only list a few of them that are both representative and important for the task, such as room size {accommodates, bathrooms, bedrooms, beds, ...}, location {neighbourhood, latitude, longitude, ...}, facilities {amenities, property type, ...}, and booking related {availability, cancellation policy, host response rate, ...}. The ground-truth label is the actual base price, and we use a variety of regression approaches including linear regression, ridge regression, lasso regression, as well as XG boost, to predict the value.

## Methods and Techniques

As the target variable to be predicted is the price, which is a continuous value, we can apply regression models like linear regression, ridge regression etc., but unable to apply classification models like k-nearest neighbors, svc etc. we used the default settings for all machine learning approaches imported from the sklearn package. Note that we have experimented with different parameters, like learning rate, n-estimators in XG Boost., and the output metrics stay largely unchanged.We have applied these machine learning approaches with different label transformations, with the target variable as an integer and as logarithmic value and we found that logarithmic transformation largely improves the model performance

<img width="838" alt="Screenshot 2023-10-24 at 5 40 18 PM" src="https://github.com/SaiVivekAlli09/AirBnB-Price-Prediction/assets/126822808/50f3cfcb-0ef0-4e7d-8528-937438fb7bc3">

## Exploratory Data Analysis

**Data Cleaning**: Missing values in both train and test data are found and filled them with appropriate values. Feature Selection:
Some of the features, as below, which doesn’t show any significant differences in the price prediction are eliminated.

* name
* thumbnail_url 
*  description
* host_since
*  first_review
* last_review
* instant_bookable

Based on the low correlation values between the numerical features and the log price (target variable), below features are eliminated/dropped.
* id
*  host_has_profile_pic o host_identity_verified
*  host_response_rate o latitude
* longitude

<img width="717" alt="Screenshot 2023-10-24 at 5 41 06 PM" src="https://github.com/SaiVivekAlli09/AirBnB-Price-Prediction/assets/126822808/bfbfb66e-5ac0-48fd-8f15-8f5294eb0bce">

Though number_of__reviews and review_score_rating have low correlation values, they are not dropped as they have significant impact in price as shown in below plot.

<img width="904" alt="Screenshot 2023-10-24 at 5 41 27 PM" src="https://github.com/SaiVivekAlli09/AirBnB-Price-Prediction/assets/126822808/229ca794-de23-4831-995f-a2c02f26a30b">

## Feature Engineering

The distribution of train and test data in certain area in the city of DC as an example is as follows, with train data points in red and test data points in blue colour.

<img width="884" alt="Screenshot 2023-10-24 at 5 53 45 PM" src="https://github.com/SaiVivekAlli09/AirBnB-Price-Prediction/assets/126822808/c1b64331-4629-4b2b-b8f4-1cb45359f955">

The dataset is classified into three sets of data: categorical, continuous and a set of categorical values.
Categorical Data: Features like, property_type, room_type, bed_type, city, cancellation_policy are converted into numeric format by one-hot encoding using pd.get_dummies().

**Set of Categorical Data:** Only one feature, amenities, comes under this category where a bunch of categorical values are arranged as a set, which are split into a numpy array and concatenated to the data set in binary format.

**Continuous Data:** As the continuous data is in already in numeric format, no changes are applied on this data. The features like, bathrooms, accommodates etc., comes under continuous data type.

This above data is converted into vectorized form using PCA – principal component analysis.


## Building a Machine Learning Model

We experimented with machine learning models for price prediction. As this is the regression task, the evaluation metric chosen was mean squared error (MSE). For accuracy, we have calculated r squared value for each model produced.

**Linear regressor**: We have used this model to define the relation between features and target variable through an equation that tries to represent the relationship between one dependent and multiple independent variables.

**Ridge regressor**: As per the evaluation metrics, there is a slight improvement in this model because the value of the R-squared has been increased. This reduced the model complexity by coefficient shrinkage because it uses L2 regularization technique.

**Lasso regressor**: This model is quite similar to ridge, but after evaluation metrics we have observed that both the RMSE and R-square for this model has increased. Therefore, lasso model is predicting better than both linear and ridge. This model is generally used when there are more number of features as it automatically does feature selection.

**XG Boost**: XG Boost is a powerful approach for building supervised regression models. It contains loss function and a regularization term. It tells about the difference between actual values and predicted values, i.e., how far the model results are from the real values. Compared to other regressor model, this gave best metrics.

## Evaluation Metrics

We have used RMSE and R2 metrics in this project to evaluate the training set of combined train data and to compare the difference in the predicted price value of the test data between the combined test data set and individual cities, used RMSE.

## Experimental Results

We have experimented our data with different combinations of features and labels as follows,

**Feature: Continuous, Label: Int Price and Log Price –** Applied different models by using only the continuous features of the data set to predict int price and log price. Below is the table with RMSE and R2 metric values on train data set.

<img width="756" alt="Screenshot 2023-10-24 at 5 42 32 PM" src="https://github.com/SaiVivekAlli09/AirBnB-Price-Prediction/assets/126822808/6c268054-81c7-4037-ac50-133cf839a149">

**Feature: Continuous, Categorical and with Amenities, Label:** Int Price and Log Price – Applied different models on using the continuous, categorical and amenities features of the data set to predict int price and log price.

<img width="760" alt="Screenshot 2023-10-24 at 5 42 45 PM" src="https://github.com/SaiVivekAlli09/AirBnB-Price-Prediction/assets/126822808/e8d365f4-f60c-4c73-a2d8-0bae73088b8c">

We find that the R2 score of Ridge regression on continuous, categorical and amenities features to predict int price is the highest score in all of our experimental results.
This table shows the RMSE scores of individual cities b/w the predicted price of individual test data and combined test data.

<img width="775" alt="Screenshot 2023-10-24 at 5 43 09 PM" src="https://github.com/SaiVivekAlli09/AirBnB-Price-Prediction/assets/126822808/f2f472ec-2053-441c-812b-2626c1d7963c">

## Conclusion

Overall, we have performed extensive feature selection and engineering, and experimented with various machine learning approaches in predicting Airbnb listing price. We showed that XG boost and lasso regression out-perform other approaches, and achieve r-squared value around 0.6. We also showed that by training the datasets from different cities individually.

* Linear regression worked correctly when only continuous values are included, with categorical values are included and K-fold is applied, few splits are seen with very higher values of RMSE and R2 in the range of e+17.

* When tried to implement the regression models, the values were not consistent and are largely differed from each run.

* Unable to apply k-nearest or svc models on this data set as the target variable is a continuous value and can be applied on regression models. Though random forest model is working of this data set, it is taking a lot of time in order to compute the results and the results were comparatively similar to XG boost.





