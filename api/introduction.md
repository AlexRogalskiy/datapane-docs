# Introduction & Installation

Datapane provides an API which can be accessed using the Python library or CLI. This API allows you to deploy datasets and assets from your existing environment, and build boards programmatically.

## Installation

Installing the Datapane package will install both the CLI and client library.

```text
$ pip3 install datapane
```

## Configure

### Login

Login to your datapane account. To login, you first need an API token. Once you have created an account on Datapane, you can find your token in your [Settings Page](https://datapane.com/settings).

```text
$ datapane login
Enter API Token: [Enter your API Token here]
```

### Ping

Check you are connected to a Datapane server.

```text
$ datapane ping
```

## Documentation

Full documentation is available in the [Readme of the Python package repository](https://pypi.org/project/datapane/).

