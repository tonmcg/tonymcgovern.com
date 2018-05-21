+++
title = "Get U.S. Geographies"
date = "2018-05-21"
+++

## Preliminaries
```javascript
let
    // load custom GetGeographies function from GitHubGist
    GetGeographies = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/USCensusGetGeographies")),
        [
            #"Binary.Buffer" = Binary.Buffer,
            #"Binary.Decompress" = Binary.Decompress,
            #"BinaryFormat.Binary" = BinaryFormat.Binary,
            #"BinaryFormat.Byte" = BinaryFormat.Byte,
            #"BinaryFormat.ByteOrder" = BinaryFormat.ByteOrder,
            #"BinaryFormat.Choice" = BinaryFormat.Choice,
            #"BinaryFormat.List" = BinaryFormat.List,
            #"BinaryFormat.Record" = BinaryFormat.Record,
            #"BinaryFormat.Text" = BinaryFormat.Text,
            #"BinaryFormat.Transform" = BinaryFormat.Transform,
            #"BinaryFormat.UnsignedInteger16" = BinaryFormat.UnsignedInteger16,
            #"BinaryFormat.UnsignedInteger32" = BinaryFormat.UnsignedInteger32,
            #"ByteOrder.LittleEndian" = ByteOrder.LittleEndian,
            #"Compression.Deflate" = Compression.Deflate,
            #"Csv.Document" = Csv.Document,
            #"Expression.Evaluate" = Expression.Evaluate,
            #"Int64.Type" = Int64.Type,
            #"List.Contains" = List.Contains,
            #"List.First" = List.First,
            #"List.RemoveLastN" = List.RemoveLastN,
            #"List.Select" = List.Select,
            #"List.Transform" = List.Transform,
            #"Number.From" = Number.From,
            #"Number.FromText" = Number.FromText,
            #"QuoteStyle.None" = QuoteStyle.None,
            #"Table.AddColumn" = Table.AddColumn,
            #"Table.AddIndexColumn" = Table.AddIndexColumn,
            #"Table.Column" = Table.Column,
            #"Table.ColumnNames" = Table.ColumnNames,
            #"Table.FromRecords" = Table.FromRecords,
            #"Table.PromoteHeaders" = Table.PromoteHeaders,
            #"Table.RenameColumns" = Table.RenameColumns,
            #"Table.SelectColumns" = Table.SelectColumns,
            #"Table.TransformColumnTypes" = Table.TransformColumnTypes,
            #"Text.From" = Text.From,
            #"Text.FromBinary" = Text.FromBinary,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type,
            #"Web.Contents" = Web.Contents
        ]
    )
in
     GetGeographies
```

## Get U.S. Counties as of 2017
```javascript
let 
    GetGeographies = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/USCensusGetGeographies")),
        [
            #"Binary.Buffer" = Binary.Buffer,
            #"Binary.Decompress" = Binary.Decompress,
            #"BinaryFormat.Binary" = BinaryFormat.Binary,
            #"BinaryFormat.Byte" = BinaryFormat.Byte,
            #"BinaryFormat.ByteOrder" = BinaryFormat.ByteOrder,
            #"BinaryFormat.Choice" = BinaryFormat.Choice,
            #"BinaryFormat.List" = BinaryFormat.List,
            #"BinaryFormat.Record" = BinaryFormat.Record,
            #"BinaryFormat.Text" = BinaryFormat.Text,
            #"BinaryFormat.Transform" = BinaryFormat.Transform,
            #"BinaryFormat.UnsignedInteger16" = BinaryFormat.UnsignedInteger16,
            #"BinaryFormat.UnsignedInteger32" = BinaryFormat.UnsignedInteger32,
            #"ByteOrder.LittleEndian" = ByteOrder.LittleEndian,
            #"Compression.Deflate" = Compression.Deflate,
            #"Csv.Document" = Csv.Document,
            #"Expression.Evaluate" = Expression.Evaluate,
            #"Int64.Type" = Int64.Type,
            #"List.Contains" = List.Contains,
            #"List.First" = List.First,
            #"List.RemoveLastN" = List.RemoveLastN,
            #"List.Select" = List.Select,
            #"List.Transform" = List.Transform,
            #"Number.From" = Number.From,
            #"Number.FromText" = Number.FromText,
            #"QuoteStyle.None" = QuoteStyle.None,
            #"Table.AddColumn" = Table.AddColumn,
            #"Table.AddIndexColumn" = Table.AddIndexColumn,
            #"Table.Column" = Table.Column,
            #"Table.ColumnNames" = Table.ColumnNames,
            #"Table.FromRecords" = Table.FromRecords,
            #"Table.PromoteHeaders" = Table.PromoteHeaders,
            #"Table.RenameColumns" = Table.RenameColumns,
            #"Table.SelectColumns" = Table.SelectColumns,
            #"Table.TransformColumnTypes" = Table.TransformColumnTypes,
            #"Text.From" = Text.From,
            #"Text.FromBinary" = Text.FromBinary,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type,
            #"Web.Contents" = Web.Contents
        ]
    ),
    results = GetGeographies("2017","County")
in
    results
```
|     |geoid |name            |year |geography_type |index
|:---:|:-----|:---------------|----:|:--------------|---:
|1	  |01001 |Autauga County  |2017 |County         |1
|2	  |01003 |Baldwin County  |2017 |County         |2
|3	  |01005 |Barbour County  |2017 |County         |3
|...  |...   |...             |...  |...            |...
|3220 |72153 |Yauco Municipio |2017 |County         |3220

