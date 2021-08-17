---
description: >-
  Datapane supports all major Python visualization libraries, allowing you to
  add interactive plots and visualizations to your report.
---

# Plots and Visualizations

The `dp.Plot` block takes a plot object from one of the supported Python visualisation libraries and renders it in your report. 

{% hint style="info" %}
Datapane will automatically wrap your visualization or plot in a `dp.Plot` block if you pass it into your report directly.
{% endhint %}

You can also pass in  `name` and `caption` parameters into `dp.Plot` to style your plots, like this: 

```python
dp.Plot(fig, name="fig1", caption="Chart showing average life expectancy")
```

Datapane currently supports the following libraries:

| Library | Site / Docs |
| :--- | :--- |
| Altair | [https://altair-viz.github.io/](https://altair-viz.github.io/) |
| Matplotlib / Seaborn | [https://matplotlib.org/](https://matplotlib.org/) / [https://seaborn.pydata.org/](https://seaborn.pydata.org/) |
| Bokeh | [https://bokeh.org/](https://bokeh.org/) |
| Plotly | [https://plotly.com/python/](https://plotly.com/python/) |
| Folium | [https://python-visualization.github.io/folium/](https://python-visualization.github.io/folium/) |

If you're using another visualization library e.g. Pyvis for networks, try saving your chart as a local HTML file and wrapping that in a [dp.HTML](text-code-and-html.md#html) block. 

## Altair

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

dp.Report(dp.Plot(alt_chart)).upload(name='time_interval')
```

{% embed url="https://datapane.com/u/datapane/reports/time-interval/" %}

## Bokeh

Bokeh is an interactive visualization library which provides elegant, concise construction of versatile graphics, and affords high-performance interactivity over large datasets. 

To get started using Bokeh to make your visualizations, begin with Bokeh's [User Guide](https://docs.bokeh.org/en/latest/docs/user_guide.html#userguide).

```python
from bokeh.plotting import figure, output_file, show
from bokeh.sampledata.iris import flowers
import datapane as dp 

# Create scatter plot with Bokeh
colormap = {'setosa': 'red', 'versicolor': 'green', 'virginica': 'blue'}
colors = [colormap[x] for x in flowers['species']]

bokeh_chart = figure(title = "Iris Morphology")
bokeh_chart.xaxis.axis_label = 'Petal Length'
bokeh_chart.yaxis.axis_label = 'Petal Width'

bokeh_chart.circle(flowers["petal_length"], flowers["petal_width"],
         color=colors, fill_alpha=0.2, size=10)

# Upload the report
dp.Report(dp.Plot(bokeh_chart)).upload(name='bokeh_plot')
```

{% embed url="https://datapane.com/u/datapane/reports/bokeh-plot/" %}

## Matplotlib

[Matplotlib](https://matplotlib.org) is the original Python visualisation library, often supported and used with [Jupyter Notebooks](https://jupyter.org/). Matplotlib plots are not interactive in Datapane Reports, but are saved as SVGs so can be viewed at high fidelity.

Higher-level matplotlib libraries such as [Seaborn](https://seaborn.pydata.org/) are also supported, and can be used in a similar way to the matplotlib example below,

```python
import matplotlib.pyplot as plt
import pandas as pd
import datapane as dp
from vega_datasets import data as vega_data

gap = pd.read_json(vega_data.gapminder.url)
# gap.plot.scatter()
fig = gap.plot.scatter(x='life_expect', y='fertility')
dp.Report(dp.Plot(fig)).upload(name="test_mpl")
```

{% embed url="https://datapane.com/u/datapane/reports/test-mpl/" %}

{% hint style="info" %}
You can pass either a `matplotlib` `Figure` or `Axes` object to `dp.Plot`,  you can obtain the current global figure from `matplotlib` by running `plt.gcf()`
{% endhint %}

## Plotly

[Plotly's Python graphing library](https://plotly.com/python/) makes interactive, publication-quality graphs.

```python
import plotly.express as px
import datapane as dp

df = px.data.gapminder()

plotly_chart = px.scatter(df.query("year==2007"), x="gdpPercap", y="lifeExp",
	         size="pop", color="continent",
                 hover_name="country", log_x=True, size_max=60)
plotly_chart.show()

dp.Report(dp.Plot(plotly_chart)).upload(name='bubble')
```

{% embed url="https://datapane.com/u/datapane/reports/bubble/" %}

## Folium

[Folium](https://python-visualization.github.io/folium/) makes it easy to visualize data that’s been manipulated in Python on an interactive leaflet map. It enables both the binding of data to a map for `choropleth` visualizations as well as passing rich vector/raster/HTML visualizations as markers on the map.

The library has a number of built-in tilesets from OpenStreetMap, Mapbox, and Stamen, and supports custom tilesets with Mapbox or Cloudmade API keys. 

{% hint style="info" %}
If your folium map consumes live data which expires after a certain time, you can automate it to refresh the map on a cadence. See [Automation](../automation-with-github-actions.md).
{% endhint %}

```python
import folium
import datapane as dp 

m = folium.Map(location=[45.5236, -122.6750])

dp.Report(dp.Plot(m)).upload(name='folium_map')
```

{% embed url="https://datapane.com/u/datapane/reports/folium-map/" %}



