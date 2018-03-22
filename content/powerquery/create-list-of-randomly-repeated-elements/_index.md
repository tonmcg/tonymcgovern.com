+++
title = "Create List of Randomly Repeated Elements"
date = "2018-04-01"
+++

## Preliminaries
```javascript
let
    // Load custom RandomSelections function from Github Gist
    Source = Text.FromBinary(Web.Contents("https://goo.gl/6kyLBe")),
    RandomSelections = Expression.Evaluate(Source, #shared)
in
    RandomSelections
```

## Create List of Character Elements
```javascript
let
    Source = Text.FromBinary(Web.Contents("https://goo.gl/6kyLBe")),
    RandomSelections = Expression.Evaluate(Source, #shared),
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

## Create List of Randomly Repeated Elements from Initial List
```javascript
let
    // Randomly select values from a list of countries that results in an output list of 1,000,000 items
    Source = Text.FromBinary(Web.Contents("https://goo.gl/6kyLBe")),
    RandomSelections = Expression.Evaluate(Source, #shared),
    list = {"Portugal", "United Kingdom", "Germany", "New Zealand", "Australia", "Belgium", "France"},
    results = RandomSelections(list, 1000000)		
in
    results
```
|        |List 	         
|:------:|:--------------
|1	     |United Kingdom 
|2	     |New Zealan
|3	     |Australia
|4       |New Zealand
|...     |...
|1000000 |France

## References
1. [RandomSelections](https://gist.github.com/tonmcg/e85642d99f2f7d365382a2d06006f618) by Tony McGovern
2. [Text.FromBinary](https://msdn.microsoft.com/en-us/library/mt253365.aspx), Power Query M function reference
3. [Web.Contents](https://msdn.microsoft.com/en-us/library/mt260892.aspx), Power Query M function reference