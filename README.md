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

## Recommendation List by the Simple Recommender Approach

	![image](https://user-images.githubusercontent.com/68769656/157386872-eb49580f-afd2-4ba2-9aa7-27a903f9132f.png)


