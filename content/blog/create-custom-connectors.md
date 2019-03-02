+++
author = "Tony McGovern"
categories = ["Power Query M, Flow, Power BI, PowerApps, Flow, Azure Logic Apps, custom connectors, Socrata"]
tags = ["post"]
date = "2019-02-27"
description = "Using Postman to create custom connectors to the Socrata Open Data Network in Power BI, PowerApps, Flow, and Azure Logic Apps"
draft = "true"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Creating a Custom Data Connection to the Socrata Open Data API on the Power Platform"
type = "post"

+++

# Creating a Custom Data Connector to the Socrata Open Data API on the Power Platform

I've found that with the right workflow, creating a custom data connector on the Microsft Power Plaftorm can be relatively straightforward. This post is the first in a series that describes the methods and tools I use to create custom data connectors to any RESTful API. To illustrate this workflow, this post will walk through building custom data connectors for Power BI, Flow, and Azure Logic Apps to the Socrata Open Data API (SODA)<sup>[[1]](#soda)</sup>. I'll show my method to define, test, and debug requests with Postman. I'll also highlight important REST API concepts, such as request endpoints, headers, and methods. Along the way, I'll walk through creating an OpenAPI (formerly known as Swagger) file, an industry-standard specification that defines requests to a RESTful API. I'll use this Swagger file to help me create a custom data connector in Flow and Azure Logic Apps; it will also serve as the blueprint as I do the same in Power BI.

