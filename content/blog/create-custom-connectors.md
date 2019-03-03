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

I've found that with the right workflow, creating a custom data connector on the Microsoft Power Plaftorm can be relatively straightforward. This post is the first in a series that describes the methods and tools I use to create custom data connectors to RESTful APIs with Power BI, Flow, and by extenstion, Azure Logic Apps. To illustrate this workflow, I'll build a custom connector to the Socrata Open Data API (SODA)<sup>[[1]](#soda)</sup>. I'll walk through my method to define, test, and debug requests with Postman<sup>[[2]](#postman)</sup>. I'll highlight important RESTful concepts, such as request endpoints, headers, and methods. Along the way, I'll show how to create a Swagger file<sup>[[3]](#swagger)</sup>, an industry-standard specification that defines requests to any RESTful API. Finally, this Swagger file will help me create the custom SODA connector in Flow and Azure Logic Apps; it will also serve as a blueprint as I do the same in Power BI.

This first post focuses on creating a custom SODA connector that makes requests to the API without any authentication<sup>[[4]](#app-token)</sup>. Future posts will build on this custom data connector, incorporating application keys and OAuth 2.0 authentication methods.

# A Simplified Workflow

One reason I chose the SODA API as my example is that it supports four common ways for applications to request data through its service: 1) no authentication (open access), 2) application key or token, 3) authentication with HTTP Basic, and 4) authentication through the OAuth 2.0 protocol. This means the methods I describe in this post to create a custom SODA connector will apply to other custom data connectors for hundreds of other RESTful APIs, like the U.S. Census Bureau API, Google Sheets, Dropbox, and LinkedIn, just to name a few.

