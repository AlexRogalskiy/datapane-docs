---
description: >-
  Reports present results of analyses, such as datasets and plots, as web pages
  which you can share and embed.
---

# Creating a Report

Datapane allows you to programatically create reports from the objects in your Python analyses, such as pandas DataFrames, plots from visualisation libraries, and Markdown. Reports contain various components which contain these assets.

For instance, Datapane provides a Table component which takes a pandas DataFrame. We can `create` a Table component, and then create a Report with that single component in it.

```python
import pandas as pd
from datapane import Report, Table

df = pd.DataFrame({
    'A': np.random.normal(-1, 1, 5000),
    'B': np.random.normal(1, 2, 5000),
    'C': np.random.normal(3, 0.5, 5000),
    'D': np.random.normal(-3, 0.5, 5000)
})

table = Table.create(df)
report = Report.create(table)
```

When we create a report, it is hosted on the Datapane server which we logged into using the CLI. To get the URL of our new report, we can look at the `web_url` property on the report object. In this example, we will have something like [this](https://acme.datapane.com/reports/Bj3LQ7Q/).

![A simple report with a single table](../.gitbook/assets/image%20%2829%29.png)

This report is pretty basic, but we can jazz it up by adding some plots and markdown. Datapane supports Python visualisation libraries such as Altair and Bokeh. Let's create a plot with Altair, and add it to our report, along with some example Markdown text. This time, we pass a list of components to our Report.

```python
import pandas as pd
import numpy as np
import altair as alt
from datapane import Report, Table, Plot, Markdown

df = pd.DataFrame({
    'A': np.random.normal(-1, 1, 5000),
    'B': np.random.normal(1, 2, 5000),
    'C': np.random.normal(3, 0.5, 5000),
    'D': np.random.normal(-3, 0.5, 5000)
})

chart = alt.Chart(df).encode(
    x=alt.X('A', bin=alt.Bin(maxbins=10)), 
    y='count()'
).mark_area(opacity=0.3)

table = Table.create(df)
plot = Plot.create(chart)

report = Report.create(
    Markdown("## Look at the data!"),
    Table.create(df),
    Markdown("## Look at the plots!"),
    Plot.create(chart)
)
```

This generates the following

![A more exciting report](../.gitbook/assets/image%20%2873%29.png)

If you're iterating a report, it's a bit annoying to have to open a new window to view it. If you're in a Jupyter Notebook, you can preview a report inside your notebook by running `report.preview()`.

![](../.gitbook/assets/image%20%2866%29.png)

When you preview a report, you are embedding it into your notebook. You can embed reports into other places, such as Notion, Confluence, Reddit, a blog post, or your own web page. To embed a report, you can use the `embed_url` on the report object. 

Reports on Datapane have visibility settings_**.**_ By default, only you can view your reports on your web, but you can make your report public when you create it. If you're on a private Datapane instance, you can also restrict it to everyone on your domain.

```python
## Publicly available
Report.create(Table(df), visibility='PUBLIC')

## Available to everyone on your Datapane instance
Report.create(Table(df), visibility='DOMAIN')
```

{% hint style="info" %}
If you want to share or embed your report with certain people, but keep it private from everyone else, you can also use an access token. 
{% endhint %}

## Next steps

Although we can create, share, and embed our report, it's static at the moment. It's often useful to allow other people to generate your reports dynamically. For instance, your report might pull some new data from a database or API, or you may want users to pass in some parameters. In the next section, we'll explore deploying your code so that others run it to generate reports themselves.

