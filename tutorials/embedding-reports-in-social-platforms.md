# Embedding Reports

## Overview

When you publish a report to a Datapane instance, you can embed it into other platforms. 

![](../.gitbook/assets/image%20%28106%29.png)

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

![Screenshot 2020-08-17 at 19.34.11.png](https://storage.googleapis.com/datapane-files-prod/images/Screenshot_2020-08-17_at_19.34.11.width-800.png?Expires=1601557562&GoogleAccessId=gcs-files%40datapane-env-prod.iam.gserviceaccount.com&Signature=hwYXNc3yPaFYYCN9raMOIYFrgTScOAXbRGwol3Fk7pl1QtRWV6%2B7bzjTQ%2BYnC0X4o%2By9PIuRjDE04PsK4IOiaTRYIO4eQK7Cy3FqMXJMsP1T0FpOUsqkKmx%2FUbuwc8SwvCbeYmJf8rjtmcpbx0SUSRvRAaxxy2gHrKS%2F%2B6V5f7M13pK2MTWEmr3Vr1EUnYzJCkhlTTMpoSz5G2nR3c23Y97LCaZ9SxIIHrD8Cz6oj30jjPjZleBEshHEesCPIo6OvKGPpBjq5Ft6AiyY%2FamtQ1l%2FSo%2BDUZux5mvk0Phq4UpkGTgTA3PukfV%2BgHRDSoNIN4%2Bw2U%2FjYtYEBbOEY2EwXQ%3D%3D)

You can use this on any website where you control the underlying HTML code.

### Confluence 

You can embed a Datapane report into Confluence using an iframe, but instead of copying the code tag like you do when embedding in your own website, you need to use Confluence's built-in iframe support.

On your Confluence page, type `/iframe` and the iframe popup will appear.![Screenshot 2020-08-17 at 19.05.58.png](https://storage.googleapis.com/datapane-files-prod/images/Screenshot_2020-08-17_at_19.05.58.width-800.png?Expires=1601557562&GoogleAccessId=gcs-files%40datapane-env-prod.iam.gserviceaccount.com&Signature=aOqyYMEUI1GQHi8yWc4B9%2FwN85UoTRmN0kYRHNdOx%2FdA%2FcBWFQrYXeYRsjWZKD8ucgklnZqyvHwbkVq6GwkLSTgqrmRVJGrh4cKZCQDxo219VcJOy3QeZwk8rJ4A83uK27Y9NQmLEv1H0vTUi1GzOOBGy1b4nnsgPiWCAHys5k7dr%2FZWDNPwmDjIimglA%2F596zXB6SORHdmhHD7PrUrp3u99shmJpOYXLCYvenSGJtlTh75tkRvgzyDGCEmVakN0GlzgLvZGQvjULddQ08XkJE7Lmex3XaIN0lBZ5YSLtkojfn4XPgBkRaXSo07V7eUgz%2FMUePBE%2Bty0zf7Ti7OyaQ%3D%3D)

Similarly to when you are embedding in your own website, when you embed into Confluence, make sure you add `/embed` to the end of your URL, as Confluence does not automatically add it as Notion, Medium, and Reddit do.

Confluence will then display your report or visualization in the page:

![Screenshot 2020-08-17 at 19.11.47.png](https://storage.googleapis.com/datapane-files-prod/images/Screenshot_2020-08-17_at_19.11.47.width-800.png?Expires=1601557562&GoogleAccessId=gcs-files%40datapane-env-prod.iam.gserviceaccount.com&Signature=mqloaYxVnYAfxXTDnSX69IgoIdz8cJGnndcdMXUW%2FAf3cv%2F6dDxXo9JqIAFxEGkS0xBlnOzCSMsfN3jrEQ73ESbum2tdUL%2BysxE0GmFTkDTsM%2FCjeWpxTHxzhfOo%2BtspRyqUtZoLXFEwHWPBQeeT6Y7oqQxoLZl4emCMM4vLQ2LkMk0eU5lG7zRK6JcSmOu8jXwwERS7HIXIBC6L3ocIgquOwDsmk17O7g%2BGDa4vat8MJBEjEn%2B3orck9c4%2BAXpvf7fMoNJhL32KtC%2Fcy%2FLC9SYj%2BLbzNInLkoQq0BOp0XFj55iO6OeI5jfGQrD0tacItS%2Fc2q8tbDJTfRVyOn61aA%3D%3D)

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



