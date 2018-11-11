+++
title = "Read Socrata"
date = "2018-11-11"
+++

## Preliminaries
```javascript
let
    // load custom Socrata.ReadData function from GitHub
    Source = 
        Expression.Evaluate(
            Text.FromBinary(
                Web.Contents(
                    "https://raw.githubusercontent.com/tonmcg/powersocrata/master/M/Socrata.ReadData.pq"
                )
            ),
            #shared
        )
in
     Source
```

## Get the first 1,000 records from the San Francisco Police Department Calls for Service
```javascript
let 
  ReadSocrata = 
    Expression.Evaluate(
      Text.FromBinary(
        Web.Contents(
          "https://raw.githubusercontent.com/tonmcg/powersocrata/master/M/Socrata.ReadData.pq"
        )
      ),
      #shared
    ),
  data = ReadSocrata("https://data.sfgov.org/resource/fjjd-jecq.json", null, null)
in
    data
```

| |crime_id |original_crimetype_name |... |
|:---:|:----|:----|:---:|
|1 |160903280 |Assault / Battery |...|
|2 |160912272 |Homeless Complaint |...|
|3 |160912590 |Susp Info |...|
|...|...|...|...|
|1000 |160921628 |Assault / Battery |...|


## Get the first 100K calls since 2016 categorized as "Homeless Complaint" 
Requires the use of an application token (*app token*)<sup id="a1">[[1]](#token)</sup>
```javascript
let 
  ReadSocrata = 
    Expression.Evaluate(
      Text.FromBinary(
        Web.Contents(
          "https://raw.githubusercontent.com/tonmcg/powersocrata/master/M/Socrata.ReadData.pq"
        )
      ),
      #shared
    ),
  data = ReadSocrata("https://data.sfgov.org/resource/fjjd-jecq.json?$where=original_crimetype_name='Homeless+Complaint'+AND+call_dttm>'2016-01-01T00:00:00.000'", <APP TOKEN>, 100000)
in
    data
```

||crime_id |original_crimetype_name |...|
|:---:|:---|:---|:---:|
|1|160912272 |Homeless Complaint |...|
|2|160913050 |Homeless Complaint |...|
|3|160913056 |Homeless Complaint |...|
|...|...|...|...|
|100000|160991515 |Homeless Complaint |...|


## References
### Custom Function Reference
+ [PowerSocrata](https://github.com/tonmcg/powersocrata) by Tony McGovern

## Footnotes
<ol><li id="token">Returning more than 1,000 rows requires the use of an <em>app token</em>.<a href="https://github.com/tonmcg/powersocrata/blob/master/README.md#with-app-token-parameter"> See here about getting and using an <em>app token</em></a>.</li></ol>