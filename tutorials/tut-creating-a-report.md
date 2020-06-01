---
description: >-
  Reports present results of analyses, such as datasets and plots, as web pages
  which you can share and embed.
---

# Creating a Report

## Introduction

A lot of times you're analysing data and only want to share a specific user-facing part of it rather than the whole analysis. This often is of the form of a standalone product that non-technical people can view direct from their existing tools - e.g. browsers, email, Slack, etc., and without the overhead and requirements of Python and Jupyter.

Datapane allows you to programatically create reports from the objects in your Python analyses, such as pandas DataFrames, plots from visualisation libraries, and Markdown text. Reports contain various components which contain these assets.

## Creating a report

Datapane provides a Python API that allow you to create, save and publish reports comprised from a collection of data-centric components.

For instance, Datapane provides a Table component which takes a pandas DataFrame. We can create a Table component by passing a DataFrame into it, and create a Report with that single component in it.

{% code title="simple\_report.py" %}
```python
import pandas as pd
import datapane as dp

df = pd.DataFrame({
    'A': np.random.normal(-1, 1, 5000),
    'B': np.random.normal(1, 2, 5000),
})

table = dp.Table(df)
report = dp.Report(table)
report.save(path='test.html')
```
{% endcode %}

Copying this code into a new script and running it will generate the report. Try it for yourself:

```bash
$ python3 simple_report.py
```

![A simple report with a single table](../.gitbook/assets/image%20%2829%29.png)

### A more complex report

That report was pretty basic, but we can jazz it up quite easily by adding some plots and markdown. Datapane supports Python visualisation libraries such as [Altair](https://altair-viz.github.io/) and [Bokeh](https://bokeh.org/). 

Let's take the following code which pulls stock data from Yahoo finance and create a report with some live plots along with some example Markdown text. 

{% hint style="info" %}
We'll use the below sample code snippet for the rest of the tutorials, so feel free to copy and paste it into a new Python script or Jupyter notebook and follow along
{% endhint %}

{% code title="financial\_report.py" %}
```python
import pandas as pd
import altair as alt
import datapane as dp

from scipy.stats import zscore

tickers = ['GOOG']
dfs = []

for t in tickers:
    t_df = pd.read_csv(f'https://query1.finance.yahoo.com/v7/finance/download/{t}?period1=1553600505&period2=1585222905&interval=1d&events=history')
    t_df['ticker'] = t
    dfs.append(t_df)

stock_data = pd.concat(dfs)
stock_data['Date'] = pd.to_datetime(stock_data['Date'])
stock_data['zscore'] = stock_data.groupby('ticker')['Close'].transform(lambda x: zscore(x))

plot = alt.Chart(stock_data).encode(x='Date:T',y='zscore', color='ticker').mark_line()

report = dp.Report(
    dp.Markdown("## Stock Report"),
    dp.Table(stock_data),
    dp.Plot(plot)
 )
 
 report.save(path='test.html')
```
{% endcode %}

When this python script is run, using the same command as earlier, we get the following report:

![](../.gitbook/assets/image%20%2882%29.png)

The existing components and report API are described in more detail in the [API reference](../reference/reports/).

## Viewing your report

As described above, we can easily view our report in a browser. However there are other ways to view and share our report whilst developing it.

Datapane has specific integration into for Jupyter notebooks: for instance, If you're iterating a report, instead of having to open a new window to view it, you can preview a report directly from inside your notebook by calling `report.preview()` - embedding it live into your notebook

![](../.gitbook/assets/image%20%2884%29.png)

## Publish your report

{% hint style="info" %}
This feature requires use of the free Datapane public host
{% endhint %}

So far we've demonstrated how to build and view reports locally - however one of the most powerful features of Datapane is the ability to publish your report straight from your code and share it directly with your team or the wider world.

Once you've [logged in](tut-getting-started.md#logging-in), call `report.publish(name='your-report-name')` in your script and your report will be published to your Datapane instance for sharing and viewing using the web. This will return the URL of the report that you can share, tweet, etc.

```python
report = dp.Report(plot, table)
report.publish(name='test-report')
"http://datapane.com/datapane/reports/test-report"
```

From here, you can embed reports into other places, such as Notion, Confluence, Reddit, Slack, a blog post, or your own web page. 

We can view the report demonstrated in this tutorial on Datapane[ here](https://acme.datapane.com/reports/Bj3LQ7Q/), and you can we have more on [gallery page](www.datapane.com/gallary/). 

Reports on Datapane have visibility settings_**.**_ By default, only you can view your reports on your web, but you can make your report public when you create it. If you're on a private Datapane instance, you can also restrict it to everyone on your domain.

```python
# Publicly available
report.publish(visibility='PUBLIC')

# Available to everyone on your Datapane instance
report.publish(visibility='ORG')
```

{% hint style="info" %}
Datapane also supports access tokens to allow you to share or embed your report with certain people, but keep it private from everyone else.  
{% endhint %}

## Next steps

Although we can create, share, and embed our report, it's static at the moment: we run the Python code locally to publish a complete artifact. This is great for certain use-cases, like generating in CI or within a batched process running on a tool like Airflow \(as described in our [guides](../guides/calling-from-your-existing-workflow-tools.md).\)

However, it's often useful to allow other people to generate your reports dynamically. For instance, your report might pull some new data from a database or API, or you may want users to pass in some parameters. In the next section, we'll explore deploying your code so that others run it to generate reports themselves.

