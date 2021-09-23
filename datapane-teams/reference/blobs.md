---
description: >-
  Blobs are files and objects which you can store on Datapane and use in your
  scripts.
---

# Blobs

{% hint style="success" %}
Please see the [Blob API Reference](https://datapane.github.io/datapane/teams.html#datapane.client.api.teams.Blob) for more details.
{% endhint %}

It is often necessary to make use of non-code assets such as datasets, models, or files when generating reports. In many situations, deploying these alongside your script is not ideal.

1. If they are deployed on a **different cadence** to your script; for instance, you want to make use of a model which is trained on a daily cadence, even though the code of your script remains static.
2. If they are deployed from a **different environment** than your script; for instance, you may train a model on Sagemaker and want to use it in your script.
3. If they are **large**, and re-uploading them each time you deploy your script is cumbersome.

For these use-cases, Datapane provides a Blob API which allows you to upload files from any Python or CLI environment, and access them inside your scripts or through the CLI. See the Python [API Docs](https://datapane.github.io/datapane/teams.html#datapane.client.api.teams.Blob) for more information on using Blobs.

{% hint style="info" %}
This feature is only available on Datapane Teams plans at present
{% endhint %}

## **CLI**

### `upload`

Upload a file and return an id and a url which you can use to retrieve the blob.

```text
datapane blob upload <name> <filename>
```

### `download`

Download a blob and save it to a file.

```text
datapane blob download <name> <filename> [--version=version]
```

## Python 

### `upload_df, upload_file, upload_obj`

#### Parameters

All upload methods take the object to upload as the first parameter. Depending on the method, this can be a file path, DataFrame, or a Python object. 

All methods have the additional parameters:

| Parameter | Description | Required |
| :--- | :--- | :--- |
| `name` | The value of your variable | True |

```python
import datapane as dp

# Upload a DataFrame
b = dp.Blob.upload_df(df, name='my_df')

# Upload a file
b = dp.Blob.upload_file("~/my_dataset.csv", name='my_ds')

# Upload an object
b = dp.Blob.upload_obj([1,2,3], name='my_list')
```

### `download_df, download_file, download_obj`

Download a DataFrame, file, or object. All download operations have the following parameters:

|  |  | Required |
| :--- | :--- | :--- |
| `name` | The name of your blob | True |
| `owner` | The owner of the blob.  | False |

{% hint style="warning" %}
If you want other people inside your organization to run your scripts which access a blob which you created, you must specify yourself as the `owner` in this method. When someone runs your script, it runs under their name, and if you do not set an explicitly specify the `owner` , it will try and look for the blob under their name and fail.

```python
dp.Blob.get(name='foo', owner='linus')
```
{% endhint %}

```python
import datapane as dp

# Download a DataFrame
blob = dp.Blob.get(name="blob_id")

# Download a DataFrame
b = blob.download_df()

# Download a file
b = blob.download_file("~/my_dataset.csv")

# Download an object
b = blob.download_obj()
```

If your teammates within your private workspace want to access your blob, they need to specifying the name of the blob and your username in `owner`

```python
blob = dp.Blob.get(name='myblob', owner='khuyentran')

# Retrieve blob
b = blob.download_df() # Or download_file(), download_obj()
```

Now others can use your blob for their code!

