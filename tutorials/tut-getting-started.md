---
description: Installing and setting up the Datapane API & CLI on your device
---

# Getting Started

## Installation

Datapane's Python library and CLI can be installed using either `conda` or `pip`. 

{% hint style="info" %}
Datapane support Python 3.6 onwards, instructions for installing Python can be found at [https://wiki.python.org/moin/BeginnersGuide/Download](https://wiki.python.org/moin/BeginnersGuide/Download)
{% endhint %}

### conda
If you use `conda`, you can install it with:

```bash
$ conda install -c conda-forge datapane
```

### pip

If you use `pip`, you can install it with:

```bash
$ pip3 install datapane
```

### Windows Users

{% hint style="info" %}
Windows users please read :)
{% endhint %}

A couple of extra notes for Windows users,

- We recommend installing via `conda` over `pip` on Windows as it's easier to install all the required dependencies.
- If you do install via `pip` and encounter issues during install or usage, first try installing installing the [Visual C++ Redistributable for Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145) from Microsoft, and secondly ensure you have a 64-bit version of Python installed.
- The latest versions of Windows 10 can install Python for you automatically - just run `python` from the command-prompt and it will take you to the Windows Store where you can download an [official version](https://docs.python.org/3/using/windows.html#the-microsoft-store-package).
- Finally, if you get "Command Not Found" errors when running `datapane` on the command line, either adjust your `%PATH%` to ensure it includes both the Python application and `Scripts` paths (see [https://datatofish.com/add-python-to-windows-path/](here)), or try running `python3.exe -m datapane.client` instead


For all users, once installed, you can use the API described in this tutorial and in the [reference](../reference/reference-overview.md) to build and export your own reports locally and share them as HTML files. From there, you can also sign up to the free hosted server to deploy your reports and run scripts.

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

