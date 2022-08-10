# Twitter System Design

Functional requirements :

1. User should be able to tweet
2. User should be able to follow another user
3. User should be able to see their home timeline (the main page)
4. User should be able to see their user timeline (their own tweets)

Non functional requirements :

1. Twitter is read heavy system, most users are mostly reading the tweets rather than writing a tweet
2. The system should be highly available
3. The system should have very low latency


## How to construct a user's (X) home timeline 
First we need to know which all users, user X is following. e.g. lets say user X is following 150 users, and we have a graph service which can return the list of users
a user is following. We can call the graph service passing the user X's userId to get the list of users, that user X is following.

As you can see creating a user's home timeline on the fly is time consuming and requires some amount of processing.
You need to fetch the users, whom user X is following, next get the tweets of each user, whom user X is following. Next sort the tweets in reverse chronological order.

# Cache the Home Timeline of active users

!["Fanout on a new tweet post"](fanout.PNG?raw=true)

# Precompute the user's home timeline

!["Twitter"](twitter.PNG?raw=true)

## Fanout Service :
Fanout service  is responsible for spreading out the tweet to all people following the author by inserting the tweet in to all their home timelines. 
Home timelines for active users are stored in redis cluster.

!["Fanout Service"](fan-out-write-tweet.PNG?raw=true)

**Don't fanout write for users with large number of followers. Only do fanout for users with small number of followers.**

## Hydrating the tweet
Since the timeline only contains tweet IDs they must “hydrate” those tweets, that is find the text of the tweets. Given an array of IDs they can do a multiget and get the tweets in parallel from T-bird.

## Timeline Service




# References :
How We Learned to Stop Worrying and Love Fan-In at Twitter by Mike Cvet :
https://www.youtube.com/watch?v=WEgCjwyXvwc
