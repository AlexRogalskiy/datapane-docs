---
description: >-
  Reports wrap the results of your Python analyses, such as datasets and plots,
  into interactive documents which you can share.
---

# What is a Report?

## Introduction

For many Python data analyses, you only want to share a specific user-facing part of it rather than the whole code or notebook. This often is of the form of a standalone product that non-technical people can view directly from their existing tools - e.g. browsers, email, Slack, etc., and without the overhead and requirements of Python and Jupyter.

Datapane allows you to programmatically create reports from the objects in your Python analyses, such as pandas DataFrames, plots from visualization libraries, and Markdown text. 

## Creating a report

Datapane provides a Python API that allows you to create, save, and upload reports comprised of a collection of data-centric blocks. 

{% hint style="info" %}
Detailed API docs for Datapane Reports can be found at [https://datapane.github.io/datapane/](https://datapane.github.io/datapane/).
{% endhint %}

{% page-ref page="blocks/" %}

For instance, Datapane provides a `Table` block that takes a pandas `DataFrame`. We can create a `Table` block by passing a `DataFrame` into it, and create a `Report` with that single block in it as follows:

{% code title="simple\_report.py" %}
```python
import pandas as pd
import datapane as dp

df = pd.read_csv('https://covid.ourworldindata.org/data/vaccinations/vaccinations-by-manufacturer.csv', parse_dates=['date'])
df = df.groupby(['vaccine', 'date'])['total_vaccinations'].sum().tail(1000).reset_index()

report = dp.Report(
    dp.Table(df)
)
report.save(path='report.html', open=True)
```
{% endcode %}

As seen above, `Reports` can be saved to local `HTML` files. Copying this code into a new script and running it will generate the report. 

```bash
$ python3 simple_report.py
```

{% embed url="https://datapane.com/u/datapane/reports/docs-report-2/" %}

If you send this HTML file to somebody \(or [upload it on _Datapane Studio_](publishing-and-sharing/#publish-your-report)\), they will be able to view your dataset, sort and filter it, and download it as a CSV.

## A richer report

That report was pretty basic, but we can jazz it up by adding some plots and Markdown text. Unlike a traditional BI tool, Datapane does not rely on a proprietary visualization engine; instead, it natively supports Python visualization libraries such as [Altair](https://altair-viz.github.io/), [Plotly](https://plotly.com/python/), [Bokeh](https://bokeh.org/), and [Folium](https://python-visualization.github.io/folium/).

We also support an advanced Table component, called `DataTable`, which allows sorting, filtering, and interactive analysis - however it requires uploading your report to a Datapane server to function.

Let's take the example above, and plot some data using the Python library Altair and add some text.

{% hint style="info" %}
We'll use the below sample code snippet for the rest of the tutorials, so feel free to copy and paste it into a new Python script or Jupyter notebook and follow along
{% endhint %}

{% code title="richer\_report.py" %}
```python
import pandas as pd
import altair as alt
import datapane as dp

# download data & group by manufacturer
df = pd.read_csv('https://covid.ourworldindata.org/data/vaccinations/vaccinations-by-manufacturer.csv', parse_dates=['date'])
df = df.groupby(['vaccine', 'date'])['total_vaccinations'].sum().tail(1000).reset_index()

# plot vaccinations over time using Altair
plot = alt.Chart(df).mark_area(opacity=0.4, stroke='black').encode(
    x='date:T',
    y=alt.Y('total_vaccinations:Q'),
    color=alt.Color('vaccine:N', scale=alt.Scale(scheme='set1')),
    tooltip='vaccine:N'
).interactive().properties(width='container')

# tablulate total vaccinations by manufacturer
total_df = df[df["date"] == df["date"].max()].sort_values("total_vaccinations", ascending=False).reset_index(drop=True)
total_styled = total_df.style.bar(subset=["total_vaccinations"], color='#5fba7d', vmax=total_df["total_vaccinations"].sum())

# embed into a Datapane Report
report = dp.Report(
    "## Vaccination Report",
    dp.Plot(plot, caption="Vaccinations by manufacturer over time"),
    dp.Table(total_styled, caption="Current vaccination totals by manufacturer"),
    dp.Table(df, caption="Initial Dataset")
)
report.save("report.html", open=True)
```
{% endcode %}

When this python script is run, we get the following report.

{% embed url="https://datapane.com/u/datapane/reports/covid-vaccinations/" caption="A richer Datapane report" %}

Next, we will explore the blocks that make up a report, followed by how to upload and optionally share reports online. 

