+++
title = "Rescale A List"
date = "2018-04-01"
+++

## Preliminaries
```javascript
let
    // load custom MinMaxScaler function from Github Gist
    Source = Text.FromBinary(Web.Contents("https://goo.gl/MHMTMX")),
    MinMaxScaler = Expression.Evaluate(Source, #shared)
in
    MinMaxScaler
```

## Create a List
```javascript
let
    Source = Text.FromBinary(Web.Contents("https://goo.gl/MHMTMX")),
    MinMaxScaler = Expression.Evaluate(Source, #shared),
    list = {1,2,3,4,5,6,7,8,9}
in
    list
```
|    |List 	         
|---:|----
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
    // Rescale list values between 0 and 1 (default)
    Source = Text.FromBinary(Web.Contents("https://goo.gl/MHMTMX")),
    MinMaxScaler = Expression.Evaluate(Source, #shared),
    list = {1,2,3,4,5,6,7,8,9},
    results = MinMaxScaler(list)
in
    results
```
|     |List
|----:|---
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
    // Rescale list values between -1 and 1
    Source = Text.FromBinary(Web.Contents("https://goo.gl/MHMTMX")),
    MinMaxScaler = Expression.Evaluate(Source, #shared),
    list = {1,2,3,4,5,6,7,8,9},
    results = MinMaxScaler(list, {-1,1})
in
    results
```
|     |List
|----:|---
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
1. [MinMaxScaler](https://gist.github.com/tonmcg/36f23a0e3d3cec71577cc59ba6b9298c) by Tony McGovern