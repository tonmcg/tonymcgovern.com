+++
title = "Get U.S. States"
date = "2018-05-21"
+++

## Preliminaries
```javascript
let
    // load custom GetStates function from GitHubGist
    GetStates = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/USCensusGetStates")),
        [
            #"Expression.Evaluate" = Expression.Evaluate,
            #"Table.Combine" = Table.Combine,
            #"Table.FromRecords" = Table.FromRecords,
            #"Table.RenameColumns" = Table.RenameColumns,
            #"Table.SelectColumns" = Table.SelectColumns,
            #"Table.SelectRows" = Table.SelectRows,
            #"Text.BetweenDelimiters" = Text.BetweenDelimiters,
            #"Text.Combine" = Text.Combine,
            #"Text.From" = Text.From,
            #"Text.FromBinary" = Text.FromBinary,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type,
            #"Web.Contents" = Web.Contents,
            #"Web.Page" = Web.Page
        ]
    )
in
     GetStates
```

## Get U.S. States Including Territories
```javascript
let 
    GetStates = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/USCensusGetStates")),
        [
            #"Expression.Evaluate" = Expression.Evaluate,
            #"Table.Combine" = Table.Combine,
            #"Table.FromRecords" = Table.FromRecords,
            #"Table.RenameColumns" = Table.RenameColumns,
            #"Table.SelectColumns" = Table.SelectColumns,
            #"Table.SelectRows" = Table.SelectRows,
            #"Text.BetweenDelimiters" = Text.BetweenDelimiters,
            #"Text.Combine" = Text.Combine,
            #"Text.From" = Text.From,
            #"Text.FromBinary" = Text.FromBinary,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type,
            #"Web.Contents" = Web.Contents,
            #"Web.Page" = Web.Page
        ]
    ),
    results = GetStates(true)
in
    results
```
|     |state_name          |state_fips |state_abbr
|:---:|:-------------------|:----------|:----
|1	  |Alabama             |01         |AL
|2	  |Alaska              |02         |AK
|3	  |Arizona             |04         |AZ
|...  |...                 |...        |...
|60	  |U.S. Virgin Islands |78         |VI

## Get U.S. States Not Including Territories
```javascript
let 
    GetStates = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/USCensusGetStates")),
        [
            #"Expression.Evaluate" = Expression.Evaluate,
            #"Table.Combine" = Table.Combine,
            #"Table.FromRecords" = Table.FromRecords,
            #"Table.RenameColumns" = Table.RenameColumns,
            #"Table.SelectColumns" = Table.SelectColumns,
            #"Table.SelectRows" = Table.SelectRows,
            #"Text.BetweenDelimiters" = Text.BetweenDelimiters,
            #"Text.Combine" = Text.Combine,
            #"Text.From" = Text.From,
            #"Text.FromBinary" = Text.FromBinary,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type,
            #"Web.Contents" = Web.Contents,
            #"Web.Page" = Web.Page
        ]
    ),
    results = GetStates(false)
in
    results
```
|     |state_name          |state_fips |state_abbr
|:---:|:-------------------|:----------|:----
|1	  |Alabama             |01         |AL
|2	  |Alaska              |02         |AK
|3	  |Arizona             |04         |AZ
|...  |...                 |...        |...
|51   |Wyoming             |56         |WY

## References
### Custom Function Reference
+ [GetStates](https://gist.github.com/tonmcg/a545cd94d798c687fc3ced139093e23e) by Tony McGovern

### Power Query M Reference
+ {{<urls function="expression-evaluate">}}Expression.Evaluate{{</urls>}}
+ {{<urls function="table-combine">}}Table.Combine{{</urls>}}
+ {{<urls function="table-fromrecords">}}Table.FromRecords{{</urls>}}
+ {{<urls function="table-renamecolumns">}}Table.RenameColumns{{</urls>}}
+ {{<urls function="table-selectcolumns">}}Table.SelectColumns{{</urls>}}
+ {{<urls function="table-selectrows">}}Table.SelectRows{{</urls>}}
+ {{<urls function="text-betweendelimiters">}}Text.BetweenDelimiters{{</urls>}}
+ {{<urls function="text-combine">}}Text.Combine{{</urls>}}
+ {{<urls function="text-from">}}Text.From{{</urls>}}
+ {{<urls function="text-frombinary">}}Text.FromBinary{{</urls>}}
+ {{<urls function="value-replacemetadata">}}Value.ReplaceMetadata{{</urls>}}
+ {{<urls function="value-replacetype">}}Value.ReplaceType{{</urls>}}
+ {{<urls function="value-type">}}Value.Type{{</urls>}}
+ {{<urls function="web-contents">}}Web.Contents{{</urls>}}
+ {{<urls function="web-page">}}Web.Page{{</urls>}}