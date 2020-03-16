## CAPSTONE PROJECT:
## DEVELOPMENT OF A RESTAURANT WINE LIST RECOMMENDER



## PROBLEM STATEMENT:

In the United States in 2018, the share of on-premise wine sales in the domestic market stood at 43.9 percent. The wine industry, for the most part, maintained their pricing and wrestled with new ways to reach and engage a new audience of millenials in the wine market to spur category growth. For restauranteurs seeking to differentiate their establishment and appeal to their targeted patrons, a lot of time, effort and expense is invested in the development of their restaurant's wine lists to drive sales for and differentiate their establishment.

Wine lists can be extraordinarily personal to restaurant owners, as they offer a method by which to engage their customers. Great lists can effectively compliment the food that is served and depending on the establishment they can be extraordinarily profitable. Some wine lists, such as Smith and Willensky's Steakhouse in Manhattan offer over 1000 different bottles of wine to serve an affluent "expense report" crowd. Neighborhood restaurants try to find a balance between an engaging wine list and effectively managing inventory so no customer is disappointed by an out of stock. That's why it's not unheard of for some wine lists created by neighborhood restaurants to reflect over 150 different labels and take over three months for the owners to cultivate.

It is entirely feasible that the current, somewhat antiquated process of wine list development can be bolstered by harnessing new technology. Through the use of Data Science, restauranteurs can benefit from real-time consumer driven information that will allow them put their finger on the pulse of what their local market may already be responding to, and reduce the time and resources it currently takes to create their establishment's wine list, while increasing sales of proven, popular wine brands and styles.

This capstone project will seek to create a model for the generation of a restaurant's wine list, using a database of wine products that have been pre-rated by consumers, and adding features that can allow a level of personalization to compliment multiple establishments.


## OBJECTIVES:

-  Establish a database based on data procured from the popular consumer wine app Vivino
-  Goal: Target and procure approximately 40000 products, user ratings, and prices for the database
-  Construct a Recommender System for Wine List Generation
-  Apply additional feature engineering to add to customization of the final wine list


## EXECUTIVE SUMMARY:

Our first step was to review potential sources that we could access to obtain the type of data required. Our search brought us to Vivino, an online wine marketplace and wine app founded in 2010 that claims to have a wine database containing more than 10 million different wines, and had 35 million users globally. Consumers are invited to register online, join the global Vivino community and then rate wine they are familiar with, search the ratings for new discoveries, and order if so inclined.

After deeper research on Vivino, I realized that any attempt to scrape their data would have to account for their site's architecture which utilizes webpages that deliver results on an infinite scrolling page. The best option to deliver an effective scrape in this scenario would be to utilize the Selenium package in Python, which is an app development and evaluation tool, but can effectively be used for web scraping. For more insight as to how Selenium was used in this particular project, you can read my two blogs related to this subject:

*“Harvesting Data: How web scraping can increase your insight into the wine market”*

https://medium.com/@tony.bosco/harvesting-data-how-web-scraping-can-increase-your-insight-into-the-wine-market-8c533f1f64f8

*"Crushing Retail Wine Data with Selenium and Python"*

https://medium.com/@tony.bosco/crushing-retail-wine-data-with-selenium-and-python-f16a13e046a4

Overall, I was able to generate over 33,163 individual wine product observations as a result of scraping efforts utilizing Selenium. These observations included the name of the winery (or vineyard), the wine (containing label and varietal information), the Vivino consumer rating, the price per bottle, and the location (country and region or appellation) of the winery.

Timing was a factor, as our initial scrapes had higher yields of observations than our later scrapes, potentially due to traffic on the website. As the scraped observation counts became inconsistent in totals, I needed to approach the data from several different angles with multiple scrapes determined by sets of altered site based search engine parameters. This provided four separate files to build each wine category (Red, White, Sparkling, Rose, Dessert, and Port), which were then cleaned, structured, and stitched together in a master catalog and subject to additional global cleaning functions prior to gathering insights through Exploratory Data Analysis (EDA).

Initial EDA of the master wine catalog file produced the following key insights regarding the master wine catalog dataset:


1) Wine Category observation quantities:


| Wine Category	|# Bottles/Observations|
|---------------|----------------------|
|   White	    |        11,325        |
|   Sparkling   |	     10,300        |
|   Red	        |         9,500        |
|   Port	    |           873        |
|   Dessert	    |           845        |
|   Rose	    |           320        |




2) The majority of the database (over 50%) reflects wine bottle pricing under $100.00


3) The order of the top five wine styles Chardonnay, Brut Champagne, Blended table wine, Cabernet Sauvignon and Sauvignon Blanc respectively, reflect current national retail sales trends tracking consumer demand.


4) There is a high correlation between ratings and higher priced entries in the White and Red categories at price levels from  120.00𝑡𝑜 315.00 a bottle.


5) There is a high correlation between price and ratings which we will explore further. Most notably we see a strong correlation across all price points between ratings of 4.1 and 4.7.


While extremely pleased with the amount of data scraped for this project, one key limitation encountered was access to user profile data. Unlike the ability to access a closed system internally, web scraping sometimes requires you access and stitch together fragments of data based on what is available from the website. As we approached this project we realized the following challenges with user profile data on Vivino:

