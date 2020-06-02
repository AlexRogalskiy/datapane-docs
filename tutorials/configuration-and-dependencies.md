# Configuration & Dependencies

## Configuration

In the previous example, we are deploying a single script and providing configuration through command-line arguments. This works well for simple scripts, but scripts often need other configuration, such as parameters definitions, other files or folders to deploy, and pip requirements.

Datapane allows you to provide a configuration file called `datapane.yaml`. When you `deploy`, Datapane looks for this file automatically. Before we continue, create a project structure with the `script init` command, which creates a sample `datapane.yaml` and a simple script.

```bash
 ~/C/d/d/my-new-proj> datapane script init
Created script 'my-new-proj', edit as needed and upload
 ~/C/d/d/my-new-proj> ls
datapane.yaml dp-script.py
```

We already have a script, so we can delete the sample `dp-script.py` and copy in our own notebook or script. Because we're replacing the default script, we should specify this in our `datapane.yaml` using the `script` field. Whilst we're there, we can also add the name of our script.

We already have a script, so we can delete the sample `dp-script.py` and copy in our own notebook. Because we're replacing the default script, we should specify this in our `datapane.yaml` using the `script` field. Whilst we're there, we can also add the name and title for our script.

{% code title="datapane.yaml" %}
```yaml
name: stock_plotter
script: financial_report.py # this could also be ipynb if it was a notebook
```
{% endcode %}

See the [API reference](../reference/scripts/datapane.yaml.md) for all the available configuration fields.

## Dependencies and Requirements

Your Python script or notebook may have requirements on external libraries. Datapane supports both the ability to add local folders and files, which you can deploy alongside your script, the ability to include `pip` requirements, which are made available to your script when it is run on Datapane, and the ability to specify a Docker container which your script runs in. These are all configured in your `datapane.yaml`.

### Including pip dependencies

In the above example, we may want to use the [yfinance](https://pypi.org/project/yfinance/) library in Python, instead of manually reading a CSV from Yahoo. To do this, we can add it to a `requirements` list in our `datapane.yaml`

{% code title="datapane.yaml" %}
```yaml
name: stock_plotter
script: financial_report.py # this could also be ipynb if it was a notebook

requirements:
  - yfinance
```
{% endcode %}

### Including additional files and folders

Additionally, we may want to include a local Python folder or file. Imagine we have a separate Python file, `stock_scaler.py` which helps us scale the values of our stocks, and which we want to use in our notebook.

```text
~/C/d/d/my-new-proj> ls
dp-script.ipynb datapane.yaml stock_scaler.py
```

To include this in the deploy, we can add it to `include` .

{% code title="datapane.yaml" %}
```yaml
name: stock_plotter
script: financial_report.py # this could also be ipynb if it was a notebook
parameters:
  - name: tickers
    type: list
    default: ['GOOG', 'ZM']
    description: "Tickers to plot"

requirements:
  - yfinance
include:
  - stock_scaler.py
```
{% endcode %}

#### Specifying Docker dependencies

By default, scripts on Datapane run in our standard docker container. By default, this includes the following libraries:

