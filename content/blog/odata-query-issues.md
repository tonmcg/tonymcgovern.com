+++
author = "Tony McGovern"
categories = ["Power Query M, Flow, Power BI, PowerApps, Flow, Azure Logic Apps, custom connectors, Socrata"]
tags = ["post"]
date = "2019-02-28"
description = "OData.Feed has issues with query parameters"
draft = "true"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "OData Query Parameter Issues in Power BI"
type = "post"

+++

# OData Query Paramter Issues

The `Odata.Feed` Power Query M function has many issues
Odata endpoints follow the ___ specification => 
query parameters can be used to filter results

When I try to filter results on SODA using query parameters, it doesn't work.

Example:

Say I want to see data about police department beats from the [City of Seattle](https://data.seattle.gov/Public-Safety/Seattle-Police-Department-Beats/nnxn-434b). The Socrata Open Data API allows me to use an OData protocal to connect to this data. 

Using Postman, when I make this request the response I get is an array containing 8 objects:
[image of Postman response]

Using the `OData.Feed` function in Power BI, I get the same results:

``` javascript
let
    Source = OData.Feed("https://data.seattle.gov/api/odata/v4/nnxn-434b")
in
    Source
```

I can ask the endpoint to filter these results in a number ways. For example, by adding the `$top` query parameter, I can limit the results to return only a top number of results. Here I ask for only the top 3 results:
[Postman image of results with only 3 results using `top` parameter]



SODA provides a few different ways to get to connect to this endpoint:
RESTful Http request
OData version 2
OData version 4
<enter helpful link about OData v RESTful API>
