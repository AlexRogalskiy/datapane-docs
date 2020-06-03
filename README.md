---
description: High-level introduction to Datapane
---

# Welcome to Datapane

## What is Datapane?

Datapane is for people who analyse data in Python and want an easy way to share their results.

It provides a library which allows you to create reports programatically from components that wrap around the common objects in analyses: [Pandas DataFrames](https://pandas.pydata.org/), plots from Python visualisation libraries such as [Bokeh](https://bokeh.org/) and [Altair](https://altair-viz.github.io/), and Markdown text. Once created, reports can be published on the web, dynamically generated in the cloud, or embedded into your own application, where data can be explored, and visualisations can be used interactively.

{% tabs %}
{% tab title="Code" %}
{% code title="stocks.py" %}
```python
import altair as alt
import pandas as pd
import datapane as dp

df = pd.read_csv('https://query1.finance.yahoo.com/v7/finance/download/GOOG?period1=1553600505&period2=1585222905&interval=1d&events=history')
chart = alt.Chart(df).encode(x='Date', y='High', y2='Low').mark_area(opacity=0.5).interactive()

dp.Report(dp.Table(df['High']), dp.Plot(chart)).publish(name='stock_analysis')
```
{% endcode %}
{% endtab %}

{% tab title="Report" %}
![](.gitbook/assets/image%20%2835%29.png)
{% endtab %}
{% endtabs %}

If you want your report to be generated dynamically by other people, for instance your team, you can deploy your Python script or notebook to a Datapane hosted server using Datapane's CLI. If you share it with others, they are able to provide parameters through a friendly web form, which are passed into your script. 

{% tabs %}
{% tab title="Code" %}
{% code title="stocks.ipynb" %}
```python
import altair as alt
import pandas as pd
import datapane as dp

ticker = dp.Params.get('ticker')
df = pd.read_csv(f"https://query1.finance.yahoo.com/v7/finance/download/{ticker}?period1=1553600505&period2=1585222905&interval=1d&events=history")
chart = alt.Chart(df).encode(x='Date', y='High', y2='Low').mark_area(opacity=0.5).interactive()

dp.Report(dp.Table(df['High']), dp.Plot(chart)).publish(name='stock_analysis')
```
{% endcode %}

{% code title="CLI" %}
```python
$ datapane script deploy --script=script.ipynb --name=stock_analyser
```
{% endcode %}

```yaml
name: stock_plot

# Script parameter definitions
parameters:
  - name: ticker
    type: string
    default: GOOG
    required: True
  - name: start_date
    type: date
  - name: end_date
    type: date
```
{% endtab %}

{% tab title="Web form" %}
![](.gitbook/assets/image%20%285%29.png)
{% endtab %}
{% endtabs %}

## Our Mission

Although there are many enterprise BI and reporting tools with drag and drop interfaces, using SQL with Python is often the best combination for querying, analysing, and visualising data. Datapane's goal is to provide an API-first way to provide the last mile of sharing results, so you can analyse data in your existing environment, instead of using "Yet Another BI Platform".

