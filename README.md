# Revisiting the West Nile Virus

## *Motivation and Overview*

10 years on from the Kaggle West Nile Virus competition, and there is still no cure and no vaccine for West Nile Virus. It is one of the biggest threats to humans from mosquitos, and still remains a global concern, including in the city of Chicago where the kaggle competition was initially focussed. Since then, our planet has seen a global pandemic, which has highlighted the need for us to understand the spread of disease, and consolodate our knowledge on the underlying mechanisms of disease transmission.

The original aim of the competition was to predict outbreaks of West Nile virus in mosquitos in Chicago, to help the authorities allocate resources towards targetted interventions. This project will take a practical approach to developing a model that is not only accurate but also interpretable and actionable. The goal is to create a generalised model that can provide clear recommendations for targeted interventions, and contribute to our understanding on mosquito transmitted disease.

## *Methods & Approach* 

*Data*: 

This project used the datasets provided with the West Nile Virus competition on kaggle. This included a training/testing dataset of locations, as well as weather data from the two international airports in Chicago. The weather from these two locations were combined to an average, with careful consideration of wind speeds and for the combining of categorical weather states. Daily sunlight was calculated as the time between sunrise and sunset. Missing values were handled by sourcing data from the other location (if available) or by multiple imputation where necessary. Several variables were removed due to excessive missing data. From our test/training data, location was given by Lattitude/Longitude, and other location information relating to the address was removed. Location accuracy, mosquito trap type, and mosquito species were accounted for by including them as features in the model.

Additionally, data of mosquito spraying efforts of the Chicago authorities was provided, although dates and locations did not match up well between it and the test/train data. The data was used to compare current efforts to combat mosquitos, compared with what might be possible by using the model. 

*Model Fitting*:

As the data corresponds to spatial and temporal patterns, a linear model would be unsuitable as it assumes the residuals will be uncorrelated. Additionally, Spatial modelling would be computationally expensive and may not scale well to our large dataset. Additionally, the algorithm must be able to predict as a probability. Therefore, this project considers tree methods to model the data. Both random forests and a gradient boosted tree method were considered. As expected, the gradient boosted model came out as preferable after validation.


*Significant Features:*

In the initial data processing, variables were examined for outliers and multicollinearity. Several temperature features were removed due to correlations, with careful consideration to determine which features to keep. 











