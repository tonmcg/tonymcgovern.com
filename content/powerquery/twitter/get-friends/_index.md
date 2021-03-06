+++
title = "Get Twitter Friends"
date = "2018-04-02"
+++

## Prerequisites
1. Follow the [Twitter authorization steps shown here](../get-token)

## Preliminaries
```javascript
let
    // load custom GetOAuthToken and GetFriends functions from Github
    GetOAuthToken = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/GetOAuth")),
        [
            #"Binary.ToText" = Binary.ToText,
            #"Json.Document" = Json.Document,
            #"Text.ToBinary" = Text.ToBinary,
            #"Web.Contents" = Web.Contents
        ]
    ),
    GetFriends = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TwitterGetFriends")),
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
    GetFriends
```

## Get Friends Ids
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
    GetFriends = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TwitterGetFriends")),
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
    // Get the first 150 user ids from those that the MSPowerBI Twitter account follows
    friends = GetFriends("MSPowerBI", 150)
in
    friends
```

||id
|---:|--:
|1|264501255
|2|268575250
|3|794575879
|4|61598916
|...|...
|150|26157564

## References
1. [Get Friends Ids](https://developer.twitter.com/en/docs/accounts-and-users/follow-search-get-users/api-reference/get-friends-ids), Twtter API Reference
2. [Twitter.GetFriends](https://github.com/tonmcg/powertweet/blob/master/M/Twitter.GetFriends.pq) by Tony McGovern