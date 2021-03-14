---
description: Embed your reports across the web and in internal tools
---

# Embedding Reports

## Overview

When you publish a report to a Datapane instance, you can embed it into other platforms. 

![](../../.gitbook/assets/image%20%28107%29.png)

Datapane reports can be embedded into any websites that supports either HTML `iframes` or the [oEmbed](https://oembed.com/) protocol. Sites that support oEmbed allow you to just paste the link to the report, and will handle the embedding automatically. Sites that support iframes, including your own websites and pages, simply require you to copy and paste the embed code from your report page on Datapane. You can find examples of embedded reports throughout these docs.

We support the following third-party sites, and any others that are compatible with either oEmbed or HTML iframes.

| Site | Embed Type |
| :--- | :--- |
| Notion | oEmbed |
| Confluence | HTML iframe |
| Coda | oEmbed |
| Reddit | oEmbed |
| Medium | oEmbed |
| Salesforce | HTML iframe |
| Personal / Internal sites and pages | HTML iframe |

## Business Integrations

### Notion

[Example ](https://www.notion.so/datapane/Google-Trends-Dashboard-929a1cd8612c4983ab10e2eaaeccb339)

If you are already using Notion in your company, and you're comfortable analyzing data using Python, you might not need to buy a clunky BI tool. Instead, you can generate interactive, dynamic reports in Python and embed them straight into your Notion documents.

To embed the chart in Notion, just paste the link into any page. A menu will pop up asking you **Create Embed**. If you click this, Notion will embed your report inside your page.

### Confluence 

You can embed a Datapane report into Confluence using an iframe, but instead of copying the code tag like you do when embedding in your own website, you need to use Confluence's built-in iframe support.

On your Confluence page, type `/iframe` and the iframe popup will appear.

![](../../.gitbook/assets/image%20%28100%29.png)

Similarly to when you are embedding in your own website, when you embed into Confluence, make sure you add `/embed` to the end of your URL, as Confluence does not automatically add it as Notion, Medium, and Reddit do.

Confluence will then display your report or visualization in the page:

![](../../.gitbook/assets/image%20%28110%29.png)

### Personal / Internal / Company websites

You can embed into your own website using an iframe. After publishing your report, and select **Embed** in the share menu on the report. This will provide a simple piece of HTML code that you can copy and paste into your webpage.

Datapane Enterprise also provides the ability for private reports, which can be securely embedded in your own tools to make use of Datapane reports across your applications.

{% hint style="info" %}
You can use this method on any website where you control the underlying HTML code.
{% endhint %}

## Social Integrations

### Medium

[Example ](https://medium.com/@leo_26134/embedding-with-datapane-366e60434b5f)

To embed a report on Medium, publish it to Datapane, and copy and paste the full URL of your report into your Medium editor. Medium will automatically create the embed, and you will see the following. In edit mode, your embed will not be interactive - you must publish the post first to use interactive elements.

![Edit mode](../../.gitbook/assets/image%20%2893%29.png)

![Interactive plot in view mode](../../.gitbook/assets/image%20%2892%29.png)

### Reddit

[Example](https://old.reddit.com/r/dataisbeautiful/comments/h7nspg/oc_when_the_bookies_were_wrong_premier_league/)

To embed a report into reddit, create a post with your report's main URL. If your report does not appear inline at first, click the **Retry Thumb** button to prompt Reddit to reload it.

In the post view, your report will be able to be used interactively.

![Datapane Report embedded in Reddit](../../.gitbook/assets/image%20%2894%29.png)

