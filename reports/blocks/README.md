---
description: All the blocks you can use to develop and layout your reports
---

# Building Reports

## Overview

Reports are comprised of multiple Blocks, which wrap up Python objects, such as Pandas DataFrames, Visualisations, and Markdown. Datapane also includes layout blocks to add tabs, pages, and interactive selects to your reports.&#x20;

We are always adding new components, and if you have some ideas on what you would like to use in your reports, please [start a discussion on GitHub](https://github.com/datapane/datapane/discussions).

In this section we describe the Block types and provide examples. More detailed API usage can be found in our [API docs](https://datapane.github.io/datapane/report.html).&#x20;

{% hint style="info" %}
If you pass your Python object into your without wrapping it in a specific block component, Datapane will try and automatically choose the best block type.&#x20;
{% endhint %}

## Report Widths

Datapane has three standard report, which dictate sizing and layout:

* **Medium:** medium width and margins, optimized for mixed content
* **Narrow:** small width and large margins, optimized for long-form text
* **Full: **full-width with no margins, optimized for grid layout and visualizations

The default type is Medium, and you can choose other widths when you create your report as follows:

```python
import datapane as dp

# Create a report (default)
report.upload(..., formatting=dp.ReportFormatting(width=dp.ReportWidth.MEDIUM))

# Create a dashboard
report.upload(..., formatting=dp.ReportFormatting(width=dp.ReportWidth.FULL))

# Create an article
report.upload(..., formatting=dp.ReportFormatting(width=dp.ReportWidth.NARROW))
```

{% content-ref url="../configuring-reports/styling.md" %}
[styling.md](../configuring-reports/styling.md)
{% endcontent-ref %}

## Block Types

{% content-ref url="tables-and-data.md" %}
[tables-and-data.md](tables-and-data.md)
{% endcontent-ref %}

{% content-ref url="plots-and-visualizations.md" %}
[plots-and-visualizations.md](plots-and-visualizations.md)
{% endcontent-ref %}

{% content-ref url="text-code-and-html.md" %}
[text-code-and-html.md](text-code-and-html.md)
{% endcontent-ref %}

{% content-ref url="layout-pages-and-selects.md" %}
[layout-pages-and-selects.md](layout-pages-and-selects.md)
{% endcontent-ref %}

{% content-ref url="files-and-images.md" %}
[files-and-images.md](files-and-images.md)
{% endcontent-ref %}

Except for Page blocks, every block can be nested inside a layout block, meaning you can build arbitrarily complex reports. In addition, most blocks take optional `name` and `caption` parameters and display those to your viewers.&#x20;

## Default Block Handling

As well as explicitly specifying your block type (for instance, by using `dp.Plot`), Datapane will try and choose the best block for your object if you pass it in directly, for instance as follows:

```python
import datapane as dp
import pandas as pd

d = {'col1': [1, 2], 'col2': [3, 4]}
df = pd.DataFrame(data=d)

dp.Report(
  df,
  "This is text"
)
```

&#x20;The defaults are as follows:

| Object Type           | Datapane Block |
| --------------------- | -------------- |
| pandas DataFrame      | `dp.Table`     |
| string                | `dp.Text`      |
| Altair                | `dp.Plot`      |
| Bokeh                 | `dp.Plot`      |
| Folium                | `dp.Plot`      |
| Matplotlib / Seaborn  | `dp.Plot`      |
| Plotly                | `dp.Plot`      |
