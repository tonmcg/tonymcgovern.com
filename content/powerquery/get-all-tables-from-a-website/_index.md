+++
title = "Get All Tables From A Website"
date = "2018-05-19"
+++

## Preliminaries
```javascript
let
     GetTables = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/HtmlGetTables")),
        [
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
     GetTables
```

## Get All Tables from a Website
```javascript
let
     GetTables = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/HtmlGetTables")),
        [
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
    // return all tables on the United States Census Bureau 
    // website showing U.S. states, areas, and territories
    results = GetTables("https://www.census.gov/geo/reference/ansi_statetables.html")
    // The 'Data' column contains the tables on the website
in
    results
```
|     |Caption |Source |ClassName |Id      |Data     |
|:---:|-------:|:------|---------:|-------:|:--------|
|1	  |*null*  |Table  |*null*    |*null*  |**Table**|
|2	  |*null*  |Table  |*null*    |*null*  |**Table**|
|3	  |*null*  |Table  |*null*    |*null*  |**Table**|

## References
### Custom Function Reference
+ [GetTables](https://gist.github.com/tonmcg/1173759b95943b2b9ed290b9edbe74d3) by Tony McGovern

### Power Query M Reference
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