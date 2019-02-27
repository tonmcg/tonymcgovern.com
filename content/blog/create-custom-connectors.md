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
title = "Using Postman to Create a Custom Connection"
type = "post"

+++

#
Posts: Socrata no auth -> Socrata app token -> (Socrata) Oauth 2.0

## Connecting to Socrata Open Data Network
love Socrata Open Data Network
free, easy to use, deep and rich datasets, great documentation, no authentication

Discovery Metadata API
is a RESTful web service [link to why this is important]
tells me what's in the Open Data Network [link to what's her name's Shiny app]
each dataset provides a title, description, and tags useful for discovery and text analysis
I've performed Azure Cognitive Services TExt Analytics to find key phrases in each dataset
contains download and usage information on each dataset
endless possibilities with this API
equally as important, their documentation is first rate

Workflow: Define, Test, Document REST API in Postman -> Convert to Swagger -> Import/Recreate

Why not start in Power BI?
Convergence of tools on the Power Platform
PowerApps and Azure Logic Apps accept OpenAPI spec files (Swagger files) to define an API and how to coneect to its endpoints
There's a ton of trial-and-error with creating `Web.Contents` calls in Power BI to REST APIs
As I show later, starting with Postman will make debugging `Web.Contents` infinitely easier and save a ton of time in the long run
I've found it's best to start with the documentation at hand and define all the endpoints with Postman
Credentials and authentication => though SODA is open, good developers should adhere to the API providers guidelines. In this case, SODA asks that we register an app for a key so that they can manage the wear on their servers


Why Postman?
de-facto tool to help interact, understand, and define how to connect to REST APIs
extremely intuitive, lets you define variables at different levels, I can test endpoints and be able to get and set all request and response information are readily available
less trial-and-error
gives you response codes as well
can simultaneously create connections in Power BI, inspect the request and response headers in the Postman console, and verify the results
see the results and understand what to expect in Power BI

When you're satisfied with the results, 
Postman creates a JSON-formatted schema that defines endpoints in your API
When building a custom connector in in Azure Logic Apps and Flow, this definition can easily be converted to another JSON-formatted schema => an OpenAPI (formerly Swagger) file

Postman can even import already-defined Swagger files and to be used a Postman Collections

Swagger files are the chief way to define a custom connector in Azure Logic Apps and Flow

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

## Resources You Should Know

+ <https://www.jsonschema.net/>
+ <https://www.metaweather.com/api/location/search/?query=washington>
