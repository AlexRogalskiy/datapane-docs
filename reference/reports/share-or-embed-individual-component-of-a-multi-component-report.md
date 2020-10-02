# Querying Components

## Overview

Many times, your report will have different components, but you may wish to embed or share only a subset. Instead of creating a report for each component, you can extract the relevant components by specifying a query in your report URL \(`blocksquery=`\). 

{% hint style="info" %}
This functionality uses XPath to query your report. In addition to the uses below, you can use any valid XPath query.
{% endhint %}

## Component Types

There are four component types you can specify in the link to access different components

* `Table` : Extract dataframe 
* `Plot` : Extract plots
* `Text` : Extract markdown
* `File` : Extract files or images

## Querying a specific type of component 

Let's say there are both Markdown and Plot components in your report, and you want to share the plots in your report. Simply add `/?blocksquery=//Plot` at the end of the report link.

For example, to extract the plots in this report: [https://datapane.com/ryancahildebrandt/reports/movies\_dashboard\_3496f91c/](https://datapane.com/ryancahildebrandt/reports/movies_dashboard_3496f91c/)

![](../../.gitbook/assets/screenshot-from-2020-08-10-11-53-16.png)

we add `/?blocksquery=//Plot`at the end of the link like this: 

[https://datapane.com/ryancahildebrandt/reports/movies\_dashboard\_3496f91c/embed/?blocksquery=//Plot](https://datapane.com/ryancahildebrandt/reports/movies_dashboard_3496f91c/embed/?blocksquery=//Plot). 

Now we will just see the plot components:

![](../../.gitbook/assets/screenshot-from-2020-08-10-11-54-42.png)

## Access a Specific Component

If you want to solely include the first plot of the report, add `/?blocksquery=//Plot[1]` at the end of the link. For example, this link [https://datapane.com/ryancahildebrandt/reports/movies\_dashboard\_3496f91c/?blocksquery=//Plot\[1\]](https://datapane.com/ryancahildebrandt/reports/movies_dashboard_3496f91c/embed/?blocksquery=//Plot[1]) will give you the first plot

![](../../.gitbook/assets/screenshot-from-2020-08-10-12-01-17.png)

And this [https://datapane.com/ryancahildebrandt/reports/movies\_dashboard\_3496f91c/?blocksquery=//Plot\[2\]](https://datapane.com/ryancahildebrandt/reports/movies_dashboard_3496f91c/embed/?blocksquery=//Plot[2]) will give you the second plot

![](../../.gitbook/assets/screenshot-from-2020-08-10-12-01-33.png)



