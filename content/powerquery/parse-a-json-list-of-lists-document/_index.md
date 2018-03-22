+++
title = "Parse a JSON List of Lists Document"
date = "2018-04-01"
+++

## Create a JSON List of Lists Text String
```javascript
let
    Source = "[[""CustomerID"",""Name"",""Phone""],[""1"",""Julia"",""987-6543""],[""2"",""Riley"",""123-4567""],[""3"",""Jim"",""543-7890""]]"
in
    Source
```
`[["CustomerID","Name","Phone"],["1","Julia","987-6543"],["2","Riley","123-4567"],["3","Jim","543-7890"]]`

## Parse JSON Document
```javascript
let
    Source = "[[""CustomerID"",""Name"",""Phone""],[""1"",""Julia"",""987-6543""],[""2"",""Riley"",""123-4567""],[""3"",""Jim"",""543-7890""]]",
    ListOfLists = Json.Document(Source)
in
    ListOfLists
```
|     |List
|----:|:---
|1	  |**List**
|2	  |**List**
|3	  |**List**
|4	  |**List**

## Parse JSON List of Lists Document
```javascript
let
    Source = "[[""CustomerID"",""Name"",""Phone""],[""1"",""Julia"",""987-6543""],[""2"",""Riley"",""123-4567""],[""3"",""Jim"",""543-7890""]]",
    ListOfLists = Json.Document(Source),
    results = Table.FromRows(ListOfLists)
in
    results
```
|     |Column1   |Column2 |Column3 |
|----:|:---------|:-------|:-------|
|1	  |CustomerID|Name    |Phone   |
|2	  |1         |Julia   |987-6543|
|3	  |2         |Riley   |123-4567|
|4	  |3         |Jim     |543-7890|

## References
1. [Json.Document](https://msdn.microsoft.com/en-us/library/mt260861.aspx), Power Query M function reference
1. [Table.FromRows](https://msdn.microsoft.com/en-us/library/mt260791.aspx), Power Query M function reference