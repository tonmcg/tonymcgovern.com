+++
title = "Search Tweets"
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
    // load custom SearchTweets function from Github Gist
    Token = Text.FromBinary(Web.Contents("https://goo.gl/AmHvpe")),
    GetOAuthToken = Expression.Evaluate(Token, #shared),
    Search = Text.FromBinary(Web.Contents("https://goo.gl/sMVJxY")),
    SearchTweets = Expression.Evaluate(Search, #shared)
in
    SearchTweets
```

## Search Twitter for 'Power BI' Tweets
```javascript
let
    Token = Text.FromBinary(Web.Contents("https://goo.gl/AmHvpe")),
    GetOAuthToken = Expression.Evaluate(Token, #shared),
    Search = Text.FromBinary(Web.Contents("https://goo.gl/sMVJxY")),
    SearchTweets = Expression.Evaluate(Search, #shared),
    // search Twitter for tweets about 'powerbi' from the last 6 to 9 days
    PowerBI = SearchTweets("powerbi", 100)
in
    PowerBI
```

|     |created_at|id |id_str|text|...|
|:---:|:---|:---|:---|:---|:---|
|1    |Sun Mar 25 19:59:03 +0000 2018|977998315349512000|977998315349512192|Agradecimentos ao professor @rommelcarneiro por sempre apoiar as iniciativas de intermediar o relacionamento entre… https://t.co/hJBDyTcLlF|...|
|2    |Sun Mar 25 19:54:16 +0000 2018|977997113941545000|977997113941544961|"RT @GuyInACube: Use the #PowerBI Rebind API to move from cached to #AzureAS https://t.co/ar2cb9uHuS https://t.co/qBICmwY545"|...|
|3    |Sun Mar 25 19:54:09 +0000 2018|977997082912023000|977997082912022528|Wie können Sie Ihre Daten auf Karten darstellen? Mit #PowerView. |...|
|4    |Sun Mar 25 19:53:18 +0000 2018|977996868671058000|977996868671057921|testing out the PowerBI #PSConfEU app|...|
|...  |...|...|...|...|...|
|4300 |Mon Mar 19 01:49:31 +0000 2018|975549797905117000|975549797905117184|RT @AndreiFateevUS: Make sure you're all caught up on #PowerBI! Watch on-demand sessions or register for upcoming live webinars: https://t.…|...|

## References
1. [Get Data from Twitter API](https://chris.koester.io/index.php/2015/07/16/get-data-from-twitter-api-with-power-query/) by Chris Koester
2. [GetOAuthToken](https://gist.github.com/tonmcg/d07ddacf298fc3977cc31d8e9788421b) by Tony McGovern
3. [SearchTweets](https://gist.github.com/tonmcg/6c0ee19cdf2965e911fc176290ed2f16) by Tony McGovern
4. [Search Tweets Twitter API Reference](https://developer.twitter.com/en/docs/tweets/search/api-reference/get-search-tweets)