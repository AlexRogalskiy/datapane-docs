---
description: Laying out your report to make it more readable and understandable
---

# Layout, Pages, and Selects

## Overview

Although you are you probably analyzing data in a list of sequential steps, it may not the best way to present results -- especially if you are sharing with people who are used to a traditional dashboard or BI tool. 

Datapane's report library provides components which allow for more flexible grid-style layouts, multiple pages, tabs, and selects, which allows you to create powerful custom interfaces without knowledge of HTML or CSS.

## Grid layouts

If you pass a list of blocks \(such as `Plot` and `Table`\) to a Report, they are -- by default -- laid out in a single column with a row per block. If you would like to customize the rows and columns, Datapane provides a `Group` component which takes a list of blocks and a number of columns and/or rows and lays them out in a grid.

If we take the example in the earlier tutorial, but want to lay the plot and dataset side-by-side, we can use specify this using `Group` and specifying the number of columns.

{% code title="simple\_report.py" %}
```python
import pandas as pd
import altair as alt
import datapane as dp

dataset = pd.read_csv('https://covid.ourworldindata.org/data/owid-covid-data.csv')
df = dataset.groupby(['continent', 'date'])['new_cases_smoothed_per_million'].mean().reset_index()

plot = alt.Chart(df).mark_area(opacity=0.4, stroke='black').encode(
    x='date:T',
    y=alt.Y('new_cases_smoothed_per_million:Q', stack=None),
    color=alt.Color('continent:N', scale=alt.Scale(scheme='set1')),
    tooltip='continent:N'
).interactive().properties(width='container')

dp.Report(
    dp.Group(
        dp.Plot(plot), 
        dp.Table(df),
        columns=2
    )
).publish(name='covid_report', open=True)
```
{% endcode %}

![](../../.gitbook/assets/image%20%28104%29.png)

You can also find an example [here](https://datapane.com/u/leo/reports/dp-docs-layout/).

{% hint style="info" %}
As `Group` components are components themselves, they are composable, and you can create more custom layers of nested blocks, for instance nesting 2 rows in the left column of a 2 column layout
{% endhint %}

### Customizing width

By default, reports appear in a portrait orientation. This works well for many reports, but may not be desired if you have multiple columns of blocks. You can opt to make your report full-width by setting the `full_width` property to `True` on the `Report` object. This allows the creation of rich, dashboard-like layouts such as the following:  


![](../../.gitbook/assets/image%20%28106%29.png)

## Pages 

Reports on Datapane can have multiple pages, which are presented to users as tabs at the top of your report. These can be used similarly to sheets in an Excel document.

To add a page, use the `dp.Page` block at the top-level of your report, and give it a title with the `label` parameter.

{% hint style="info" %}
Pages cannot be nested, and can only exist at the root level of your `dp.Report` object
{% endhint %}

```python
import datapane as dp
...

dp.Report(
    dp.Page(
        blocks=[
            dp.Group(md_block, md_block, columns=2),
            dp.Select(blocks=[md_block, group], type=dp.SelectType.DROPDOWN),
        ],
        label="Page Uno",
    ),
    dp.Page(
        blocks=[
            dp.Group(select, select, columns=2),
            dp.Select(blocks=[md_block, md_block, md_block], type=dp.SelectType.TABS),
        ],
        label="Page Duo",
    )
)
```

## Tabs and Selects

In addition to top-level pages elements, you can include tabs and dropdown selects inside your reports. This allows users to switch between multiple blocks interactively and allows the creation of very interactive reports. It is also useful for providing background context to another block - for instance, to add source code to a plot or a dataset.

Datapane provides two select options on the `dp.Select` block: drop downs and tabs. Tabs are recommended if you have only a few choices and dropdowns are recommended for selects which have more than five options.

```python
import datapane as dp

md_two = dp.Text("""
# This is our first block
""", label="First text block")

md_two = dp.Text("""
# This is our second block
""", label="Second text block")

tab_report = dp.Report(
    dp.Select(
        blocks=[
          md_one,
          md_two
        ], 
        type=dp.SelectType.dp.SelectType.DROPDOWN
    )
)

tab_report = dp.Report(
    dp.Select(
        blocks=[
          md_one,
          md_two
        ], 
        type=dp.SelectType.dp.SelectType.TABS
    )
)
```

