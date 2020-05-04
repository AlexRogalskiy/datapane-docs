# Getting Started

## Installation

Datapane's Python library and CLI can be installed using pip.

```text
pip3 install datapane
```

## Authentication

As well as a Python client, Datapane has a server component which allows you to both share reports and execute your scripts to allow others generate reports dynamically.

### Public Server

datapane.com is available as a free, public server, which you can use to share reports and scripts. The API and CLI is configured to use this server by default. 

To use Datapane, first [sign up for a free account](https://datapane.com/accounts/signup/) and copy the API key provided in the web interface. Next, login using the CLI using this key. All requests from both the CLI and Python library will now be authenticated.

```text
$ datapane login
Enter your API Key: [paste your API key here]
```

### Private Servers

If you have a private single-tenant server on _your-company_.datapane.com, your credentials will have been shared with you via email. Similarly to using the public instance, you will be able authenticate by passing in your API key to the login command. You can pass in the full URL of your server to the login command as follows.

```text
$ datapane login --server=https://acme.datapane.com
Enter your API Key: [paste your API key here]
```

{% hint style="info" %}
If you're signing into a private server, remember to include the `https://` in your server URL!
{% endhint %}





