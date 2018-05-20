+++
title = "Create List of Randomly Repeating Elements"
date = "2018-05-19"
+++

## Preliminaries
```javascript
let
     // Load custom GenerateRandom function from Github Gist
     GenerateRandom = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/ListGenerateRandom")),
        [
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"Number.Random" = Number.Random,
            #"Number.RandomBetween" = Number.RandomBetween,
            #"Number.Round" = Number.Round,
            #"Number.RoundDown" = Number.RoundDown,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type
        ]
    )
in
     GenerateRandom
```

## Create List of Character Elements
```javascript
let
     GenerateRandom = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/ListGenerateRandom")),
        [
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"Number.Random" = Number.Random,
            #"Number.RandomBetween" = Number.RandomBetween,
            #"Number.Round" = Number.Round,
            #"Number.RoundDown" = Number.RoundDown,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type
        ]
    ),
    // create list of character or numerical elements
    list = {"Portugal", "United Kingdom", "Germany", "New Zealand", "Australia", "Belgium", "France"}
in
    list
```
|     |List 	         
|:---:|:--------------
|1	  |Portugal	     
|2	  |United Kingdom
|3	  |Germany
|4    |New Zealand
|5	  |Australia
|6	  |Belgium
|7	  |France

## Create List of Randomly Repeated Elements from Initial List
```javascript
let
     GenerateRandom = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/ListGenerateRandom")),
        [
            #"List.Buffer" = List.Buffer,
            #"List.Count" = List.Count,
            #"List.Generate" = List.Generate,
            #"Number.Random" = Number.Random,
            #"Number.RandomBetween" = Number.RandomBetween,
            #"Number.Round" = Number.Round,
            #"Number.RoundDown" = Number.RoundDown,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type
        ]
    ),
    list = {"Portugal", "United Kingdom", "Germany", "New Zealand", "Australia", "Belgium", "France"},
    // Randomly select values from a list of countries that results in an output list of 1,000,000 items
    results = GenerateRandom(list, 1000000)		
in
    results
```
|        |List 	         
|:------:|:--------------
|1	     |United Kingdom 
|2	     |New Zealand
|3	     |Australia
|4       |New Zealand
|...     |...
|1000000 |Germany

## References
### Custom Function Reference
+ [GenerateRandom](https://gist.github.com/tonmcg/e85642d99f2f7d365382a2d06006f618) by Tony McGovern

### Power Query M Reference
+ {{<urls function="list-buffer">}}List.Buffer{{</urls>}}
+ {{<urls function="list-count">}}List.Count{{</urls>}}
+ {{<urls function="list-generate">}}List.Generate{{</urls>}}
+ {{<urls function="number-random">}}Number.Random{{</urls>}}
+ {{<urls function="number-randombetween">}}Number.RandomBetween{{</urls>}}
+ {{<urls function="number-round">}}Number.Round{{</urls>}}
+ {{<urls function="number-rounddown">}}Number.RoundDown{{</urls>}}