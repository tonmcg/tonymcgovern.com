+++
author = "Tony McGovern"
categories = ["Power Query M", "Flow", "Power BI", "PowerApps", "Flow", "Azure Logic Apps", "custom connectors", "Socrata"]
tags = ["post"]
date = "2019-03-02"
description = "A simplified workflow to create custom data connectors to the Socrata Open Data Network in Power BI, PowerApps, Flow, and Azure Logic Apps"
draft = "true"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Creating a Custom Data Connector to RESTful APIs on the Power Platform"
type = "post"

+++

# Creating a Custom Data Connector to RESTful APIs on the Power Platform

I've found that with the right workflow, creating a custom data connector on the Microsoft Power Platform can be relatively straightforward. This post is the first in a series that describes the methods and tools I use to create custom data connectors to RESTful APIs with Power BI, PowerApps, Flow, and Azure Logic Apps. To illustrate this workflow, I'll build a custom connector to the Socrata Open Data API (SODA)[^1]. I'll walk through the methods I use to define Http requests with Postman[^2]. Along the way, I'll show how to create an OpenAPI Specification file[^3] and use it to help design the custom SODA connector in PowerApps, Flow, and Azure Logic Apps; the OpenAPI file will also serve as a blueprint as I do the same in Power BI.

For those new to building applications on top of RESTful APIs, I also try to demystify the process by defining fundamental Http request concepts. This means you can take the concepts, methods, and tools outlined here and create custom data connectors for hundreds of other RESTful APIs on the Power Platform. You’ll find most RESTful APIs follow a certain pattern and once learned, you’ll want to use this pattern to create a variety of other custom data connectors that enrich your Power Platform apps, allowing your users to more easily analyze, act, and automate. The possibilities are limitless.

This first post focuses on creating a custom data connector that makes requests to the SODA API without any authentication[^4]. Future posts will build on this custom SODA connector, incorporating application keys and OAuth 2.0 authentication methods.[^5]

A Simplified Workflow
=====================

My simplified workflow to build a custom data connector for the Power Platform
is:
1.  Define Http Requests with Postman
2.  Create an OpenAPI Definition File for PowerApps, Flow, and Azure Logic Apps
3.  Use the OpenAPI Definition as a Blueprint (Power BI only)

Define Http Requests with Postman
---------------------------------

Let's say I want to download a dataset containing fire emergency calls in the City of Seattle.[^6] In Postman I make a Http request to that dataset’s URL, `https://data.seattle.gov/resource/grwu-wqtk.json`:

![Emergency Calls Get Request Postman](img/main/Emergency Calls Get Request Postman.png)

which returns the following results:

![Emergency Calls Postman](img/main/Emergency Calls Postman.png)

Using the `Web.Contents` function in Power Query M, I make the same request in Power BI, which also yields similar results:
```javascript
let
    Source = Json.Document(Web.Contents("https://data.seattle.gov/resource/grwu-wqtk.json")),
    #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", Record.FieldNames(#"Converted to Table"{0}[Column1]))
in
    #"Expanded Column1"
```

![Emergency Calls Power BI](img/main/Emergency Calls Power BI.png)

Using this simple Http request to the SODA API in both Postman and Power Query M, I’m now ready to create an OpenAPI definition file that will help me build a custom data connector to the SODA API in PowerApps, Flow, and Azure Logic Apps.

Create an OpenAPI Definition File for PowerApps, Flow, and Azure Logic Apps
---------------------------------------------------------------------------

Postman has a concept of a *collection*, which is a group of saved requests organized into folders. In the example above, I create an Http request to the fire emergency calls dataset and save this request in a Postman collection[^7] I call "City of Seattle", under a folder titled, "Http Requests":

![Postman collection](media/0954cf1b03264b7b1944f79143f2f7ba.png)

