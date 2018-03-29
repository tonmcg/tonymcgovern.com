+++
title = "Standardize A List"
date = "2018-04-01"
+++

## Preliminaries
```javascript
let
    // load custom StandardScaler function from Github Gist
    Source = Text.FromBinary(Web.Contents("https://goo.gl/obJuVM")),
    StandardScaler = Expression.Evaluate(Source, #shared)
in
    StandardScaler
```

## Create a List
```javascript
let
    Source = Text.FromBinary(Web.Contents("https://goo.gl/obJuVM")),
    StandardScaler = Expression.Evaluate(Source, #shared),
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
    Source = Text.FromBinary(Web.Contents("https://goo.gl/obJuVM")),
    StandardScaler = Expression.Evaluate(Source, #shared),
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
1. [StandardScaler](https://gist.github.com/tonmcg/1630f9f4faa17a6d6a7eed5d10eb310f) by Tony McGovern
2. [Text.FromBinary](https://msdn.microsoft.com/en-us/library/mt253365.aspx), Power Query M function reference
3. [Web.Contents](https://msdn.microsoft.com/en-us/library/mt260892.aspx), Power Query M function reference