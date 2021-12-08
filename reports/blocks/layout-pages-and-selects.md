---
description: Laying out your report to make it more readable and understandable
---

# Layout, Pages, and Selects

## Overview

Although you are you probably analyzing data in a list of sequential steps, it may not the best way to present results -- especially if you are sharing with people who are used to a traditional dashboard or BI tool.&#x20;

Datapane's report library provides components which allow for more flexible grid-style layouts, multiple pages, tabs, and selects, which allows you to create powerful custom interfaces without knowledge of HTML or CSS.

## Grid layouts

If you pass a list of blocks (such as `Plot` and `Table`) to a Report, they are -- by default -- laid out in a single column with a row per block. If you would like to customize the rows and columns, Datapane provides a `Group` component which takes a list of blocks and a number of columns and/or rows and lays them out in a grid.

If we take the example in the earlier tutorial, but want to lay the plot and dataset side-by-side, we can use specify this using `Group` and specifying the number of columns.

{% tabs %}
{% tab title="Python" %}
{% code title="simple_report.py" %}
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

{% tab title="Text Report" %}
````
```datapane
block: Group
columns: 2
blocks: 
  - block: asset
    label: plot-1
  - block: asset
    label: table-2
```
````
{% endtab %}
{% endtabs %}

![](<../../.gitbook/assets/image (104).png>)

If you're generating your plots programmatically or have a lot of plots, you can pass them into the Group block as a list, using the `blocks` parameter. We can rewrite the previous example as follows :&#x20;

{% tabs %}
{% tab title="Python" %}
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

# You could also generate these in a loop/function
my_plots = [dp.Plot(plot), dp.DataTable(df)]

dp.Report(
    dp.Group(
        blocks=my_plots,
        columns=2
    )
).upload(name='covid_report', open=True)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
As `Group`components are components themselves, they are composable, and you can create more custom layers of nested blocks, for instance nesting 2 rows in the left column of a 2 column layout
{% endhint %}

### Customizing width

To customize the width of your report, you can set the [report type](./#report-types).

## Pages&#x20;

Reports on Datapane can have multiple pages, which are presented to users as tabs at the top of your report. These can be used similarly to sheets in an Excel document.

To add a page, use the `dp.Page` block at the top-level of your report, and give it a title with the `title` parameter.

{% hint style="info" %}
Pages cannot be nested, and can only exist at the root level of your `dp.Report` object. If you're using pages, all other blocks must be contained inside a Page block.&#x20;
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

{% tab title="Text Report" %}
````
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
````
{% endtab %}
{% endtabs %}

{% embed url="https://datapane.com/u/datapane/reports/altair-example-pages/?utm_medium=embed&utm_content=viewfull" %}

## Tabs and Selects

In addition to top-level pages elements, you can include tabs and dropdown selects inside your reports using the `dp.Select` block. This allows users to switch between multiple blocks interactively and can even function as a basic filter. It is also useful for providing background context to another block - for instance, to add source code to a plot or a dataset.

The `dp.Select` block has two modes:&#x20;

* **Tabs** are used for less than 5 options - you can override this default by passing in the parameter `type=dp.SelectType.TABS`
* **Drop downs** are used for 5 or more options - you can override this default by passing in the parameter`type = dp.SelectType.DROPDOWN`. In addition, a search bar will appear if the block contains more than 10 options.&#x20;

To set the option names, make sure each block contained inside your `dp.Select` has a `label` - see example:

{% tabs %}
{% tab title="Python" %}
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
{% endtab %}

{% tab title="Text Report" %}
````
```datapane
block: Select
blocks: 
  - block: asset
    name: table-1
    label: Data Description
  - block: asset
    name: table-2
    label: Whole Dataset
  - block: code
    label: Source code
    value: |
        titanic = sns.load_dataset("titanic")
        
        dp.Report(
            dp.Select(blocks=[
                dp.Table(titanic.describe(), label='Data Description'),
                dp.DataTable(titanic, label='Whole Dataset'),
                dp.Code(code, label='Source code')
            ])
        ).upload(name='altair_example_select')   
```    
````
{% endtab %}
{% endtabs %}

{% embed url="https://datapane.com/u/datapane/reports/altair-example-select/?utm_medium=embed&utm_content=viewfull" %}

