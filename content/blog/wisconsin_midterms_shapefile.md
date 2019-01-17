+++
author = "Tony McGovern"
categories = ["Mapbox","elections","Power BI","shapefiles","mapshaper"]
tags = ["post"]
date = "2018-12-12"
description = "See a map of the Wisconsin 2018 midterm election results from data provided by John Johnson."
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Map of Wisconsin midterm election results for 2018"
type = "post"

+++

## Wisconsin Midterm Election Results for 2018
When voters in the state of Wisconsin cast ballots for national and state elections, they do so at the "ward"-level, an administrative boundary created to conduct elections. Yet when it comes to tabulating and reporting election results, Wisconsin officials combine wards into "reporting units", larger administrative boundaries comprised of one or more wards. But there is no authoritative body that produces shapefiles for these reporting unit boundaries, leaving analysts, journalists, and the public little opportunity to visualize Wisconsin election results in any meaningful way.

For the 2018 midterm elections, John Johnson, Research Fellow at the Marquette Law School, filled in that gap and created a [shapefile that provides high-quality, consistent boundaries and election results for Wisconsin](https://johndjohnson.info/2018/12/10/a-shapefile-of-wisconsin-november-2018-reporting-units/). Expanding on his work, I used his shapefile -- which also contains election data -- and visualized the results.
![Wisconsin Midterm Results](/img/main/Wisconsin_2018_Midterms.gif)

I created the map in Power BI using the Mapbox Visual. I converted the shapefile to geojson with [mapshaper](https://mapshaper.org/) and simplified its features using [mapshaper's command line tools](https://github.com/mbloch/mapshaper/wiki/Command-Reference). I applied the Visvalingam simplification method through the following statement:

```
-simplify 10%
```

I converted the geojson into a tileset using [Mapbox Studio](https://www.mapbox.com/studio/tilesets/).