---
description: >-
  This example walks through creating an account on Datapane, installing the
  library, and using the API to create, deploy, and share a board from Python.
---

# Quickstart

## Create an account 

Browse to [datapane.com](https://datapane.com) and create a free account. This will prompt you to confirm your email address and choose a username.

## Install the Datapane library and login

Once you have logged in, you can find your API token on your [Settings Page](https://datapane.com/settings/). Copy this token to your clipboard.

Next, install the Datapane Python library from pip. This includes the CLI and Python API, which allow you to deploy datasets, plots, and create boards programmatically. Once installed, you can login using your API token.

```bash
$~> pip3 install -U datapane
...
$~> datapane login
Your API Token: [Enter your API token here]
```

After logging into the CLI, you can use datapane from either Python or the terminal. 

The following example demonstrates creating a board from an interactive Python environment, such as Jupyter or IPython. 

## Importing our libraries

We are going to use pandas and Altair to plot some sample data. First, we can import these two libraries along with datapane and the datapane API. Next, we run `api.init()` to initialise datapane.

```python
import pandas as pd
import altair as alt
# If you are using Jupyter, enable the renderer with alt.renderers.enable('notebook') 

import datapane as dp
from datapane import api

api.init()
```

## Creating sample data and plots

Next, let's create a sample Pandas DataFrame with some dummy data, and create a plot using Altair.

```python
# Taken from https://altair-viz.github.io/gallery/line_chart_with_points.html
x = np.arange(100)
source = pd.DataFrame({
  'x': x,
  'f(x)': np.sin(x / 5)
})

plot = alt.Chart(source).mark_line(point=True).encode(x='x', y='f(x)').interactive()
```

If you are in Jupyter, you will be able to view this plot inline, and use it interactively.

![](.gitbook/assets/image%20%2814%29.png)

## Deploying to Datapane

Now that we have some data and plots, we can deploy them to Datapane using our API, and use them to programmatically create a dashboard which we can share.

```python
# Upload a dataset
dp_df = api.Dataset.upload_df(source, make_public=True)

# Upload your plot
dp_plot = api.Asset.upload_obj(plot)

# Create a dashboard from your dataset and plot
dashboard = api.Datapane.create(dp_df, dp_plot, make_public=True)
```

We can now find the id of our dashboard on `dashboard.id` and view it on Datapane by going to the following URL: `https://datapane.com/datapanes/ + dashboard.id` \(for instance, [https://datapane.com/datapanes/J35q030](https://datapane.com/datapanes/J35q030)\).

## Finishing our dashboard

Because we are the owner of this dashboard, we will automatically enter edit mode, and will be able to see the dataset and visualisation we uploaded. 

![](.gitbook/assets/image%20%2852%29.png)

Let's also drag in a text block from from the right hand side, and write a description of our Datapane. 

![](.gitbook/assets/image%20%2856%29.png)

Next, we can preview it by switching the **Preview Toggle.**

![](.gitbook/assets/image%20%2836%29.png)

## Sharing our dashboard

Our Datapane can now be shared with others or embedded into other platforms. Datapane is natively supported in rich-text Reddit posts and Medium: on these platforms, you simply need to paste the URL of your board and it will automatically expand and be usable. It can also be embedded as an iframe using the URL in the action bar of the web interface.

