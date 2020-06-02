---
description: 'Creating and sharing reports, locally and hosted on Datapane'
---

# Reports

## Creating a report object

A report object takes a list of [Components](components.md). See a full list of supported components on the respective reference page.

{% page-ref page="components.md" %}

```python
# Create a report
import datapane as dp

# Create a markdown component
markdown = dp.Markdown("# My report")

# Create a report object
report = dp.Report(markdown)
```

## Saving a report

Reports can be saved to local HTML files, passed in via `path`.

```python
report.save(path='file.html')
```

## Publishing a report

If you are logged into a Datapane instance, you can publish a report on the web. All reports must have a `name`. To open the URL automatically, pass in the `open` boolean parameter. If you are not logged in, this method will fail.

```python
report.publish(name='my_report', open=True)
```

Publishing a report prints the URL to standard out, and returns a report object.

## Previewing a report

If you are in a Jupyter Notebook, you can preview your report inline.

```python
report.preview()
```

This embeds the local file as an iframe inside your Notebook session.



