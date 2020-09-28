# Configuration & Dependencies

## Configuration

In the previous example, we are deploying a single script and providing configuration through command-line arguments. This works well for simple scripts, but scripts often need other configurations, such as parameters definitions, other files or folders to deploy, and pip requirements.

Datapane allows you to provide a configuration file called `datapane.yaml`. When you `deploy`, Datapane looks for this file automatically. Before we continue, create a project structure with the `script init` command, which creates a sample `datapane.yaml` and a simple script.

```bash
 ~/C/d/d/my-new-proj> datapane script init
Created script 'my-new-proj', edit as needed and upload
 ~/C/d/d/my-new-proj> ls
datapane.yaml dp-script.py
```

We already have a script, so we can delete the sample `dp-script.py` and copy in our own. Because we're replacing the default script, we should specify this in our `datapane.yaml` using the `script` field. Whilst we're there, we can also add the name.

{% code title="datapane.yaml" %}
```yaml
name: stock_plotter
script: financial_report.py # this could also be ipynb if it was a notebook
```
{% endcode %}

See the [API reference](../reference/scripts/datapane.yaml.md) for all the available configuration fields.

## Dependencies and Requirements

Your Python script or notebook may have requirements on external libraries. Datapane supports the ability to add local folders and files, which you can deploy alongside your script, the ability to include `pip` requirements, which are made available to your script when it is run on Datapane, and the ability to specify a Docker container which your script runs in. These are all configured in your `datapane.yaml`.

### Including pip dependencies

In the above example, we may want to use the [yfinance](https://pypi.org/project/yfinance/) library in Python, instead of manually reading a CSV from Yahoo. To do this, we can add it to a `requirements` list in our `datapane.yaml`

{% code title="datapane.yaml" %}
```yaml
...

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

To include this in the deploy, we can add it to `include` 

{% code title="datapane.yaml" %}
```yaml
...

include:
  - stock_scaler.py
```
{% endcode %}

#### Specifying Docker dependencies

By default, scripts on Datapane run in our standard docker container. By default, this includes the following libraries.

```python
seaborn == 0.10.*
altair-recipes ~= 0.8.0
git+https://github.com/altair-viz/altair_pandas@master#egg=altair-pandas
git+https://github.com/altair-viz/pdvega@master#egg=pdvega

scipy == 1.4.*
scikit-learn == 0.22.*
patsy ~= 0.5.1
lightgbm ~= 2.2.3
lifetimes == 0.11.*
lifelines == 0.23.*
./wheels/fbprophet-0.5-py3-none-any.whl
adtk ~= 0.6.2

# data access
sqlalchemy ~= 1.3
psycopg2-binary ~= 2.8
PyMySQL ~= 0.9.3
google-cloud-bigquery[pandas, pyarrow] ~= 1.17
boto3 ~= 1.12.6
requests ~= 2.23.0
ftpretty ~= 0.3.2
pymongo ~= 3.10.1

# misc
dnspython ~= 1.16.0 # requirement for pymongo to connect to certain instances
sh ~= 1.13.0
```

If you want your script to run in your own Docker container, you can specify your own. 

{% hint style="info" %}
This support currently only supports **public** Docker images, and we're adding support for private repositories in the near future. With that in mind, for anything that you don't want to be public, we would recommend continuing to upload private directories through the regular `include` the mechanism, and including OS requirements or pip `requirements.txt` in the Dockerfile.
{% endhint %}

Although you can use any base for your Docker image, we would recommend inheriting off ours. To do this, create a Docker image which inherits from our base image \(`nstack/datapane-python-runner`\) and adds your required dependencies. 

```yaml
from nstack/datapane-python-runner:latest
COPY requirements.txt .
RUN pip3 install --user -r requirements.txt
```

If you build this and push it to Dockerhub, you can then specify it in your `datapane.yaml` as follows:

```yaml
container_image_name: your-image-name
```

When you run a script, it will run inside this Docker container. Note that the first run may take a bit longer, as it needs to pull the image from Dockerhub. Once it pulls it once, it's cached for future runs.

