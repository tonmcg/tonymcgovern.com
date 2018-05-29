+++
title = "Get Top Colors From A Website"
date = "2018-05-28"
+++

## Preliminaries
```javascript
let
     // Load custom SwatchDiscoveryAPI function from Github Gist
     SwatchDiscoveryAPI = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/SwatchDiscoveryAPI")),
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
     SwatchDiscoveryAPI
```

## Get Popular Colors from BBC.com
```javascript
let
     SwatchDiscoveryAPI = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/SwatchDiscoveryAPI")),
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
    // get top colors used on bbc.com
    results = SwatchDiscoveryAPI("www.bbc.com")
in
    results
```
{&#39;name&#39;:&#39;Facebook&#39;,&#39;dataColors&#39;:[&#39;&#35;3b5998&#39;,&#39;&#35;8b9dc3&#39;,&#39;&#35;dfe3ee&#39;,&#39;&#35;f7f7f7&#39;,&#39;&#35;fff&#39;],&#39;background&#39;:&#39;&#35;ffffff&#39;,&#39;foreground&#39;:&#39;&#35;8b9dc3&#39;,&#39;tableAccent&#39;:&#39;&#35;3b5998&#39;}


## References
### Custom Function Reference
+ [SwatchDiscoveryAPI](https://gist.github.com/tonmcg/8d63177f6b23947ed21d5941464fb7d1) by Tony McGovern
+ [Swatch Discovery API]()

### Power Query M Reference
+ {{<urls function="list-buffer">}}List.Buffer{{</urls>}}
+ {{<urls function="list-count">}}List.Count{{</urls>}}
+ {{<urls function="list-generate">}}List.Generate{{</urls>}}
+ {{<urls function="number-random">}}Number.Random{{</urls>}}
+ {{<urls function="number-randombetween">}}Number.RandomBetween{{</urls>}}
+ {{<urls function="number-round">}}Number.Round{{</urls>}}
+ {{<urls function="number-rounddown">}}Number.RoundDown{{</urls>}}