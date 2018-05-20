+++
title = "Create Amortization Schedule"
date = "2018-05-19"
+++

## Preliminaries
```javascript
let
     // load custom CreateAmortization function from Github Gist
     CreateAmortization = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/TableCreateAmortization")),
        [
            #"Date.AddDays" = Date.AddDays,
            #"Date.AddMonths" = Date.AddMonths,
            #"Date.AddQuarters" = Date.AddQuarters,
            #"Date.AddYears" = Date.AddYears,
            #"ExtraValues.Error" = ExtraValues.Error,
            #"Int64.Type" = Int64.Type,
            #"List.First" = List.First,
            #"List.Generate" = List.Generate,
            #"List.Select" = List.Select,
            #"Number.Power" = Number.Power,
            #"Number.Round" = Number.Round,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Table.FromList" = Table.FromList,
            #"Table.FromRecords" = Table.FromRecords,
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
        Text.FromBinary(Web.Contents("http://bit.ly/TableCreateAmortization")),
        [
            #"Date.AddDays" = Date.AddDays,
            #"Date.AddMonths" = Date.AddMonths,
            #"Date.AddQuarters" = Date.AddQuarters,
            #"Date.AddYears" = Date.AddYears,
            #"ExtraValues.Error" = ExtraValues.Error,
            #"Int64.Type" = Int64.Type,
            #"List.First" = List.First,
            #"List.Generate" = List.Generate,
            #"List.Select" = List.Select,
            #"Number.Power" = Number.Power,
            #"Number.Round" = Number.Round,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Table.FromList" = Table.FromList,
            #"Table.FromRecords" = Table.FromRecords,
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
        Text.FromBinary(Web.Contents("http://bit.ly/TableCreateAmortization")),
        [
            #"Date.AddDays" = Date.AddDays,
            #"Date.AddMonths" = Date.AddMonths,
            #"Date.AddQuarters" = Date.AddQuarters,
            #"Date.AddYears" = Date.AddYears,
            #"ExtraValues.Error" = ExtraValues.Error,
            #"Int64.Type" = Int64.Type,
            #"List.First" = List.First,
            #"List.Generate" = List.Generate,
            #"List.Select" = List.Select,
            #"Number.Power" = Number.Power,
            #"Number.Round" = Number.Round,
            #"Splitter.SplitByNothing" = Splitter.SplitByNothing,
            #"Table.ExpandRecordColumn" = Table.ExpandRecordColumn,
            #"Table.FromList" = Table.FromList,
            #"Table.FromRecords" = Table.FromRecords,
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
### Custom Function Reference
+ [CreateAmortization](https://gist.github.com/tonmcg/0748ad9fcfb542aada7a2c153cfb0fb9) by Tony McGovern

### Power Query M Reference
+ {{<urls function="date-adddays">}}Date.AddDays{{</urls>}}
+ {{<urls function="date-addmonths">}}Date.AddMonths{{</urls>}}
+ {{<urls function="date-addquarters">}}Date.AddQuarters{{</urls>}}
+ {{<urls function="date-addyears">}}Date.AddYears{{</urls>}}
+ {{<urls function="extravalues-error">}}ExtraValues.Error{{</urls>}}
+ {{<urls function="list-first">}}List.First{{</urls>}}
+ {{<urls function="list-generate">}}List.Generate{{</urls>}}
+ {{<urls function="list-select">}}List.Select{{</urls>}}
+ {{<urls function="number-power">}}Number.Power{{</urls>}}
+ {{<urls function="number-round">}}Number.Round{{</urls>}}
+ {{<urls function="splitter-splitbynothing">}}Splitter.SplitByNothing{{</urls>}}
+ {{<urls function="table-expandrecordcolumn">}}Table.ExpandRecordColumn{{</urls>}}
+ {{<urls function="table-fromlist">}}Table.FromList{{</urls>}}
+ {{<urls function="table-fromrecords">}}Table.FromRecords{{</urls>}}
+ {{<urls function="value-replacetype">}}Value.ReplaceType{{</urls>}}

## Appendix: For Use on Power BI Service
<div style="table-layout:fixed;display:table;width:100%;">
{{< gist tonmcg 0748ad9fcfb542aada7a2c153cfb0fb9 >}}
</div>