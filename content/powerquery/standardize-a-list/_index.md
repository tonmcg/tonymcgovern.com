+++
title = "Standardize A List"
date = "2018-05-19"
+++

## Preliminaries
```javascript
let
    // load custom StandardScaler function from Github Gist
     StandardScaler = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/ListStandardScaler")),
        [
            #"List.Average" = List.Average,
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"List.StandardDeviation" = List.StandardDeviation,
            #"List.Transform" = List.Transform,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
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
        Text.FromBinary(Web.Contents("http://bit.ly/ListStandardScaler")),
        [
            #"List.Average" = List.Average,
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"List.StandardDeviation" = List.StandardDeviation,
            #"List.Transform" = List.Transform,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
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
        Text.FromBinary(Web.Contents("http://bit.ly/ListStandardScaler")),
        [
            #"List.Average" = List.Average,
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"List.StandardDeviation" = List.StandardDeviation,
            #"List.Transform" = List.Transform,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
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
+ {{<urls function="list-average">}}List.Average{{</urls>}}
+ {{<urls function="list-buffer">}}List.Buffer{{</urls>}}
+ {{<urls function="list-count">}}List.Count{{</urls>}}
+ {{<urls function="list-generate">}}List.Generate{{</urls>}}
+ {{<urls function="list-standarddeviation">}}List.StandardDeviation{{</urls>}}
+ {{<urls function="list-transform">}}List.Transform{{</urls>}}
+ {{<urls function="value-replacemetadata">}}Value.ReplaceMetadata{{</urls>}}
+ {{<urls function="value-replacetype">}}Value.ReplaceType{{</urls>}}
+ {{<urls function="value-type">}}Value.Type{{</urls>}}