---
description: All the components you can use to develop and layout your reports
---

# Components

## Overview

Components are used to create [Reports](./). Components are compatible with Python objects, such as Pandas DataFrames, visualisations, and Markdown. We are always adding new components, and if you have some ideas on what you would like to use in your reports, please [open an issue on GitHub](https://github.com/datapane/datapane).

## Plot

The plot component takes a supported plot object and renders it in your report.

Currently, supported libraries are:

* Matplotlib and seaborn figures
* Plotly plots
* Bokeh plots
* Altair plots
* Folium plots
* SVG images

### Altair

[Altair](https://altair-viz.github.io/) is a declarative statistical visualization library for Python, based on [Vega](http://vega.github.io/vega) and [Vega-Lite](http://vega.github.io/vega-lite). Altair’s API is simple, friendly and consistent and built on top of the powerful [Vega-Lite](http://vega.github.io/vega-lite) visualization grammar. This elegant simplicity produces beautiful and effective visualizations with a minimal amount of code.

To get started using Altair to make your visualizations, begin with Altair's [Documentation](https://altair-viz.github.io/)

```python
import altair as alt
import datapane as dp
from vega_datasets import data as vega_data
gap = pd.read_json(vega_data.gapminder.url)

select_year = alt.selection_single(
    name='select', fields=['year'], init={'year': 1955},
    bind=alt.binding_range(min=1955, max=2005, step=5)
)
alt_chart = alt.Chart(gap).mark_point(filled=True).encode(
    alt.X('fertility', scale=alt.Scale(zero=False)),
    alt.Y('life_expect', scale=alt.Scale(zero=False)),
    alt.Size('pop:Q'),
    alt.Color('cluster:N'),
    alt.Order('pop:Q', sort='descending'),
).add_selection(select_year).transform_filter(select_year)

dp.Report(dp.Plot(alt_chart)).publish(name='time_interval')
```

![](../../.gitbook/assets/screenshot-from-2020-06-26-11-30-58.png)

### Bokeh

Bokeh is an interactive visualization library which provides elegant, concise construction of versatile graphics, and affords high-performance interactivity over large datasets. 

To get started using Bokeh to make your visualizations, begin with Bokeh's [User Guide](https://docs.bokeh.org/en/latest/docs/user_guide.html#userguide).

```python
from bokeh.plotting import figure, output_file, show
from bokeh.sampledata.iris import flowers
import datapane as dp 

# Create scatter plot with Bokeh
colormap = {'setosa': 'red', 'versicolor': 'green', 'virginica': 'blue'}
colors = [colormap[x] for x in flowers['species']]

bohek_chart = figure(title = "Iris Morphology")
bokeh_chart.xaxis.axis_label = 'Petal Length'
bokeh_chart.yaxis.axis_label = 'Petal Width'

bokeh_chart.circle(flowers["petal_length"], flowers["petal_width"],
         color=colors, fill_alpha=0.2, size=10)

output_file("iris.html", title="iris.py example")

# View the plot
dp.Report(dp.Plot(bokeh_chart)).preview()

# Publish the report
dp.Report(dp.Plot(bokeh_chart)).publish(name='bokeh_plot')
```

![](../../.gitbook/assets/screenshot-from-2020-06-26-11-27-46.png)

### Plotly

[Plotly's Python graphing library](https://plotly.com/python/) makes interactive, publication-quality graphs.

```python
import plotly.express as px
import datapane as dp

df = px.data.gapminder()

plotly_chart = px.scatter(df.query("year==2007"), x="gdpPercap", y="lifeExp",
	         size="pop", color="continent",
                 hover_name="country", log_x=True, size_max=60)
plotly_chart.show()

dp.Report(dp.Plot(plotly_chart)).publish(name='bubble')
```

![](../../.gitbook/assets/screenshot-from-2020-06-26-11-46-31.png)

## Markdown

**Markdown** is a lightweight markup language that allows you to include text in your report.

```python
import datapane as dp

dp.Report(dp.Markdown("My awesome markdown"))
```

To include more sentences and format the words, use `"""Some words"""`

```python
import datapane as dp

report = dp.Report(dp.Markdown( "# Altair"),
         dp.Plot(alt_chart),
         dp.Markdown("""
* There is a negative relation between life expectanty and fertility
* The number of population with high life expactancy increases as time increase
          """)
report.publish(name = 'results')

```

![Graph with Markdown ](../../.gitbook/assets/screenshot-from-2020-06-26-15-02-29.png)

Check [here](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) for more information on how to format your text with markdown.

## Table

The Table component takes a pandas DataFrame and renders an interactive, sortable, searchable table in your Datapane report. Viewers can also download the table from the website. Tables can typically render datasets up to 2-3m cells without performance problems. 

{% hint style="info" %}
By default, a Table only displays the first 1000 rows, and gives the user the option to load the entire dataset.
{% endhint %}

```python
import datapane as dp
import pandas as pd

df = pd.DataFrame({
    'A': np.random.normal(-1, 1, 5000),
    'B': np.random.normal(1, 2, 5000),
})

table = dp.Table(df)
report = dp.Report(table)
report.publish(name='sample_table')
```

![](../../.gitbook/assets/table.png)

## PivotTable

{% hint style="info" %}
Coming soon!
{% endhint %}

