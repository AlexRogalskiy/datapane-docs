---
description: 'Variables are secret, encrypted values which you can share between scripts.'
---

# Variables

{% hint style="success" %}
Please see the [Variable API Reference](https://datapane.github.io/datapane/teams.html#datapane.client.api.teams.Variable) for more details.
{% endhint %}

## Overview

Scripts often contain variables such as database keys and passwords, which you do not want embedding in your source code and visible to the outside world. Datapane's `Variable` object provides a safe and secure way to create, store, and retrieve values which your scripts require. See the Python [API Docs](https://datapane.github.io/datapane/teams.html#datapane.client.api.teams.Variable) for more information on using Variables.

## CLI

### `create`

#### Parameters

| Parameter | Description | Required |
| :--- | :--- | :--- |
| `name` | The name of your variable | True |
| `value` | The value of your variable | True |
| `visibility` | The visibility setting \(`ORG`, `PRIVATE`, or `PUBLIC`\) | False |

#### Response

A Variable object

#### Description & Example

Create a new user variable. Adding multiple versions of variables with the same name will create new versions.

By default, variables are private to the creator's account, but they can be shared across an organization. To set visibility, use the `--visibility` flag \(or `visibility` field in Python\) with `OWNER_ONLY` or `ORG`. 

{% hint style="warning" %}
If you want other people inside your organisation to run your scripts, your variable must be `ORG`, as scripts are executed under their user account.
{% endhint %}

{% tabs %}
{% tab title="CLI" %}
```text
~/> datapane variable add <variable_name> <variable_value>
Created variable: <variable_name>
```
{% endtab %}

{% tab title="Python" %}
```python
import datapane as dp
v = dp.Variable.create(name, value, visibility='ORG')
```
{% endtab %}
{% endtabs %}

### `list`

#### Parameters

None

#### Response

A list of variable names and versions

#### Description & Example

{% tabs %}
{% tab title="CLI" %}
```text
~/> datapane variable list
Available Variables:
    name      versions
--  ------  ----------
 0  foo              1

```
{% endtab %}
{% endtabs %}

### `get`

#### Parameters

| Parameter | Description | Required |
| :--- | :--- | :--- |
| `name` | The name of your variable | True |
| `version` | The version of the variable to retrieve | False |
| `owner` | The owner of the variable. _This defaults to the person running the script, so should be set explicitly if you want other people to run scripts with a user variable you created._ | False |

#### Response

A single Variable object

#### Description & Example

By default, `get` will retrieve the latest version of your variable. 

{% hint style="warning" %}
If you want other people inside your organisation to run your scripts with a variable which you created, you must specify yourself as the `owner` in this method. When someone runs your script, it runs under their name, and if you do not set an explicitly specify the `owner` , it will try and look for the variable under their name and fail.

```python
foo = dp.Variable.get(name='foo', owner='linus')
```
{% endhint %}

{% tabs %}
{% tab title="CLI" %}
```text
~/> datapane variable get foo
Available Variable:
    name     value    visibility
--  -------  -------  ------------
 0  foo      bar      OWNER_ONLY
```
{% endtab %}

{% tab title="Python" %}
```python
import datapane as dp

v = dp.Variable.get(name='foo')
```
{% endtab %}
{% endtabs %}

**Get Value**

To get the value that you save in foo, use:

```text
foo_value = foo.value
```

### `delete`

#### Parameters

| Parameter | Description | Required |
| :--- | :--- | :--- |
| `name` | The name of your variable | True |

#### Response

None

#### Description & Example

{% tabs %}
{% tab title="CLI" %}
```text
~/> datapane variable delete foo   
Deleted variable foo
```
{% endtab %}
{% endtabs %}





