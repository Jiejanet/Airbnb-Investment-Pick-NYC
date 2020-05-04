# Airbnb-Investment-Pick-NYC
Where should you invest an apartment and turn into Airbnb in NYC? 
# Project Overview

A friend of mine got into a side hussle of being Airbnb host few years ago. Renting undervalued apartments, renovating into Instagrammable home, and then list on Airbnb. It has since became a handsome additional cash flow stream. He asked me where to invest at NYC in 2020 that would be more profitable, if he wanted to buy a two bedroom house and turned it into Airbnb for most of the time. 

I don't call myself a data analyst for giving out recommendation without data backing it. I looked into Kaggle, and boy, there are a trove of Zillow and Airbnb dataset, so the game is on!

Full disclosure, the analysis was done in 2019 and the quanrentine life is a perfect time to reflec and put things together. 2020 supposed to be a stellar year for Airbnb, and then we all know the pandemic hit. 

## Data Source

> Zillow https://www.kaggle.com/paultimothymooney/zillow-house-price-data#City_MedianRentalPrice_2Bedroom.csv

Zillow dataset has the median housing value history of a 2bedroom apartment for NYC to the latest of 2017. 

> Airbnb [New York City, New York, United States] > [ listings.csv.gz](http://data.insideairbnb.com/united-states/ny/new-york-city/2020-04-08/data/listings.csv.gz)]


This is a very detailed listings data for New York City. Encompassing all the information from price, number of beds/rooms, availability, number of reviews and latest review date, etc.

Based on the 2 files, I am able to calculate the median 2 bedroom house value for NYC and the median Airbnb rental price of a 2bedroom  

## Set Up Metric

What would be considered a profitable region(zip code) for my friend to buy house and later rent out on Airbnb? 

A 2bedroom apartment in a region where the occupancy rate for majority of the listings are at least above 50%, and the payback period are as short as possible among all the other locatioons at NYC.

Short-term rental business is a well sought after business model. All the investors out there should have exploited all the possibility. It is reasonable to think that if there is zero Airbnb listing on certain locations, it probably suggests that those zipcodes are not suitable for Airbnb business. After filtering for zipcodes that have Airbnb rental price, the potential profitable zipcodes went down from 200+ toaround 30. 

### ROI

**ROI = (Annual Rental Revenue – Expenses and Costs) / Property Price** 

**Annual Rental Revenue:** median of daily rental price * 75% * 365

2bedroom apartment can usually charge higher amount and have higher occupancy rate compared with of 1bedroom apartment comparable square, because it will be able to accommodate more travelers, and the per person booking cost for traveler would be lower.

1. Assuming median rental price for each zip code will not fluctuate a lot in one or two years, for each zip code, the median Airbnb rental price scraped in 2019 are used as an estimated daily rental revenue for that area. 
2. Normal property type such as, condominium and apartment are more suitable investment for my friend, so I only included rental price of 2bedroom apartment-like properties to calculate the median daily rental revenue. 
3. Furthermore, the listing data were scraped from Airbnb, there would be some listing that hosts do not intend to rent out anymore but don't care deactivate the listing. These inactive listings are considered as invalid priced for rental market. Therefore, I only keep listings that have the last reviews at most 1 year ago and new listings with 0 review which has availability updated within 2 months in calculation.
4. In addition, 75% occupancy rate is used to estimate the annually rental revenue, which is totally a no-brainer. That's why I deduced occupancy rate from rental data availability later in the article to weigh against.

**Expenses and costs:** 0

The cost relating to keep the business running and own the houses, such as, utility, cleaning services and property taxes. The cleaning fee travelers pay would be enough to cover the utility and cleaning services. For simplicity, property taxes are not taken into account. 

**Property Price:** predicted median home value for 2020

The zillow dataset only have median house value data that date back to as recent as June 2017, whereas, the investments are yet to be made in 2020. Therefore, I predict the median home value in 2020 using Facebook Prophet packages.



### Payback Period 

The payback period is the reciprocal of ROI. If you are interested you can [download the dashboard workbook here](https://github.com/Jiejanet/Airbnb-Investment-Pick-NYC) to interactively play with the plots. As we can see that the winner is zipcode 11434, adjoining JFK airport, has a payback period of 13 years. The runner up is zipcode 10305 on Staten Island where you can Ferry to Manhattan, with 14 years to payback. The third place goes to zipcode 10036 where Time Sqaure lit, hitting almost 20 years to payback.  Brooklyn area also has relatively better payback period.

![Payback Period](https://github.com/Jiejanet/Airbnb-Investment-Pick-NYC/blob/master/1.png)




### Occupancy rate

The rough assumption of 75% yearly occupancy rate for ROI is straight arbitrarily. Therefore, I set up additional metric to pick investment location,  the popularity with regards to occupancy rate for majority of listings within that location.

According to [NYC Company market report]( https://assets.simpleviewinc.com/simpleview/image/upload/v1/clients/newyorkcity/FYI_Hotel_reports_December_2019_29ba065f-346d-4f4a-8ad8-1f95e62ad938.pdf), New York enjoys its busiest travel season from July to September with hotel occupancy rate hitting 90%. Since all the rental data were scraped in 2019 July, the `availability_90` show a cursory estimation of how popular an area is during busiest travel season, July to September. 

Indeed, some Airbnb listings have better amenities and better reviews, thus are more likely to be fully booked. But if 75% of the listings in an area enjoy higher occupancy rate than other locations, then that place is more likely to have rental demands await to be met. Therefore, my friend can easily fill out vacancy for his property in that area, without striving for `Super Host` title and review exposure desperately. 

There are 3 quantile occupancy rate for each zip code.

1. 25 quantile (the occupancy rate that the top 25% of occupied listings at least have), 
2. 50 quantile (median occupancy rate for listings in that zip codes) 
3. 75 quantile (the occupancy rate that the top 75% of occupied listings at least have) 

![ROI vs. Occupancy rate](https://github.com/Jiejanet/Airbnb-Investment-Pick-NYC/blob/master/2.png)

The X axis is the 75 quantile of Occupancy and Y axis is the ROI. The color represents the 25 quantile, the greener it gets the lower the 25 quantile occupancy rate. the more pinkish the higher 25 quantile occupancy rate. The mark size shows the 50 quantile rate, the bigger it gets the higher the 50 quantile rate.

We can see that 11434(JFK airport) and 10305(Staten Island) high up in the ROI metric had really low occupancy rate for majority of the listings in that area. Even the leading sheep of top 25% listings has only 50% occuoancy rate for 10305(Staten Island) and 75% occupancy rate for 11434(JFK airport). 

Timesqaure(zipcode 10036) is a pretty saturated market area. You have to spearhead for the top25% listings to have 93% occupancy rate. However, the 75 quantile occupancy rate is only 33% below NYC's median of 40%. If you are hosting an Airbnb in this region, you probably will find yourself curating an social media presence, making booking more flexible, turning on instant booking to gain better SEO. And my friend doesn't like last minute booking, so this location is out

## Final Recommendation

Finally, my best pick for him would be zipcode 11231 and 11215 in Brooklyn, and 10025, Manhattan Valley near the Columbia University. The 3 locations have above the median ROI, and the median occupancy rate is hitting around 90%. 

Surprisingly, Time Square, Greenwich Village, Central Park, these famous touristy areas do not come as the top investment recommendations. Because their 75 quantile occupancy rates are all below the New York City median. Part of the reasons might be that those areas are full of competitors, so if your listings are the low dog in that area, you probably won’t be able to get much occupancy even during the travel season. Whereas, Brooklyn are thriving areas with more and more demands and less competitive. 

 

## Data Cleaning

### Zillow

1.Check for anomaly. Make sure all the housing value are reasonable. No 0, 1 dollar house value.

2.If there is any gap between history, use linear imputation to fill the missing values

3.Use Python Prophet package to make predictions of the housing price in 2020, based on price history.

### Airbnb Rental

1.Check for anomaly rental price. 

2.`City` and `State` level information are quite messey. For example, '纽约', 'Ny' and 'NY' all refer to New York. Luckily, `neighbourhood_cleansed`and `neighbourhood_group_cleansed` are complete, and can be used reliably for location information. Although, there are 109 missing values in `market` column, the information can be restored by referring to `State` column, that all the missing records come from NY. In addition, I use `uszipcode` library to infer 517 missing zipcodes based on latitude and longitude.

3.Filter for 2 bed room, entire place apartment in calculating rental price. No treehouse, boathouse, tent, RV, and all sorts of unique property types. 

