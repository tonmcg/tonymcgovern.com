+++
title = "Standardize A List"
date = "2018-05-16"
+++

## Preliminaries
```javascript
let
    // load custom StandardScaler function from Github Gist
    StandardScaler = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/obJuVM")), 
        [
            #"List.Average" = List.Average,
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"List.StandardDeviation" = List.StandardDeviation,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
        ]
    )
in
    StandardScaler
```

## Create a List
```javascript
let
    StandardScaler = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/obJuVM")), 
        [
            #"List.Average" = List.Average,
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"List.StandardDeviation" = List.StandardDeviation,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
        ]
    ),
    list = {1,2,3,4,5,6,7,8,9}
in
    list
```
|    |List
|:---:|---:
|1	 |1
|2	 |2
|3	 |3
|4   |4
|5	 |5
|6	 |6
|7	 |7
|8	 |8
|9	 |9

## Standardize a List
```javascript
let
    // Rescale list values to have a mean of 0 and a standard deviation of 1
    StandardScaler = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/obJuVM")), 
        [
            #"List.Average" = List.Average,
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"List.StandardDeviation" = List.StandardDeviation,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
        ]
    ),
    list = {1,2,3,4,5,6,7,8,9},
    results = StandardScaler(list)
in
    results
```
|     |List
|:---:|---:
|1    |-1.460593487
|2    |-1.095445115
|3    |-0.730296743
|4    |-0.365148372
|5    |0
|6    |0.365148372
|7    |0.730296743
|8    |1.095445115
|9    |1.460593487

## References
### Custom Function Reference
+ [StandardScaler](https://gist.github.com/tonmcg/1630f9f4faa17a6d6a7eed5d10eb310f) by Tony McGovern

### Power Query M Reference
+ [List.Average](https://msdn.microsoft.com/en-us/query-bi/m/list-average)
+ [List.Buffer](https://msdn.microsoft.com/en-us/query-bi/m/list-buffer)
+ [List.Count](https://msdn.microsoft.com/en-us/query-bi/m/list-count)
+ [List.Generate](https://msdn.microsoft.com/en-us/query-bi/m/list-generate)
+ [List.StandardDeviation](https://msdn.microsoft.com/en-us/query-bi/m/list-standarddeviation)