---
description: High-level introduction to Datapane
---

# Welcome to Datapane

## What is Datapane?

Datapane is for people who analyse data in Python and need a way to share their results.

It provides a way to create reports programatically from components that wrap around the common objects in analyses, such as:

* [Pandas DataFrames](https://pandas.pydata.org/)
* Plots from Python visualisation libraries such as [Bokeh](https://bokeh.org/), [Altair](https://altair-viz.github.io/), [Plotly](https://plotly.com/python/), and [Folium](https://python-visualization.github.io/folium/quickstart.html)
* Markdown and text

Once created, reports can be published on the web, dynamically generated in the cloud, or embedded into your own application, where data can be explored, and visualisations can be used interactively.

```python
import pandas as pd
import altair as alt
import datapane as dp

df = pd.read_csv('https://query1.finance.yahoo.com/v7/finance/download/GOOG?period1=1553600505&period2=1585222905&interval=1d&events=history')
chart = alt.Chart(df).encode(x='Date', y='High', y2='Low').mark_area(opacity=0.5).interactive()

dp.Report(
  dp.Table(df['High']), 
  dp.Plot(chart)
).save(path='stocks.html')
```

If you have already signed in with token, publishing your report could be done in one line of code.

```python
dp.Report(
  dp.Table(df['High']), 
  dp.Plot(chart)
).publish(name='stocks_report', open=True)
```

And a website on Datapane will be automatically created for you!

![Standalone output report](.gitbook/assets/image%20%2888%29.png)

You could either share this link on social media or with your team.  

![](.gitbook/assets/image%20%2896%29.png)

If you want your report to be generated dynamically by other people \(such as your team or stakeholders\) you can deploy your Python script or notebook to a Datapane hosted server using Datapane's CLI. If you share it with others, they are able to provide parameters through a friendly web form, which are passed into your script, and generate your reports dynamically.

![](.gitbook/assets/image%20%2889%29.png)

## Our Mission

Although there are many enterprise BI and reporting tools with drag and drop interfaces, using SQL with Python is often the best combination for querying, analysing, and visualising data. Datapane's goal is to provide an API-first way to provide the last mile of sharing results, so you can analyse data in your existing environment, instead of using "Yet Another BI Platform".

