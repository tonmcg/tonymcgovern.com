+++
title = "Get Tweet Timelines"
date = "2018-04-02"
+++

## Prerequisites
1. Follow the [Twitter authorization steps shown here](../get-token)

## Preliminaries
```javascript
let
    // load custom GetOAuthToken and GetTimeline functions from Github
    GetOAuthToken = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/GetOAuth")),
        [
            #"Binary.ToText" = Binary.ToText,
            #"Json.Document" = Json.Document,
            #"Text.ToBinary" = Text.ToBinary,
            #"Web.Contents" = Web.Contents
        ]
    ),
    GetTimeline = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/GetTweetTimeline")),
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
    GetTimeline
```

## Get Tweet Timelines
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
    GetTimeline = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/GetTweetTimeline")),
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
    // Get the last 5,000 user statuses from the MSPowerBI Twitter account timeline
    statuses = GetTimeline("MSPowerBI", 5000),
    RemovedDuplicates = Table.Distinct(statuses, {"id_str"})
in
    RemovedDuplicates
```

|   |created_at|id|id_str|text
|---:|:---|---:|:---|:---|
|1|Sat Mar 31 02:31:25 +0000 2018|9.79909E+17|979908996307005440|@JWood We’d love to see these future projects so keep us posted! If you ever need ideas: https://t.co/ThTiyZGP3h
|2|Fri Mar 30 23:15:00 +0000 2018|9.7986E+17|979859569634693120|Are you curious about the new spring release of #PowerBI Insights? CRN took a look at a couple of big highlights.… https://t.co/8S4iH9FoNU
|3|Fri Mar 30 19:17:00 +0000 2018|9.798E+17|979799672993800198|Learn how @discoverRB is empowering employees with end-to-end analytics on @Azure. #SQLDW https://t.co/a6NMWLFpH1
|4|Fri Mar 30 18:00:00 +0000 2018|9.7978E+17|979780295762247680|Watch recorded sessions anytime or register for upcoming live webinars to hear from our #PowerBI experts. Learn mor… https://t.co/p7tI4AkUSB
|5|Fri Mar 30 16:40:02 +0000 2018|9.7976E+17|979760170245992448|Dive into the details of how to successfully implement a #data governance strategy w/ #PowerBI. Watch on-demand:… https://t.co/Af9dpzhsZZ
|...|...|...|...|
|5000|Wed Mar 28 18:00:00 +0000 2018|9.79056E+17|979055521486508032|Find out how @MSExcel users can save time analyzing #data in #PowerBI. Here are six ways: https://t.co/VYogVJQEbG

## References
1. [Get Tweet Timelines](https://developer.twitter.com/en/docs/tweets/timelines/api-reference/get-statuses-user_timeline), Twtter API Reference
2. [Twitter.GetTimeline](https://github.com/tonmcg/powertweet/blob/master/M/Twitter.GetTimeline.pq) by Tony McGovern