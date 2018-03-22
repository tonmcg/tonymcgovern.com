+++
title = "Impute Missing Value with Means"
date = "2018-04-01"
+++

## Preliminaries
```javascript
let
    // load custom ImputerColumn function from Github Gist
    source = Text.FromBinary(Web.Contents("https://goo.gl/AZXSfv")),
    ImputerColumn = Expression.Evaluate(source, #shared)
in
    ImputerColumn
```

## Create Column with Numerical Series Including a Randomly Positioned Null Value
```javascript
let
    source = Text.FromBinary(Web.Contents("https://goo.gl/AZXSfv")),
    ImputerColumn = Expression.Evaluate(source, #shared),
    // create a one-column table that's a numerical series
    // from 0 to 10 with a null value
    // randomly placed within
    numItems = 10,
    values = 
        Table.FromList(
            List.ReplaceRange(
                List.Generate(
                    ()=> 0, 
                    each _ < numItems, 
                    each _ + 1
                ), 
                Number.RoundDown(Number.RandomBetween(0,numItems)), // randomly chosen postiion
                1,
                {null} // value to replace with
            ), 
        Splitter.SplitByNothing(), {"value"}, null, ExtraValues.Error)
in
    values
```
|    |value 
|:---|-----:|
|1	 |0 	  |
|2	 |1     |
|3	 |2	    |
|4	 |3 	  |
|5	 |*null*|
|6	 |5     |
|7	 |6 	  |
|8	 |7     |
|9	 |8 	  |
|10	 |9 	  |

## Impute Missing Value with Means
```javascript
let
    source = Text.FromBinary(Web.Contents("https://goo.gl/AZXSfv")),
    ImputerColumn = Expression.Evaluate(source, #shared),
    numItems = 10,
    values = 
        Table.FromList(
            List.ReplaceRange(
                List.Generate(
                    ()=> 0, 
                    each _ < numItems, 
                    each _ + 1
                ), 
                Number.RoundDown(Number.RandomBetween(0,numItems)),
                1,
                {null}
            ), 
        Splitter.SplitByNothing(), {"value"}, null, ExtraValues.Error),
    results = ImputerColumn(values, "Average", "value", null, null)
in
    results
```
|    |value 
|:---|-----:|
|1	 |0 	  |
|2	 |1     |
|3	 |2	    |
|4	 |3 	  |
|5	 |4.5555555555555554|
|6	 |5     |
|7	 |6 	  |
|8	 |7     |
|9	 |8 	  |
|10	 |9 	  |

Results will vary because of the random position of the null value being imputed

## References
1. [ImputerColumn](https://gist.github.com/ImkeF/ebb803f10ba17af6bb6a5d11d9a22c44) by Imke Feldmann
2. [Text.FromBinary](https://msdn.microsoft.com/en-us/library/mt253365.aspx), Power Query M function reference
3. [Web.Contents](https://msdn.microsoft.com/en-us/library/mt260892.aspx), Power Query M function reference
4. [Table.FromList](https://msdn.microsoft.com/en-us/library/mt260762.aspx), Power Query M function reference
5. [List.Generate](https://msdn.microsoft.com/en-us/library/mt260751.aspx), Power Query M function reference
6. [List.ReplaceRange](https://msdn.microsoft.com/en-us/library/mt260738.aspx), Power Query M function reference
7. [Number.RandomBetween](https://msdn.microsoft.com/en-us/library/mt253327.aspx), Power Query M function reference
6. [Number.RoundDown](https://msdn.microsoft.com/en-us/library/mt253362.aspx), Power Query M function reference
