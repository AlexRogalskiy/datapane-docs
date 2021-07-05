---
description: Uploading your report so you can share it with others
---

# Uploading and Sharing

{% hint style="info" %}
This feature requires use of the free _Datapane_ hosted platform or a private _Datapane Teams_ instance
{% endhint %}

## Upload your report

So far we've demonstrated how to build and view reports locally; however, one of the most powerful features of Datapane is the ability to upload your report straight from your code and share it directly with your team or the wider world.

Once you've [logged in](../../tut-getting-started.md#authentication) to your chosen Datapane server, call `report.upload(name='Your report name')` in your script and your report will be uploaded to your Datapane instance for viewing online. This will return the URL of the report that you can share.

{% hint style="info" %}
`Report.upload` was previously called `Report.publish.` The old syntax will still work but has been deprecated. 
{% endhint %}

If we take the report from the previous example, all we need to do is change `.save` to `.upload` and choose a name for our report. We also change the `dp.Table` to use `dp.DataTable` - an interactive table component which supports larger dataframes and additional analysis options. To open the report afterwards automatically, set the `open` boolean parameter.

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
report.upload(name='Covid Vaccinations',
               description="Covid Vaccinations report, using data from ourworldindata", open=True)
```
{% endcode %}

Once uploaded, you can share the link with others so they can view your report and comment on it. Public reports created are viewable and shareable by default. In future sections, we will also explore how to embed your report into a range of other platforms so you can share it with a wider audience.

## Report Visibility and Sharing

Datapane provides a free community platform where reports can be uploaded and shared publicly. Reports are unlisted by default, which means the following: 

* They won't appear on your profile
* They won't appear in search results
* They won't appear on the Explore pages
* Anyone with the URL can access it. This is not a truly private mechanism, so make sure you aren't uploading very sensitive information. 

You can also choose to publish your report to the Datapane community, meaning it will be featured on our [Explore](https://datapane.com/explore/) page and potentially our social media accounts. This is a great way to gain an audience and receive feedback on your reports! 

{% hint style="info" %}
\_\_[_Datapane Teams_](../../datapane-teams/introduction.md) __provides additional options to share reports securely across your company.
{% endhint %}

