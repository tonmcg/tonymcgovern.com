+++
title = "Get U.S. States"
date = "2018-04-01"
+++

## Preliminaries
```javascript
let
    // load custom GetStates function from Github Gist
    Source = Text.FromBinary(Web.Contents("https://goo.gl/wVisz9")),
    GetStates = Expression.Evaluate(Source, #shared)
in
    GetStates
```

## Get U.S. States Including Territories
```javascript
let 
    Source = Text.FromBinary(Web.Contents("https://goo.gl/wVisz9")),
    GetStates = Expression.Evaluate(Source, #shared),
    results = GetStates(true)
in
    results
```
|    |state_name          |state_fips |state_abbr
|---:|:-------------------|:----------|:----
|1	 |Alabama             |01	        |AL
|2	 |Alaska              |02	        |AK
|3	 |Arizona             |04	        |AZ
|... |... 	              |...        |...
|60	 |U.S. Virgin Islands |78         |VI

## Get U.S. States Not Including Territories
```javascript
let 
    Source = Text.FromBinary(Web.Contents("https://goo.gl/wVisz9")),
    GetStates = Expression.Evaluate(Source, #shared),
    results = GetStates(false)
in
    results
```
|    |state_name          |state_fips |state_abbr
|---:|:-------------------|:----------|:----
|1	 |Alabama             |01	        |AL
|2	 |Alaska              |02	        |AK
|3	 |Arizona             |04	        |AZ
|... |... 	              |...        |...
|51  |Wyoming             |56         |WY

## References
1. [GetStates](https://gist.github.com/tonmcg/a545cd94d798c687fc3ced139093e23e) by Tony McGovern
2. [Text.FromBinary](https://msdn.microsoft.com/en-us/library/mt253365.aspx), Power Query M function reference
3. [Web.Contents](https://msdn.microsoft.com/en-us/library/mt260892.aspx), Power Query M function reference