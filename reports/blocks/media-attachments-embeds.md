# Media, Attachments, and Embeds

## Images and Media

**Requires Datapane version 0.13+**

The Media block allows you to include images, GIFs, video and audio in your reports. If the file is in a supported format, it will be displayed inline in your report. To include an image, you can use `dp.Media` and pass the path.

```python
dp.Media(file='./image.jpeg')
```

In the following example, Datapane will attempt to display images in your report for viewers and allow users to download them.

```python
import datapane as dp

report = dp.Report(
   dp.Media(file="./image.jpeg", name="Plot 1", caption="A pretty image"),
)
report.upload(name='Image Sample')
```

## Attachments and Python objects

If you want to include static files like PDFs or Excel docs in your report, use the `dp.Attachment` block.  You can also pass in a Python object directly. Once you upload the report, your users will be able to explore and download these attachments. &#x20;

```python
import datapane as dp

myfilepath = "./financial_model.xlsx"

thisdict = {
  "brand": "Ford",
  "model": "Mustang",
  "year": 1964
}

dp.Report(
  dp.Attachment(file = myfilepath),
  dp.Attachment(thisdict, name='thisdict')
).upload(name='json')
```

## Embeds

The Embed block lets you embed content from other platforms e.g. Youtube, Spotify. This is how you'd use it:&#x20;

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
````
```datapane
block: embed
url: https://www.youtube.com/watch?v=dQw4w9WgXcQ
caption: Youtube video
```
````
{% endtab %}
{% endtabs %}

You don't need to use this block for simple embeds on TextReports like GIFs. For those, just use Markdown syntax i.e. `![](https://my-example-gif.gif)`

{% hint style="info" %}
If you're trying to embed an `iframe,` you can wrap it in an [HTML block](text-code-and-html.md#html).&#x20;
{% endhint %}
