+++
title = "Unzip File"
date = "2018-04-01"
+++

## Preliminaries
```javascript
let
    // load custom UnZIP function from Github Gist
    Source = Text.FromBinary(Web.Contents("https://goo.gl/LovP6p")),
    UnZIP = Expression.Evaluate(Source, #shared)
in
    UnZIP
```

## Unzip Web File
```javascript
let
    // unzip and parse a tsv file from the United States Census Bureau 
    // that provides a list of all the U.S. counties as of 2017
    Source = Text.FromBinary(Web.Contents("https://goo.gl/LovP6p")),
    UnZIP = Expression.Evaluate(Source, #shared),
    // For more on this function, see the link to Mark White's blog in the References section below
    file = UnZIP(Web.Contents("https://goo.gl/e4UY3P")), // for local files, use File.Contents instead
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
|     |Column1 	|Column2 |Column3   |Column4        |
|:----|:--------|:-------|:---------|:--------------|
|1	  |USPS	    |GEOID   |ANSICODE  |NAME           |
|2	  |AL       |01001	 |00161526	|Autauga County |
|3	  |AL       |01003	 |00161527	|Baldwin County |
|...  |... 	    |...     |...       |...            |
|3221	|PR       |72153 	 |01804557	|Yauco Municipio|


## References
1. [Reading Zip files in PowerQuery / M](http://sql10.blogspot.sg/2016/06/reading-zip-files-in-powerquery-m.html) by Mark White
2. [Web.Contents](https://msdn.microsoft.com/en-us/library/mt260892.aspx), Power Query M function reference
3. [Text.FromBinary](https://msdn.microsoft.com/en-us/library/mt253365.aspx), Power Query M function reference
4. [Csv.Document](https://msdn.microsoft.com/en-us/library/mt260840.aspx), Power Query M function reference