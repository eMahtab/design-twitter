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

## Fan out :

Don't fanout write for users with large number of followers. Only do fan out for users with small number of followers.

# References :