I've codified these methods into a simplified workflow:

  - [Define, Test, and Debug API Requests](#define-test-and-debug-api-requests)
  - [Create Swagger Definition File for Flow (or Azure Logic Apps)](#create-swagger-definition-file-for-flow-or-azure-logic-apps)
  - [Use Swagger as a Blueprint (Power BI only) ](#use-swagger-as-a-blueprint-power-bi-only)

## Define, Test, and Debug API Requests

Let's say I want to download the latest fire emergency calls in the City of Seattle. The [SODA dataset documentation page](https://dev.socrata.com/foundry/data.seattle.gov/grwu-wqtk) provides a tool that lets me see the results of this request right in the browser:

![Emergency calls](/img/main/Emergency Calls.png)

I use the same URL as in the documentation page, `https://data.seattle.gov/resource/grwu-wqtk.json`, and make a request in Postman, which returns similar results:

![Emergency Calls Postman](/img/main/Emergency Calls Postman.png)

Using the `Web.Contents`<sup>[[4]](#odata)</sup> function, I make the same request in Power BI, which also yields similar results:

``` javascript
let
    Source = Json.Document(Web.Contents("https://data.seattle.gov/resource/grwu-wqtk.json")),
    #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", Record.FieldNames(#"Converted to Table"{0}[Column1]))
in
    #"Expanded Column1"
```

![Emergency Calls Power BI](/img/main/Emergency Calls Power BI.png)

### Using the OData Specification

The SODA API also provides access the same dataset but with results described instead by the OData specification<sup>[[6]](#odata-spec)</sup>. In the example above, I asked the SODA API for the fire emergency calls dataset via a request to `https://data.seattle.gov/resource/grwu-wqtk.json`. This returned a dataset as an array of objects, with little information about the dataset itself or about the fields and their data types.

Results returned by an OData resource, on the other hand, come with more information, like dataset metadata and information about the properties in the array of objects. In other words, I ask the SODA API for the same fire emergency calls dataset via an OData URL and the service returns results that *adhere to the OData specification*. 

To illustrate, I use Postman to request the same fire emergency calls dataset, but this time at the `https://data.seattle.gov/api/odata/v4/kzjm-xkqj` OData URL. The API returns the following results:

![Emergency Calls Postman OData Context](/img/main/Emergency Calls Postman OData Context.png)

These results include an `@odata.context` property that provides a URL which contains information on field data types. I perform a request on `https://data.seattle.gov/api/odata/v4/$metadata#kzjm-xkqj` in Postman and the XML response defines the dataset's data types:

![Emergency Calls Postman OData Metadata](/img/main/Emergency Calls Postman OData Metadata.png)

In Power BI I use the `OData.Feed` function to make the same request. It also yields similar results:

``` javascript
let
    Source = OData.Feed("https://data.seattle.gov/api/odata/v4/kzjm-xkqj", null, [Implementation="2.0"])
in
    Source
```

![Emergency Calls Power BI OData](/img/main/Emergency Calls Power BI OData.png)

Notice how Power BI defines the column data types in this table? That's because the `OData.Feed` function reads the URL provided by the `@odata.context` property and makes a *separate* request to that metadata resource. The function then applies the field data types defined in the XML file to the columns in the resource automatically. Additionally, this OData-defined dataset also comes with an `@odata.nextLink` property that provides information about the *next* set of results in the same dataset.

![Emergency Calls Postman OData NextLink](/img/main/Emergency Calls Postman OData NextLink.png)

### Anatomy of an API Request

So what happens behind the scenes as I make these requests? What information is sent? What other information is returned?

Before creating a custom data connector with this request, I'll now talk about what goes into an API request. Understanding the structure of request and response headers is important since it will figure promintently in future posts when I talk about creating a custom data connector using either application keys or the OAuth 2.0 authentication protocol.

The main concepts defining an API request are:

- [Endpoint](#endpoint)
- [Method](#method)
- [Headers](#headers)
- [Body (or Data or Message)](#body-or-data-or-message)

#### Endpoint

An endpoint is a unique address that represents an object or collection of objects on a server. Well what does that mean? My request in the example above asks for an object on the SODA API server, namely, the fire emergency calls dataset. To do this, I make a call to the unique `https://data.seattle.gov/resource/grwu-wqtk.json` address. This is an endpoint. 

This endpoint consists of a `https://` scheme, a `data.seattle.gov` domain name, and a `/resource/grwu-wqtk.json` path that represents the location of the dataset. For every endpoint on the SODA API, the path consists of a unique target identifier for a dataset, also known as a resource. Here, the target for the resource is `/grwu-wqtk.json`.

#### Methods

The method is the type of request sent to the RESTful API. Generally, there are five methods to choose from: `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`. For custom data connectors in Power BI or Flow, I focus only on `GET` and `POST` requests, which map to `READ` and `CREATE` operations, respectively.

The request I make to *get* the fire emergency calls dataset does just that; it performs a `GET` request. A `GET` request asks the API to look for data at the address I provide and respond with the data I ask for. In Postman, it looks like this: 

![Emergency Calls Get Request Postman](/img/main/Emergency Calls Get Request Postman.png)

A `POST` request creates a record on the API's server and then returns a response to tell me if it succeeded in creating that record. In future posts, when I talk about sending authorization credientials to authenticate against a RESTful service, `POST` requests become important.

#### Headers

Headers pass additional information within the [request and the response](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers). Any time I make a request for a resource on a RESTful API, both my request and the returned response contain additional information within their respective headers. Here is the additional information that is passed in both my fire emergency calls request and the API response<sup>[[4]](#postman-headers)</sup>:

![Emergency Calls Headers Postman](/img/main/Emergency Calls Headers Postman.png)

Web browsers also allow me to see what is passed in the request and response headers. I make a request to the fire emergency calls in Firefox and monitor the headers in the Developer Console<sup>[[5]](#dev-console)</sup>:

![Emergency Calls Headers Firefox](/img/main/Emergency Calls Headers Firefox.png)

Not to be outdone, the headers for my request in Power BI look like this in Fiddler:

![Emergency Calls Headers Power BI](/img/main/Emergency Calls Headers Power BI.png)

#### Body (or Data or Message)

The body (or data or message) contains information sent to the server. Request bodies are typically used with `CREATE` operations (`POST` requests). I don't make a `POST` request in this custom SODA connector. But future posts devoted to authenticating through Oauth 2.0 will make use of the body in a `POST` request.

## Create Swagger Definition File for Flow (or Azure Logic Apps)

## Use Swagger as a Blueprint (Power BI only) 



# Things to Download
- Postman Collection file
- Swagger definition file
- Flow Custom Connector
- Power BI Custom Connector

# Resources You Should Know
+ <https://www.smashingmagazine.com/2018/01/understanding-using-rest-api/>
+ <https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Identifying_resources_on_the_Web>
+ <https://www.apimatic.io/transformer>
+ <https://www.jsonschema.net/>

# Footnotes
<ol>
<li id="soda">The SODA API is a RESTful service that offers developers tens of thousands of datasets from hundreds of Open Data Network entities like governments, non-profits, and NGOs from around the world. SODA is not only easy to use and understand, it also comes with great documentation that lets me interactively request the latest data.</li>
<li id="app-token"><strong>Note</strong>: While the SODA API allows for unlimited requests to its service, calls without an application token are <a href= "https://dev.socrata.com/docs/app-tokens.html#throttling-limits">subject to throttling limits</a>. While not absolutely necessary to follow along with this post, you'll need an application token to follow along in future posts. Please do consider <a href="https://dev.socrata.com/docs/app-tokens.html#obtaining-an-application-token">obtaining one here</a>.</li>
<li id="postman">Why Postman? It's the <em>de-facto</em> tool tha helps developers quickly define requests for any given REST API. It requires far less trial-and-error to construct good requests with it than say, using <a href="https://blog.crossjoin.co.uk/2018/05/03/troubleshooting-data-refresh-performance-issues-with-odata-data-sources-in-power-bi-and-excel-using-fiddler/">Power BI and Fiddler </a>alone. And it lets me easily see every bit of information I need about my requests, making it easier to debug my custom SODA connector in Power BI, Flow, or Azure Logic Apps. More importantly, Postman translates the definitions I created for each endpoint and creates a JSON-formatted schema for the custom data connector, which I then convert to a Swagger definition file.</li>
<li id="postman-headers">Here I'm showing header and payload information on both the request and the response in <a href= "https://learning.getpostman.com/docs/postman/sending_api_requests/debugging_and_logs/#network-calls-with-postman-console">Postman Console</a>.</li>
<li id="dev-console">I prefer working in Firefox. But most other browsers have a version of the Network Monitor. Here's how you access the Network panel in <a href="https://developers.google.com/web/tools/chrome-devtools/network/">Google Chrome DevTools</a>. Here it is in <a href="https://docs.microsoft.com/en-us/microsoft-edge/devtools-guide/network">Microsoft Edge DevTools</a>. Here it is in <a href="https://developer.apple.com/safari/tools/">Safari Web Inspector</a>. And if you're still using Internet Explorer, here is the Network tab in <a href="https://msdn.microsoft.com/en-us/library/hh968260(v=vs.85).aspx#NetworkTab">the F12 Developer Tools Interface</a>.</li>
<li id="odata-spec">The Open Data Protocol defines a set of best practices for building and consuming RESTful APIs. It is a specification that <a href="https://www.odata.org/documentation/odata-version-3-0/odata-version-3-0-core-protocol/#overview">describes both the data and the data model</a>.</li>
<li id="restful-spec">This means that for every resource on the SODA API, there is corresponding OData endpoint for that resource. In effect, the SODA API has two copies of every dataset on its service: one defined by Socrata and another defined by OData. I like to think of REST as a protocol that defines <em>how</em> an application requests resources on a server. The OData specification defines <em>what</em> data the application should expect from the server.</li>
</ol>