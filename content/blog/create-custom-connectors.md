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

## Creating a Custom Data Connector to RESTful APIs with the Power Platform
One of my favorite data sources is the Socrata Open Data API (SODA), a RESTful API that offers developers tens of thousands of datasets from hundreds of Open Data Network entities like governments, non-profits, and NGOs from around the world. SODA is not only easy to use and understand, but among other things, also comes with great documentation that lets me interactively request the latest real-world results.

This post is the first in a series that describes the methods and tools I use to create custom data connectors in Microsoft's Power Platform (Power BI, PowerApps, and Flow). I'll show how I use Postman to construct, test, and debug requests with SODA. I'll also highlight important REST API concepts, such as request endpoints, headers, and methods. Along the way, I'll walk through creating an OpenAPI (formerly known as Swagger) file, an industry-standard specification that defines requests to a RESTful API. Swagger is also the chief way to define a custom data connector in Flow. I'll use this Swagger file to help me create a custom SODA connector in Flow; it will also serve as the blueprint as I do the same in Power BI.

Finally, I choose the Socrata Open Data API to show how I build custom data connectors for another important reason. Their service supports four common ways applications request access to data behind a RESTful API: 1) open access, 2) application key or token, 3) authentication with HTTP Basic, and 4) authentication through the OAuth 2.0 protocol. This post focuses on creating a custom SODA connector with an application key or token. Other open data APIs use this method as well, most notably the United States Census Bureau's API. The concepts I present in this post will apply equally to those APIs as well.

Future posts will build on this custom SODA connector, incorporating OAuth 2.0 authentication methods, the same protocol used to authenticate to other popular RESTful APIs, including Dropbox, Google Sheets, and LinkedIn.

## A Simplified Workflow
Here's my simplified workflow to create custom Power BI and Flow (Azure Logic Apps) connectors for a REST API:

**[Define and Test API Requests with Postman](#define-and-test-api-requests-with-postman)
**[Convert Postman Collection Swagger Definition](#convert-postman-collection-swagger-definition)
**[Import Swagger in Flow (Azure Logic Apps)](#import-swagger-in-flow-(azure-logic-apps))
**[(For Power BI only) Use Swagger as a Blueprint](#(for-power-bi-only)-use-swagger-as-a-blueprint)

### Define and Test API Requests with Postman
Postman is the de-facto tool that helps developers quickly define requests for any given REST API. It requires far less trial-and-error to construct good requests with it than say, using Power BI alone. And it lets me easily see every bit of information I need about my requests, making it easier to debug my custom SODA connector in Power BI or Flow (Azure Logic Apps).

Most importantly, Postman creates a JSON-formatted schema that defines endpoints for the REST API, which I can easily convert to a Swagger definition file.

Let's say I wanted to get the latest Fire 911 calls for the City of Seattle. The [dataset page](https://dev.socrata.com/foundry/data.seattle.gov/grwu-wqtk) gives me an inline tool where I can request data from this endpoint:
[Socrata Seattle page reactive cell image here]

In Postman, I create the same request and see the same results:
[Postman screenshot of City of Seattle Fire 911 calls endpoint and results]

What happens behind the scenes when I make a request to an endpoint? What information is sent? What information 

#### Anatomy of an API Request


### Convert Postman Collection Swagger Definition

### Import Swagger in Flow (Azure Logic Apps)

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
