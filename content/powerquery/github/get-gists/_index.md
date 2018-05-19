+++
title = "Get All Gists"
date = "2018-05-19"
+++

## Preliminaries
```javascript
let
  GetGists = Expression.Evaluate(
    Text.FromBinary(Web.Contents("http://bit.ly/GithubGetGists")),
    [
      #"ExtraValues.Error" = ExtraValues.Error,
      #"JoinKind.FullOuter" = JoinKind.FullOuter,
      #"Json.Document" = Json.Document,
      #"List.Count" = List.Count,
      #"List.Generate" = List.Generate,
      #"Record.AddField" = Record.AddField,
      #"Record.Field" = Record.Field,
      #"Record.FieldValues" = Record.FieldValues,
      #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
      #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
      #"Table.ExpandTableColumn" = Table.ExpandTableColumn,
      #"Table.FromList" = Table.FromList,
      #"Table.NestedJoin" = Table.NestedJoin,
      #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
      #"Value.ReplaceType" = Value.ReplaceType,
      #"Value.Type" = Value.Type,
      #"Web.Contents" = Web.Contents
    ]
  )
in
  GetGists
```

## Get All Gists for a Specified User
```javascript
let
  GetGists = Expression.Evaluate(
    Text.FromBinary(Web.Contents("http://bit.ly/GithubGetGists")),
    [
      #"ExtraValues.Error" = ExtraValues.Error,
      #"JoinKind.FullOuter" = JoinKind.FullOuter,
      #"Json.Document" = Json.Document,
      #"List.Count" = List.Count,
      #"List.Generate" = List.Generate,
      #"Record.AddField" = Record.AddField,
      #"Record.Field" = Record.Field,
      #"Record.FieldValues" = Record.FieldValues,
      #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
      #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
      #"Table.ExpandTableColumn" = Table.ExpandTableColumn,
      #"Table.FromList" = Table.FromList,
      #"Table.NestedJoin" = Table.NestedJoin,
      #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
      #"Value.ReplaceType" = Value.ReplaceType,
      #"Value.Type" = Value.Type,
      #"Web.Contents" = Web.Contents
    ]
  ),
  // return meta data and specific data on all Gists by Imke Feldmann 
  results = GetGists("ImkeF")
in
  results
```
|    |id                               |filename                |type       |... |truncated
|:--:|:--------------------------------|:-----------------------|:----------|:---|--------:
|1    |6adb85af867e89da77d0db4e98b83ee8 |Record.AllQueries.pq    |text/plain |... |FALSE
|2    |0b26e771dbcfba362a97d7bf09d4cb33 |Text.QueryNames.pq      |text/plain |... |FALSE
|...  |...                              |...                     |...        |... |...
|23   |f62623fbef37b08c55b37cc07e6f4b07 |R Trend.pq              |text/plain |... |FALSE
|24   |ea234e7a77f2ccae9d798f2a9ad76701 |ExportCsv.pq            |text/plain |... |FALSE

## References
### Custom Function Reference
+ [GetGists](https://gist.github.com/tonmcg/f3b59569787f8bd397c8d3a192616b0e) by Tony McGovern

### Power Query M Reference
+ {{<urls function="extravalues-error">}}ExtraValues.Error{{</urls>}}
+ {{<urls function="joinkind-fullouter">}}JoinKind.FullOuter{{</urls>}}
+ {{<urls function="json-document">}}Json.Document{{</urls>}}
+ {{<urls function="list-count">}}List.Count{{</urls>}}
+ {{<urls function="list-generate">}}List.Generate{{</urls>}}
+ {{<urls function="record-addfield">}}Record.AddField{{</urls>}}
+ {{<urls function="record-field">}}Record.Field{{</urls>}}
+ {{<urls function="record-fieldvalues">}}Record.FieldValues{{</urls>}}
+ {{<urls function="splitter-splitbynothing">}}Splitter.SplitByNothing{{</urls>}}
+ {{<urls function="table-expandrecordcolumn">}}Table.ExpandRecordColumn{{</urls>}}
+ {{<urls function="table-expandtablecolumn">}}Table.ExpandTableColumn{{</urls>}}
+ {{<urls function="table-fromlist">}}Table.FromList{{</urls>}}
+ {{<urls function="table-nestedjoin">}}Table.NestedJoin{{</urls>}}
+ {{<urls function="value-replacemetadata">}}Value.ReplaceMetadata{{</urls>}}
+ {{<urls function="value-replacetype">}}Value.ReplaceType{{</urls>}}
+ {{<urls function="value-type">}}Value.Type{{</urls>}}
+ {{<urls function="web-contents">}}Web.Contents{{</urls>}}