## Get U.S. Census Tracts as of 2017
```javascript
let 
    GetGeographies = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/USCensusGetGeographies")),
        [
            #"Binary.Buffer" = Binary.Buffer,
            #"Binary.Decompress" = Binary.Decompress,
            #"BinaryFormat.Binary" = BinaryFormat.Binary,
            #"BinaryFormat.Byte" = BinaryFormat.Byte,
            #"BinaryFormat.ByteOrder" = BinaryFormat.ByteOrder,
            #"BinaryFormat.Choice" = BinaryFormat.Choice,
            #"BinaryFormat.List" = BinaryFormat.List,
            #"BinaryFormat.Record" = BinaryFormat.Record,
            #"BinaryFormat.Text" = BinaryFormat.Text,
            #"BinaryFormat.Transform" = BinaryFormat.Transform,
            #"BinaryFormat.UnsignedInteger16" = BinaryFormat.UnsignedInteger16,
            #"BinaryFormat.UnsignedInteger32" = BinaryFormat.UnsignedInteger32,
            #"ByteOrder.LittleEndian" = ByteOrder.LittleEndian,
            #"Compression.Deflate" = Compression.Deflate,
            #"Csv.Document" = Csv.Document,
            #"Expression.Evaluate" = Expression.Evaluate,
            #"Int64.Type" = Int64.Type,
            #"List.Contains" = List.Contains,
            #"List.First" = List.First,
            #"List.RemoveLastN" = List.RemoveLastN,
            #"List.Select" = List.Select,
            #"List.Transform" = List.Transform,
            #"Number.From" = Number.From,
            #"Number.FromText" = Number.FromText,
            #"QuoteStyle.None" = QuoteStyle.None,
            #"Table.AddColumn" = Table.AddColumn,
            #"Table.AddIndexColumn" = Table.AddIndexColumn,
            #"Table.Column" = Table.Column,
            #"Table.ColumnNames" = Table.ColumnNames,
            #"Table.FromRecords" = Table.FromRecords,
            #"Table.PromoteHeaders" = Table.PromoteHeaders,
            #"Table.RenameColumns" = Table.RenameColumns,
            #"Table.SelectColumns" = Table.SelectColumns,
            #"Table.TransformColumnTypes" = Table.TransformColumnTypes,
            #"Text.From" = Text.From,
            #"Text.FromBinary" = Text.FromBinary,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type,
            #"Web.Contents" = Web.Contents
        ]
    ),
    results = GetGeographies("2017","Census Tract")
in
    results
```
|      |geoid       |name        |year |geography_type |index
|:----:|:-----------|:-----------|----:|:--------------|---:
|1     |01001020100 |01001020100 |2017 |Census Tract   |1
|2     |01001020200 |01001020200 |2017 |Census Tract   |2
|3     |01001020300 |01001020300 |2017 |Census Tract   |3
|...   |...         |...         |...  |...            |...
|74001 |72153750602 |72153750602 |2017 |Census Tract   |74001

