# Data, Model, and Asset Storage

It is often neccesary to make use of non-code assets such as datasets, models, or files when generating reports. In many situations, deploying these alongside your script is not ideal.

1. If they are deployed on a **different cadence** to your script; for instance, you want to make use of a model which is trained on a daily cadence, even though the code of your script remains static.
2. If they are deployed from a **different environment** than your script; for instance, you may [train a model on Sagemaker]() and want to use it in your script.
3. If they are **large**, and re-uploading them each time you deploy your script is cumbersome.

For these use-cases, Datapane provides a Blob API which allows you to upload files from any Python or CLI environment, and access them inside your scripts or through the CLI.

For API documentation of the Blob functionality, see the following documentation:

{% page-ref page="../reference/blobs.md" %}



