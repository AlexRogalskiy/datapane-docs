---
description: >-
  Datapane allows you to deploy your Python scripts and notebooks, so that other
  people can run them to generate reports dynamically from their browser
---

# Deploying a Script

## Overview

In the [previous section](creating-a-report.md), we explored building and sharing a report programatically using Datapane's Python library. This tutorial shows you how to deploy your script or Jupyter Notebook so other people can use them to generate reports dynamically.

You create a script on datapane by deploying your Python code or Jupyter Notebook using Datapane's CLI. Scripts on Datapane are exposed to users through web forms which can be run in the browser to generate reports. This means that people can generate reports without worrying about code, notebooks, or setting up a Python environment. Scripts can take parameters from users which are passed into your Python code at runtime.

![](../.gitbook/assets/image%20%2876%29.png)

## Writing a script

If you have a local Python script or notebook which creates a report using Datapane's `Report.create` method \(see [Creating a Report](creating-a-report.md)\), you can upload it straight to Datapane using the CLI.

```text
$ datapane script deploy --script=analysis.ipynb
```

Let's take the following code which pulls stock data from Yahoo finance and deploy it using Datapane's CLI. At the bottom, we have a `Report.create` method which constructs our report, which will be returned to the user when they run our script using the Datapane web interface.

{% hint style="info" %}
Make sure you only only call `Report.create` a single time for each script you deploy to Datapane. If there are multiple reports created in a script, you will get an error or they will be ignored.
{% endhint %}

{% code title="analysis.ipynb" %}
```python
import pandas as pd
import altair as alt
from datapane import Table, Plot, Markdown, Report

tickers = ['GOOG']
dfs = []

for t in tickers:
    t_df = pd.read_csv(f'https://query1.finance.yahoo.com/v7/finance/download/{t}?period1=1553600505&period2=1585222905&interval=1d&events=history')
    t_df['ticker'] = t
    dfs.append(t_df)

stock_data = pd.concat(dfs)
stock_data['Date'] = pd.to_datetime(stock_data['Date'])
stock_data['pct_change'] = stock_data.groupby('ticker')['Close'].pct_change()
stock_data['cum_prod'] = (1 + stock_data['pct_change']).cumprod()

plot = alt.Chart(stock_data).encode(x='Date:T',y='cum_prod', color='ticker').mark_line()

Report.create(
    Markdown("## Stock Report"),
    Table.create(stock_data),
    Plot.create(plot)
 )
```
{% endcode %}

To deploy it, use Datapane's CLI.

```bash
$ datapane script deploy --script=analysis.ipynb --title="Simple Script" --name=simple_script
[-] Script created: https://datapane.com/scripts/deadbeef
```

This makes your script available on datapane.com \(or your private instance\).

![](../.gitbook/assets/image%20%2842%29.png)

When a user runs your script, they will be able to generate the report from the previous example dynamically. For this specific report, this isn't particularly helpful yet, as it is similar each time. Next, we will make it dynamic by configuring it with some parameters which users can send into our code.

## Configuration

In the above example, we are deploying a single notebook and providing configuration -- such as title -- through command-line arguments. This works well for simple scripts, but scripts often need other configuration, such as parameters definitions, other files or folders to deploy, and pip requirements.

Datapane allows you to provide a configuration file called `datapane.yaml`. When you `deploy`, Datapane looks for this file automatically.

Before we continue, create a project structure with the `script init` command, which creates a sample `datapane.yaml` and a simple script.

```bash
 ~/C/d/d/my-new-proj> datapane script init
Created script 'my-new-proj', edit as needed and upload
 ~/C/d/d/my-new-proj> ls
datapane.yaml dp-script.py
```

We already have a script, so we can delete the sample `dp-script.py` and copy in our own notebook. Because we're replacing the default script, we should specify this in our `datapane.yaml` using the `script` field. Whilst we're there, we can also add the name and title for our script.

{% code title="datapane.yaml" %}
```yaml
title: Stock plotter
name: stock_plotter

script: analysis.ipynb
```
{% endcode %}

See the [API reference](../reference/datapane.yaml.md) for all the available configuration fields.

