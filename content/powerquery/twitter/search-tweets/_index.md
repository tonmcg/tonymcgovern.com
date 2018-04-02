+++
title = "Search Tweets"
date = "2018-04-02"
+++

## Prerequisites
1. Follow the [Twitter authorization steps shown here](../get-token)

## Preliminaries
```javascript
let
    // load custom GetOAuthToken and SearchTweets functions from Github
    GetOAuthToken = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/GetOAuth")),
        [
            #"Binary.ToText" = Binary.ToText,
            #"Json.Document" = Json.Document,
            #"Text.ToBinary" = Text.ToBinary,
            #"Web.Contents" = Web.Contents
        ]
    ),
    SearchTweets = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TwitterSearchTweets")),
        [
            #"Binary.ToText" = Binary.ToText,
            #"Json.Document" = Json.Document,
            #"List.Generate" = List.Generate,
            #"Logical.ToText" = Logical.ToText,
            #"Number.ToText" = Number.ToText,
            #"Number.RoundUp" = Number.RoundUp,
            #"Record.AddField" = Record.AddField,
            #"Record.RemoveFields" = Record.RemoveFields,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Table.ExpandListColumn" = Table.ExpandListColumn,
            #"Table.FromList" = Table.FromList,
            #"Text.Combine" = Text.Combine,
            #"Text.ToBinary" = Text.ToBinary,
            #"Uri.Combine" = Uri.Combine,
            #"Uri.EscapeDataString" = Uri.EscapeDataString,
            #"Uri.Parts" = Uri.Parts,
            #"Web.Contents" = Web.Contents,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"ExtraValues.Error" = ExtraValues.Error,
            #"GetOAuthToken" = GetOAuthToken,
            #"consumer_key" = consumer_key,
            #"consumer_secret" = consumer_secret
        ]
    )
in
    SearchTweets
```

## Search Tweets
```javascript
let
    GetOAuthToken = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/GetOAuth")),
        [
            #"Binary.ToText" = Binary.ToText,
            #"Json.Document" = Json.Document,
            #"Text.ToBinary" = Text.ToBinary,
            #"Web.Contents" = Web.Contents
        ]
    ),
    SearchTweets = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TwitterSearchTweets")),
        [
            #"Binary.ToText" = Binary.ToText,
            #"Json.Document" = Json.Document,
            #"List.Generate" = List.Generate,
            #"Logical.ToText" = Logical.ToText,
            #"Number.ToText" = Number.ToText,
            #"Number.RoundUp" = Number.RoundUp,
            #"Record.AddField" = Record.AddField,
            #"Record.RemoveFields" = Record.RemoveFields,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Table.ExpandListColumn" = Table.ExpandListColumn,
            #"Table.FromList" = Table.FromList,
            #"Text.Combine" = Text.Combine,
            #"Text.ToBinary" = Text.ToBinary,
            #"Uri.Combine" = Uri.Combine,
            #"Uri.EscapeDataString" = Uri.EscapeDataString,
            #"Uri.Parts" = Uri.Parts,
            #"Web.Contents" = Web.Contents,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"ExtraValues.Error" = ExtraValues.Error,
            #"GetOAuthToken" = GetOAuthToken,
            #"consumer_key" = consumer_key,
            #"consumer_secret" = consumer_secret
        ]
    ),
    // Get the 500 most recent tweets that include the "powerbi" keyword
    tweets = SearchTweets("powerbi", 500, "recent")
in
    tweets
```

||statuses.created_at|statuses.id|statuses.id_str|statuses.text
|---:|:---|:---|---:|:---
|1|Thu Mar 29 13:19:08 +0000 2018|9.79347E+17|979347224080666624|RT @brianknight: Blog: Power BI Developer community March update https://t.co/tgT7rbxHaJ #PowerBI
|2|Thu Mar 29 13:17:01 +0000 2018|9.79347E+17|979346691018121217|Blog: Power BI Developer community March update https://t.co/tgT7rbxHaJ #PowerBI
|3|Thu Mar 29 13:16:26 +0000 2018|9.79347E+17|979346546885058561|#allsvenskan #sverige #fotboll #powerbi  En beräkning över hur många snittpoäng ett lag behöver ta för guld(63 poän… https://t.co/DGThlKpFLR
|...|...|...|...|...
|500|Thu Mar 29 11:05:40 +0000 2018|9.79314E+17|979313635616018432|RT @denglishbi: Looks like a #PowerBI #SSRS update is being released today to resolve some of the issues with the March 2018 release, not a…

## References
1. [Search Tweets](https://developer.twitter.com/en/docs/tweets/search/api-reference/get-search-tweets), Twtter API Reference
2. [Twitter.SearchTweets](https://github.com/tonmcg/powertweet/blob/master/M/Twitter.SearchTweets.pq) by Tony McGovern