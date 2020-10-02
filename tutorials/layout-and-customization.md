# Layout and Customization

Although you are you probably analyzing data in a list of sequential steps, it may not the best way to present results -- especially if you are sharing with people who are used to a traditional dashboard or BI tool. Datapane's report library provides components which allow for more flexible grid-style layouts, allowing you to create custom interfaces without HTML or CSS.

## Building grids

If you can pass a list of components \(such as `Plot` and `Table`\) to a Report, they are -- by default -- laid out in a single column with a row per component. If you would like to customize the rows and columns, Datapane provides a `Blocks` component which takes a list of components and a number of columns and/or rows and lays them out in a grid.

If we take the example in the earlier tutorial, but want to lay the plot and dataset side-by-side, we can use specify this using `Blocks` and specifying the number of columns.

{% code title="simple\_report.py" %}
```python
import pandas as pd
import altair as alt
import datapane as dp

dataset = pd.read_csv('https://covid.ourworldindata.org/data/owid-covid-data.csv')
df = dataset.groupby(['continent', 'date'])['new_cases_smoothed_per_million'].mean().reset_index()

plot = alt.Chart(df).mark_area(opacity=0.4, stroke='black').encode(
    x='date:T',
    y=alt.Y('new_cases_smoothed_per_million:Q', stack=None),
    color=alt.Color('continent:N', scale=alt.Scale(scheme='set1')),
    tooltip='continent:N'
).interactive().properties(width='container')

dp.Report(
    dp.Blocks(
        dp.Plot(plot), 
        dp.Table(df),
        columns=2
    )
).publish(name='covid_report', open=True)
```
{% endcode %}

![](../.gitbook/assets/image%20%28103%29.png)

{% hint style="info" %}
As `Blocks` are components themselves, it means they are composable, and you can create more custom layers of nested blocks.
{% endhint %}

## Customizing Width

By default, reports appear in a portrait orientation. This works well for many reports, but may not be desired if you have multiple columns of blocks. You can opt to make your report full-width by setting the `full_width` property to `True` on the `Report` object. This allows the creation of rich, dashboard-like layouts such as the following:  


![](../.gitbook/assets/image%20%28105%29.png)



