---
description: >-
  Deployed scripts can be parameterised, making them a web form you can share
  with others
---

# Parameterising a Script

## Overview

When you add parameters to your script, they are presented in your browser as web forms which users can run to generate reports. This means that people can generate reports without worrying about code, notebooks, or setting up a Python environment. 

![](../.gitbook/assets/image%20%2876%29.png)

## Running & Parameters

Input parameters are passed into your code at runtime and are defined in your `datapane.yaml` . In Python, they are accessible in the `Params` dictionary. When we are developing locally, `Params` is empty by default, but you can load in the default parameters from your `datapane.yaml` \(or any other YAML file\) using the following command:

```python
Params.load_defaults('datapane.yaml')
```

Let's allow users to provide the stock tickers they are interested in through a `tickers` parameter. To create an input for our `tickers` parameter on the web interface, we must add it to our `datapane.yaml` from the previous section. 

{% hint style="info" %}
Full details of parameter configuration and available fields are provided in the [API reference](../reference/scripts/datapane.yaml.md#parameters).
{% endhint %}

{% code title="datapane.yaml" %}
```yaml
script: analysis.ipynb
name: stock_plotter

requirements:
  - yfinance

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

## 

