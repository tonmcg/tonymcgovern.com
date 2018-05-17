+++
title = "Impute Missing Value with Means"
date = "2018-05-16"
+++

## Preliminaries
```javascript
let
    // load custom ImputerColumn function from Github Gist
    ImputerColumn = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/AZXSfv")), 
        [
            #"Expression.Evaluate" = Expression.Evaluate,
            #"List.Accumulate" = List.Accumulate,
            #"List.Average" = List.Average,
            #"List.Buffer" = List.Buffer,
            #"List.Median" = List.Median,
            #"List.Mode" = List.Mode,
            #"List.First" = List.First,
            #"List.Last" = List.Last,
            #"List.Min" = List.Min,
            #"List.Max" = List.Max,
            #"List.Select" = List.Select,
            #"List.Sum" = List.Sum,
            #"Record.Field" = Record.Field,
            #"Record.FieldValues" = Record.FieldValues,
            #"Replacer.ReplaceValue" = Replacer.ReplaceValue,
            #"Table.AddColumn" =Table.AddColumn,
            #"Table.Column" = Table.Column,
            #"Table.ColumnNames" = Table.ColumnNames,
            #"Table.RemoveColumns" = Table.RemoveColumns,
            #"Table.ReplaceValue" = Table.ReplaceValue,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
        ]
    )
in
    ImputerColumn
```

## Create Column with Numerical Series Including a Randomly Positioned Null Value
```javascript
let
    ImputerColumn = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/AZXSfv")), 
        [
            #"Expression.Evaluate" = Expression.Evaluate,
            #"List.Accumulate" = List.Accumulate,
            #"List.Average" = List.Average,
            #"List.Buffer" = List.Buffer,
            #"List.Median" = List.Median,
            #"List.Mode" = List.Mode,
            #"List.First" = List.First,
            #"List.Last" = List.Last,
            #"List.Min" = List.Min,
            #"List.Max" = List.Max,
            #"List.Select" = List.Select,
            #"List.Sum" = List.Sum,
            #"Record.Field" = Record.Field,
            #"Record.FieldValues" = Record.FieldValues,
            #"Replacer.ReplaceValue" = Replacer.ReplaceValue,
            #"Table.AddColumn" =Table.AddColumn,
            #"Table.Column" = Table.Column,
            #"Table.ColumnNames" = Table.ColumnNames,
            #"Table.RemoveColumns" = Table.RemoveColumns,
            #"Table.ReplaceValue" = Table.ReplaceValue,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
        ]
    ),
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
    ImputerColumn = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/AZXSfv")), 
        [
            #"Expression.Evaluate" = Expression.Evaluate,
            #"List.Accumulate" = List.Accumulate,
            #"List.Average" = List.Average,
            #"List.Buffer" = List.Buffer,
            #"List.Median" = List.Median,
            #"List.Mode" = List.Mode,
            #"List.First" = List.First,
            #"List.Last" = List.Last,
            #"List.Min" = List.Min,
            #"List.Max" = List.Max,
            #"List.Select" = List.Select,
            #"List.Sum" = List.Sum,
            #"Record.Field" = Record.Field,
            #"Record.FieldValues" = Record.FieldValues,
            #"Replacer.ReplaceValue" = Replacer.ReplaceValue,
            #"Table.AddColumn" =Table.AddColumn,
            #"Table.Column" = Table.Column,
            #"Table.ColumnNames" = Table.ColumnNames,
            #"Table.RemoveColumns" = Table.RemoveColumns,
            #"Table.ReplaceValue" = Table.ReplaceValue,
            #"Value.ReplaceType" = Value.ReplaceType,
            #"Value.ReplaceMetadata" = Value.ReplaceMetadata,
            #"Value.Type" = Value.Type
        ]
    ),
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
### Custom Function Reference
+ [ImputerColumn](https://gist.github.com/ImkeF/ebb803f10ba17af6bb6a5d11d9a22c44) by Imke Feldmann

### Power Query M Reference
+ [Expression.Evaluate](https://msdn.microsoft.com/en-us/query-bi/m/expression-evaluate)
+ [List.Accumulate](https://msdn.microsoft.com/en-us/query-bi/m/list-accumulate)
+ [List.Average](https://msdn.microsoft.com/en-us/query-bi/m/list-average)
+ [List.Buffer](https://msdn.microsoft.com/en-us/query-bi/m/list-buffer)
+ [List.Median](https://msdn.microsoft.com/en-us/query-bi/m/list-median)
+ [List.Mode](https://msdn.microsoft.com/en-us/query-bi/m/list-mode)
+ [List.First](https://msdn.microsoft.com/en-us/query-bi/m/list-first)
+ [List.Last](https://msdn.microsoft.com/en-us/query-bi/m/list-last)
+ [List.Min](https://msdn.microsoft.com/en-us/query-bi/m/list-min)
+ [List.Max](https://msdn.microsoft.com/en-us/query-bi/m/list-max)
+ [List.Select](https://msdn.microsoft.com/en-us/query-bi/m/list-select)
+ [List.Sum](https://msdn.microsoft.com/en-us/query-bi/m/list-sum)
+ [Record.Field](https://msdn.microsoft.com/en-us/query-bi/m/record-field)
+ [Record.FieldValues](https://msdn.microsoft.com/en-us/query-bi/m/record-fieldvalues)
+ [Replacer.ReplaceValue](https://msdn.microsoft.com/en-us/query-bi/m/replacer-replacevalue)
+ [Table.AddColumn](https://msdn.microsoft.com/en-us/query-bi/m/table-addcolumn)
+ [Table.Column](https://msdn.microsoft.com/en-us/query-bi/m/table-column)
+ [Table.ColumnNames](https://msdn.microsoft.com/en-us/query-bi/m/table-columnnames)
+ [Table.RemoveColumns](https://msdn.microsoft.com/en-us/query-bi/m/table-removecolumns)
+ [Table.ReplaceValue](https://msdn.microsoft.com/en-us/query-bi/m/table-replacevalue)