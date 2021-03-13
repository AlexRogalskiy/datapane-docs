# Script and Jupyter Deployment

## Introduction

_Datapane for Teams_ provides a Script Runner, which allows you to deploy Jupyter Notebooks or Python scripts and run them in the cloud with parameters. This means you can generate reports in an automated fashion, in addition to creating them in your local environment.

Once you deploy your script or notebook, it can be run in three ways:

#### Web Forms

Scripts can be run with parameters entered through friendly web forms, which allows you to create interactive, self-service reporting tools for stakeholders.

{% page-ref page="tut-parameterising-a-script.md" %}

#### On a schedule

Scripts can generate and update reports on a schedule, allowing you to create "live" dashboards and automated reports.

{% page-ref page="scheduling.md" %}

#### Through an API

You can trigger report generation through our API, which allows you to generate reports in response to events from other tools, such as Slack and Teams, or your own product.

## Deploying a script

If you have a local Python script or notebook which creates a report using Datapane's `Report.publish` method \(see [Creating a Report](../reports/tut-creating-a-report.md)\), you can deploy it to Datapane using the CLI. Let's take our COVID script from before deploy it using Datapane's CLI. The report we publish in this code will be returned to the user when they run our script using the Datapane web interface.

{% hint style="info" %}
We recommend creating only one report per script. As many can be created as needed; however, only the last one in each script will be tracked in the web interface.
{% endhint %}

{% code title="simple\_script.py" %}
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

To deploy it, use Datapane's CLI.

```bash
datapane script deploy --script=simple_script.py --name=covid_script
Uploaded simple_script.py to https://acme.datapane.net/leo/scripts/covid_script/
```

This makes your script available on your private instance, where you can share it with other users. If you send them your script, they are able to generate the report from the previous example dynamically by hitting the **Run** button.

![](../.gitbook/assets/image%20%28105%29.png)

Every time the script is run, it pulls new COVID data and generates a fresh report, which can be shared or embedded.

![](../.gitbook/assets/image%20%28113%29.png)

## Configuration

In the previous example, we are deploying a single script and providing the name and file location through command-line arguments. This works well for simple scripts, but scripts often need other configuration, such as [parameter definitions](tut-parameterising-a-script.md), other files or folders to deploy, and Python or OS requirements.

Datapane allows you to provide a configuration file called `datapane.yaml`. When you run `deploy`, Datapane looks for this file automatically. Before we continue, create a project structure with the `script init` command, which creates a sample `datapane.yaml` and a simple script.

```bash
 ~/C/d/d/my-new-proj> datapane script init
Created script 'my-new-proj', edit as needed and upload
 ~/C/d/d/my-new-proj> ls
datapane.yaml dp-script.py
```

We already have a script from our previous example, so we can delete the sample `dp-script.py` and copy in our own. Because we're replacing the default script, we should specify the filename of our script in `datapane.yaml` using the `script` field. Whilst we are there, we can also choose a name.

{% code title="datapane.yaml" %}
```yaml
name: covid_script
script: simple_script.py # this could also be ipynb if it was a notebook
```
{% endcode %}

If we run `datapane script deploy` in this directory, Datapane will deploy our code with the configuration in `datapane.yaml`. Because we have given the script the same name as our previous one, this will create version 2 of `covid_script`. 

{% hint style="info" %}
Like reports, scripts can have multiple versions, allowing you to update and iterate on a single Python project.
{% endhint %}

In the next section, we will explore adding parameters to your script, to enable reports to be generated dynamically based on user inputs.

