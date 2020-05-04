---
description: Datasets are resources of tabular data that can be added to your boards.
---

# Datasets

## Importing Datasets

Datapane supports datasets comprised of tabular data \(such as an Excel sheet or a database table\). Datasets can be imported by:

* Uploading a file using the Web UI
* Uploading a file using the Datapane CLI
* Uploading a pandas DataFrame using the Python API

## Viewing Datasets

Datasets are displayed inside your boards, where they can be viewed, sorted, and filtered.

![](../.gitbook/assets/image%20%2811%29.png)



If you would like to work with a dataset outside of Datapane, you can also download it as a CSV, or use it in Python as a Pandas DataFrame using Datapane's Python library.

By default, only the first 10,000 cells of data are loaded into the dataset viewer. You can load the entire dataset by clicking the respective button.

### Column Types

For all datasets, Datapane infers the type \(or _schema_\) of each column, including the following,

| Type | Description |
| :--- | :--- |
| String | A variable-length string |
| Integer | A positive or negative 64-bit whole number |
| Float | A 64-bit double floating point value |
| Category | A string column is interpreted as a category if more than 10% of the values are repeated |
| Boolean | A true or false value |
| Timestamp | A point in time, can be a date, time, or both date and time |

