# Files, Images and Embeds

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
   dp.File(file="./image.jpeg", caption = "Example image from Unsplash"),
   dp.File(file="./data.xlsx"),
   dp.File(file="./config.json")
)
report.upload(name='Files Sample')
```

{% hint style="info" %}
Try adding a `name` and a `caption` to your images to make them stand out and add attribution. 
{% endhint %}

## Python objects

Alternatively, instead of passing in a path of a file, you can pass in an object such as a Python dictionary, and use `is_json` to tell Datapane to attempt to render it in JSON format in the front-end, where the user will be able to explore and download it.

```python
import datapane as dp

thisdict = {
  "brand": "Ford",
  "model": "Mustang",
  "year": 1964
}

dp.Report(dp.File(thisdict, is_json=True, name='thisdict')).upload(name='json')
```

{% embed url="https://datapane.com/u/datapane/reports/json/" %}

## Embeds

The Embed block lets you embed content from other platforms e.g. Youtube, Spotify. This is how you'd use it: 

{% tabs %}
{% tab title="Python" %}
```python
import datapane as dp

dp.Report(
    dp.Embed(url='https://www.youtube.com/watch?v=dQw4w9WgXcQ')
).upload(name='Embex example')
```
{% endtab %}

{% tab title="Text Report" %}
    ```datapane
    block: embed
    url: https://www.youtube.com/watch?v=dQw4w9WgXcQ
    caption: Youtube video
    ```
{% endtab %}
{% endtabs %}

You don't need to use this block for simple embeds on TextReports like GIFs. For those, just use Markdown syntax i.e. `![](https://my-example-gif.gif)`

{% hint style="info" %}
If you're trying to embed an `iframe,` you can wrap it in an [HTML block](text-code-and-html.md#html). 
{% endhint %}

