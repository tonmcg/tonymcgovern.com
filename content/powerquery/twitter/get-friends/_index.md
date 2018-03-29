+++
title = "Get Twitter Friends"
date = "2018-04-01"
+++

## Prerequisites
1. Sign up for a [Twitter account](http://twitter.com/signup)
2. Sign in to the [Twitter Apps page](https://apps.twitter.com) and create a new application
3. Get your Consumer Key and Consumer Secret
4. Copy your Consumer Key and Consumer Secret into the GetOAuthToken function
5. Access Twitter API through other custom Power Query functions like [Get Followers](../get-followers/) or [Get Friends](../get-friends/) or [SearchTweets](../search-tweets/)

## Preliminaries
```javascript
let
    // load custom GetOAuthToken, GetFollowers, and LookupUsers functions from Github Gist
    OAuth = Text.FromBinary(Web.Contents("https://goo.gl/AmHvpe")),
    Followers = Text.FromBinary(Web.Contents("https://goo.gl/SsK6j8")),
    Users = Text.FromBinary(Web.Contents("https://goo.gl/2ZLUCd")),
    GetOAuthToken = Expression.Evaluate(OAuth, #shared),
    GetFollowers = Expression.Evaluate(Followers, #shared),
    LookupUsers = Expression.Evaluate(Users, #shared)
in
    LookupUsers
```

## Get Twitter Followers
```javascript
let
    OAuth = Text.FromBinary(Web.Contents("https://goo.gl/AmHvpe")),
    Followers = Text.FromBinary(Web.Contents("https://goo.gl/ptJJFt")),
    Users = Text.FromBinary(Web.Contents("https://goo.gl/ptJJFt")),
    GetOAuthToken = Expression.Evaluate(OAuth, #shared),
    GetFollowers = Expression.Evaluate(Followers, #shared),
    LookupUsers = Expression.Evaluate(Users, #shared),
    Followers = 
in
    GetOAuthToken
```
<pre>bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX</pre>

The OAuth token is masked here for privacy. Your token should be alphanumeric, consisting of numerous letters and numbers preceded by "bearer".

## References
1. [Get Data from Twitter API](https://chris.koester.io/index.php/2015/07/16/get-data-from-twitter-api-with-power-query/) by Chris Koester
2. [GetOAuthToken](https://gist.github.com/tonmcg/d07ddacf298fc3977cc31d8e9788421b) by Tony McGovern