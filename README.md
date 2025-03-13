# Revisiting the West Nile Virus

## *Motivation and Overview*

10 years on from the Kaggle West Nile Virus competition, and there is still no cure and no vaccine for West Nile Virus. It is one of the biggest threats to humans from mosquitos, and still remains a global concern, including in the city of Chicago where the kaggle competition was initially focussed. Since then, our planet has seen a global pandemic, which has highlighted the need for us to understand the spread of disease, and consolodate our knowledge on the underlying mechanisms of disease transmission.

The original aim of the competition was to predict outbreaks of West Nile virus in mosquitos in Chicago, to help the authorities allocate resources towards targetted interventions. This project will take a practical approach to developing a model that is not only accurate but also interpretable and actionable. The goal is to create a generalised model that can provide clear recommendations for targeted interventions, and contribute to our understanding on mosquito transmitted disease.

## *Methods & Approach* 

*Data*: 

This project used the datasets provided with the West Nile Virus competition on kaggle. This included a training/testing dataset of locations, as well as weather data from the two international airports in Chicago. The weather from these two locations were combined to an average, with careful consideration of wind speeds and for the combining of categorical weather states. Daily sunlight was calculated as the time between sunrise and sunset. Missing values were handled by sourcing data from the other location (if available) or by multiple imputation where necessary. Several variables were removed due to excessive missing data. From our test/training data, location was given by Lattitude/Longitude, and other location information relating to the address was removed. Location accuracy, mosquito trap type, and mosquito species were accounted for by including them as features in the model.

Additionally, data of mosquito spraying efforts of the Chicago authorities was provided, although dates and locations did not match up well between it and the test/train data. The data was used to compare current efforts to combat mosquitos, compared with what might be possible by using the model. 

*Model Fitting*:

As the data corresponds to spatial and temporal patterns, a linear model would be unsuitable as it assumes the residuals will be uncorrelated. Additionally, Spatial modelling would be computationally expensive and may not scale well to our large dataset. Additionally, the algorithm must be able to predict as a probability. Therefore, this project considers tree methods to model the data. Both random forests and a gradient boosted tree method were considered. As expected, the gradient boosted model came out as preferable after validation. When using more complex tree-based methods, they can be seen as 'black/grey box' models, meaning they are difficult to interpret compared to traditional methods. However, we can use feature importance methods to still gain some understanding of our features.


*Significant Features:*

In the initial data processing, variables were examined for outliers and multicollinearity. Several temperature features were removed due to correlations, with careful consideration to determine which features to keep. After model fitting, features were removed based on their feature importance scores, which reflect their individual contributions to reducing model's gini impurity. We therefore remove variables that do not significantly contribute to the model's prediction. Through cross-validation, it was confirmed that removing these variables had no significant impact on either the ROC under the curve score, or the accuracy metrics of predictions. 



## *Results and Suggestions for the Chicago Authorities** 

- In examining the features found to be significant to our model, 'Sunlight' is found to be the most important, meaning that mosquitos with the West Nile Virus are strongly influenced by longer hours of sunlight. Temperature also plays a key role. This supports current research, that says longer days increase mosquito activity. The City of Chicago should expect to spend the larger part of their budget in the summer months, particularly while days are very long. 

- Mist and Fog also show up as important features, which may be due to their association with increased humidity. Unfortunately, we are unable to determine whether there is a positive or negative association with the features in our model. However, previous research has shown that low humidity can negatively impact virus spread, as well as mosquito activity.

- Longitude, Lattitude as well as Address accuracy all contribute significantly to the model, indicating a meaningful spatial pattern in the data. This could be investigated further by taking advantage of spatial clustering techniques, however the analysis may be limited as points of mosquito detection are limited and don't span the full area. 


Overall, the XGboost model has an AUC score of 0.83, indicating good performance. However during cross-validation the model struggled to correctly identify positive virus cases when using the default threshold for classification, likely due to the rarity of positive cases compared to negative ones. Attempts to balance the classes, including using SMOTE, did not lead to significant improvements. However, by reducing the threshold to improve sensitivity, our model can still be used to target instances where the virus is present. 




In essence, choosing the threshold(P*) involves a trade-off between capturing more areas with virus-positive mosquitoes and minimizing false positives (areas predicted as positive that do not actually contain the virus). The table below shows some value choices for our threshold, along with what this corresponds to.

A threshold choice of 0.25% is recommended, as it captures 90% of virus-positive areas. This is only slightly lower than the 94% captured at a 0.1% threshold, but significantly reducing the predicted at-risk points from 31,000 to 19,000. Putting it another way, 11% of spray locations would be reaching areas where the virus is present. 


| P* Threshold (default = 50%) | Estimated Percent(%) of Virus-Positive Areas Captured | Total Time-Locations to Spray | "Percentage of Sprays Targeting Virus-Active Areas (PPD)" |
|-----------------------------|-------------------------------------------------------|------------------------------|-----------------------------------------------|
| 0.1%                        | 0.939891                                              | 31152                        | 0.097718                                      |
| 0.25%                       | 0.901639                                              | 19019                        | 0.110879                                      |
| 0.5%                        | 0.852459                                              | 12526                        | 0.127589                                      |
| 1%                          | 0.786885                                              | 8031                         | 0.145473                                      |
| 5%                          | 0.595628                                              | 2547                         | 0.198962                                      |
| 10%                         | 0.480874                                              | 1380                         | 0.234211                                      |



To demonstrate how the model might be useful in this way, the sample data from 2011 was predicted using the model, and compared to the data of actual sprays by the city of chicago in 2011. From this, we know there were 14,835 mosquito spraying efforts in 2011. Using our model, we could have narrowed the scope to 856 time-location points while still capturing ~90% of points where virus-positive mosquitoes were detected. ~10% of the selected time-locations actually contained virus-positive mosquitoes. These are known metrics as we have the target values for this data.


To conclude, we recommend that the Chicago authorities should focus spraying efforts for time-locations that our model predicts has a greater than 0.25% chance of holding a virus-positive mosquito. It is able to narrow down the data to exclude many areas where the virus is not present, whilst having good coverage of areas that do contain the virus. Although this means that around 90% of spraying efforts will be in areas without the virus, this is not wasted as mosquito spraying can be otherwise beneficial to the general public.





























