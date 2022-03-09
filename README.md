# Recommendation System- An Introduction

![image](https://user-images.githubusercontent.com/68769656/157385989-4f50187e-77a5-4ce6-a4ed-7bdd364b8ff9.png)

A recommendation system provides suggestions to the users through a filtering process that is based on user preferences and browsing history. The information about the user is taken as an input. The information is taken from the input that is in the form of browsing data. This information reflects the prior usage of the product as well as the assigned ratings. A recommendation system is a platform that provides its users with various contents based on their preferences and likings. A recommendation system takes the information about the user as an input. The recommendation system is an implementation of the machine learning algorithms.

A recommendation system also finds a similarity between the different products. For example, Netflix Recommendation System provides you with the recommendations of the movies that are similar to the ones that have been watched in the past. Furthermore, there is a collaborative content filtering that provides you with the recommendations in respect with the other users who might have a similar viewing history or preferences. There are two types of recommendation systems – Content-Based Recommendation System and Collaborative Filtering Recommendation.

# A walkthrough of the Project

![image](https://user-images.githubusercontent.com/68769656/157386116-0210ca65-ec19-4b92-825d-07afc8ff2a76.png)

In this project, we will attempt at implementing a few recommendation algorithms (content based, popularity based and collaborative filtering) and try to build an ensemble of these models to come up with our final recommendation system. With us, we have two MovieLens datasets.

The Full Dataset: Comprises of 26,000,000 ratings and 750,000 tag applications applied to 45,000 movies by 270,000 users. Includes tag genome data with 12 million relevance scores across 1,100 tags.
The Small Dataset: Comprises of 100,000 ratings and 1,300 tag applications applied to 9,000 movies by 700 users.

We will build a Simple Recommender using movies from the Full Dataset whereas all personalised recommender systems will make use of the small dataset (due to the computing power I possess being very limited). As a first step, we will build a simple recommender system.

# Introduction to the Speciality Library: Surprise

![image](https://user-images.githubusercontent.com/68769656/157386211-7ac6a2cc-eeb0-440d-ac45-28eb7388d3a6.png)

The name SurPRISE (roughly :) ) stands for Simple Python RecommendatIon System Engine. Surprise is a Python scikit for building and analyzing recommender systems that deal with explicit rating data.

Surprise was designed with the following purposes in mind:

Give users perfect control over their experiments. To this end, a strong emphasis is laid on documentation, which we have tried to make as clear and precise as possible by pointing out every detail of the algorithms.
Alleviate the pain of Dataset handling. Users can use both built-in datasets (Movielens, Jester), and their own custom datasets.
Provide various ready-to-use prediction algorithms such as baseline algorithms, neighborhood methods, matrix factorization-based ( SVD, PMF, SVD++, NMF), and many others. Also, various similarity measures (cosine, MSD, pearson…) are built-in.
Make it easy to implement new algorithm ideas. Provide tools to evaluate, analyse and compare the algorithms’ performance.  

Please refer to the following link for any further information: http://surpriselib.com/

# Major Approaches followed for the Project

# 1. The Simple Recommender Approach

The Simple Recommender offers generalized recommnendations to every user based on movie popularity and (sometimes) genre. The basic idea behind this recommender is that movies that are more popular and more critically acclaimed will have a higher probability of being liked by the average audience. This model does not give personalized recommendations based on the user.
The implementation of this model is extremely trivial. All we have to do is sort our movies based on ratings and popularity and display the top movies of our list. As an added step, we can pass in a genre argument to get the top movies of a particular genre

We will use the IMDB Ratings to come up with our Top Movies Chart. I will use IMDB's weighted rating formula to construct my chart. Mathematically, it is represented as follows:

   Weighted Rank (WR) = (v ÷ (v+m)) × R + (m ÷ (v+m)) × C
where,   
  R = average for the movie (mean) = (Rating)
  v = number of votes for the movie = (votes)
  m = minimum votes required to be listed in the Top 250 
  C = the mean vote across the whole report 

The next step is to determine an appropriate value for m, the minimum votes required to be listed in the chart. We will use 90th percentile as our cutoff. In other words, for a movie to feature in the charts, it must have more votes than at least 96% of the movies in the list.

### 1.1. Additionally, we have pivoted them by the Movie Genres. 

# 2. Content Based Recommender

The recommender we built in the previous section suffers some severe limitations. For one, it gives the same recommendation to everyone, regardless of the user's personal taste.

For a person who loves codemy movies (and hates drama) were to look at our Top 15 Chart,he/she wouldn't probably like most of the movies. If he/she were to go one step further and look at our charts by genre, still wouldn't be getting the best recommendations.

For instance, consider a person who loves Dilwale Dulhania Le Jayenge, My Name is Khan and Kabhi Khushi Kabhi Gham. One inference we can obtain is that the person loves the actor Shahrukh Khan and the director Karan Johar. Even if he/she were to access the romance chart, they wouldn't find these as the top recommendations.

To personalise our recommendations more, we will be going to build an engine that computes similarity between movies based on certain metrics and suggests movies that are most similar to a particular movie that a user liked.

Since we will be using movie metadata (or content) to build this engine, this also known as Content Based Filtering.

We will build two Content Based Recommenders based on:

### 2.1. Movie Overviews and Taglines
### 2.2. Movie Cast, Crew, Keywords and Genre

To build our standard metadata based content recommender, we will need to merge our current dataset with the crew and the keyword datasets. Let us prepare this data as our first step.

We now have our cast, crew, genres and credits, all in one dataframe. Let us wrangle this a little more using the following intuitions:

Crew: From the crew, we will only pick the director as our feature since the others don't contribute that much to the feel of the movie.The director's touch was the most important parameter as we have seen in the above segment.

Cast: Choosing Cast is a little more tricky. Lesser known actors and minor roles do not really affect people's opinion of a movie. Therefore, we must only select the major characters and their respective actors. Arbitrarily we will choose the top 4 actors that appear in the credits list.

Our approach to building the recommender is going to be extremely hacky. What we plan on doing is creating a metadata dump for every movie which consists of genres, director, main actors and keywords. Then we use a similar apporach as above-i.e. Count Vectorizer to create our count matrix as we did in the Description Recommender. The remaining steps would be similar to what we did earlier: we calculate the cosine similarities and return movies that are most similar.

These are steps that we need to follow in the preparation of genres and credits data:

Strip Spaces and Convert to Lowercase from all our features. This way, our engine will not confuse between George Clooney and George Hamilton.
Mention Director 3 times to give it more weight relative to the entire cast

# 3. Collaborative Filtering Application

Uptill now the recommender that we have created, our content based engine suffers from some severe limitations. It is only capable of suggesting movies which are close to a certain movie. That is, it is not capable of capturing tastes and providing recommendations across genres.

Also, the engine that we built is not really personal in that it doesn't capture the personal tastes and biases of a user. Anyone querying our engine for recommendations based on a movie will receive the same recommendations for that movie, regardless of who s/he is.

Therefore, in this section, we will use a technique called Collaborative Filtering to make recommendations to Movie Watchers. Collaborative Filtering is based on the idea that users similar to a me can be used to predict how much I will like a particular product or service those users have used/experienced but I have not.

We shall not be implementing Collaborative Filtering from scratch. Instead, I will use the Surprise library that used extremely powerful algorithms like Singular Value Decomposition (SVD) to minimise RMSE (Root Mean Square Error) and give great recommendations.

To get an understanding of the algorithm, please go through the following link: https://towardsdatascience.com/understanding-singular-value-decomposition-and-its-application-in-data-science-388a54be95d#:~:text=In%20linear%20algebra%2C%20the%20Singular,important%20applications%20in%20data%20science.

# 4. A Combination Recommender

![image](https://user-images.githubusercontent.com/68769656/157388529-aa94283b-64e8-4fd6-a369-a92d2d0c0413.png)

In this section, we will try to build a movie recommender that brings together techniques we have implemented in the content based and collaborative filter based engines. This is how it will work:

For this we shall be creating a function recommend_my_movie()- which will be responsible to provide out the recommendation list

Input: User ID and the Title of a Movie  
Output: Similar movies sorted on the basis of expected ratings by that particular user.


# Conclusions

In this notebook, we have built 4 different recommendation engines based on different ideas and algorithms. They are as follows:

**Simple Recommender:** This system used overall TMDB Vote Count and Vote Averages to build Top Movies Charts, in general and for a specific genre. The IMDB Weighted Rating System was used to calculate ratings on which the sorting was finally performed.

**Content Based Recommender:** We built two content based engines; one that took movie overview and taglines as input and the other which took metadata such as cast, crew, genre and keywords to come up with predictions. We also deviced a simple filter to give greater preference to movies with more votes and higher ratings.

**Collaborative Filtering Approach:** We used the powerful Surprise Library to build a collaborative filter based on single value decomposition. The RMSE obtained was less than 1 and the engine gave estimated ratings for a given user and movie.

**Combined Recommendation Engine:** We brought together ideas from content and collaborative filterting to build an engine that gave movie suggestions to a particular user based on the estimated ratings that it had internally calculated for that user.

Overall the data source is very well oriented, and not much of a cleaning task was required. Will be adding on many more updates!!!

![image](https://user-images.githubusercontent.com/68769656/157389136-0bce3d05-f40e-4115-bc65-df50943b35fc.png)




