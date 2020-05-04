---
description: datapane.yaml is the configuration file for your script.
---

# datapane.yaml

Inside your `datapane.yaml`, you can set the following fields.

| Field | Description |
| :--- | :--- |
| public | Whether or not your script is public \(boolean, default false\) |
| title | The user-facing title of your script |
| script | A custom path to your script  |
| name | The referenceable, unique name of your script  |
| parameters | A list of parameters for your script \(see below\) |
| include | Additional files or folders to **include** in the script deployment |
| exclude | Files or folders to **exclude** in the script deployment |
| requirements | pip requirements for your script |

## Parameters

### Parameters Options

When you define your list of parameters, all can include the following options.

| Field | Required | Description |
| :--- | :--- | :--- |
| name  | True | The name of the parameter. This must be a combination of lower case letters, numbers, and dashes and must be unique to your script. |
| type | True | The type of the parameter \(see options below\) |
| description  | False | A description which is presented to the user when they run your form. |
| required  | False | Whether your parameter is required or optional. Defaults to true. |

### Parameter Form Fields

The `type` of your parameter and other settings dictates how it is presented in your form. Datapane supports the following form fields.

| **Form Field** | **`type`** | Extra options | **Default value**  |
| :--- | :--- | :--- | :--- |
| Text input | `string` |  | `””` |
| Slider | `integer` | Both `min` and`max`properties set. | `min` value |
| Integer input | `integer` | One of `min`/`max` not set | `undefined` |
| Float input | `float` |  | `undefined` |
| Boolean input | `boolean` |  | `false` |
| Dropdown | `enum` |  | The first choice supplied |
| Date picker | `date` |  | The current date |
| Time picker | `time` |  | The current time |
| Date and time picker | `datetime` |  | The current date and time |
| List Input | `list` | If provides with no `choices` | `[]` |
| Multi Select Input | `list` | If provided with `choices`: an array of strings and numbers. | `[]` |

An example with all implemented form fields can be found here, and is available on GitHub [here](https://github.com/datapane/datapane-demos/blob/master/all-params/datapane.yaml).

