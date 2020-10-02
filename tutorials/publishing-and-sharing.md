# Publishing and Sharing

{% hint style="info" %}
This feature requires use of the free _Datapane Public_ hosted platform or a private _Datapane for Teams_ instance
{% endhint %}

## Publish your report

So far we've demonstrated how to build and view reports locally; however, one of the most powerful features of Datapane is the ability to publish your report straight from your code and share it directly with your team or the wider world.

Once you've [logged in](../tut-getting-started.md#authentication) to your chosen Datapane server, call `publish(name='your-report-name')` in your script and your report will be published to your Datapane instance for viewing online. This will return the URL of the report that you can share.

If we take the report from the previous example, all we need to do is change `.save` to `.publish` and choose a name for our report.

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
    dp.Plot(plot), 
    dp.Table(df)
).publish(name='covid_report', open=True)
```
{% endcode %}

We can view the report demonstrated in this tutorial on Datapane[ here](https://acme.datapane.com/reports/Bj3LQ7Q/), and you can we have examples from the community on our [gallery page](www.datapane.com/gallary/). 

### Versioning

Reports published to _Datapane Public_ and _Datapane for Teams_ are versioned. If you publish a report with the **same name** as a previous report, it will increment the version. Using the Datapane web UI, you can view previous versions of reports. 

![](../.gitbook/assets/image%20%28110%29.png)

## Public sharing <a id="nextshare-stepsyour-report"></a>

{% hint style="info" %}
If you are using Datapane for Teams, your reports are **not public** by default and can be private or shared only with your organisation. Please see the [relevant section](../datapane-teams/untitled.md) for more information.
{% endhint %}

Once you have published your report, you can copy the link and share it with others.

![](../.gitbook/assets/image%20%2899%29.png)

In the next section, we will explore how to embed your report into a range of other platforms. 