## References
### Custom Function Reference
+ [GetGeographies](https://gist.github.com/tonmcg/a22faeafbe20a7becb57b8ba36098b29) by Tony McGovern

### Power Query M Reference
+ {{<urls function="binary-buffer">}}Binary.Buffer{{</urls>}}
+ {{<urls function="binary-decompress">}}Binary.Decompress{{</urls>}}
+ {{<urls function="binaryformat-binary">}}BinaryFormat.Binary{{</urls>}}
+ {{<urls function="binaryformat-byte">}}BinaryFormat.Byte{{</urls>}}
+ {{<urls function="binaryformat-byteorder">}}BinaryFormat.ByteOrder{{</urls>}}
+ {{<urls function="binaryformat-choice">}}BinaryFormat.Choice{{</urls>}}
+ {{<urls function="binaryformat-list">}}BinaryFormat.List{{</urls>}}
+ {{<urls function="binaryformat-record">}}BinaryFormat.Record{{</urls>}}
+ {{<urls function="binaryformat-text">}}BinaryFormat.Text{{</urls>}}
+ {{<urls function="binaryformat-transform">}}BinaryFormat.Transform{{</urls>}}
+ {{<urls function="binaryformat-unsignedinteger16">}}BinaryFormat.UnsignedInteger16{{</urls>}}
+ {{<urls function="binaryformat-unsignedinteger32">}}BinaryFormat.UnsignedInteger32{{</urls>}}
+ {{<urls function="byteorder-littleendian">}}ByteOrder.LittleEndian{{</urls>}}
+ {{<urls function="compression-deflate">}}Compression.Deflate{{</urls>}}
+ {{<urls function="csv-document">}}Csv.Document{{</urls>}}
+ {{<urls function="expression-evaluate">}}Expression.Evaluate{{</urls>}}
+ {{<urls function="list-contains">}}List.Contains{{</urls>}}
+ {{<urls function="list-first">}}List.First{{</urls>}}
+ {{<urls function="list-removelastn">}}List.RemoveLastN{{</urls>}}
+ {{<urls function="list-select">}}List.Select{{</urls>}}
+ {{<urls function="list-transform">}}List.Transform{{</urls>}}
+ {{<urls function="number-from">}}Number.From{{</urls>}}
+ {{<urls function="number-fromtext">}}Number.FromText{{</urls>}}
+ {{<urls function="quotestyle-none">}}QuoteStyle.None{{</urls>}}
+ {{<urls function="table-addcolumn">}}Table.AddColumn{{</urls>}}
+ {{<urls function="table-addindexcolumn">}}Table.AddIndexColumn{{</urls>}}
+ {{<urls function="table-column">}}Table.Column{{</urls>}}
+ {{<urls function="table-columnnames">}}Table.ColumnNames{{</urls>}}
+ {{<urls function="table-fromrecords">}}Table.FromRecords{{</urls>}}
+ {{<urls function="table-promoteheaders">}}Table.PromoteHeaders{{</urls>}}
+ {{<urls function="table-renamecolumns">}}Table.RenameColumns{{</urls>}}
+ {{<urls function="table-selectcolumns">}}Table.SelectColumns{{</urls>}}
+ {{<urls function="table-transformcolumntypes">}}Table.TransformColumnTypes{{</urls>}}
+ {{<urls function="text-from">}}Text.From{{</urls>}}
+ {{<urls function="text-frombinary">}}Text.FromBinary{{</urls>}}
+ {{<urls function="value-replacemetadata">}}Value.ReplaceMetadata{{</urls>}}
+ {{<urls function="value-replacetype">}}Value.ReplaceType{{</urls>}}
+ {{<urls function="value-type">}}Value.Type{{</urls>}}
+ {{<urls function="web-contents">}}Web.Contents{{</urls>}}