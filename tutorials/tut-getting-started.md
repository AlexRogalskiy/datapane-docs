---
description: Installing and setting up the Datapane API & CLI on your device
---

# Getting Started

## Installation

Datapane's Python library and CLI can be installed using either `conda` or `pip` on either macOS, Windows, or Linux.

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

### Windows Tips and Troubleshooting

{% hint style="info" %}
Having problems running on Windows? Please read...
{% endhint %}

We generally recommend installing via `conda` over `pip` on Windows as it's easier to install all the required dependencies. 

If you need to install Python first, the latest versions of Windows 10 can install Python for you automatically - running `python` from the command-prompt will take you to the Windows Store where you can download an [official version](https://docs.python.org/3/using/windows.html#the-microsoft-store-package).
We also strongly recommend using a 64-bit rather than 32-bit version of Python, you can check this by running the command `python -c "import struct; print(struct.calcsize('P')*8, 'bit')"` from the Command Prompt.

Also note that on Windows, you can run the `datapane` command either by running `datapane` or `datapane.exe` on the command-line.

Some specific issues you may encounter on Windows include:

#### Import errors when running/importing datapane

You may encounter errors such as `ImportError: DLL load failed` when running datapane or importing it within your Python code.

If so, try installing installing the [Visual C++ Redistributables for Windows](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) from Microsoft and running again (you most likely want to download the version for x64, i.e. `vc_redist.x64.exe`)

#### Datapane install errors trying to compile `pyarrow` using Visual C++

This usually occurs when you are running a 32-bit version of Python and installing via `pip`. Either try using `conda` or install a 64-bit version of Python (for example from the Windows Store as mentioned above).

This may also occur when using Windows 7 - we only support directly Windows 10, however it may be worth trying to install via `conda` instead if you are stuck on Windows 7.

#### 'datapane.exe' is not recognized as an internal or external command

This occurs when your Windows `%PATH%` doesn't include all the Python directories, specifically the `Scripts` directory.

You may notice during the datapane install messages such as (or similar to):

```
The script datapane.exe is installed in 'C:\users\<USERNAME>\appdata\local\programs\python\python37\Scripts' which is not on PATH.
Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
```

To fix this, adjust your `%PATH%` to include your specific `Scripts` path as mentioned in the `pip` warning (see [https://datatofish.com/add-python-to-windows-path/](here) for more detailed instructions).
Alternatively, you can try running the datapane client directly, using the command `python3.exe -m datapane.client` instead.
  

## Post-installation

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

