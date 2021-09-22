---
description: Uploading your report so you can share it with others
---

# Uploading and Sharing

{% hint style="info" %}
This feature requires use of the free _Datapane Studio_ hosted platform or a private _Datapane Teams_ instance
{% endhint %}

## Upload your report

So far we've demonstrated how to build and view reports locally; however, one of the most powerful features of Datapane is the ability to upload your report straight from your code and share it directly with your team or the wider world.

Once you've [logged in](../../tut-getting-started.md#authentication) to your chosen Datapane server, call `report.upload(name='Your report name')` in your script and your report will be uploaded to your Datapane instance for viewing online. This will return the URL of the report that you can share.

{% hint style="info" %}
`Report.upload` was previously called `Report.publish.` The old syntax will still work but has been deprecated. 
{% endhint %}

Let's see an example report uploaded to Datapane.com, with the ```upload```syntax. To open the report afterwards automatically, set the `open` boolean parameter.

{% code title="richer\_report.py" %}
```python
import altair as alt
from vega_datasets import data
import datapane as dp

source = data.cars()

plot1 = alt.Chart(source).mark_circle(size=60).encode(
    x='Horsepower', 
    y='Miles_per_Gallon', 
    color='Origin',
    tooltip=['Name', 'Origin', 'Horsepower', 'Miles_per_Gallon']
).interactive()

report  = dp.Report(
    dp.Plot(plot1),
    dp.DataTable(source)
)

# report.save(path='report.html', open=True)
.upload(name="My first report")

```
{% endcode %}

Once uploaded, you can share the link with others so they can view your report and comment on it. Public reports created are viewable and shareable by default. In future sections, we will also explore how to embed your report into a range of other platforms so you can share it with a wider audience.

## Report Visibility and Sharing

Datapane Studio provides a free platform for uploading reports, with the following options for report visibility:  

1. **Default \(unlisted\):** You have unlimited default reports, which allow anyone with the URL to access them, but they won't appear on your profile or in search results. This is not a truly private system, so make sure you aren't uploading very sensitive information.
2. **Portfolio:** You can also choose to add the report to your public portfolio \([see example](https://datapane.com/u/johnmicahreid/)\) which you can share with potential employers/readers. This is a great way to gain an audience and receive feedback on your reports! 
3. **Private:** Your Community account comes with a limited number of private reports if you need to share data confidentially within your organization. Private reports are shared through the [Report Notifications](report-notifications.md) mechanism. 

You can set these via the Report Settings page, or in Python as follows: 

```python
import datapane as dp

report = dp.Report(...)

# Default Report
report.upload(name='default report', visibility = dp.Visibility.DEFAULT)

# Portfolio Report
report.upload(name='portfolio report',visibility = dp.Visibility.PORTFOLIO)

# Private Report
report.upload(name='report',visibility = dp.Visibility.PRIVATE)
```

{% hint style="info" %}
\_\_[_Datapane Teams_](../../datapane-teams/introduction.md) __provides additional options to share reports securely across your company.
{% endhint %}

