+++
title = "Lookup Twitter Users"
date = "2018-04-02"
+++

## Prerequisites
1. Follow the [Twitter authorization steps shown here](../get-token)

## Preliminaries
```javascript
let
    // load custom GetOAuthToken and LookupUsers functions from Github
    GetOAuthToken = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/GetOAuth")),
        [
            #"Binary.ToText" = Binary.ToText,
            #"Json.Document" = Json.Document,
            #"Text.ToBinary" = Text.ToBinary,
            #"Web.Contents" = Web.Contents
        ]
    ),
    LookupUsers = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TwitterLookupUsers")),
        [
            #"Binary.ToText" = Binary.ToText,
            #"Json.Document" = Json.Document,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"List.Range" = List.Range,
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
            #"Text.Split" = Text.Split,
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
    LookupUsers
```

## Lookup Friends
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
    LookupUsers = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TwitterLookupUsers")),
        [
            #"Binary.ToText" = Binary.ToText,
            #"Json.Document" = Json.Document,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"List.Range" = List.Range,
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
            #"Text.Split" = Text.Split,
            #"Uri.Combine" = Uri.Combine,
            #"Web.Contents" = Web.Contents,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"ExtraValues.Error" = ExtraValues.Error,
            #"GetOAuthToken" = GetOAuthToken,
            #"consumer_key" = consumer_key,
            #"consumer_secret" = consumer_secret
        ]
    ),
    // best when used in conjunction with the GetFriends or GetFollowers functions
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
    ),
    // Get the first 150 user ids from those that the MSPowerBI Twitter account follows
    friendIds = GetFriends("MSPowerBI", 150),
    // Pass these user ids to the LookupUsers function to return a full user object complement
    friendObjects = LookupUsers(Text.Combine(friendIds[id],","))
in
    friendObjects
```

||id|id_str|name|screen_name|location|description
|---:|---:|:---|:---|:---|:---|:---
|1|264501255|264501255|Eric Horvitz|erichorvitz|Seattle WA  USA|Director, Microsoft Research Labs
|2|268575250|268575250|Dustin Ryan|SQLDusty|Macclenny, FL|TSP @Microsoft, author, blogger & speaker, interested in #Azure #SSAS #PowerBI #SQLServer #edtech
|3|794575879|794575879|Justyna Lucznik|JustynaLucznik|Redmond, WA|Program Manager on the Microsoft Power BI team. Passionate about statistics, ML and data visualization.
|4|61598916|61598916|Seth Bauer|Eno1978||I was: An Artist, Sommelier, Climber. I am: A Microsoft MVP, Technical BI Architect, Power BI Enthusiast, Husband, Father. I will be: Always striving to be more
|...|...|...|...|...|...|...
|150|87746197|87746197|Christian Wade|_christianWade||"Program manager for Power BI at Microsoft. Creator of the BISM Normalizer. Coined the term ""clicky clicky draggy droppy data analysis"". Tweets are my own"

## References
1. [Get Friends Ids](https://developer.twitter.com/en/docs/accounts-and-users/follow-search-get-users/api-reference/get-friends-ids), Twtter API Reference
2. [Twitter.LookupUsers](https://github.com/tonmcg/powertweet/blob/master/M/Twitter.LookupUsers.pq) by Tony McGovern