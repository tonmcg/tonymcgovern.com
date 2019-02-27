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
title = "Using Postman to Create a Custom Connection to Socrata on the Power Platform"
type = "post"

+++

# 
Posts: Socrata no auth -> Socrata app token -> (Socrata) Oauth 2.0

## Connecting to REST APIs with the Power Platform
Microsoft's Power Platform tools (Power BI, PowerApps, Flow) each allow me to connect to a variety of data sources on the web. One of my favorite data sources is the Socrata Open Data API (SODA), a RESTful API that offers developers tens of thousands of datasets from hundreds of Open Data Network entities like governments, non-profits, and NGOs from around the world. Among other things, SODA provides an API that's not only easy to use and understand, but also comes with great documentation that lets me interactively request real-world results.

This post is the first in a series that describes the methods and tools I use to create custom connectors in Power BI and Microsoft Flow (and by extension, Azure Logic Apps). I'll use SODA to illustrate important REST API concepts, such as requests endpoints, headers, methods, and body (or data). I'll also try to convince you that

My choice of SODA is important for another reason. Their API supports three of the most common ways developers access data through REST: 1) open access, 2) access keys, and 3) authorization tokens through the OAuth 2.0 protocol. This post focuses on creating custom connectors on REST APIs with open access. Future posts will focus on REST APIs that require the use of an access key and those that authenticate through the various OAuth 2.0 methods.

## A Simplified Workflow
Here's my simplified workflow to create custom Power BI and Flow (Azure Logice Apps) connectors for any REST API:

- [Define and Test API Requests with Postman](define-and-test-api-requests-with-postman)
- Convert Postman Collection JSON to OpenAPI (Swagger) Definition JSON
- Import Swagger to Flow (or Azure Logic Apps) as a Custom Connector
- (For Power BI only) Use Swagger as a Blueprint for a Custom Connector

### Define and Test API Requests with Postman
A quick note on Postman: it is the de-facto tool that helps developers quickly define requests for any given REST API. It requires far less trial-and-error to construct good requests with it than say, using Power BI alone. And it lets me easily see **all** the results of my requests, making it easy to debug my custom connectors in Power BI or Flow (Azure Logic Apps).

Above all, Postman creates a JSON-formatted schema that defines endpoints for the REST API, which I easily convert to a Swagger definition file. Swagger files are the chief way to define a custom connector in Flow (Azure Logic Apps).




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

## Resources You Should Know

+ <https://www.jsonschema.net/>
+ <https://www.metaweather.com/api/location/search/?query=washington>
