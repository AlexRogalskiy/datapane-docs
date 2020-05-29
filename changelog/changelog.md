# Changelog

## 0.6.0 - 29-05-2020

### Key Features

* A new script interface, which has the output report inline with the script when it is run
* Ability to see filter reports by public, private, domain, and those owned by you
* Versioning and naming: reference blobs, variables, and scripts by name, and deploy multiple versions of them
* Local report generation: generate local reports as standalone HTML files. These can be emailed, hosted as static sites, and it's also considerably quicker for testing locally.
* Better logging, so you can see the STDOUT/STDERR of scripts in the web UI and CLI when you run them
* Support for running scripts in your own custom docker image
* Multiple stability and performance improvements
* New logo!

### Required changes

Along with updating your Datapane local library \(`pip3 install -U datapane`\), the following code changes are required:

#### Scripts

Due to the new script versioning system, we would recommend redeploying all scripts on the platform. Scripts now also have a name property, instead of title. Scripts deployed the same name will increment the version.

```text
name: my_script
```

#### User Variables & Blobs

User variables and blobs also require recreation and a Python API change to make use of the new naming and versioning API. They can then be fetched by this name using the `get` method.

```text
d = dp.Blob.get(name='my_name')
```

{% hint style="info" %}
All blob/secret operations around versioning and naming are available on the CLI, which has inline documentation.
{% endhint %}

#### Permission update: DOMAIN → ORG

`DOMAIN` has been renamed to `ORG` and should be updated accordingly in your code.

#### Publishing reports

The report API has changed in the following ways.

1. You no longer need to call `.create()` on components, such as Plot, Table, Report, etc., you can just pass the object to the constructor \(i.e. `dp.Table(df)`\)
2. If you are creating a report on Datapane itself \(either from your local machine, or inside your script\), you call `publish`, which takes an explicit name.

```text
r = dp.Report(dp.Table(df))
r.publish(name='my_report', open=True)
```

1. If you want to render a report locally, you can use the `save` method. The output of this is a standalone HTML file which does not have network requirements.

```text
r = dp.Report(dp.Table(df))
r.save(path='report.html', open=True)
```

### Docker image support

Datapane scripts now have the option to run inside a user-provided Docker image. This currently supports **public** Docker images, and we're adding support for private repositories in the near future. With that in mind, for anything that you don't want to be public, we would recommend continuing to upload private directories through the regular `include` mechanism, and including OS requirements or pip `requirements.txt` in the Dockerfile.

To do this, create a Docker image which inherits from the our base image and adds your required dependencies. Although you can use any base for your Docker image, we would recommend inhereting off ours for the time being.

For instance:

```text
from datapane/python3-default:0.0.0
COPY requirements.txt .
RUN pip3 install --user -r requirements.txt
```

If you build this and push it to Dockerhub, you can then specify it in your `datapane.yaml` as follows:

```text
container-image: your-image-name
```

When you run a script, it will run inside this Docker container. Note that the first run may take a bit longer, as it needs to pull the image from Dockerhub. Once it pulls it once, it's cached for future runs so will be instantaneous.



