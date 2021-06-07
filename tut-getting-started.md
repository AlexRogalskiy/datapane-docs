---
description: Installing and setting up the Datapane library and API on your device
---

# Getting Started

## Installation

Datapane's Python library and CLI can be installed using either `conda` or `pip` on macOS, Windows, or Linux. Datapane supports Python **3.6 - 3.9**.

{% hint style="info" %}
Instructions for installing Python can be found at [https://wiki.python.org/moin/BeginnersGuide/Download](https://wiki.python.org/moin/BeginnersGuide/Download). 
{% endhint %}

### conda

If you use `conda`, you can install it with:

```bash
$ conda install -c conda-forge "datapane>=0.10.0"
```

{% hint style="warning" %}
Conda sometimes installs an older version of datapane. If you receive errors, please check the version and try running `conda update --all` or try in a new conda environment \(`conda create -n ENV` and `conda activate ENV)`
{% endhint %}

### pip

If you use `pip`, you can install it with:

```bash
$ pip3 install -U datapane
```

### Upgrading

We upgrade datapane regularly to include new features, both in the client and on the hosted version. From time to time your client may no longer be compatible with the datapane server when publishing a report.. If this happens, you receive an error like the following,

```text
IncompatibleVersionError: Your client is out-of-date (version 0.9.2) and may be causing errors, "
please upgrade to version 0.10.2
```

In such an event, please upgrade your datapane cli via `pip` or `conda` and try again.

**Upgrading via pip**

If you installed datapane via pip, run the following command, adding `--user` flag if needed

```text
pip install -U datapane OR pip install --user -U datapane
```

**Upgrading via conda**

If you installed `datapane` via conda, run the following command, adding the `--all` flag if needed. As above, if you receive errors please try using a fresh conda environment.

```text
conda update datapane OR conda update --all
```

### Windows Tips and Troubleshooting

{% hint style="info" %}
Having problems running on Windows? Please read on...
{% endhint %}

We generally recommend installing via `conda` over `pip` on Windows as it's easier to install all the required dependencies.

If you need to install Python first, the latest versions of Windows 10 can install Python for you automatically - running `python` from the command-prompt will take you to the Windows Store where you can download an [official version](https://docs.python.org/3/using/windows.html#the-microsoft-store-package). We also strongly recommend using a 64-bit rather than the 32-bit version of Python, you can check this by running the command `python -c "import struct; print(struct.calcsize('P')*8, 'bit')"` from the Command Prompt.

```text
python -c "import struct; print(struct.calcsize('P')*8, 'bit')"
```

Also note that on Windows, you can run the `datapane` command either by running `datapane` or `datapane.exe` on the command-line.

Some specific issues you may encounter on Windows include:

#### Import errors when running/importing datapane

You may encounter errors such as `ImportError: DLL load failed` when running datapane or importing it within your Python code.

If so, try installing the [Visual C++ Redistributables for Windows](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) from Microsoft and running again \(you most likely want to download the version for x64, i.e. `vc_redist.x64.exe`\)

#### Datapane install errors trying to compile `pyarrow` using Visual C++

This usually occurs when you are running a 32-bit version of Python and installing via `pip`. Either try using `conda` or install a 64-bit version of Python \(for example from the Windows Store as mentioned above\).

This may also occur when using Windows 7 - we only support directly Windows 10, however, it may be worth trying to install via `conda` instead, if you are stuck on Windows 7.

#### `'datapane.exe' is not recognized as an internal or external command`

This occurs when your Windows `%PATH%` doesn't include all the Python directories, specifically the `Scripts` directory.

You may notice during the datapane install messages such as \(or similar to\):

```text
The script datapane.exe is installed in 'C:\users\<USERNAME>\appdata\local\programs\python\python37\Scripts' which is not on PATH.
Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
```

To fix this, adjust your `%PATH%` to include your specific `Scripts` path as mentioned in the `pip` warning \(see [https://datatofish.com/add-python-to-windows-path/](https://github.com/datapane/datapane-docs/tree/5f551e8c5b2748f0785683bbd62cb59f1dfe46ca/tutorials/here/README.md) for more detailed instructions\). Alternatively, you can try running the datapane client directly, using the command `python3.exe -m datapane.client` instead.

{% hint style="info" %}
If you are still having problems installing, please ask on the [Datapane Forum](https://discuss.datapane.com), and someone will come to help you.
{% endhint %}

## Authentication

As well as a local Python framework for generating reports, Datapane has a server component which is accessed through the CLI and Python library and requires an authentication token. You can authenticate through either the CLI or the Python library, and all future requests from both the CLI and Python library will automatically be authenticated.

### Datapane.com

_Datapane_ is hosted on [datapane.com](https://datapane.com) and is available as a free server where you can publish reports. Reports can be public or private, and shared securely or with the entire community. The API and CLI are configured to use this server by default. After you [sign up for a free account](https://datapane.com/accounts/signup/), you will see a code snippet which includes a login command, `dp.login`, with your token. 

Login by running this code snippet or Python library using this key. All future requests from both the CLI and Python library will automatically be authenticated.

{% hint style="info" %}
If you need your API key in the future, you can find it in [your settings page](https://datapane.com/settings).
{% endhint %}

{% tabs %}
{% tab title="Python Library" %}
```python
import datapane as dp
dp.login(token=your_token)
```
{% endtab %}

{% tab title="CLI" %}
```text
$ datapane login 
Enter your API Key: [paste your API key here]
```
{% endtab %}
{% endtabs %}

### Datapane Teams

_Datapane Teams_ provides private hosted servers and supports on-premise instances for organizations. In such a case, log in to your instance, for instance `https://your-company.datapane.net` , using the credentials provided to you by your admin.

Similar to when using the free _Datapane_ instance, your home page will indicate your API key and you will be able to authenticate by passing in your API key to the login command. You will need to pass in the full URL of your server \(including the `https://`\) to the login command as follows.

{% tabs %}
{% tab title="CLI" %}
```text
$ datapane login --server=https://[your-server].datapane.net
Enter your API Key: [paste your API key here]
```
{% endtab %}

{% tab title="Python Library" %}
```python
import datapane as dp
dp.login(token=your_token, server='https://[your-server].datapane.net')
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The CLI supports multiple profiles using the --env flag, so you can easily work with both the default _Datapane_ instance and your private enterprise instance at the same time

```bash
$ datapane --env default login
$ datapane --env acme login --server=https://acme.datapane.net
```
{% endhint %}

### Check your Authentication

To check you have access and which account you are logged in as, run:

{% tabs %}
{% tab title="CLI" %}
```text
$ datapane ping
```
{% endtab %}

{% tab title="Python" %}
```python
import datapane as dp
dp.ping()
```
{% endtab %}
{% endtabs %}

