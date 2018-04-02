+++
title = "Get U.S. Geographies"
date = "2018-04-02"
+++

## Preliminaries
```javascript
let
    // load custom GetGeographies function from Github Gist
    Source = Text.FromBinary(Web.Contents("https://goo.gl/va8bYn")),
    GetGeographies = Expression.Evaluate(Source, #shared)
in
    GetGeographies
```

## Get U.S. Counties as of 2017
```javascript
let 
    Source = Text.FromBinary(Web.Contents("https://goo.gl/va8bYn")),
    GetGeographies = Expression.Evaluate(Source, #shared),
    results = GetGeographies("2017","County")
in
    results
```
|     |geoid |name            |year |geography_type |index
|:---:|:-----|:---------------|----:|:--------------|---:
|1	  |01001 |Autauga County  |2017 |County         |1
|2	  |01003 |Baldwin County  |2017 |County         |2
|3	  |01005 |Barbour County  |2017 |County         |3
|...  |...   |...             |...  |...            |...
|3220 |72153 |Yauco Municipio |2017 |County         |3220

## Get U.S. Census Tracts as of 2017
```javascript
let 
    Source = Text.FromBinary(Web.Contents("https://goo.gl/va8bYn")),
    GetGeographies = Expression.Evaluate(Source, #shared),
    results = GetGeographies("2017","Census Tract")
in
    results
```
|      |geoid       |name        |year |geography_type |index
|:----:|:-----------|:-----------|----:|:--------------|---:
|1     |01001020100 |01001020100 |2017 |Census Tract   |1
|2     |01001020200 |01001020200 |2017 |Census Tract   |2
|3     |01001020300 |01001020300 |2017 |Census Tract   |3
|...   |...         |...         |...  |...            |...
|74001 |72153750602 |72153750602 |2017 |Census Tract   |74001

## References
1. [GetGeographies](https://gist.github.com/tonmcg/a22faeafbe20a7becb57b8ba36098b29) by Tony McGovern
2. [Text.FromBinary](https://msdn.microsoft.com/en-us/library/mt253365.aspx), Power Query M function reference
3. [Web.Contents](https://msdn.microsoft.com/en-us/library/mt260892.aspx), Power Query M function reference

## Appendix: For Use on Power BI Service
<div style="table-layout:fixed;display:table;width:100%;">
{{< gist tonmcg a22faeafbe20a7becb57b8ba36098b29 >}}
</div>