---
description: >-
  Datapane allows you to deploy your Python scripts and notebooks, so that
  others can run them to generate reports dynamically.
---

# Deploying a Script

## Overview

In the [previous section](creating-a-report.md), we explored building and sharing a report programatically using Datapane's Python library. This tutorial shows you how to deploy your script or Jupyter Notebooks to allow other people generate reports dynamically.

Scripts on Datapane are exposed to users through web forms which can be run in the browser to generate reports. This means that people can generate reports without worrying about code, notebooks, or setting up a Python environment. Scripts can take parameters from users which are passed into your Python code at runtime.

![](../.gitbook/assets/image%20%2876%29.png)

## Writing a script

We can upload existing Python scripts or Jupyter Notebook to Datapane using the CLI.

```text
$ datapane script upload --script=script.ipynb
```

When a script is uploaded, others can run it through the web UI. Before uploading your script, you need to add some code to tell Datapane what your output report will look like.

This is largely the same process as the steps in [Creating a Report](creating-a-report.md), but, instead of calling `Report.create`, you return a list of components from a `render` method. When a user runs your script, your code is executed in its entirety, and Datapane subsequently runs this method to generate a report, which is then presented to the user in the Datapane web interface.

{% hint style="info" %}
Although adding a `render` method to your notebook or script is the recommend approach, as it keeps your existing code portable and does not pollute the namespace, you can alternatively add components to a `report` variable in your code which Datapane will look for if it can't find a render method.

```text
report = [dp.Plot.create(plot), dp.Table.create(df)]
```
{% endhint %}

Let's take the following code which pulls stock data from Yahoo finance and deploy it using Datapane's CLI. At the bottom, we've told Datapane that we want to create a report with a single table component in it containing our dataframe.

{% code title="analysis.ipynb" %}
```python
import pandas as pd
import altair as alt
from datapane import Table, Plot, Markdown

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

def render():
  return [
    Markdown("## Stock Report"),
    Table.create(stock_data),
    Plot.create(plot)
  ]
```
{% endcode %}

To deploy it, use Datapane's CLI.

```bash
$ datapane script upload analysis.ipynb --title=simple_script
[-] Script created: https://datapane.com/scripts/deadbeef
```

This makes your script available on datapane.com \(or your private instance\).

![](../.gitbook/assets/image%20%2842%29.png)

When a user runs your script, they will be able to generate the report from the previous example dynamically. For this script, this isn't particularly helpful yet -- the report is similar each time. Let's make it dynamic by adding some parameters which users can send into our code.

## Configuration

In the above example, we are deploying a single notebook and providing configuration  -- such as title -- through command-line arguments. It's often useful to create a more structured project which contains the config for your script. You can put your configuration in a special file called `datapane.yaml`. When you run a command such as a `upload`, Datapane looks for this file.

Before we continue, let's create a project structure with the `script init` command, which creates a sample configuration file and a simple script.

```bash
 ~/C/d/d/my-new-proj> datapane script init
Created script 'my-new-proj', edit as needed and upload
 ~/C/d/d/my-new-proj> ls
datapane.yaml template.py
```

Inside your `datapane.yaml`, you can set the following fields.

| Field | Description |
| :--- | :--- |
| public | Whether or not your script is public \(boolean, default false\) |
| title | The user-facing title of your script |
| parameters | A list of parameters for your script \(see below\) |

If there is a config file, when you upload your script, you don't need to provide the path of your analysis. Instead, your script lives in `template.py` or `template.ipynb` by default.

```python
~/C/d/d/my-new-proj> datapane script upload
16:39:39 [INFO ] Uploading template.py
Uploaded template.py to https://datapane.com/scripts/deadbeef/
```

## Running & Parameters

Input parameters are passed through a web form and are passed into your code at runtime. These are defined in your `datapane.yaml` and are accessible on a `params` dictionary in your code, which is available in the global namespace. In this case, let's allow users to provide the stock tickers they are interested in. 

{% code title="analysis.ipynb" %}
```python
import pandas as pd
import altair as alt
from datapane import Table, Plot, Markdown

# tickers = ['GOOG']
tickers = params.get('tickers', 'GOOG')
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

def render():
  return [
    Markdown("## Stock Report"),
    Table.create(stock_data),
    Plot.create(plot)
  ]
```
{% endcode %}

## Defining parameters

If you want to take input parameters, you can provide a list of `parameters`in your `datapane.yaml` . These define the fields in your form. For instance, in the following report, we may want to take a list of stock tickers to plot.

We would define the following `datapane.yaml`

{% code title="datapane.yaml" %}
```yaml
# Script title
title: Stock plotter

# Script parameter definitions - see docs.data
# see xyz for configuration docs
parameters:
  - name: tickers
    type: list
    description: "Tickers to plot"
```
{% endcode %}

If we run `script upload`, Datapane will use configuration to generate the following form.

![](../.gitbook/assets/image%20%286%29.png)

Users can now enter various stocks which they would like to plot, and receive a dynamic report each time.

![](../.gitbook/assets/image%20%2826%29.png)



## 

