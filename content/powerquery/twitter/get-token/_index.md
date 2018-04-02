+++
title = "Get Twitter OAuth Token (OAuth 2.0 Method)"
date = "2018-04-02"
+++

## Prerequisites
1. Sign up for a [Twitter account](http://twitter.com/signup)
2. Sign in to the [Twitter Apps page](https://apps.twitter.com) and create a new application
3. Get your Consumer Key and Consumer Secret
4. Create `consumer_key` and `consumer_secret` variables as queries or global parameters and assign your Consumer Key and Consumer Secret to them, respectively

## Get Twitter OAuth Token
```javascript
let
    // load custom GetOAuthToken function from Github
    GetOAuthToken = Expression.Evaluate(
        Text.FromBinary(Web.Contents("http://bit.ly/GetOAuth")),
        [
            #"Binary.ToText" = Binary.ToText,
            #"Json.Document" = Json.Document,
            #"Text.ToBinary" = Text.ToBinary,
            #"Web.Contents" = Web.Contents
        ]
    )
in
    GetOAuthToken
```
<pre>bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX</pre>

The OAuth token is masked here for privacy. Your token should be alphanumeric, consisting of numerous letters and numbers preceded by "bearer".

## References
1. [Get Data from Twitter API](https://chris.koester.io/index.php/2015/07/16/get-data-from-twitter-api-with-power-query/) by Chris Koester
2. [Twitter.GetOAuthToken](https://github.com/tonmcg/powertweet/blob/master/M/Twitter.GetOAuthToken.pq) by Tony McGovern