This first post focuses on creating a custom SODA connector that makes requests to the service with an application key or token. More importantly, the SODA API<sup>[[2]](#app-token)</sup> supports four common ways applications request access to data to a RESTful service: 1) open access, 2) application key or token, 3) authentication with HTTP Basic, and 4) authentication through the OAuth 2.0 protocol. This means the methods used to creats this custom data connector will apply equally to hundreds of other RESTful APIs, like the U.S. Census Bureau API, Google Sheets, Dropbox, and LinkedIn, just to name a few.

Future posts will build on this custom SODA connector, incorporating OAuth 2.0 authentication methods.

## A Simplified Workflow

Here's my simplified workflow to create custom Power BI and Flow connectors for a REST API:

- [Define, Test, and Debug API Requests](#define-test-and-debug-api-requests)
- [Import Postman Collection in Flow (or Azure Logic Apps)](#import-swagger-in-flow-(azure-logic-apps))
- [(For Power BI only) Use Swagger as a Blueprint](#(for-power-bi-only)-use-swagger-as-a-blueprint)

### Define, Test, and Debug API Requests

Let's say I want to download the latest fire emergency calls in the City of Seattle. On the [dataset documentation page](https://dev.socrata.com/foundry/data.seattle.gov/grwu-wqtk) there is a runtime tool that let's me request and see the results of this dataset right in the browser:

![Emergency calls](/img/main/Emergency Calls.png)

In Postman<sup>[[3]](#postman)</sup>, I create the same request, which returns the same results:

![Emergency Calls Postman](/img/main/Emergency Calls Postman.png)

Making the same request in Power BI using the `Web.Contents`<sup>[[3]](#odata)</sup> function also yeilds the same results:

``` javascript
let
    Source = Json.Document(Web.Contents("https://data.seattle.gov/resource/grwu-wqtk.json")),
    #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", Record.FieldNames(#"Converted to Table"{0}[Column1]))
in
    #"Expanded Column1"
```

![Emergency Calls Power BI](/img/main/Emergency Calls Power BI.png)

But what happens behind the scenes as I make these requests? What information is sent? What other information is returned? I'd like to switch gears and talk about what goes into an API request. Understanding the structure of request and response headers is important since it will figure promintently in future posts when I talk about creating a custom connector using the OAuth 2.0 authentication protocol.

#### Anatomy of an API Request

- Endpoint
- Method
- Headers
- Data (or Body or Message)

My request for fire emergency callss called the `https://data.seattle.gov` endpoint, which is the starting point of an API. Likewise, the starting point for Google Sheets API is `https://sheets.googleapis.com`. The one for LinkedIn is `https://api.linkedin.com`. The endpoint for the Power BI API is `https://api.powerbi.com`.



### Import Postman Collection in Flow (or Azure Logic Apps)

### (For Power BI only) Use Swagger as a Blueprint



Why not start in Power BI?
Convergence of tools on the Power Platform
PowerApps and Azure Logic Apps accept OpenAPI spec files (Swagger files) to define an API and how to coneect to its endpoints
There's a ton of trial-and-error with creating `Web.Contents` calls in Power BI to REST APIs
As I show later, starting with Postman will make debugging `Web.Contents` infinitely easier and save a ton of time in the long run
I've found it's best to start with the documentation at hand and define all the endpoints with Postman
Credentials and authentication => though SODA is open, good developers should adhere to the API providers guidelines. In this case, SODA asks that we register an app for a key so that they can manage the wear on their servers


To recap:
1. Postman helps me understand everything associated with a request and response to any REST API endpoint; request and response headers, data returned by the API, and logs these in a console
2. When creating definitions for Logic Apps and Flow, these expect a JSON-formatted definition Swagger file => it's easy to convert one JSON-formatted definition file to another

HERE'S THE REALLY IMPORTANT PART: if the API I'm connecting to requires a token or key, or needs to authenticate -- particularly through the complex OAuth protocol -- Postman helps me construct and define these steps from soup-to-nuts.

Popular services like LinkedIn, Dropbox, and Google Sheets require authentication via the OAuth 2.0 protocol

MY WORKFLOW:

Define API Calls in Postman --> Transform Postman Collection (JSON) to Swagger (JSON) 
                                                                                      --> Import in Logic Apps/Flow
                                                                                      --> Create Power BI connector


## Connecting to Socrata in PowerApps, Flow, 

## Goal: each blog post should be read as a single unit and be understood

## Outcomes of each post

+ be able to build a connector in Flow, PowerApps, LogicApps, and Power BI
+ drive web traffic to emdata.ai
+ leave them wanting more

## Creating Http requests with Postman

+ What is Postman?
+ Why do I need it?
  + Defines the connector => extremely important later in Flow
+ Can't I just skip this step and go straight to Flow/Power BI/Logic Apps?

## Dugging Http requests with Fiddler

+ What is Fiddler?
+ Why do I need it?
+ What information does it provide?
+ Can't I just skip this step and go straight to Flow/Power BI/Logic Apps?

## Anatomy of an Http request

+ Simple (MetaWeather)
  + Request headers
  + Response headers
  + What it looks like in the Developer Console
  + What it looks like in Postman
  + What it looks like in Fiddler
+ Authenticated
  + API Key (Socrata, NASA)
    + Request headers
    + Response headers
  + OAuth 2.0 (Github)
    + What is OAuth 2.0?
    + Why is it important?
    + [Types of OAuth 2.0 Grant Types](https://auth0.com/docs/api-auth/which-oauth-flow-to-use)
      + Client credentials
      + Authorization code (server-based flow with a callback URL)
      + Resource owner password credentials
      + Implicit (superseded by the Authorization code flow)
      + Extension

### Things to Download
** Postman Collection file
** Swagger definition file
** Flow Custom Connector
** Power BI Custom Connector

### Resources You Should Know
+ <https://www.apimatic.io/transformer>
+ <https://www.jsonschema.net/>
+ <https://www.metaweather.com/api/location/search/?query=washington>

<ol>
<li id="soda">One of my favorite data sources is the Socrata Open Data API, a RESTful API that offers developers tens of thousands of datasets from hundreds of Open Data Network entities like governments, non-profits, and NGOs from around the world. SODA is not only easy to use and understand, but among other things, it also comes with great documentation that lets me interactively request the latest data.</li>
<li id="app-token"><strong>Note</strong>: While the Socrata Open Data API allows for unlimited requests to its service, calls without an application token are <a href= "https://dev.socrata.com/docs/app-tokens.html#throttling-limits">subject to throttling limits</a>. While not absolutely necessary to follow along with this post, you'll need an application token to follow along in future posts. Please do consider <a href="https://dev.socrata.com/docs/app-tokens.html#obtaining-an-application-token">obtaining one here</a>.</li>
<li id="postman">Why Postman? It's the <em>de-facto</em> tool tha helps developers quickly define requests for any given REST API. It requires far less trial-and-error to construct good requests with it than say, using Power BI and Fiddler(link to Chris Webb blog post). And it lets me easily see every bit of information I need about my requests, making it easier to debug my custom SODA connector in Power BI or Flow.

More importantly, Postman creates a JSON-formatted schema that defines endpoints I created for the custom SODA connector, which I then convert to a Swagger definition file. (I talk about converting Postman Collections to Swagger definition files [later](#convert-postman-collection-swagger-definition).)</li></ol>