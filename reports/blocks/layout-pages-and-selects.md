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

{% tabs %}
{% tab title="Python" %}
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
        dp.DataTable(df),
        columns=2
    )
).upload(name='covid_report', open=True)
```
{% endcode %}
{% endtab %}

{% tab title="Web Report" %}
```python
# Python Code

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

dp.TextReport(
    dp.Plot(plot), 
    dp.DataTable(df)
).upload(name='covid_report')


# Markdown 

"""
```datapane
block: Group
columns: 2
blocks: 
  - block: asset
    label: plot-1
  - block: asset
    label: table-2
```
"""
```
{% endtab %}
{% endtabs %}

![](../../.gitbook/assets/image%20%28104%29.png)

You can also find an example [here](https://datapane.com/u/leo/reports/dp-docs-layout/).

{% hint style="info" %}
As `Group`components are components themselves, they are composable, and you can create more custom layers of nested blocks, for instance nesting 2 rows in the left column of a 2 column layout
{% endhint %}

### Customizing width

To customize the width of your report, you can set the [report type](./#report-types).

## Pages 

Reports on Datapane can have multiple pages, which are presented to users as tabs at the top of your report. These can be used similarly to sheets in an Excel document.

To add a page, use the `dp.Page` block at the top-level of your report, and give it a title with the `title` parameter.

{% hint style="info" %}
Pages cannot be nested, and can only exist at the root level of your `dp.Report` object
{% endhint %}

{% tabs %}
{% tab title="Python" %}
```python
import seaborn as sns
import altair as alt 
import datapane as dp

titanic = sns.load_dataset("titanic")

points = alt.Chart(titanic).mark_point().encode(
    x='age:Q',
    color='class:N',
    y='fare:Q',
).interactive().properties(width='container')

dp.Report(
  dp.Page(
    title="Titanic Dataset",
    blocks=["### Dataset", titanic]
  ),
  dp.Page(
    title="Titanic Plot",
    blocks=["### Plot", points]
  )
).upload(name='altair_example_pages')

```
{% endtab %}

{% tab title="Web Report" %}
```python
# Python Code
import altair as alt 
import datapane as dp

titanic = sns.load_dataset("titanic")

points = alt.Chart(titanic).mark_point().encode(
    x='age:Q',
    color='class:N',
    y='fare:Q',
).interactive().properties(width='container')

dp.Report(
  dp.Page(
    title="Titanic Dataset",
    blocks=["### Dataset", titanic]
  ),
  dp.Page(
    title="Titanic Plot",
    blocks=["### Plot", points]
  )
).upload(name='altair_example_pages')

# Markdown 

"""
```datapane
block: page
title: Titanic Dataset
```
### Dataset

```datapane
block: asset
label: datatable-1
```

```datapane
block: page
title: Titanic Ploc
```
### Plot

```datapane
block: asset
label: plot-2
```
"""
```
{% endtab %}
{% endtabs %}

{% embed url="https://datapane.com/u/datapane/reports/altair-example-pages/?utm\_medium=embed&utm\_content=viewfull" %}

## Tabs and Selects

In addition to top-level pages elements, you can include tabs and dropdown selects inside your reports. This allows users to switch between multiple blocks interactively and allows the creation of very interactive reports. It is also useful for providing background context to another block - for instance, to add source code to a plot or a dataset.

Datapane provides two select options on the `dp.Select` block: drop downs and tabs. Tabs are recommended if you have only a few choices and dropdowns are recommended for selects which have more than five options.

```python
import seaborn as sns
import altair as alt 
import datapane as dp

code = '''
titanic = sns.load_dataset("titanic")

dp.Report(
    dp.Select(blocks=[
        dp.Table(titanic.describe(), label='Data Description'),
        dp.DataTable(titanic, label='Whole Dataset'),
        dp.Code(code, label='Source code')
    ])
).upload(name='altair_example_select')
'''

dp.Report(
    "# Titanic overview",
    dp.HTML('<html><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fd/RMS_Titanic_3.jpg/1599px-RMS_Titanic_3.jpg" style="height:400px;display:flex;margin:auto"></img></html>'),
    dp.Select(blocks=[
        dp.Table(titanic.describe(), label='Data Description'),
        dp.DataTable(titanic, label='Whole Dataset'),
        dp.Code(code, label='Source code')
    ])
).upload(name='altair_example_select')

```

{% embed url="https://datapane.com/u/datapane/reports/altair-example-select/?utm\_medium=embed&utm\_content=viewfull" %}



