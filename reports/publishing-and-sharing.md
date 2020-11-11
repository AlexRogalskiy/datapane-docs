---
description: Publishing your report so you can share it with others
---

# Publishing and Sharing

{% hint style="info" %}
This feature requires use of the free _Datapane Public_ hosted platform or a private _Datapane for Teams_ instance
{% endhint %}

## Publish your report

So far we've demonstrated how to build and view reports locally; however, one of the most powerful features of Datapane is the ability to publish your report straight from your code and share it directly with your team or the wider world.

Once you've [logged in](../tut-getting-started.md#authentication) to your chosen Datapane server, call `report.publish(name='Your report name')` in your script and your report will be published to your Datapane instance for viewing online. This will return the URL of the report that you can share.

If we take the report from the previous example, all we need to do is change `.save` to `.publish` and choose a name for our report. To open the report afterwards automatically, set the `open` boolean parameter. Publishing a report prints the URL to standard out, and returns a report object.

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

report = dp.Report(
    dp.Plot(plot), 
    dp.Table(df)
)
# report.save(path='report.html', open=True)
report.publish(name='Covid Report', open=True)
```
{% endcode %}

## Sharing

Once published, you can share the link with others so they can view your report and comment on it. 

You can control who can access your report by setting the `visibiliy` option when running `report.publish` . Datapane Public is created as a public reports repository, hence reports are created as public and shareable by default, however you can mark your report as unlisted to remove it from indexing by setting `visibility="UNLISTED"`.

{% hint style="info" %}
_Datapane for Teams_ provides additional visibility options to share reports with team members only, or keep them fully private, along with expiring access tokens to share securely with select outside members - ideal for users who require more security over their data and analysis.
{% endhint %}

In future sections, we will explore how to embed your report into a range of other platforms so you can share it with a wider audience.

