+++
title = "Get All Tables From A Website"
date = "2018-04-01"
+++

## Preliminaries
```javascript
let
    // load custom GetTables function from Github Gist
    Source = Text.FromBinary(Web.Contents("https://goo.gl/WkPWo9")),
    GetTables = Expression.Evaluate(Source, #shared)
in
    GetTables
```

## Get All Tables from a Website
```javascript
let
    Source = Text.FromBinary(Web.Contents("https://goo.gl/WkPWo9")),
    GetTables = Expression.Evaluate(Source, #shared),
    // return all tables on the United States Census Bureau 
    // website showing U.S. states and territories
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
1. [GetTables](https://gist.github.com/tonmcg/1173759b95943b2b9ed290b9edbe74d3) by Tony McGovern