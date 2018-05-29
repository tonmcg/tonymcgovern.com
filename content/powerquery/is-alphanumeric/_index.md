+++
title = "Test Text for AlphaNumeric Characters"
date = "2018-05-28"
+++

## Preliminaries
```javascript
let
    // load custom IsAlphaNumeric function from GitHubGist
    IsAlphaNumeric = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TextIsAlphaNumeric")),
        [
            #"Character.FromNumber" = Character.FromNumber,
            #"List.Contains" = List.Contains,
            #"List.Generate" = List.Generate,
            #"List.Transform" = List.Transform,
            #"Number.ToText" = Number.ToText,
            #"Text.At" = Text.At,
            #"Text.Length" = Text.Length,
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
            #"Character.FromNumber" = Character.FromNumber,
            #"List.Contains" = List.Contains,
            #"List.Generate" = List.Generate,
            #"List.Transform" = List.Transform,
            #"Number.ToText" = Number.ToText,
            #"Text.At" = Text.At,
            #"Text.Length" = Text.Length,
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
            #"Character.FromNumber" = Character.FromNumber,
            #"List.Contains" = List.Contains,
            #"List.Generate" = List.Generate,
            #"List.Transform" = List.Transform,
            #"Number.ToText" = Number.ToText,
            #"Text.At" = Text.At,
            #"Text.Length" = Text.Length,
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
+ {{<urls function="character-fromnumber">}}Character.FromNumber{{</urls>}}
+ {{<urls function="list-contains">}}List.Contains{{</urls>}}
+ {{<urls function="list-generate">}}List.Generate{{</urls>}}
+ {{<urls function="list-transform">}}List.Transform{{</urls>}}
+ {{<urls function="number-totext">}}Number.ToText{{</urls>}}
+ {{<urls function="text-at">}}Text.At{{</urls>}}
+ {{<urls function="text-length">}}Text.Length{{</urls>}}
+ {{<urls function="value-replacemetadata">}}Value.ReplaceMetadata{{</urls>}}
+ {{<urls function="value-replacetype">}}Value.ReplaceType{{</urls>}}
+ {{<urls function="value-type">}}Value.Type{{</urls>}}