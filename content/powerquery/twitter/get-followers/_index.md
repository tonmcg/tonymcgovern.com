+++
title = "Get Twitter Followers"
date = "2018-04-02"
+++

## Prerequisites
1. Follow the [Twitter authorization steps shown here](../get-token)

## Preliminaries
```javascript
let
    // load custom GetOAuthToken and GetFollowers functions from Github
    GetOAuthToken = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/GetOAuth")),
        [
            #"Binary.ToText" = Binary.ToText,
            #"Json.Document" = Json.Document,
            #"Text.ToBinary" = Text.ToBinary,
            #"Web.Contents" = Web.Contents
        ]
    ),
    GetFollowers = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TwitterGetFollowers")),
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
            #"Text.ToBinary" = Text.ToBinary,
            #"Uri.Combine" = Uri.Combine,
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
    GetFollowers
```

## Get Followers Ids
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
    GetFollowers = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TwitterGetFollowers")),
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
            #"Text.ToBinary" = Text.ToBinary,
            #"Uri.Combine" = Uri.Combine,
            #"Web.Contents" = Web.Contents,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"ExtraValues.Error" = ExtraValues.Error,
            #"GetOAuthToken" = GetOAuthToken,
            #"consumer_key" = consumer_key,
            #"consumer_secret" = consumer_secret
        ]
    )
    // Get the first 150 user ids from those that follow the MSPowerBI Twitter account
    followers = GetFollowers("MSPowerBI", 150)
in
    followers
```

||id|
|---:|---:|
|1|484853112
|2|3313513940
|3|68508911
|...|...
|150|972211134156619776

## References
1. [Get Followers Ids](https://developer.twitter.com/en/docs/accounts-and-users/follow-search-get-users/api-reference/get-followers-ids), Twtter API Reference
2. [Twitter.GetFollowers](https://github.com/tonmcg/powertweet/blob/master/M/Twitter.GetFollowers.pq) by Tony McGovern