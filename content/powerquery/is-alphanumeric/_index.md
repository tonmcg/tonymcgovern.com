+++
title = "Test Text for AlphaNumeric Characters"
date = "2018-05-28"
+++

## Preliminaries
```javascript
let
     // Load custom IsAlphaNumeric function from Github Gist
     IsAlphaNumeric = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TextIsAlphaNumeric")),
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
     IsAlphaNumeric
```

## Create Text
```javascript
let
     IsAlphaNumeric = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TextIsAlphaNumeric")),
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
    // create text of alphanumeric characters
    text = "AlbrechtMayer1stOboistBerlinerPhilharmoniker"
in
    text
```
AlbrechtMayer1stOboistBerlinerPhilharmoniker

## Test Text If All Characters are AlphaNumeric
```javascript
let
     IsAlphaNumeric = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TextIsAlphaNumeric")),
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
    text = "AlbrechtMayer1stOboistBerlinerPhilharmoniker",
    results = IsAlphaNumeric(text)		
in
    results
```
TRUE

## References
### Custom Function Reference
+ [IsAlphaNumeric](https://gist.github.com/tonmcg/8d63177f6b23947ed21d5941464fb7d1) by Tony McGovern

### Power Query M Reference
+ {{<urls function="list-buffer">}}List.Buffer{{</urls>}}
+ {{<urls function="list-count">}}List.Count{{</urls>}}
+ {{<urls function="list-generate">}}List.Generate{{</urls>}}
+ {{<urls function="number-random">}}Number.Random{{</urls>}}
+ {{<urls function="number-randombetween">}}Number.RandomBetween{{</urls>}}
+ {{<urls function="number-round">}}Number.Round{{</urls>}}
+ {{<urls function="number-rounddown">}}Number.RoundDown{{</urls>}}