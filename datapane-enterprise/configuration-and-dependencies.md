# Configuration and Dependencies

Your Python script or notebook may have requirements on external libraries and require certain environment values. Datapane supports the ability to,

* configure the environment your script runs in
* add local folders and files, which you can deploy alongside your script
* include `pip` requirements, which are made available to your script when it is run on Datapane
* specify a Docker container which your script runs in. 

These are all configured in your `datapane.yaml`.

{% hint style="info" %}
For complex dependencies and internal libraries, we highly recommend creating a Docker container.
{% endhint %}

## Environment Variables

You may require certain environment variables to be set when running your script - these could be static values such as the number of iterations for a algorithm, or a dynamic, secret value, such as a database password.

{% hint style="info" %}
Environment variables are different to [parameters](reference/scripts/parameters.md), in that they can not be viewed or configured by the script runner
{% endhint %}

Your list of environment values can be configured in your `datapane.yaml` as follows,

```yaml
env:
  - name: ENV_VAR
    value: env_value
```

This will set an environment value called `ENV_VAR` with the value `env_value` that can be accessed from python as usual - e.g. `os.environ["ENV_VAR"]` .

Such environment values are static within your script, you may also need make use of more dynamic and private values, such as cloud or database credentials. To support this, Datapane also supports referencing Datapane [User Variables / Secrets](reference/variables.md) directly from your `env` settings and loading them into your environment.

```yaml
env:
  - name: SECRET_VAR
    variable: owner/var_name  # owner is required here
```

This will automatically look for a variable called `var_name`, created by `owner`, and make it available in your script as the environment variable `SECRET_VAR`. Every time the variable is updated, i.e. if credentials change, your script will automatically use the latest version. See the following for more information on Variables

{% page-ref page="reference/variables.md" %}

## Python dependencies

If we were building a reporting tool to pull down financial data, we may want to use the [yfinance](https://pypi.org/project/yfinance/) library in Python. To do this, we could add it to a `requirements` list in our `datapane.yaml`

{% code title="datapane.yaml" %}
```yaml
...

requirements:
  - yfinance
```
{% endcode %}

## Additional files and folders

Additionally, we may want to include a local Python folder or file. Imagine we have a separate Python file, `stock_scaler.py` which helps us scale the values of our stocks, and which we want to use in our script, or a folder of SQL scripts which we want to use in Python.

```text
~/C/d/d/my-new-proj> ls
dp-script.py datapane.yaml stock_scaler.py
```

To include this in the deploy, we could add it to `include` 

{% code title="datapane.yaml" %}
```yaml
...

include:
  - stock_scaler.py
```
{% endcode %}

## Docker dependencies

By default scripts on Datapane run using our default Docker image, which, in addition to including Datapane and its supported visualisation libraries, includes the following libraries,

```python
# datapane cli includes plotting and basic DS libs, e.g. pandas

# additional visualisations
seaborn == 0.11.*
altair-recipes ~= 0.9.0
git+https://github.com/altair-viz/altair_pandas@master#egg=altair-pandas

# analytics libraries
scipy == 1.5.*
scikit-learn == 0.23.*
patsy ~= 0.5.1
lightgbm ~= 3.1.0
lifetimes == 0.11.*
lifelines == 0.25.*
./wheels/fbprophet-0.7.1-py3-none-any.whl
adtk ~= 0.6.2

# data access
sqlalchemy ~= 1.3
psycopg2-binary ~= 2.8
PyMySQL ~= 0.10.1
google-cloud-bigquery[pandas, pyarrow] ~= 2.4.0
boto3 ~= 1.16.19
requests ~= 2.25.0
ftpretty ~= 0.3.2
pymongo ~= 3.11.0

# misc
dnspython ~= 2.0.0
sh ~= 1.13.0
```

If you want your script to run in your own Docker container, you can specify your own. Although you can use any base for your Docker image, we recommend inheriting from ours. To do this, create a Docker image that inherits from the our base images \(such as `datapane/dp-runner-py38`\) and add your required dependencies - see [https://hub.docker.com/u/datapane](https://hub.docker.com/u/datapane) for all our images.

```yaml
from datapane/dp-runner-py38:latest
COPY requirements.txt .
RUN pip3 install --user -r requirements.txt
```

If you build this and push it to Dockerhub, you can then specify it in your `datapane.yaml` as follows:

```yaml
container_image_name: your-image-name
```

When you run a script, it will run inside this Docker container. Note that the first run may take a bit longer, as it needs to pull the image from DockerHub. Once it pulls it once, it's cached for future runs.

{% hint style="info" %}
We support both public and private Docker images, so you can add private internal libraries, for instance, to your Docker image. You can add your registry credentials to your Datapane Server from the server settings page.
{% endhint %}

