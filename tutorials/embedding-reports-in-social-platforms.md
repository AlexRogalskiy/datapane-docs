# Embedding Reports

## Overview

When you publish a report to a Datapane instance, you can embed it into other platforms. 

![](../.gitbook/assets/image%20%28107%29.png)

Datapane reports can be embedded into anywhere that supports iframes or oembed, including the following third-party tools:

* Notion
* Confluence
* Coda
* Reddit
* Medium
* Salesforce
* Your own webpage

## Business Tooling

### Notion

[Example ](https://www.notion.so/datapane/Google-Trends-Dashboard-929a1cd8612c4983ab10e2eaaeccb339)

If you are already using Notion in your company, and you're comfortable analyzing data using Python, you might not need to buy a clunky BI tool. Instead, you can generate interactive, dynamic reports in Python and embed them straight into your Notion documents.

To embed the chart in Notion, just paste the link into any page. A menu will pop up asking you **Create Embed**. If you click this, Notion will embed your report inside your page.

### Your website

You can embed into your own website using an iframe. After publishing your report, if you browse to it's page on Datapane.com, and select **Embed** in the share menu, you will be able to copy and paste a snippet of code to embed it into your page.

You can use this on any website where you control the underlying HTML code.

### Confluence 

You can embed a Datapane report into Confluence using an iframe, but instead of copying the code tag like you do when embedding in your own website, you need to use Confluence's built-in iframe support.

On your Confluence page, type `/iframe` and the iframe popup will appear.

![](../.gitbook/assets/image%20%28100%29.png)

Similarly to when you are embedding in your own website, when you embed into Confluence, make sure you add `/embed` to the end of your URL, as Confluence does not automatically add it as Notion, Medium, and Reddit do.

Confluence will then display your report or visualization in the page:

![](../.gitbook/assets/image%20%28110%29.png)

## Social Integrations

### Medium

[Example ](https://medium.com/@leo_26134/embedding-with-datapane-366e60434b5f)

To embed a report on Medium, publish it to Datapane, and copy and paste the full URL of your report into your Medium editor. Medium will automatically create the embed, and you will see the following. In edit mode, your embed will not be interactive - you must publish the post first to use interactive elements.

![Edit mode](../.gitbook/assets/image%20%2893%29.png)

![Interactive plot in view mode](../.gitbook/assets/image%20%2892%29.png)

### Reddit

[Example](https://old.reddit.com/r/dataisbeautiful/comments/h7nspg/oc_when_the_bookies_were_wrong_premier_league/)

To embed a report into reddit, create a post with your report's main URL. If your report does not appear inline at first, click the **Retry Thumb** button to prompt Reddit to reload it.

In the post view, your report will be able to be used interactively.

![](../.gitbook/assets/image%20%2894%29.png)

## Extracting a specific component 

It's often useful to embed a specific chart or dataset into another platform, especially if you are refreshing it on a cadence. To allow this, Datapane provides an `blocksquery` parameter which allows you to query your report using [XPath](https://developer.mozilla.org/en-US/docs/Web/XPath). A report is comprised of the following components which can be queried using XPath:

* `Plot`
* `Table`
* `Text`

A common method is to query the components using an index. For instance, in order to extract the first plot of [this report](https://datapane.com/leo/reports/continent_covid_cases), you can use the following query: [https://datapane.com/leo/reports/continent\_covid\_cases/?xpath=//Plot\[1\]](https://datapane.com/leo/reports/continent_covid_cases/?blocksquery=//Plot[1])

For more information, see the relevant documentation:

{% page-ref page="../reference/reports/share-or-embed-individual-component-of-a-multi-component-report.md" %}



