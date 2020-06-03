---
description: 'Variables are secret, encrypted values which you can share between scripts.'
---

# Variables

## Overview

Scripts often contain variables such as database keys and passwords, which you do not want embedded in your source code and visible to the outside world. Datapane's `Variable` object provides a safe and secure way to create, store, and retrieve values which your scripts require.

## Creating a variable

Variables can be created through the CLI or Python library.

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

## Listing current variables

{% tabs %}
{% tab title="CLI" %}
```text
~/> datapane variable list
Available Variables:
    id
--  -------
 0  XBAmDk1
```
{% endtab %}
{% endtabs %}

## Retrieving a variable

{% tabs %}
{% tab title="CLI" %}
```text
~/> datapane variable get <variable-name> [--version=version_number]
Available Variable:
    name     value    visibility
--  -------  -------  ------------
 0  foo      bar      OWNER_ONLY
```
{% endtab %}

{% tab title="Python" %}
```python
import datapane as dp

v = dp.Variable.get(name='my_var')
```
{% endtab %}
{% endtabs %}

## Deleting a variable

{% tabs %}
{% tab title="CLI" %}
```text
~/> datapane variable delete <variable-name>    
Deleted variable foo
```
{% endtab %}
{% endtabs %}





