---
description: >-
  Datapane has various blocks for adding datasets to your reports, from simple
  tables to interactive drilldowns.
---

# Tables, Data and Big Numbers

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
report.upload(name='sample_table')
```

{% embed url="https://datapane.com/u/datapane/reports/sample-table/" %}

If your DataFrame includes [DataFrame Styles](https://pandas.pydata.org/pandas-docs/stable/user_guide/style.html), these will be included in your report. DataFrame styles allows you create custom formatted tables; for instance, to show trends, highlight cells, add bar charts, or display correlations. 

{% embed url="https://datapane.com/u/datapane/reports/df-styled-table/?utm\_medium=embed&utm\_content=viewfull" %}

{% hint style="info" %}
Table is the best option for displaying multidimensional DataFrames, as `DataTable` will flatten your data.
{% endhint %}

## DataTable

The DataTable block takes a pandas DataFrame and renders an interactive, sortable, searchable table in your report, along with advanced analysis options such as [Sanddance](https://www.microsoft.com/en-us/research/project/sanddance/) and [Pandas Profiling](https://pandas-profiling.github.io/pandas-profiling/). It supports large datasets and viewers can also download the table from the website as a CSV or Excel file.

{% hint style="info" %}
DataTable works for reports uploaded to Datapane.com as well as locally saved reports.
{% endhint %}

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
report.upload(name='sample_table')
```

{% embed url="https://datapane.com/u/datapane/reports/sample-datatable/" %}

## Python Dictionary & JSON 

To include Python objects and JSON, we recommend using the `File` block.

{% page-ref page="files-and-images.md" %}

## Big Number

A single number or change can often be the most important thing in a report. The `BigNumber`component allows you to present KPIs, changes, and statistics in a friendly way to your viewers. You can optionally set intent, and pass in numbers or text. 

For full reference on styling your number, see the [API Documentation](https://datapane.github.io/datapane/report.html#datapane.client.api.report.BigNumber).

{% tabs %}
{% tab title="Python" %}
```python
import datapane as dp

dp.Report(
   dp.Group(columns=2,
      dp.BigNumber(
         heading="Number of percentage points", 
         value="84%",
         change="2%",
         is_upward_change=True
      ),
      dp.BigNumber(
         heading="Simple Statistic", 
         value=100
      )
   )
)
```
{% endtab %}

{% tab title="Text Report" %}
    ```datapane
    block: Group
    columns: 2
    blocks: 
      - block: BigNumber
        heading: Number of percentage points
        value: 84%
        change: 2%
        is_upward_change: True
      - block: BigNumber
        heading: Simple Statistic
        value: 100
    ```
{% endtab %}
{% endtabs %}

{% embed url="https://datapane.com/u/datapane/reports/big-number/" %}



