# Libraries and Dependencies

Your Python script or notebook may have requirements on external libraries. Datapane supports the ability to add local folders and files, which you can deploy alongside your script, the ability to include `pip` requirements, which are made available to your script when it is run on Datapane, and the ability to specify a Docker container which your script runs in. These are all configured in your `datapane.yaml`.

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

If you want your script to run in your own Docker container, you can specify your own. 

{% hint style="info" %}
This support works with both public and private Docker images, so you can add private internal libraries, for instance, to your Docker image and just add login credentials for the Docker registry to your _Datapane Enterprise_ settings
{% endhint %}

Although you can use any base for your Docker image, we would recommend inheriting off ours. To do this, create a Docker image which inherits from the our base image \(`nstack/datapane-python-runner`\) and adds your required dependencies. 

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

