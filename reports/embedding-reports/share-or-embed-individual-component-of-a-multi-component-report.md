---
description: Learn how to embed subsets of your reports via queries
---

# Embed specific blocks

{% hint style="warning" %}
This feature is in beta and may change
{% endhint %}

## Overview

Many times, your report will have different blocks, but you may wish to share only a subset, for instance it's often useful to embed a specific chart or dataset into another platform - especially if you are refreshing it on a cadence. 

Instead of creating a report for each block, Datapane provides an `block_name` query parameter you can add to your report link that allows you to query your report, so you can embed only the relevant blocks.

Here are a few of the most common examples and use-cases you will encounter.

## Querying by Block name

The easiest way to embed only a single block is to assign a `name` to your block when creating it, e.g. `dp.Plot(plot, name="my-plot")`, and then add a query param to your embed URL as follows: 

```text
https://datapane.com/u/user/reports/report-name?block_name=my-plot
```

where `?block_name=my-plot` is a URL param that will extract any report block with the `name` of `my-plot`. Note that your block name cannot contain spaces. 

For instance, the plot in [this report](https://datapane.com/leo/reports/continent_covid_cases) has a name of `block-7`, and so the following query will select just that plot: [https://datapane.com/leo/reports/continent\_covid\_cases/?block\_name=block-7](https://datapane.com/leo/reports/continent_covid_cases/?block_name=block-7)

{% hint style="info" %}
These queries can be applied to embedded reports, e.g.

[https://datapane.com/u/leo/reports/continent-covid-cases/embed/?block\_name=block-7](https://datapane.com/u/leo/reports/continent-covid-cases/embed/?block_name=block-7)
{% endhint %}

## Querying by Block type and index

As mentioned, Reports are comprised of multiple block types, such as `Plot`, `Table`, `Text`, and so on - we can select just blocks of a specific type using [XPath](https://developer.mozilla.org/en-US/docs/Web/XPath), and even select the specific index of a particular block type or a range.

Let's say there are both Table and Plot blocks in your report, and you want to only embed the plots from your report. Simply add `/?blocksquery=//Plot` at the end of the report link. For example, to extract all the plots in [this report](https://datapane.com/u/leo/reports/google-trends/) you can use the following query,

[https://datapane.com/u/leo/reports/google-trends/embed/?blocksquery=//Plot](https://datapane.com/u/leo/reports/google-trends/embed/?blocksquery=//Plot)

If you want to query only the first plot of the report, just add an index to the Plot query, i.e., `/?blocksquery=//Plot[1]`, e.g.

[https://datapane.com/u/leo/reports/google-trends/embed/?blocksquery=//Plot\[1\]](%20https://datapane.com/u/leo/reports/google-trends/embed/?blocksquery=//Plot[1]) will select the first plot, and [https://datapane.com/u/leo/reports/google-trends/embed/?blocksquery=//Plot\[2\]](%20https://datapane.com/u/leo/reports/google-trends/embed/?blocksquery=//Plot[2]) will the second.

{% hint style="info" %}
Querying by Block type will also remove all layout information from the report, making it easier to view when embedded
{% endhint %}

