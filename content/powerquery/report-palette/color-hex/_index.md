+++
title = "Get Popular Color Palettes"
date = "2018-05-28"
+++

## Prerequisities
1. Navigate to http://www.color-hex.com/color-palettes/popular.php/
2. Choose a color palette; get the name of the palette.
3. Use that name as an input for the _PopularPalettes_ function below.

## Preliminaries
```javascript
let
    // load custom PopularPalettes function from GitHubGist
    PopularPalettes = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/PopularPalettes")),
        [
            #"ExtraValues.Error" = ExtraValues.Error,
            #"Json.FromValue" = Json.FromValue,
            #"List.Generate" = List.Generate,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"Table.ExpandListColumn" = Table.ExpandListColumn,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Table.FromList" = Table.FromList,
            #"Table.SelectRows" = Table.SelectRows,
            #"Text.BetweenDelimiters" = Text.BetweenDelimiters,
            #"Text.Clean" = Text.Clean,
            #"Text.From" = Text.From,
            #"Text.FromBinary" = Text.FromBinary,
            #"Text.Trim" = Text.Trim,
            #"Text.TrimEnd" = Text.TrimEnd,
            #"Text.TrimStart" = Text.TrimStart,
            #"Web.BrowserContents" = Web.BrowserContents,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type
        ]
    )
in
     PopularPalettes
```

## Get the Twitter Color Palette from color-hex.com
```javascript
let
    PopularPalettes = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/PopularPalettes")),
        [
            #"ExtraValues.Error" = ExtraValues.Error,
            #"Json.FromValue" = Json.FromValue,
            #"List.Generate" = List.Generate,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"Table.ExpandListColumn" = Table.ExpandListColumn,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Table.FromList" = Table.FromList,
            #"Table.SelectRows" = Table.SelectRows,
            #"Text.BetweenDelimiters" = Text.BetweenDelimiters,
            #"Text.Clean" = Text.Clean,
            #"Text.From" = Text.From,
            #"Text.FromBinary" = Text.FromBinary,
            #"Text.Trim" = Text.Trim,
            #"Text.TrimEnd" = Text.TrimEnd,
            #"Text.TrimStart" = Text.TrimStart,
            #"Web.BrowserContents" = Web.BrowserContents,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type
        ]
    ),
    // Get Twitter color palette as a JSON-formatted string to use as a Report Theme in Power BI
    results = PopularPalettes("Twitter")
in
    results
```
{&#34;name&#34;:&#34;Twitter&#34;,&#34;dataColors&#34;:
  [&#34;#326ada&#34;,&#34;#d4d8d4&#34;,&#34;#433e90&#34;,&#34;#a19c9c&#34;,&#34;#d2d2d2&#34;],
  &#34;background&#34;:&#34;#ffffff&#34;,
  &#34;foreground&#34;:&#34;#d4d8d4&#34;,
  &#34;tableAccent&#34;:&#34;#326ada&#34;
}

## Notes
Works in Power BI only.

## References
### Custom Function Reference
+ [PopularPalettes](https://gist.github.com/tonmcg/93d56132f8a25b51f83eac751610d8e8) by Tony McGovern
+ [Popular Color Palettes on _color-hex.com_](http://www.color-hex.com/color-palettes/popular.php/)

### Power Query M Reference
+ {{<urls function="extravalues-error">}}ExtraValues.Error{{</urls>}}
+ {{<urls function="json-fromvalue">}}Json.FromValue{{</urls>}}
+ {{<urls function="list-generate">}}List.Generate{{</urls>}}
+ {{<urls function="splitter-splitbynothing">}}Splitter.SplitByNothing{{</urls>}}
+ {{<urls function="table-expandlistcolumn">}}Table.ExpandListColumn{{</urls>}}
+ {{<urls function="table-expandrecordcolumn">}}Table.ExpandRecordColumn{{</urls>}}
+ {{<urls function="table-fromlist">}}Table.FromList{{</urls>}}
+ {{<urls function="table-selectrows">}}Table.SelectRows{{</urls>}}
+ {{<urls function="text-betweendelimiters">}}Text.BetweenDelimiters{{</urls>}}
+ {{<urls function="text-clean">}}Text.Clean{{</urls>}}
+ {{<urls function="text-from">}}Text.From{{</urls>}}
+ {{<urls function="text-frombinary">}}Text.FromBinary{{</urls>}}
+ {{<urls function="text-trim">}}Text.Trim{{</urls>}}
+ {{<urls function="text-trimend">}}Text.TrimEnd{{</urls>}}
+ {{<urls function="text-trimstart">}}Text.TrimStart{{</urls>}}
+ {{<urls function="value-replacemetadata">}}Value.ReplaceMetadata{{</urls>}}
+ {{<urls function="value-replacetype">}}Value.ReplaceType{{</urls>}}
+ {{<urls function="value-type">}}Value.Type{{</urls>}}