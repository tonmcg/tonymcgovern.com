+++
author = "Tony McGovern"
categories = ["PowerTweet","Power Query M"]
tags = ["vignette"]
date = "2018-04-02"
description = "Introduction to PowerTweet M functions"
featured = "Influencers_Squared.png"
featuredalt = "Influencers Squared"
featuredpath = "date"
linktitle = ""
title = "PowerTweet: Twitter Data with Power Query"
type = "post"

+++

## Introduction

This post covers many of the options available in `PowerTweet: Twitter Data with Power Query`, a [series of Power Query/M functions that allow you to interact with Twitter data](../../powerquery/twitter/).

### Search Tweets

Retrieve up to 45,000 tweets when using a search term. Here we search for the 500 most recent tweets that include the "powerbi" keyword.
```javascript
let
    tweets = SearchTweets("powerbi", 500, "recent")
in
    tweets
```

### Get Friends

Retrieve up to 75,000 ids of users that a target account follows. Here we return the first 150 ids of users that the MSPowerBI Twitter account follows.
```javascript
let
    friends = GetFriends("MSPowerBI", 150)
in
    friends
```

### Get Followers

Retrieve up to 75,000 ids of users that follow a target account. Here we return the first 150 ids of users that follow the MSPowerBI Twitter account.
```javascript
let
    followers = GetFollowers("MSPowerBI", 150)
in
    followers
```

### Lookup Users

Returns up to 30,000 user objects specified by comma-separated values passed `user_id` parameter. Best used in conjunction with collections of user IDs returned from `GetFriends` or `GetFollowers`. Here we return the first 150 user ids with the `GetFriends` function and then pass those ids to the `LookupUsers` function to get a complement of information for those users.
```javascript
let
    // Get the first 150 user ids from those that the MSPowerBI Twitter account follows
    friendIds = GetFriends("MSPowerBI", 150),
    // Pass these user ids to the LookupUsers function to return a full user object complement
    friendObjects = LookupUsers(Text.Combine(friendIds[id],","))
in
    friendObjects
```


### Get Timeline

Retrieve up to 300,000 of the most recent tweets that contain the specified user. Here we return the most recent 5,000 user statuses from the MSPowerBI Twitter account timeline.
```javascript
let
    statuses = GetTimeline("MSPowerBI", 5000),
    RemovedDuplicates = Table.Distinct(statuses, {"id_str"})
in
    RemovedDuplicates
```

### PowerTweet Documentation
[Navigate you browser here](../../powerquery/twitter/) for more information on how to use `PowerTweet`, including code snippets on how to authenticate to the Twitter API and other Power Query Twitter functions.

### Sample Power BI Report
The embedded Power BI report below uses many of the functions listed here.
<iframe class="pre" width="100%" height="700" src="https://app.powerbi.com/view?r=eyJrIjoiODdhNDg3OTItODhmZS00NjFkLWIwODAtNjg2NGU2YzRmOTU4IiwidCI6ImRjNTliNTFkLWVmZDItNDYyNi04M2EyLTljMmU2MzE1MTcwZiIsImMiOjZ9" frameborder="0" allowFullScreen="true"></iframe>