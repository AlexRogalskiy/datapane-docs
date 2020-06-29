---
description: Installing and setting up the Datapane API & CLI on your device
---

# Getting Started

## Installation

Datapane's Python library and CLI can be installed using pip.

```bash
$ pip3 install datapane
```

{% hint style="info" %}
We support Python 3.6 onwards, instructions for installing Python can be found at [https://wiki.python.org/moin/BeginnersGuide/Download](https://wiki.python.org/moin/BeginnersGuide/Download)
{% endhint %}

Once installed, you can use the API described in this tutorial and in the [reference](../reference/reference-overview.md) to build and export your own reports locally and share them as HTML files. From there, you can also sign up to the free hosted server to deploy your reports and run scripts.

## Authentication

As well as a Python client, Datapane has a server component which allows you to both share reports and execute your scripts to allow others generate reports and actions dynamically.

### Public Server

[datapane.com](https://datapane.com) is available as a free, public server, which you can use to share reports and scripts. The API and CLI is configured to use this server by default.

To use Datapane, first [sign up for a free account](https://datapane.com/accounts/signup/) and copy the API key provided in the web interface. Next, login using the CLI using this key. All requests from both the CLI and Python library will now be authenticated.

```text
$ datapane login
Enter your API Key: [paste your API key here]
```

### Private Teams

Datapane provides private hosted servers and supports on-premise instances for teams and enterprises. In such a case, log in to your instance, for instance `https://your-company.datapane.com,` using your existing credentials \(these will have been provide to you by your admin\).

Similarly to using the public instance, your home page will indicate your API key and you will be able authenticate by passing in your API key to the login command. You can pass in the full URL of your server to the login command as follows.

```text
$ datapane login --server=https://acme.datapane.com
Enter your API Key: [paste your API key here]
```

{% hint style="info" %}
If you're signing into a private server, remember to include the `https://` in your server URL!
{% endhint %}

{% hint style="info" %}
The CLI supports multiple profiles using the `--env` flag, so you can easily work with both the public and your team instance at the same time

```bash
$ datapane --env public login
$ datapane --env acme login --server=https://acme.datapane.com
```
{% endhint %}



## Check your Authentication

To check which account you are logged in as, run:

```text
$ datapane ping
```

