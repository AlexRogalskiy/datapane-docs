---
description: >-
  Datapane has various blocks for adding datasets to your reports, from simple
  tables to interactive drilldowns.
---

# Tables and Data

## Table

The Table component takes a pandas DataFrame and renders an HTML table in your report. 

```python
import datapane as dp
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'A': np.random.normal(-1, 1, 5),
    'B': np.random.normal(1, 2, 5),
})

table = dp.Table(df)
report = dp.Report(table)
report.publish(name='sample_table')
```

If your DataFrame includes [DataFrame Styles](https://pandas.pydata.org/pandas-docs/stable/user_guide/style.html), these will be included in your report. DataFrame styles allows you create custom formatted tables; for instance, to show trends, highlight cells, add bar charts, or display correlations. This table is the best option for displaying multidimensional DataFrames, as `DataTable` will flatten your data.

## DataTable

The DataTable block takes a pandas DataFrame and renders an interactive, sortable, searchable table in your report. By default it displays 10 rows per page, with multiple pages which allows for arbitrarily large datasets. Viewers can also download the table from the website as a CSV file. 

```python
import datapane as dp
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'A': np.random.normal(-1, 1, 5000),
    'B': np.random.normal(1, 2, 5000),
})

table = dp.DataTable(df)
report = dp.Report(table)
report.publish(name='sample_table')
```

{% embed url="https://datapane.com/u/leo/reports/docs-report-block-table/" %}

## Python Dictionary & JSON 

To include Python objects and JSON, we recommend using the `File` block.

{% page-ref page="files-and-images.md" %}

## Big Number

A single number or change can often be the most important thing in a report. The `BigNumber`component allows you to present KPIs, changes, and statistics in a friendly way to your viewers. You can optionally set intent, and pass in numbers or text. 

For full reference on styling your number, see the [API Documentation](https://datapane.github.io/datapane/report.html#datapane.client.api.report.BigNumber).

```python
import datapane as dp

dp.Report(
   dp.BigNumber(
      heading="Number of percentage points", 
      value="84%",
      change="2%",
      is_upward_change=True
   )
)
```

![](../../.gitbook/assets/image%20%28116%29.png)

