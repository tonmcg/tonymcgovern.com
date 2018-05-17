+++
title = "Get All Tables From A Website"
date = "2018-05-16"
+++

## Preliminaries
```javascript
let
    GetTables = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/WkPWo9")), 
        [
            #"Text.FromBinary" = Text.FromBinary,
            #"Text.BetweenDelimiters" = Text.BetweenDelimiters,
            #"Text.Combine" = Text.Combine,
            #"Table.SelectRows" = Table.SelectRows,
            #"Web.Contents" = Web.Contents,
            #"Web.Page" = Web.Page,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
        ]
    )
in
    GetTables
```

## Get All Tables from a Website
```javascript
let
    GetTables = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/WkPWo9")), 
        [
            #"Text.FromBinary" = Text.FromBinary,
            #"Text.BetweenDelimiters" = Text.BetweenDelimiters,
            #"Text.Combine" = Text.Combine,
            #"Table.SelectRows" = Table.SelectRows,
            #"Web.Contents" = Web.Contents,
            #"Web.Page" = Web.Page,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
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
+ [Text.FromBinary](https://msdn.microsoft.com/en-us/query-bi/m/text-frombinary)
+ [Text.BetweenDelimiters](https://msdn.microsoft.com/en-us/query-bi/m/text-betweendelimiters)
+ [Text.Combine](https://msdn.microsoft.com/en-us/query-bi/m/text-combine)
+ [Table.SelectRows](https://msdn.microsoft.com/en-us/query-bi/m/table-selectrows)
+ [Web.Contents](https://msdn.microsoft.com/en-us/query-bi/m/web-contents)
+ [Web.Page](https://msdn.microsoft.com/en-us/query-bi/m/web-page)