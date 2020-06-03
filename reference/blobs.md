---
description: >-
  Blobs are files and objects which you can store on Datapane and use in your
  scripts.
---

# Blobs

It is often neccesary to make use of non-code assets such as datasets, models, or files when generating reports. In many situations, deploying these alongside your script is not ideal.

1. If they are deployed on a **different cadence** to your script; for instance, you want to make use of a model which is trained on a daily cadence, even though the code of your script remains static.
2. If they are deployed from a **different environment** than your script; for instance, you may [train a model on Sagemaker](../guides/guide-creating-ml-model-form.md) and want to use it in your script.
3. If they are **large**, and re-uploading them each time you deploy your script is cumbersome.

For these use-cases, Datapane provides a Blob API which allows you to upload files from any Python or CLI environment, and access them inside your scripts or through the CLI.

## CLI

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

### Upload

Upload a DataFrame, file, or object from Python. All upload operations can be passed a `visibility` parameter of `ORG`, `OWNER`, or `PUBLIC`.

```python
import datapane as dp

# Upload a DataFrame
b = dp.Blob.upload_df(df, name='my_df')

# Upload a file
b = dp.Blob.upload_file("~/my_dataset.csv", name='my_ds')

# Upload an object
b = dp.Blob.upload_obj([1,2,3], name='my_list')
```

### Download

Download a DataFrame, file, or object. All download operations can be passed a `version` parameter.

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



