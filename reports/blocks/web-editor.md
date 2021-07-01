---
description: How to build a Report on the web
---

# Web Editor

One of the challenges with building reports in Python is working with long chunks of text. To make this easier, we've introduced a Web Editor, where you can build rich long-form reports for articles or tutorials. 

To get started, create a report through the [Home](https://datapane.com/home/) or [My Reports](https://datapane.com/my-reports/) pages, and choose the 'Create Markdown Report' option. 

{% hint style="info" %}
You'll need to be logged into Datapane.com or your Teams instance to use this feature - it is not currently supported on the open-source version. 
{% endhint %}

## Writing your first report

You can get started writing straight away with Markdown  - check out [this cheat sheet](https://www.markdownguide.org/cheat-sheet/) for more information or use the formatting icons at the top of the text area. 

For example, try the following Markdown in the Text editor:  

```text
# This is a heading
**This is bold text**
*This is text in italics*
- These
- are
1. list
2. items
> This is a quote
[Click me](thisisalink.com)
```

This will generate a report which looks as follows: 

{% embed url="https://datapane.com/u/johnmicahreid/reports/testing-datatable-column-sort/" %}

## Inserting blocks

Aside from Markdown, you can enrich your report in several ways: 

**Datapane Blocks**

These are special blocks which change the look and feel of your report. You can insert them directly into your report from the editor, for more information check out the pages about them:  

{% page-ref page="text-code-and-html.md" %}

{% page-ref page="layout-pages-and-selects.md" %}

**Python** **assets**

You can push up assets to your Markdown report by using the report `ID` or report `name` . For example, say I already 

{% code title="hello.sh" %}
```bash
# Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```
{% endcode %}

Once I run this code and refresh the asset list on the right, I'll see the following: 



{% page-ref page="plots-and-visualizations.md" %}



## Previewing and saving

{% hint style="warning" %}
Make sure to save your report before previewing or sharing it, otherwise you might lose your work! 
{% endhint %}