-  Users, even power users, do not rate every observation in the database. Some only average 5 reviews each.
-  The data is collected after several windows are clicked by Selenium and then after another infinite scroll.
-  Ratings are reflected numerically by wine bottle, but in stars in user profiles, which requires translation.
-  Fragmented user review datapoints would need to be intricately aligned with each of the 33,000+ bottle observations

After conducting additional online research, I encountered a white paper developed by Neema Kotonya, Paolo De Cristofaro, and Emiliano De Cristofaro from the University College of London entitled, Of Wines and Reviews: Measuring and Modeling the Vivino Wine Social Network released on Apr 29, 2018. Their collection of user ratings was developed "over five months (November 2016 to March 2017)". Given the timeframe necessary for the delivery of this particular capstone project, I would not be able to achieve as robust a collection. For purposes of this particular project, a simulation of data was constructed off of the average of consumer ratings I scraped for every observation. This will allow for the assembly ofmthe primary components of the recommender system.

The initial test of the recommender system will focus on the relationship between wine label (the name of the wine captured for each observation) and the ratings provided by the users. Using pairwise_distance in sklearn adjusted to cosine, we will assess cosine similarity and build phase 1 of our recommender. In the near future, Phase 2 and Phase 3 will explore Pearson Correlation and a Collaborative Filtering Model using k-Nearest Neighbors, respectively.


## FINAL CONCLUSIONS AND RECOMMENDATIONS:

Overall the initial recommender shows promise, however while "functional" it does require additional development.
The most basic of recommender systems have their limitations within the boundaries of the axes their pivot table is set upon, and our first attempt is no exception.

For the depth and complexity we seek for this recommender, we need to significantly increase the value of our cosine similarity.

We believe this can be achieved by the following:

1) **Obtain a more robust user profile dataset:** Initiate either a long term six-month user profile scraping endeavor, or an investment or partnership to secure a much larger user profile dataset, with more depth of profile characteristics, that would impact the results of our recommender.


2) **Leverage KNN and pursue a Collaborative Filtering Recommender System as a new foundation:** kNN is a machine learning algorithm that will help identify clusters of similar users based on common wine ratings, and make predictions using the average rating of top-k nearest neighbors.


3) **Leverage Feature Weighting as in a Content Based Recommenders system:** In a content based recommender system, every item is represented by a feature vector or an attribute profile which helps to define the item more effectively. Our basic recommender didn't allow for some of the data we collected or features we engineered to be utilized in tandem. With this approach, we could consider a set of additional features and their individual distance measures with respect to each item.


4) **Build a HYBRID Recommender:** The goal would be to construct a recommender that can leverage the power of both Collaborative Filtering AND Feature Weighting of a Content Based Recommnder system.


5) **Consider splitting feature engineering in half:** One final consideration would be to split the feature engineering in half, consisting of an upfront filtering system and a back end Hybrid recommender. This approach may ultimately be the most effective, as it allows for a high degree of customization for the chef or bar/restaurant owner responsible for the wine list. If the dataset was filtered on wine category (red, white, sparkling, etc.) prior to running the recommender, the recommender can focus on delivering results against different features, such as varietal, region, vintage and wine pairing with meals types each of which would have its own weight value.


In conclusion, there is still a lot of potential in the opportunity to produce a recommender that can simplifies the development of a restaurant's wine list. We will continue to pursue these next steps with the long term goal of developing an app that can eventually deliver both the list and eventually deliver a tasting kit based on the results.



## SOURCE DOCUMENTATION:

1) Vivino - The world's largest online wine marketplace powered by a community of millions. www.vivino.com


2) Of Wines and Reviews: Measuring and Modeling the Vivino Wine Social Network released on Apr 29, 2018 authored by Paolo De Cristofaro, and Emiliano De Cristofaro from the University College of London - https://www.groundai.com/project/of-wines-and-reviews-measuring-and-modeling-the-vivino-wine-social-network/1


3)  Feature Weighting in content based recommendation systems using social network analysis - Conference Paper, January 2008 authored by Niloy Ganguly and Pabitra Mitra, Indian Institute of Technology, Kharagpur - https://www.researchgate.net/publication/221022528_Feature_weighting_in_content_based_recommendation_system_using_social_network_analysis



## DATA FILES¶


-  [wine_catalog.csv](#./data/wine_catalog.csv)
-  [user.csv](#./data/user.csv)



## CONTENTS

- [WEB SCRAPING CODE EXPLANATION](#01-WEB SCRAPING CODE EXPLANATION)
- [WINE CATEGORY BOOK CONSTRUCTION](#02-WINE CATGEORY BOOK ASSEMBLY)
- [MASTER WINE CATALOG ASSEMBLY](#03-MASTER WINE CATALOG ASSEMBLY)
- [USER RATING DATASET](#04-USER RATING DATASET)
- [RECOMMENDER - PHASE 1](#05-RECOMMENDER - PHASE 1)
- [RECOMMENDER - PHASE 2](#06-RECOMMENDER - PHASE 2)


