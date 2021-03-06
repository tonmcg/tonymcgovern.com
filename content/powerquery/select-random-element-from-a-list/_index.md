+++
title = "Select Random Element From A List"
date = "2018-05-16"
+++

## Create List of Character Elements
```javascript
let
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
<pre>Australia</pre>

Results will vary because of the random position of the selection

## References
### Power Query M Reference

+ [Number.RandomBetween](https://msdn.microsoft.com/en-us/library/mt253327.aspx)
+ [Number.RoundDown](https://msdn.microsoft.com/en-us/library/mt253362.aspx)
3. [List.Count](https://msdn.microsoft.com/en-us/library/mt253591.aspx)