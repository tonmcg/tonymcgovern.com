+++
author = "Tony McGovern"
categories = ["Power Query M"]
tags = ["post"]
date = "2019-01-16"
description = "Using error tables in Excel and Power BI to handle unsuccessful HTTP Requests."
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Handling Unsuccessful HTTP Requests in Power Query M"
type = "post"

+++

Many web APIs implement a standard set of HTTP response codes that indicate whether a request was successful. A request that returns expected data often comes with a successful `200` response code. Perhaps the most well-known response code is `404`, which tells us a web page doesn't exist. When I request data using the `Web.Contents` Power Query M function in Excel or Power BI and that request is unsuccessful, I can often make sense of the response code that comes with it to quickly fix my query. Most other times, I encounter unfamiliar response codes that take me far too long to resolve, if I resolve them at all.

[Inspired by a thread on a Power BI User Group forum](https://www.pbiusergroup.com/powerbiug/communities/community-home/digestviewer/viewthread?MessageKey=51aaec6c-1071-4948-9549-59476ba1dfd8), I'd like to share an error handling pattern I've recently adopted  that helps me quickly resolve unsuccessful HTTP requests using `Web.Contents`. This pattern consists of two parts: a) using the ManualStatusHandling option in the `Web.Contents` function to catch likely HTTP response codes, and b) creating error tables that explain to the user the likely reasons an HTTP request fails. With this error handling pattern in place, my queries are now more resilient and provide a better user experience. 

So how does it work? Suppose you're interested in the latest population estimates for every state in the United States from the [U.S. Census Bureau's API](https://www.census.gov/data/developers/data-sets.html). A successful request returns the expected data along with a `200` response code:

``` javascript
let
    response = Web.Contents(
        "https://api.census.gov/data/2018/pep/population?get=POP&for=state"
    ),
    json = Json.Document(response)
in
    json
```

If I misspell the URL and instead send this request:

``` javascript
let
    response = Web.Contents(
        "https://api.census.gov/data/2018/BADSPEL/pupuracion?get=POP&for=state"
    ),
    json = Json.Document(response)
in
    json
```

I get a [`404` response code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404), which tells me the page does not exist.
![404](/img/main/404_Response_Code.png)

But what happens when I misspell another part of the URL instead, say one of the parameters?

``` javascript
let
    response = Web.Contents(
        "https://api.census.gov/data/2018/pep/population?get=POP&for=states"
    ),
    json = Json.Document(response)
in
    json
```

I now get a [`400` response code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/400):
![400](/img/main/400_Response_Code.png)


Why is this? Well, the first unsuccessful query told me I misspelled the location of the resource (the portion of the URL before the '?') whereas the second unsuccessful query told me I misspelled one of the paramters in the URL. While this is useful information for me, the Power Query M developer, these sorts of subtleties are lost on an end-user. It's likely your user won't care to know the difference between a `400` and `404` response code; they just want to know why thier request failed. (There's an extra 's' in the word 'state', in case you were wondering why the second request failed.)

So how does this relate to error tables? Like most well-documented APIs, the U.S. Census Bureau API has a page devoted to listing and describing all the [possible response codes that can be returned by their service](https://factfinder.census.gov/service/ErrorCodes.html). I take this information and build an internal table within the query that defines and describes these response codes in my own words. I'm now able to throw custom messages that make the difference between a `400` response code and a `404` response code more obvious.

For example, in the code below, I use the `Error.Record` function to create individual records that allow me to catch these unsuccessful requests and throw my own custom error messages to the user. I then create an extra field in each record called 'Status', which maps each HTTP response code returned by the API to a corresponding error message of my choosing:

``` javascript
let
    // define possible errors
    errors.badRequest = Record.AddField(Error.Record("Bad Request", "this dataset does not exist. It's most likely a misspelled parameter. Double check the spelling of the geography and variables you're passing as parameters."), "Status", 400),
    errors.invalidAccessKey = Record.AddField(Error.Record("Invalid Access Key", "Make sure the provided access exists and is entered correctly."), "Status", 401),
    errors.insufficientPermissions = Record.AddField(Error.Record("Insufficient Permissions", "you don't have the right permissions to access this dataset."), "Status", 403),
    errors.resourceNotFound = Record.AddField(Error.Record("Page Does Not Exist", "this dataset does not exist. It's most likely a misspelling somewhere in the URL. Double check the URL and make sure it's spelled correctly."), "Status", 404),
    errors.tooManyRequests = Record.AddField(Error.Record("Too Many Requests", "you've reached your daily limit. Come back in 24 hours and try again."), "Status", 429),
    errors.backendError = Record.AddField(Error.Record("Server Error", "there is an unknown server error preventing this request from being executed. You've done nothing wrong. Just try again in a bit."), "Status", 500),
    errors.serviceUnavailable = Record.AddField(Error.Record("Service Unavailable", "the server is too busy right now and needs a short break. Don't worry, you can try again in a bit."), "Status", 503),
    // build an error table
    errors.table = Table.FromRecords({errors.badRequest, errors.invalidAccessKey, errors.insufficientPermissions, errors.resourceNotFound, errors.tooManyRequests, errors.backendError, errors.serviceUnavailable})
in
    errors.table
```
![ErrorTable](/img/main/Error_Table.png)

I then combine this with the ManualStatusHandling option in `Web.Contents`, where I list all the HTTP response codes provided by the API. So, if the API returns one of those response codes, ManualStatusHandling tells my request to fail silently, thus allowing me to trap the unsuccessful request in a subsequent step. Chris Webb has a [great explainer on what the ManualStatusHandling option does and provides examples on how to use it](https://blog.crossjoin.co.uk/2016/08/09/handling-404-not-found-errors-with-web-contents-in-power-query-and-power-bi). 

With both an error table and the ManualStatusHandling option implemented, my query now handles unsuccessful API requests. Here's an example where I throw a custom error message to handle a `404` response code (the 'state' parameter still has that extra 's'):

``` javascript
let
    // define possible errors
    errors.badRequest = Record.AddField(Error.Record("Bad Request", "this dataset does not exist. It's most likely a misspelled parameter. Double check the spelling of the geography and variables you're passing as parameters."), "Status", 400),
    errors.invalidAccessKey = Record.AddField(Error.Record("Invalid Access Key", "Make sure the provided access exists and is entered correctly."), "Status", 401),
    errors.insufficientPermissions = Record.AddField(Error.Record("Insufficient Permissions", "you don't have the right permissions to access this dataset."), "Status", 403),
    errors.resourceNotFound = Record.AddField(Error.Record("Page Does Not Exist", "this dataset does not exist. It's most likely a misspelling somewhere in the URL. Double check the URL and make sure it's spelled correctly."), "Status", 404),
    errors.tooManyRequests = Record.AddField(Error.Record("Too Many Requests", "you've reached your daily limit. Come back in 24 hours and try again."), "Status", 429),
    errors.backendError = Record.AddField(Error.Record("Server Error", "there is an unknown server error preventing this request from being executed. You've done nothing wrong. Just try again in a bit."), "Status", 500),
    errors.serviceUnavailable = Record.AddField(Error.Record("Service Unavailable", "the server is too busy right now and needs a short break. Don't worry, you can try again in a bit."), "Status", 503),
    // build an error table
    errors.table = Table.FromRecords({errors.badRequest, errors.invalidAccessKey, errors.insufficientPermissions, errors.resourceNotFound, errors.tooManyRequests, errors.backendError, errors.serviceUnavailable}),
    // get response
    response = Web.Contents(
        "https://api.census.gov/data/2018/pep/population?get=POP&for=states", 
        [
            ManualStatusHandling = {400,401,403,404,429,500,503}
        ]
    ),
    responseMetadata = Value.Metadata(response),
    responseCode = responseMetadata[Response.Status],
    responseHeaders = responseMetadata[Headers],
    json = if responseCode <> 200 then error errors.table{List.PositionOf(errors.table[Status],responseCode)} else Json.Document(response)
in
    json
```
![ErrorMessage](/img/main/Custom_Error_Message.png)

This error handling pattern provides a more resilient query that catches most HTTP response codes I'll likely encounter. Moreover, I get to describe these responses in my own words, allowing me to better communicate to my users unsuccessful `Web.Contents` requests.

What error handling techniques do you use in Power Query M? Comment below -- I'm interested to hear about your patterns.