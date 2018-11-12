+++
author = "Tony McGovern"
categories = ["Dataflows","Power Query M","GitHub"]
tags = ["post"]
date = "2018-11-12"
description = "See a transparent history of changes to Power Query M functions by using GitHub."
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Power Query M Version Control using GitHub"
type = "post"

+++

## Power Query M Version Control using GitHub
There is currently no obvious version control process to see a history of changes made to Power Query M functions within Excel or Power BI. The Advanced Editor in the Power Query Editor has no built-in function to capture changes made to the query. In other words, it's very difficult to roll back queries to earlier versions once they're executed.

I tend to have a fairly involved data preparation process that I build in to my Power Query M functions. It's not uncommon for my queries to [contain hundreds of lines](https://github.com/tonmcg/powersocrata/blob/master/M/Socrata.ReadData.pq). Once they become this large, managing changes to my queries starts to become onerous. It's at this point I often switch from writing Power Query M functions in the Advanced Editor to writing them in a text editor like Notepad++. One of the benefits of separating the functions from the Power BI or Excel file is that I can store the functions in a .pq file on GitHub and [see the entire timeline of changes made to the file](https://github.com/tonmcg/powersocrata/commit/52fd8905c9cbe4d1c7143dd61264af0c6f7b3a50#diff-64eaca6fc062ee30a8d65f09d2beb4aa). Storing .pq files on GitHub becomes especially [useful in collaborative envrionments](https://guides.github.com/introduction/git-handbook/) where team members work on the same set of Power Query M functions. And even if you're working by yourself, GitHub is still useful for those projects that contain a number of queries, like this library of M functions that [prepare data from the Socrata Open Data API](https://github.com/tonmcg/powersocrata), for example.

So if I'm not writing these functions in the Advanced Editor, how does the Power Query Editor know they exist? How do we execute Power Query M functions that reside somewhere else?

### Expression.Evaluate() and the `#shared` Environment
Consider the Power Query M code below:

```
let
  Source = 
    Expression.Evaluate(
      Text.FromBinary(
        Web.Contents(
          "https://raw.githubusercontent.com/tonmcg/powersocrata/master/M/Socrata.ReadData.pq"
        )
      ),
      #shared
    )
in
  Source
```

If we copy the code into the Advanced Editor, we should see the following function appear:
![Excel](/img/main/Excel_GitHub_Query.png)

The screenshot shows the Power Query Editor rendering a function with multiple parameters. Not anywhere in the code above have we defined parameters. So where did they come from? The answer is I created a custom Power Query M function called `Socrata.ReadData()` and stored it in a repository on GitHub. I'm using the `Web.Contents()` function to return the [contents of the file](https://raw.githubusercontent.com/tonmcg/powersocrata/master/M/Socrata.ReadData.pq) and then the `Text.FromBinary()` function to render the text contained within. The parameters are defined within the function itself, which again, is stored on GitHub.

The most interesting part of the function is the use of `Expression.Evaluate()`. At its simplest, it [evaluates text and returns an evaluated value](https://docs.microsoft.com/en-us/powerquery-m/expression-evaluate). In our case, the evaluated text is the text within the `Socrata.ReadData()` function. The evaluated value is the actual function itself. Put another way, `Expression.Evaluate()` allows us to make a call to GitHub and return the function's contents, render the text of the function, and tell the Power Query engine to treat the rendered text as an M function.

But that's only half of the answer. While the first parameter of the `Expression.Evaluate()` function requires an expression as text, the second parameter asks for the *environment* in which the text resides. In our example, I've defined the *envrionment* as the `#shared` variable. This pattern is what allows a Power Query M function that resides on GitHub to be executed in our Power Query Editor.

I'll not go into it here, but if you're interested in understanding the *environment* parameter and the `#shared` variable, here is a list of resources that should get you started:

1. Lars Schreiber and Imke Feldmann have done [incredible work simplifying these concepts](https://ssbi-blog.de/technical-topics-english/the-environment-concept-in-m-for-power-query-and-power-bi-desktop-part-3/)
2. Chris Webb provides simple examples that help [motivate understanding in this area](https://blog.crossjoin.co.uk/2015/02/06/expression-evaluate-in-power-querym/)
3. Finally, Imke Feldmann uses this pattern often and provides numerous examples on her blog. Here's [one example](https://www.thebiccountant.com/2018/05/17/automatically-create-function-record-for-expression-evaluate-in-power-bi-and-power-query/).

### Making it Work in Power BI Dataflows
The most exciting part, however, is that this pattern works not only in Excel and PowerBI<sup id="a1">[[1]](#warning)</sup>, but through the browser as well [with Power BI Dataflows](https://www.tonymcgovern.com/blog/power-bi-dataflows/). We can get the `Socrata.ReadData()` function to work in dataflows by using the Power Query M code above in the Power BI dataflows Query Editor:
![Dataflows](/img/main/Dataflows_GitHub_Query.png)


If you want to see what this looks like in action, I [tweeted a GIF of this working](https://twitter.com/tonmcg/status/1060617265501126656) in Power BI dataflows.

There are boundless opportunities here to use GitHub's version control system to manage the entire timeline of changes made to projects that use Power Query M functions. Distributed teams around the world can simultaneously work on the code, understand what changes have been made, and collaborate at any time while maintaining source code integrity.

Do you use this pattern as well? What else have you found? Let me know in the comments section below.

<ol><li id="warning"><b>Be aware</b>: the use of the <code>#shared</code> variable within the <code>Expression.Evaluate()</code> function is not currently a supported feature, which means it can <a href="https://ssbi-blog.de/technical-topics-english/the-environment-concept-in-m-for-power-query-and-power-bi-desktop-part-3/#comment-134"> go away at any time</a>.</li></ol>