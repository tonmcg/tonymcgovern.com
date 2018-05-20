+++
title = "Unzip File"
date = "2018-05-19"
+++

## Preliminaries
```javascript
let
     // load custom UnzipContents function from Github Gist
     UnzipContents = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/UnzipContents")),
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
            #"List.RemoveLastN" = List.RemoveLastN,
            #"List.Transform" = List.Transform,
            #"Table.FromRecords" = Table.FromRecords,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type
        ]
    )
in
     UnzipContents
```

## Unzip Web File
```javascript
let
    // unzip and parse a tsv file from the United States Census Bureau 
    // that provides a list of all the U.S. counties as of 2017
     UnzipContents = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/UnzipContents")),
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
            #"List.RemoveLastN" = List.RemoveLastN,
            #"List.Transform" = List.Transform,
            #"Table.FromRecords" = Table.FromRecords,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.Type" = Value.Type
        ]
    ),
     // For more on this function, see the link to Mark White's blog in the References section below
    file = UnzipContents(Web.Contents("https://goo.gl/e4UY3P")), // for local files, use File.Contents instead
    results = Csv.Document(
        file{0}[Content],
        [Delimiter="#(tab)",
        Columns=4,
        Encoding=1252,
        QuoteStyle=QuoteStyle.None]
    )
in
    results
```
|     |Column1 	|Column2 |Column3   |Column4
|:---:|:--------|:-------|:---------|:---
|1	  |USPS	    |GEOID   |ANSICODE  |NAME
|2	  |AL       |01001	 |00161526	|Autauga County
|3	  |AL       |01003	 |00161527	|Baldwin County
|...  |... 	    |...     |...       |...
|3221	|PR       |72153 	 |01804557	|Yauco Municipio


## References
### Custom Function Reference
+ [Reading Zip files in PowerQuery / M](http://sql10.blogspot.sg/2016/06/reading-zip-files-in-powerquery-m.html) by Mark White

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
+ {{<urls function="list-removelastn">}}List.RemoveLastN{{</urls>}}
+ {{<urls function="list-transform">}}List.Transform{{</urls>}}
+ {{<urls function="table-fromrecords">}}Table.FromRecords{{</urls>}}
+ {{<urls function="value-replacemetadata">}}Value.ReplaceMetadata{{</urls>}}
+ {{<urls function="value-replacetype">}}Value.ReplaceType{{</urls>}}
+ {{<urls function="value-type">}}Value.Type{{</urls>}}