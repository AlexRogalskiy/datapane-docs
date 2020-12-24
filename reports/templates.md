# Templates

{% hint style="success" %}
New in Datapane 0.9
{% endhint %}

Templates are reusable helpers which are contributed by the Datapane Community to make common reporting tasks easy. A template is a Python function which can take inputs \(including other blocks, pages, or reports\), and returns blocks, pages, or even whole reports. This provides an easy way to build and share helpers which can be reused across your own reports, or share your methods with the Datapane Community. 

## Example Templates

* Add an interactive source code tab to any other block \(such as a plot or a dataset\)
* Build a report from a Markdown file and a list of datasets and plots

To see current templates, please [view the API docs](https://datapane.github.io/datapane/templates.html), which are automatically updated with the latest contributions.

{% embed url="https://datapane.github.io/datapane/templates.html" %}

## Contributing a Template

We love community contributions. If you have a useful template, please add it to [`templates.py`](https://github.com/datapane/datapane/blob/33378a00c2c0586d4631e5b8258ba9530aa793cf/src/datapane/client/api/templates.py) with a doc string and open a pull request on our [GitHub Repo](https://github.com/datapane/datapane). 

