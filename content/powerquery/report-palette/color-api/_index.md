+++
title = "Create Color Scheme From Base Color"
date = "2018-05-28"
+++

## Prerequisities
1. Choose a base color.
2. Navigate to https://www.colorhexa.com/ and get the hexadecimal representation of that color.
3. Use that representation as an input for the _ColorAPI_ function below.

## Preliminaries
```javascript
let
    // load custom ColorAPI function from GitHubGist
    ColorAPI = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/ThemeColorAPI")),
        [
            #"ExtraValues.Error" = ExtraValues.Error,
            #"Json.Document" = Json.Document,
            #"Json.FromValue" = Json.FromValue,
            #"Number.ToText" = Number.ToText,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Table.FromList" = Table.FromList,
            #"Text.From" = Text.From,
            #"Text.FromBinary" = Text.FromBinary,
            #"Text.Trim" = Text.Trim,
            #"Text.TrimEnd" = Text.TrimEnd,
            #"Text.TrimStart" = Text.TrimStart,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type,
            #"Web.Contents" = Web.Contents
        ]
    )
in
     ColorAPI
```

## Create Color Scheme From Base Color
```javascript
let
    ColorAPI = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/ThemeColorAPI")),
        [
            #"ExtraValues.Error" = ExtraValues.Error,
            #"Json.Document" = Json.Document,
            #"Json.FromValue" = Json.FromValue,
            #"Number.ToText" = Number.ToText,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Table.FromList" = Table.FromList,
            #"Text.From" = Text.From,
            #"Text.FromBinary" = Text.FromBinary,
            #"Text.Trim" = Text.Trim,
            #"Text.TrimEnd" = Text.TrimEnd,
            #"Text.TrimStart" = Text.TrimStart,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type,
            #"Web.Contents" = Web.Contents
        ]
    ),
    // create a color scheme based off of one color
    results = ColorAPI("3b5998")
in
    results
```

## Create an _n_-Number Color Scheme From Base Color
```javascript
let
    ColorAPI = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/ThemeColorAPI")),
        [
            #"ExtraValues.Error" = ExtraValues.Error,
            #"Json.Document" = Json.Document,
            #"Json.FromValue" = Json.FromValue,
            #"Number.ToText" = Number.ToText,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Table.FromList" = Table.FromList,
            #"Text.From" = Text.From,
            #"Text.FromBinary" = Text.FromBinary,
            #"Text.Trim" = Text.Trim,
            #"Text.TrimEnd" = Text.TrimEnd,
            #"Text.TrimStart" = Text.TrimStart,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type,
            #"Web.Contents" = Web.Contents
        ]
    ),
    // create a 6-color scheme based off of one color
    results = ColorAPI("3b5998", 6)
in
    results
```


## References
### Custom Function Reference
+ [ColorAPI](https://gist.github.com/tonmcg/544eaace9b615b73478f02da03b73e20) by Tony McGovern
+ [The Color API](http://www.thecolorapi.com/)

### Power Query M Reference
+ {{<urls function="extravalues-error">}}ExtraValues.Error{{</urls>}}
+ {{<urls function="json-document">}}Json.Document{{</urls>}}
+ {{<urls function="json-fromvalue">}}Json.FromValue{{</urls>}}
+ {{<urls function="number-totext">}}Number.ToText{{</urls>}}
+ {{<urls function="splitter-splitbynothing">}}Splitter.SplitByNothing{{</urls>}}
+ {{<urls function="table-expandrecordcolumn">}}Table.ExpandRecordColumn{{</urls>}}
+ {{<urls function="table-fromlist">}}Table.FromList{{</urls>}}
+ {{<urls function="text-from">}}Text.From{{</urls>}}
+ {{<urls function="text-frombinary">}}Text.FromBinary{{</urls>}}
+ {{<urls function="text-trim">}}Text.Trim{{</urls>}}
+ {{<urls function="text-trimend">}}Text.TrimEnd{{</urls>}}
+ {{<urls function="text-trimstart">}}Text.TrimStart{{</urls>}}
+ {{<urls function="value-replacemetadata">}}Value.ReplaceMetadata{{</urls>}}
+ {{<urls function="value-replacetype">}}Value.ReplaceType{{</urls>}}
+ {{<urls function="value-type">}}Value.Type{{</urls>}}
+ {{<urls function="web-contents">}}Web.Contents{{</urls>}}