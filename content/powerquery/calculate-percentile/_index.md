+++
title = "Calculate Percentile"
date = "2018-05-16"
+++

## Preliminaries
```javascript
let
    // load custom Percentile function from Github Gist
    Percentile = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/THkj1A")),
        [
            #"List.Count" = List.Count,
            #"List.Sort" = List.Sort,
            #"Order.Ascending" = Order.Ascending,
            #"Number.IntegerDivide" = Number.IntegerDivide,
            #"Number.Mod" = Number.Mod,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
        ]
    )
in
    Percentile
```

## Create list of numerical elements
```javascript
let
    Percentile = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/THkj1A")),
        [
            #"List.Count" = List.Count,
            #"List.Sort" = List.Sort,
            #"Order.Ascending" = Order.Ascending,
            #"Number.IntegerDivide" = Number.IntegerDivide,
            #"Number.Mod" = Number.Mod,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
        ]
    ),
    // enter list of numerical elements
    // a reference to a table column can also be used here
    // e.g., list = Table1[Column1]
    list = {95.1772,95.1567,95.1937,95.1959,95.1442,95.061,95.1591,95.1195,95.1065,95.0925,95.199,95.1682}
in
    list
```
|    |List 
|:---|-------:|
|1	 |95.1772
|2	 |95.1567
|3	 |95.1937
|4	 |95.1959
|5	 |95.1442
|6	 |95.061
|7	 |95.1591
|8	 |95.1195
|9	 |95.1065
|10	 |95.0925
|11	 |95.199
|12	 |95.1682

## Calculate 90th percentile value from the list
```javascript
let
    Percentile = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/THkj1A")),
        [
            #"List.Count" = List.Count,
            #"List.Sort" = List.Sort,
            #"Order.Ascending" = Order.Ascending,
            #"Number.IntegerDivide" = Number.IntegerDivide,
            #"Number.Mod" = Number.Mod,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
        ]
    ),
    list = {95.1772,95.1567,95.1937,95.1959,95.1442,95.061,95.1591,95.1195,95.1065,95.0925,95.199,95.1682},
    // calculate the 90th percentile (90%)
    CalculatePercentile = Percentile(list, 0.9)
in
    CalculatePercentile
```
95.19568

## Alternative 1 - Calculate 90th percentile value from the list (R6 Method)
```javascript
let
    Percentile = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/THkj1A")),
        [
            #"List.Count" = List.Count,
            #"List.Sort" = List.Sort,
            #"Order.Ascending" = Order.Ascending,
            #"Number.IntegerDivide" = Number.IntegerDivide,
            #"Number.Mod" = Number.Mod,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
        ]
    ),
    list = {95.1772,95.1567,95.1937,95.1959,95.1442,95.061,95.1591,95.1195,95.1065,95.0925,95.199,95.1682},
    // calculate the 90th percentile (90%)
    // use the R6 Method after Hyndman and Fan (1996)
    // https://www.itl.nist.gov/div898/handbook/prc/section2/prc262.htm
    CalculatePercentile = Percentile(list, 0.9, 6)
in
    CalculatePercentile
```
95.19807

## Alternative 2 - Calculate 90th percentile value from the list (R8 Method)
```javascript
let
    Percentile = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/THkj1A")),
        [
            #"List.Count" = List.Count,
            #"List.Sort" = List.Sort,
            #"Order.Ascending" = Order.Ascending,
            #"Number.IntegerDivide" = Number.IntegerDivide,
            #"Number.Mod" = Number.Mod,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
        ]
    ),
    list = {95.1772,95.1567,95.1937,95.1959,95.1442,95.061,95.1591,95.1195,95.1065,95.0925,95.199,95.1682},
    // calculate the 90th percentile (90%)
    // use the R8 Method after Hyndman and Fan (1996)
    // https://www.itl.nist.gov/div898/handbook/prc/section2/prc262.htm
    CalculatePercentile = Percentile(list, 0.9, 8)
in
    CalculatePercentile
```
95.19713999999999

## References
### Custom Function Reference
+ [Percentile](https://gist.github.com/tonmcg/c5889375a84482f2d2862d620b6f191d) by Tony McGovern
+ [7.2.6.2.Percentiles](https://www.itl.nist.gov/div898/handbook/prc/section2/prc262.htm) NIST/SEMATECH e-Handbook of Statistical Methods

### Power Query M Reference
+ [Number.IntegerDivide](https://msdn.microsoft.com/en-us/query-bi/m/number-integerdivide)
+ [Number.Mod](https://msdn.microsoft.com/en-us/query-bi/m/number-mod)
+ [List.Sort](https://msdn.microsoft.com/en-us/query-bi/m/list-sort)
+ [List.Count](https://msdn.microsoft.com/en-us/query-bi/m/list-count)