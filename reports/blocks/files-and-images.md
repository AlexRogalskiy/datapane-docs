# Files and Images

## Files and Images

The File block allows you to include files in your reports. If the file is in a supported format, it will be displayed inline in your report. If the format cannot be rendered, it will present the viewer with the option to download the file. This block is useful for including images, JSON data, Excel files, and PDFs for your viewers.

To include a file, you can use `dp.File` and pass the path.

```python
dp.File(file='./image.jpeg')
```

In the following example, Datapane will attempt to display images and JSON in your report for viewers and allow users to download them.

```python
import datapane as dp

report = dp.Report(
   dp.File(file="./image.jpeg"),
   dp.File(file="./data.xlsx"),
   dp.File(file="./config.json")
)
report.publish(name='Files Sample')
```

## Python objects

Alternatively, instead of passing in a path of a file, you can pass in an object such as a Python dictionary, and use `is_json` to tell Datapane to attempt to render it in JSON format in the front-end, where the user will be able to explore and download it.

```python
import datapane as dp

thisdict = {
  "brand": "Ford",
  "model": "Mustang",
  "year": 1964
}

dp.Report(dp.File(thisdict, is_json=True, name='thisdict')).publish(name='json')
```

{% embed url="https://datapane.com/u/datapane/reports/json/" %}



