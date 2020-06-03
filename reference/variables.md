---
description: 'Variables are secret, encrypted values which you can share between scripts.'
---

# Variables

## Overview

Scripts often contain variables such as database keys and passwords, which you do not want embedded in your source code and visible to the outside world. Datapane's `Variable` object provides a safe and secure way to create, store, and retrieve values which your scripts require.

## `add`

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
v = dp.Variable.add(name, value)
```
{% endtab %}
{% endtabs %}

By default, variables are private to the creator's account, but they can be be shared across an organisation. To set visibility, use the `--visibility` flag with `OWNER_ONLY` or `ORG`. 

{% hint style="info" %}
If you want other people inside your organisation to run your scripts, your variable must be `ORG`, as scripts are executed under their user account.
{% endhint %}

## `list`

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

## `get`

#### Parameters

| Parameter | Description | Required |
| :--- | :--- | :--- |
| `name` | The name of your variable | True |
| `version` | The version of the variable to retrieve | False |

#### Response

A single Variable object

#### Description & Example

By default, `get` will retrieve the latest version of your variable.

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

## `delete`

#### Parameters

| Parameter | Description | Required |
| :--- | :--- | :--- |
| `name` | The name of your variable | True |

#### Response

A single Variable object

#### Description & Example

{% tabs %}
{% tab title="CLI" %}
```text
~/> datapane variable delete <variable-name>    
Deleted variable foo
```
{% endtab %}
{% endtabs %}





