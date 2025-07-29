# Final-Project-Statistical-Modelling-with-Python

## Project/Goals
(fill in your description and goals here)

## Process
### EXTRACT

1. In the first step I had to get the network name for Bike Share Toronto. There for I made an API call to 'https://api.citybik.es/v2/networks' in order to get all the network names. Stored the results in df_networks.csv

2. From the newtork names, bixi-toronto is the network names to be used. Therefore, I made an API call to "https://api.citybik.es/v2/networks/bixi-toronto" to get the stats for bike stations in toronto.

3. Parsing through the list of latitudes and longitudes from above, I initiated my API call to Foursquare to get POIs.

4. In order to make an API call to Foursquare radius are required:

f"https://places-api.foursquare.com/places/search?ll={latitude},{longitude}&radius={radius}&fsq_category_ids={categories}"

I obtained my category IDs from: https://docs.foursquare.com/data-products/docs/categories with Park, Bar and Restaurent being desired categories.


### LOAD

1. For each row in my city_bikes datarame, I made an API call to Foursquare using Lat and Long in that row in order to compile a list if dictionaries with station_id as index, place_name, lat, long, address and category

2. This was built into a dataframe and stored in fsqr_response.csv


### TRANSFORM

1. Eventhough 3 major categories were selected, Foursquare still returned 531 unique sub-categories
2. In order to clean up the data, 5 major categories Bar/Rstaurant, Bar, Restaurant, Park and Other were assigned to each row.
3. city_bikes and category_counts were merged using station_id as key, in order to conduct EDA and run the regression model

## Results
Based on the coverage of API in my area, I was not able to find a strong evidence in my statsmodel with p values (except for Bar/Restaurent) being relatively larger than α

                            OLS Regression Results                            
==============================================================================
Dep. Variable:             free_bikes   R-squared:                       0.068
Model:                            OLS   Adj. R-squared:                  0.063
Method:                 Least Squares   F-statistic:                     13.63
Date:                Tue, 29 Jul 2025   Prob (F-statistic):           7.45e-13
Time:                        10:43:41   Log-Likelihood:                -3115.4
No. Observations:                 937   AIC:                             6243.
Df Residuals:                     931   BIC:                             6272.
Df Model:                           5                                         
Covariance Type:            nonrobust                                         
==================================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
----------------------------------------------------------------------------------
const              5.1707      6.058      0.854      0.394      -6.718      17.060
Bar/Restaurant     1.2557      0.690      1.821      0.069      -0.098       2.609
Bar               -0.1453      0.630     -0.230      0.818      -1.382       1.092
Restaurant        -0.0455      0.607     -0.075      0.940      -1.236       1.145
Park               0.9509      0.627      1.517      0.130      -0.280       2.181
Other             -0.1706      0.667     -0.256      0.798      -1.479       1.138
==============================================================================
Omnibus:                      244.615   Durbin-Watson:                   1.902
Prob(Omnibus):                  0.000   Jarque-Bera (JB):              611.852
Skew:                           1.366   Prob(JB):                    1.37e-133
Kurtosis:                       5.865   Cond. No.                         195.
==============================================================================

However, Poisson regression seemed to respond better showing correlation with Bar/Restaurent and Park

 Generalized Linear Model Regression Results                  
==============================================================================
Dep. Variable:             free_bikes   No. Observations:                  937
Model:                            GLM   Df Residuals:                      931
Model Family:                 Poisson   Df Model:                            5
Link Function:                    Log   Scale:                          1.0000
Method:                          IRLS   Log-Likelihood:                -4466.3
Date:                Tue, 29 Jul 2025   Deviance:                       6174.4
Time:                        10:43:44   Pearson chi2:                 6.41e+03
No. Iterations:                     5   Pseudo R-squ. (CS):             0.3663
Covariance Type:            nonrobust                                         
=======================================================================================
                          coef    std err          z      P>|z|      [0.025      0.975]
---------------------------------------------------------------------------------------
Intercept               1.7461      0.342      5.104      0.000       1.076       2.417
Q("Bar/Restaurant")     0.1688      0.038      4.418      0.000       0.094       0.244
Bar                    -0.0273      0.036     -0.760      0.447      -0.097       0.043
Restaurant             -0.0134      0.034     -0.390      0.697      -0.081       0.054
Park                    0.1153      0.035      3.287      0.001       0.047       0.184
Other                  -0.0367      0.038     -0.967      0.334      -0.111       0.038
=======================================================================================



## Challenges 
Main challenges in this project:

1. Limit on API calls to Foursquare. I had to be very conservative and considerate with making those calls, which had an impact on how well I could experiment

2. Inconsistency in Foursquare sub-categories (over 500 unique subcategories were returned) and this forced me to create my own categories

3. It was not possible for me to create a  trial subscription with Yelp due to their requirement for a business email
## Future Goals
I would have explored other regression models and used a more granular category level from Foursquare.