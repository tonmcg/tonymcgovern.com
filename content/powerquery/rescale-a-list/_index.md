+++
title = "Rescale A List"
date = "2018-05-19"
+++

## Preliminaries
```javascript
let
    // load custom MinMaxScaler function from Github Gist
     MinMaxScaler = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/ListMinMaxScaler")),
        [
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"List.Max" = List.Max,
            #"List.Min" = List.Min,
            #"List.Transform" = List.Transform,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type
        ]
    )
in
     MinMaxScaler
```

## Create a List
```javascript
let
     MinMaxScaler = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/ListMinMaxScaler")),
        [
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"List.Max" = List.Max,
            #"List.Min" = List.Min,
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

## Rescale a List Between 0 and 1
```javascript
let
     MinMaxScaler = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/ListMinMaxScaler")),
        [
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"List.Max" = List.Max,
            #"List.Min" = List.Min,
            #"List.Transform" = List.Transform,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type
        ]
    ),
    list = {1,2,3,4,5,6,7,8,9},
    results = MinMaxScaler(list)
in
    results
```
|     |List
|:---:|---:
|1	  |0
|2	  |0.125
|3	  |0.25
|4	  |0.375
|5	  |0.5
|6	  |0.625
|7	  |0.75
|8	  |0.875
|9	  |1

## Rescale a List Between -1 and 1
```javascript
let
     MinMaxScaler = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/ListMinMaxScaler")),
        [
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"List.Max" = List.Max,
            #"List.Min" = List.Min,
            #"List.Transform" = List.Transform,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type
        ]
    ),
    list = {1,2,3,4,5,6,7,8,9},
    results = MinMaxScaler(list, {-1,1})
in
    results
```
|     |List
|:---:|---:
|1	  |-1
|2	  |-0.75
|3	  |-0.5
|4	  |-0.25
|5	  |0
|6	  |0.25
|7	  |0.5
|8	  |0.75
|9	  |1

## References
### Custom Function Reference
+ [MinMaxScaler](https://gist.github.com/tonmcg/36f23a0e3d3cec71577cc59ba6b9298c) by Tony McGovern

### Power Query M Reference
+ {{<urls function="list-buffer">}}List.Buffer{{</urls>}}
+ {{<urls function="list-count">}}List.Count{{</urls>}}
+ {{<urls function="list-generate">}}List.Generate{{</urls>}}
+ {{<urls function="list-max">}}List.Max{{</urls>}}
+ {{<urls function="list-min">}}List.Min{{</urls>}}
+ {{<urls function="list-transform">}}List.Transform{{</urls>}}
+ {{<urls function="value-replacemetadata">}}Value.ReplaceMetadata{{</urls>}}
+ {{<urls function="value-replacetype">}}Value.ReplaceType{{</urls>}}
+ {{<urls function="value-type">}}Value.Type{{</urls>}}