Postman allows me to [export this collection](https://learning.getpostman.com/docs/postman/collections/data_formats/) as a JSON-formatted definition file that can be read by most programming languages and applications, including [PowerApps, Flow, and Azure Logic Apps](https://docs.microsoft.com/en-us/connectors/custom-connectors/define-postman-collection). I export the JSON file as a Postman Collection v1:

![A screenshot of a cell phone Description automatically generated](media/96c01bbac47dfdb451d5de10b767f1be.png)

![A screenshot of a cell phone Description automatically generated](media/8710c24238c69dd6033ecb4adf9771a8.png)

This Postman collection contains much of the required information needed for a custom data connector in PowerApps, Flow, and Azure Logic Apps. Follow this tutorial on the [Microsoft custom connectors documentation
page](https://docs.microsoft.com/en-us/connectors/custom-connectors/define-postman-collection#import-the-postman-collection) to learn how to import a Postman collection through the custom connector wizard.

Since the SODA API allows for open access and therefore does not require[^8] authentication, I skip over the “[Specify authorization
type](https://docs.microsoft.com/en-us/connectors/custom-connectors/define-postman-collection#specify-authentication-type)” section of the tutorial. In a future post, as I implement access tokens, basic authentication, and OAuth 2.0 authentication, I’ll walk through how to correctly set authentication parameters in this section.

This post picks up at the “[Review and update the connector
definition](https://docs.microsoft.com/en-us/connectors/custom-connectors/define-postman-collection#review-and-update-the-connector-definition)” section of the tutorial. On the **3. Definition** page of the custom connector wizard, the UI shows Actions, Triggers, and References sections.

PowerApps, Flow, and Azure Logic Apps represent Http requests as Actions. The Action displayed below is for the fire emergency calls request I defined in the Postman collection, which I call FireEmergency. Postman collections only define Http requests and have no notion of Triggers or References, so I leave those blank.

![A screenshot of a cell phone Description automatically generated](media/601dc3190039e4b06e4b267f8b542b6e.png)

The General area displays information about the currently selected Action, or
Http request.

![A screenshot of a cell phone Description automatically generated](media/0ddbc1ff6c6e1de21c2520f00b9a2977.png)

In the Request area, I can update the actual fire emergency calls Http request.
Since it correctly shows the dataset’s
`https://data.seattle.gov/resource/grwu-wqtk.json` URL I defined in the Postman
collection, I don’t need to update this information[^9]:

![A screenshot of a social media post Description automatically generated](media/b56cb4ba27d97e17f68d30715f4b7c7e.png)

Flow and PowerApps allow me to test the connection I just created. On the **4.
Test** page, under the Connections section, I click on New connection to create
a connection to the SODA API.

![A screenshot of a cell phone Description automatically generated](media/17cdaeffae66ccd2a355aa11285349d3.png)

To test the FireEmergency Action I press **Test operation**:

![A screenshot of a social media post Description automatically generated](media/8b8f9df02c1c981087fd8be6e0bb08f9.png)

The connector calls the fire emergency calls dataset on the SODA API and the
test response returns results identical to the ones returned in Postman and
Power BI. I click Update connector to save the updates I made to the custom data
connector definition. I click close to return to the Custom connectors gallery.

Though I defined the Http request for my custom SODA connector as a Postman
collection, internally PowerApps, Flow, and Azure Logic converts the Postman
collection to a [OpenAPI 2.0
definition](https://github.com/OAI/OpenAPI-Specification), which is available to
export and reuse to create another custom SODA connector on other environments.
To export my custom SODA connector as an OpenAPI definition file, I click the
plus sign:

![A close up of a logo Description automatically generated](media/3d8e0e4dd5d17c8941d6ee02fd28a7e9.png)

In a future post, I will explore the contents of an OpenAPI definition file and
highlight some of the more important sections that are necessary to understand
to create custom data connectors using the OAuth 2.0 authentication methods.

Use the OpenAPI Definition as a Blueprint (Power BI only)
---------------------------------------------------------

Power BI custom data connectors

![A screenshot of a social media post Description automatically generated](media/d15c34f9af673beed8b0d93bf2d01de7.png)

![A screenshot of a social media post Description automatically generated](media/431bd0fe2f341a32eea5c91280cbbb8e.png)

![A screenshot of text Description automatically generated](media/2ec95a6a18f35e0d923ead7b26593e7f.png)

![A screenshot of a cell phone Description automatically generated](media/d8b8d096766ca765c04a14a6f05d29fe.png)

![A screenshot of a computer Description automatically generated](media/1b17485e1054b2c491ae2745af7b536b.png)

![A screenshot of a cell phone Description automatically generated](media/bc205e842a74a9c7bca25c612e30c7ad.png)

![A screenshot of a cell phone Description automatically generated](media/2e414498b58cb6b9b193460b95a355eb.png)

![A screenshot of a social media post Description automatically generated](media/aca64211468ad262fe159b87e5ccc6f9.png)

![A screenshot of a computer Description automatically generated](media/33de03337f880b62e0334d7b26114c2f.png)

Things to Download
==================

-   Postman

-   Fiddler

-   Power BI Desktop

-   Visual Studio Community

-   Power Query SDK

Resources That Help Make Things Easier
======================================

-   <https://www.smashingmagazine.com/2018/01/understanding-using-rest-api/>

-   <https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Identifying_resources_on_the_Web>

-   <https://www.apimatic.io/transformer>

-   <https://www.jsonschema.net/>

Appendix 1: Anatomy of an Http Request
======================================

So, what happens behind the scenes as I make requests? What information is sent?
What other information is returned?

Before creating a custom data connector with this request, I'll now talk about
what goes into an Http request. Understanding this structure is vitally
important when trying to debug issues with requests or trying to build a robust
custom data connector for a RESTful API that’s not well documented.

Request and response headers are also vitally important; these concepts will
figure prominently in future posts when I talk about creating a custom data
connector using either application keys or the OAuth 2.0 authentication
protocol.

The main concepts defining an Http request are:

1.  Endpoint

2.  Methods

3.  Headers

4.  Body (or Data or Message)

Endpoint
--------

An endpoint is a unique address that represents an object or collection of
objects on a server. Well what does that mean? All of my previous requests in
the examples above ask for an object on the SODA API server, namely, the fire
emergency calls dataset. To do this, I make a call to either the
https://data.seattle.gov/resource/grwu-wqtk.json URL or
https://data.seattle.gov/api/odata/v4/kzjm-xkqj OData URL. Both URLs are
endpoints.

This endpoint consists of a https:// scheme, a data.seattle.gov domain name, and
a path that represents the location of the dataset. For the OData endpoint, that
path is /api/odata/v4/kzjm-xkqj. For the other endpoint, that path is
/resource/grwu-wqtk.json. Every endpoint on the SODA API contains a path that
consists of a unique target identifier for a dataset, also known as a resource.
Here, the target for the OData resource is /kzjm-xkqj. The target for the other
resource is /grwu-wqtk.json.

Methods
-------

The method is the type of request sent to the RESTful API. Generally, there are
five methods to choose from: GET, POST, PUT, PATCH, and DELETE. For custom data
connectors on the Power Platform, I focus only on GET and POST requests. The
request I make to *get* the fire emergency calls dataset does just that; it
performs a GET request. A GET request asks the API to look for data at the
address I provide and respond with the data I ask for.

In Postman, it looks like this:

![A screenshot of a cell phone Description automatically generated](media/e9da4ef2a0194325ed4c8b8fabda63b1.png)

A POST request creates a record on the API's server and then returns a response
to tell me if it succeeded in creating that record. In future posts, when I talk
about sending authorization credentials to authenticate against a RESTful
service, POST requests become important.

Headers
-------

Headers pass additional information within the [request and the
response](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers). Any time I
make a request for a resource on a RESTful API, both my request and the returned
response contain additional information within their respective headers. Here is
the additional information that is passed in both my fire emergency calls
request and the API response[^10]:

![A screenshot of a social media post Description automatically generated](media/4fbeb75780e878d30cf870b0e571078f.png)

Web browsers also allow me to see what is passed in the request and response
headers. I make a request to the fire emergency calls in Firefox and monitor the
headers in the Developer Console[^11]:

![A screenshot of a cell phone Description automatically generated](media/c017f8dfec62d6bf7e5a4b902860d478.png)

For the sake of completeness, the request and response headers using Power BI
look like this in Fiddler:

![A screenshot of a social media post Description automatically generated](media/5c7b67bb729d9c907b690824c3e62067.png)

Body (or Data or Message)
-------------------------

The body (or data or message) contains information sent to the server. Request
bodies are typically used with CREATE operations (POST requests). I don't make a
POST request in this custom SODA connector. But future posts devoted to
authenticating through Oauth 2.0 will make use of the body in a POST request.

Appendix 2: Using the OData Specification
=========================================

This appendix describes Http requests that return responses defined using the
OData specification[^12]. In the examples throughout this post, I asked the SODA
API for the fire emergency calls dataset via a request to the
<https://data.seattle.gov/resource/grwu-wqtk.json> URL. The API returned a
dataset as an array of objects, with little information about the dataset itself
or about the fields and their data types.

The SODA API also provides access the same dataset but with [results described
instead by the OData
specification](https://support.socrata.com/hc/en-us/articles/115005364207).
Results returned by an OData resource come with more information, like dataset
metadata that contain information about object properties. For the SODA API, the
metadata contains information on how the fields in the dataset are defined.

In other words, the same fire emergency calls dataset is available via the SODA
API but with results that *adhere to the OData specification*. This means that
for every resource on the SODA API, there is corresponding OData URL for that
resource. In effect, an Http request to a resources on the SODA API can respond
with one of two versions of each dataset: one defined by SODA’s internal
specification or another defined by the OData specification[^13].

To illustrate, I use Postman to request the same fire emergency calls dataset,
but this time at the `https://data.seattle.gov/api/odata/v4/kzjm-xkqj` OData
URL.[^14] The API returns the following results:

![A screenshot of a social media post Description automatically generated](media/69131f40866be16526b6ebe061996975.png)

While these results are similar to the original results, the OData URL results
include an \@odata.context property that provides a metadata link which contains
information on field data types. I perform a request on this
https://data.seattle.gov/api/odata/v4/\$metadata/`#kzjm-xkqj metadata link in
Postman and the XML response shows me how field data types are defined:

![A screenshot of a social media post Description automatically generated](media/91a499daa308f25c203f5cf5aa73b737.png)

In Power BI I use the OData.Feed function to request the same fire emergency
calls dataset via the OData URL. It also yields similar results:

``` javascript
let
    Source = OData.Feed("https://data.seattle.gov/api/odata/v4/kzjm-xkqj", null, [Implementation="2.0"])
in
    Source
```

![A screenshot of a computer Description automatically generated](media/0c63068458df20decef697b0b82ad781.png)

Notice how Power BI defines the column data types in this table? That's because
the `OData.Feed` function actually makes *two* requests: an initial request to the
resource and then a *separate* request to the metadata link provided by the
\@odata.context property. By doing so, the function determines the service's
schema, data types, and any relationships to other resources. For the fire
emergency calls request, the function applies the field data types defined in
the metadata to the columns in the resource automatically.

Additionally, this OData-defined dataset also comes with an \@odata.nextLink
property that provides information about the *next* set of results in the same
dataset[^15].

![A screenshot of a social media post Description automatically generated](media/ecfb719c06f8581e830727eca42a6b12.png)

RESTful APIs like SODA that adhere to the OData specification are important for
several reasons, four of which I’ll highlight.

OData Resources Have Metadata
-----------------------------

First, as I showed above, an OData service is a self-describing service that
exposes metadata defining relationships and entity types, among other things.
This will become useful when resources on an API are related to other resources
through a *data model*. Power BI has a feature called *autodetect* that attempts
to find and create relationships from resources on an OData service.

**OData Resources are Part of an Entity Set**
---------------------------------------------

Second, OData has a concept called an *entity set*, which groups similar
resources in a collection. The City of Seattle hosts the fire emergency calls
dataset on the data.seattle.gov domain. If I want to see what other datasets the
City of Seattle makes available, I can make a request to see a collection of
other resources. For example, an Http request in Postman to
https://data.seattle.gov/api/odata/v4 retrieves the entity set for the City of
Seattle. The results tell me there are hundreds of other resources available in
addition to the fire emergency calls dataset:

![A screenshot of a cell phone Description automatically generated](media/13c09616c4a2ed45ea39f97157e03cc1.png)

OData Provides Uniform Query Options
------------------------------------

OData defines a number of queries that refine the results at the source. Query
options can be thought of as verbs that instruct the API server to perform data
transformations before returning a response. Some of the more common OData query
options are \$filter, \$orderby, \$skip, \$top, and \$select. If I want to limit
and shape the fire emergency calls results returned by the SODA resource, I can
add the one or more of these query options to the request. For instance, I want
just the report_location for the top 3 fire emergency calls. My request looks
like this:

``` javascript
https://data.seattle.gov/api/odata/v4/kzjm-xkqj?\$top=3&\$select=report_location
```

Power BI Transforms Data at the OData Source
--------------------------------------------

Finally, perhaps the most important reason is that Power BI performs *query
folding* on OData resources using the `OData.Feed` function. From the [Microsoft
documentation on implementing query folding in a custom data connector in Power
BI](https://docs.microsoft.com/en-us/power-query/handlingqueryfolding):

>   One of the most powerful capabilities of Power Query and the M Language is
>   Query Folding (also referred to as query delegation, and predicate
>   push-down). Query Folding allows the M Engine to push the transformations
>   expressed in an M query to the source, in the source's native query
>   language, resulting in more efficient data processing.

What this means is I can also perform certain transformation steps within the
Power Query Editor, and *those steps* will be pushed to the SODA API server,
resulting in faster load times and more efficient processing. Along with query
options, query folding helps shape the response from the SODA API before it
reaches the custom SODA connector[^16].

I add the `$filter`, `$orderby`, `$skip`, `$top`, and `$select` query options as a
Key to the Query Params section in Postman. I leave the corresponding Value
field blank and ensure **all key-value pairs are unchecked:**

![A screenshot of a cell phone Description automatically generated](media/5ee46ca02126b8040559c09f61f87de8.png)

[^1]: The SODA API is a RESTful service that offers developers tens of thousands
of datasets from hundreds of Open Data Network entities like governments,
non-profits, and NGOs from around the world. SODA is not only easy to use and
understand, it also comes with great documentation that lets me interactively
request the latest data.
[^2]: Why Postman? It's the *de-facto* tool that helps developers quickly define
requests for any given REST API. It requires far less trial-and-error to
construct good requests with it than say, using [Power BI and
Fiddler ](https://blog.crossjoin.co.uk/2018/05/03/troubleshooting-data-refresh-performance-issues-with-odata-data-sources-in-power-bi-and-excel-using-fiddler/)alone.
And it lets me easily see every bit of information I need about my requests,
making it easier to debug my custom SODA connector in Power BI, Flow, or Azure
Logic Apps.
[^3]: OpenAPI is an industry-standard specification that defines requests to any
RESTful API. I initially create a JSON-formatted definition file for my custom
data connector using Postman.
[^4]: **Note**: While the SODA API allows for unlimited requests to its service,
calls without an application token are [subject to throttling
limits](https://dev.socrata.com/docs/app-tokens.html#throttling-limits). While
not absolutely necessary to follow along with this post, you'll need an
application token to follow along in future posts. Please do consider [obtaining
one
here](https://dev.socrata.com/docs/app-tokens.html#obtaining-an-application-token).

[^5]: One reason I chose to build a custom data connector on the Power Platform
for the SODA API is because this API supports four common ways applications make
Http requests: 1) no authentication (open access), 2) application key or token,
1) authentication with HTTP Basic, and 4) authentication through the OAuth 2.0
protocol. This means the methods I describe in this post apply equally to other
custom data connectors for other RESTful APIs, like the U.S. Census Bureau API,
Google Sheets, Dropbox, and LinkedIn, just to name a few. This workflow helps me
quickly build custom data connectors for any new RESTful API.
[^6]: The [SODA dataset documentation
page](https://dev.socrata.com/foundry/data.seattle.gov/grwu-wqtk) provides a
tool that lets me see the results of this request right in the browser:

    ![A screenshot of a social media post Description automatically generated](media/2648aa9ac1abc54ad11bd57f499f670e.png)
[^7]: Microsoft offers similar steps on their documentation page devoted to
[creating a Postman
collection](https://docs.microsoft.com/en-us/connectors/custom-connectors/create-postman-collection).
[^8]: Again, while the SODA API does not *require* us to register an application
to make requests with an access token, proper application development principles
*compel* us to abide by that standard. Future posts will walk through how to
register an application to use access tokens in a request.
[^9]: I didn’t define any query parameters within this request. In future posts,
I’ll add query parameters that will instruct the API server to shape and
transform the data before sending a response.
[^10]: Here I'm showing header and payload information on both the request and
the response in [Postman
Console](https://learning.getpostman.com/docs/postman/sending_api_requests/debugging_and_logs/#network-calls-with-postman-console).
[^11]: I prefer working in Firefox. But most other browsers have a version of
the Network Monitor. Here's how you access the Network panel in [Google Chrome
DevTools](https://developers.google.com/web/tools/chrome-devtools/network/).
Here it is in [Microsoft Edge
DevTools](https://docs.microsoft.com/en-us/microsoft-edge/devtools-guide/network).
Here it is in [Safari Web Inspector](https://developer.apple.com/safari/tools/).
And if you're still using Internet Explorer, here is the Network tab in [the F12
Developer Tools
Interface](https://msdn.microsoft.com/en-us/library/hh968260(v=vs.85).aspx#NetworkTab).
[^12]: The Open Data Protocol defines a set of best practices for building and
consuming RESTful APIs. It is a specification that [describes both the data and
the data model](https://www.odata.org/documentation/odata-version-3-0/odata-version-3-0-core-protocol/#overview).
[^13]: This should not be confused with REST, which defines *how* an
[application connects to an API](https://www.youtube.com/watch?v=7YcW25PHnAA).
The OData specification describes *what* data the application should expect from
the server.
[^14]: The [dataset metadata documentation
page](https://data.seattle.gov/Public-Safety/Seattle-Real-Time-Fire-911-Calls/kzjm-xkqj)
makes this URL available by clicking the ellipses (**…**) button near the “View
Data" button on the top-right of the page. Once clicked, I click the “Copy”
button for the OData URL:

    ![A screenshot of a social media post Description automatically generated](media/640a63484991b8e56d1ea57eae670edf.png)
[^15]: In this case, the \@odata.nextLink property tells me that if I want to
request the next page of results, the OData URL is
https://data.seattle.gov/api/odata/v4/kzjm-xkqj?skip=1000&?top=1000. In a future
post, I’ll talk about query parameters and programmatically paging through data
to the get a full set of results.
[^16]: Chris Webb illustrates how query folding works [with a simple
example](https://blog.crossjoin.co.uk/2018/06/27/odata-performance-power-bi/).