{% hint style="info" %}
If there is a datapane.yaml, you don't need to provide the path of your analysis when you deploy. Instead, your script lives in `dp_script.py` or `dp_script.ipynb` by default.
{% endhint %}

## Running & Parameters

Input parameters are inputted through a web form and are passed into your code at runtime. These are defined in your `datapane.yaml` and are accessible in the `Params` dictionary inside your script. When we are developing locally, `Params` is empty by default, but you can load in the default parameters from your `datapane.yaml` \(or any other YAML file\) using the following command:

```python
Params.load_defaults('datapane.yaml')
```

In this case, let's allow users to provide the stock tickers they are interested in through a `tickers` parameter. To create an input for our `tickers` parameter on the web interface, we must add it to our `datapane.yaml` . 

{% hint style="info" %}
Full details of parameter configuration and available fields are provided in the [API reference](../reference/datapane.yaml.md#parameters).
{% endhint %}

{% code title="datapane.yaml" %}
```yaml
title: Stock plotter
name: stock_plotter

parameters:
  - name: tickers
    type: list
    default: ['GOOG', 'ZM']
    description: "Tickers to plot"
```
{% endcode %}

We can then access it in our code, and use `load_defaults` to load in the default values from our `datapane.yaml`

{% code title="analysis.ipynb" %}
```python
import pandas as pd
import altair as alt
from datapane import Table, Plot, Markdown, Params

# tickers = ['GOOG']
Params.load_defaults('datapane.yaml')
tickers = Params.get('tickers')

dfs = []

for t in tickers:
    t_df = pd.read_csv(f'https://query1.finance.yahoo.com/v7/finance/download/{t}?period1=1553600505&period2=1585222905&interval=1d&events=history')
    t_df['ticker'] = t
    dfs.append(t_df)

stock_data = pd.concat(dfs)
stock_data['Date'] = pd.to_datetime(stock_data['Date'])
stock_data['pct_change'] = stock_data.groupby('ticker')['Close'].pct_change()
stock_data['cum_prod'] = (1 + stock_data['pct_change']).cumprod()

plot = alt.Chart(stock_data).encode(x='Date:T',y='cum_prod', color='ticker').mark_line()

Report.create(
  Markdown("## Stock Report"),
  Table.create(stock_data),
  Plot.create(plot)
)
```
{% endcode %}

When we run `script deploy`, Datapane will use configuration to generate the following form.

![](../.gitbook/assets/image%20%286%29.png)

Users can now enter various stocks which they would like to plot, and receive a dynamic report each time.

![](../.gitbook/assets/image%20%2826%29.png)

## Dependencies and Requirements

Your Python script or notebook may have requirements on external libraries. Datapane supports both the ability to add local folders and files, which you can deploy alongside your script, and the ability to include `pip` requirements, which are made available to your script when it is run on Datapane. These are both configured in your `datapane.yaml`. 

### Including pip dependencies

In the above example, we may want to use the [yfinance](https://pypi.org/project/yfinance/) library in Python, instead of manually reading a CSV from Yahoo. To do this, we can add it to a `requirements` list in our `datapane.yaml`

{% code title="datapane.yaml" %}
```yaml
title: Stock plotter
name: stock_plotter

parameters:
  - name: tickers
    type: list
    default: ['GOOG', 'ZM']
    description: "Tickers to plot"

requirements:
  - yfinance
```
{% endcode %}

### Including additional files and folders

Additionally, we may want to include a local Python folder or file. Imagine we have a separate Python file, `stock_scaler.py` which helps us scale the values of our stocks, and which we want to use in our notebook.

```text
~/C/d/d/my-new-proj> ls
dp-script.ipynb datapane.yaml stock_scaler.py
```

To include this in the deploy, we can add it to `include` . 

{% code title="datapane.yaml" %}
```yaml
title: Stock plotter
name: stock_plotter

parameters:
  - name: tickers
    type: list
    default: ['GOOG', 'ZM']
    description: "Tickers to plot"

requirements:
  - yfinance

include:
  - stock_scaler.py
```
{% endcode %}

## 

