+++
title = "Select Random Element From A List"
date = "2018-04-01"
+++

## Create List of Character Elements
```javascript
let
    Source = Text.FromBinary(File.Contents("https://goo.gl/hqUUPR")),
    list = {"Portugal", "United Kingdom", "Germany", "New Zealand", "Australia", "Belgium", "France"}
in
    list
```
|    |List 	         
|:---|:--------------
|1	 |Portugal	     
|2	 |United Kingdom
|3	 |Germany
|4   |New Zealand
|5	 |Australia
|6	 |France
|7	 |Belgium

## Select Random Element from the List
```javascript
let
    list = {"Portugal", "United Kingdom", "Germany", "New Zealand", "Australia", "Belgium", "France"},
    selection = list{Number.RoundDown(Number.RandomBetween(0, List.Count(list)))}
in
    selection
```
Australia