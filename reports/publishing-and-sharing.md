---
description: Publishing your report so you can share it with others
---

# Publishing and Sharing

{% hint style="info" %}
This feature requires use of the free _Datapane_ hosted platform or a private _Datapane Cloud_ instance
{% endhint %}

## Publish your report

So far we've demonstrated how to build and view reports locally; however, one of the most powerful features of Datapane is the ability to publish your report straight from your code and share it directly with your team or the wider world.

Once you've [logged in](../tut-getting-started.md#authentication) to your chosen Datapane server, call `report.publish(name='Your report name')` in your script and your report will be published to your Datapane instance for viewing online. This will return the URL of the report that you can share.

If we take the report from the previous example, all we need to do is change `.save` to `.publish` and choose a name for our report. We also change the `dp.Table` to use `dp.DataTable` - an interactive table component which supports larger dataframes and additional analysis options. To open the report afterwards automatically, set the `open` boolean parameter.

{% code title="richer\_report.py" %}
```python
import pandas as pd
import altair as alt
import datapane as dp

# download data & group by manufacturer
df = pd.read_csv('https://covid.ourworldindata.org/data/vaccinations/vaccinations-by-manufacturer.csv', parse_dates=['date'])
df = df.groupby(['vaccine', 'date'])['total_vaccinations'].sum().tail(1000).reset_index()

# plot vaccinations over time using Altair
plot = alt.Chart(df).mark_area(opacity=0.4, stroke='black').encode(
    x='date:T',
    y=alt.Y('total_vaccinations:Q'),
    color=alt.Color('vaccine:N', scale=alt.Scale(scheme='set1')),
    tooltip='vaccine:N'
).interactive().properties(width='container')

# tablulate total vaccinations by manufacturer
total_df = df[df["date"] == df["date"].max()].sort_values("total_vaccinations", ascending=False).reset_index(drop=True)
total_styled = total_df.style.bar(subset=["total_vaccinations"], color='#5fba7d', vmax=total_df["total_vaccinations"].sum())

# embed into a Datapane Report
report = dp.Report(
    "## Vaccination Report",
    dp.Plot(plot, caption="Vaccinations by manufacturer over time"),
    dp.Table(total_styled, caption="Current vaccination totals by manufacturer"),
    # dp.Table(df, caption="Initial Dataset")
    dp.DataTable(df, caption="Initial Dataset")
)
# report.save(path='report.html', open=True)
report.publish(name='Covid Vaccinations',
               description="Covid Vaccinations report, using data from ourworldindata", open=True)
```
{% endcode %}

Once published, you can share the link with others so they can view your report and comment on it. Public reports created are viewable and shareable by default. In future sections, we will also explore how to embed your report into a range of other platforms so you can share it with a wider audience.

## Report Visibility and Sharing

Datapane provides a free server where reports can be uploaded, either privately or published publicly to our community platform. It includes two private reports per user, and unlimited public reports. Reports are private by default. To publish a report publicly, just set the report's visibility to `dp.Visibility.PUBLIC`, e.g.

```python
report.publish(name='Covid Vaccinations', open=True, visibility=dp.Visibility.PUBLIC)
```

Private reports can be shared securely, just press the share button on the published report page and add collaborators via email. Collaborators will receive an email containing a secure signed URL which allows them to view your report for 48 hours.

![](../.gitbook/assets/image%20%28118%29.png)

{% hint style="info" %}
\_\_[_Datapane Cloud_ ](../datapane-enterprise/tut-deploying-a-script.md)provides additional options to share reports securely across your company.
{% endhint %}

