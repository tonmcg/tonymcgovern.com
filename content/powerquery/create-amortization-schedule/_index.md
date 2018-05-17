+++
title = "Create Amortization Schedule"
date = "2018-05-16"
+++

## Preliminaries
```javascript
let
    // load custom CreateAmortization function from Github Gist
    CreateAmortization = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/v7LPME")),
        [
            #"List.First" = List.First,
            #"List.Select" = List.Select,
            #"Number.Power" = Number.Power,
            #"Int64.Type" = Int64.Type,
            #"Table.FromList" = Table.FromList,
            #"List.Generate" = List.Generate,
            #"Date.AddDays" = Date.AddDays,
            #"Date.AddMonths" = Date.AddMonths,
            #"Date.AddQuarters" = Date.AddQuarters,
            #"Date.AddYears" = Date.AddYears,
            #"Number.Round" = Number.Round,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"ExtraValues.Error" = ExtraValues.Error,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Value.ReplaceType" = Value.ReplaceType
        ]
    )
in
    CreateAmortization
```

## Create Amortization Table
```javascript
let
    // Create an amortization schedule with a 1000 initial amount, 5% interest rate, for 5 years, 
    // and is compounded semi-annually
    CreateAmortization = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/v7LPME")),
        [
            #"List.First" = List.First,
            #"List.Select" = List.Select,
            #"Number.Power" = Number.Power,
            #"Int64.Type" = Int64.Type,
            #"Table.FromList" = Table.FromList,
            #"List.Generate" = List.Generate,
            #"Date.AddDays" = Date.AddDays,
            #"Date.AddMonths" = Date.AddMonths,
            #"Date.AddQuarters" = Date.AddQuarters,
            #"Date.AddYears" = Date.AddYears,
            #"Number.Round" = Number.Round,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"ExtraValues.Error" = ExtraValues.Error,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Value.ReplaceType" = Value.ReplaceType
        ]
    ),
    results = CreateAmortization(1000, 0.05, 5, "Semi-annual")
in
    results
```
|     |n 	    |BegBalance |Interest |Principal |Payment |EndBalance |
|:---:|------:|----------:|--------:|---------:|-------:|----------:|
|1	  |0	    |           |         |          |        |1000       |
|2	  |1	    |1000	      |25	      |89.26	   |114.26	|910.74     |
|3	  |2	    |910.74	    |22.77	  |91.49	   |114.26	|819.25     |
|...  |... 	  |...        |...      |...       |...     |...        |
|11	  |10 	  |111.47	    |2.79	    |111.47	   |114.26	|0          |

## Create Amortization Table with Optional Date Column
```javascript
let
    // Create an amortization schedule with a 1000 initial amount, 5% interest rate, for 5 years, 
    // is compounded semi-annually, and starts on January 1, 2018 
    CreateAmortization = Expression.Evaluate(
        Text.FromBinary(Web.Contents("https://goo.gl/v7LPME")),
        [
            #"List.First" = List.First,
            #"List.Select" = List.Select,
            #"Number.Power" = Number.Power,
            #"Int64.Type" = Int64.Type,
            #"Table.FromList" = Table.FromList,
            #"List.Generate" = List.Generate,
            #"Date.AddDays" = Date.AddDays,
            #"Date.AddMonths" = Date.AddMonths,
            #"Date.AddQuarters" = Date.AddQuarters,
            #"Date.AddYears" = Date.AddYears,
            #"Number.Round" = Number.Round,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"ExtraValues.Error" = ExtraValues.Error,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Value.ReplaceType" = Value.ReplaceType
        ]
    ),
    results = CreateAmortization(1000, 0.05, 5, "Semi-annual", #date(2018,1,1))
in
    results
```
|     |n 	    |Date       |BegBalance |Interest |Principal |Payment |EndBalance |
|:---:|------:|----------:|----------:|--------:|---------:|-------:|----------:|
|1 	  |0	    |1/1/18	    |           |         |          |        |1000       |
|2 	  |1	    |7/1/18	    |1000	      |25	      |89.26	   |114.26	|910.74     |
|3	  |2	    |1/1/19	    |910.74	    |22.77	  |91.49	   |114.26	|819.25     |
|...  |...    |...        |...        |...      |...       |...     |...        |
|11	  |10 	  |1/1/23	    |111.47	    |2.79	    |111.47	   |114.26	|0          |

## References
+ [CreateAmortization](https://gist.github.com/tonmcg/0748ad9fcfb542aada7a2c153cfb0fb9) by Tony McGovern

## Appendix: For Use on Power BI Service
<div style="table-layout:fixed;display:table;width:100%;">
{{< gist tonmcg 0748ad9fcfb542aada7a2c153cfb0fb9 >}}
</div>