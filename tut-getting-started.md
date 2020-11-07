---
description: Installing and setting up the Datapane library and API on your device
---

# Getting Started

## Installation

Datapane's Python library and CLI can be installed using either `conda` or `pip` on macOS, Windows, or Linux.

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
$ pip3 install -U datapane
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

### Datapane Public

_Datapane Public_ is hosted on [datapane.com](https://datapane.com) and is available as a free, public server which you can use to publish and share reports. The API and CLI are configured to use this server by default. After you [sign up for a free account](https://datapane.com/accounts/signup/), copy the API key provided in the web interface. Next, login using the CLI or Python library using this key. All future requests from both the CLI and Python library will automatically be authenticated.

{% tabs %}
{% tab title="CLI" %}
```text
$ datapane login 
Enter your API Key: [paste your API key here]
```
{% endtab %}

{% tab title="Python Library" %}
```python
import datapane as dp
dp.login(token=your_token)
```
{% endtab %}
{% endtabs %}

### Datapane for Teams

_Datapane for Teams_ provides private hosted servers and supports on-premise instances for organizations. In such a case, log in to your instance, for instance `https://your-company.datapane.net,` using the credentials provided to you by your admin.

Similarly to using the public instance, your home page will indicate your API key and you will be able to authenticate by passing in your API key to the login command. You will need to pass in the full URL of your server \(including the `https://`\) to the login command as follows.

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
The CLI supports multiple profiles using the --env flag, so you can easily work with both the public and your team instance at the same time

```bash
$ datapane --env public login
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

