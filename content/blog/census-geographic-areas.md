+++
author = "Tony McGovern"
categories = ["Census","Power Query M"]
tags = ["post"]
date = "2018-12-12"
description = "Help normalize and tidy your location data with PowerCensus."
draft = "true"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Get all U.S. geographic areas with PowerCensus"
type = "post"

+++

## Working with U.S. Geographies in Excel or Power BI
When I work with U.S. based geographies, such as urban areas, counties, or cities, I often go to the United States Census Bureau's website to get data about these geographies. As anyone who's tried to map geographies understands, having geographic boundaries display correctly in Excel or Power BI can be quite difficult.

Part of the difficulty lies with the fact that boundaries contain unique attributes that when layered on a map, must be bound to data one-for-one. So if I have demographic data on Washington County and want to visualize that data in a Power BI map, I must tell the visual *which* Washington County I'd like to map. Currently, there are over 30 counties in the United States with the name Washington County.

Moreover, over time geographies often change boundaries or their names. In July of 2013, Bedford County in the state of Virginia subsumed the neighboring independent city of Bedford to form a larger county. If my dataset has information on Bedford city, that data will not make it on the map. The same goes for Ogala Lakota County, South Dakota, which until recently, was known as Shannon County. If my data is for Shannon County, that data will not make it on to the map.

No where is this more apparent than when working with U.S. election data at the local level. 

