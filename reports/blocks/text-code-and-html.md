---
description: >-
  Datapane allows you to add markdown text to your reports, including helpers
  for creating text-heavy Python reports.
---

# Text, Formulas, Code, and HTML

## Text

The best way to include long-form text is to use the [Web Editor,](web-editor.md) which allows you to add and edit text directly in the browser, and preview the result. 

However, you can still add text from within your Python script, as follows: 

**Markdown** is a lightweight markup language that allows you to include formatted text in your report, and can be access through `dp.Text`, or by passing in a string directly. 

```python
import datapane as dp

dp.Report(dp.Text("__My awesome markdown__"))

# or dp.Report("__My awesome markdown__")
```

To include multi-line text and formatting the words, use triple-quoted string, e.g. `"""Some words"""`

```python
import datapane as dp

md = """
Quas *diva coeperat usum*; suisque, ab alii, prato. Et cornua frontes puerum,
referam vocassent **umeris**. Dies nec suorum alis adstitit, *temeraria*,
anhelis aliis lacunabant quoque adhuc spissatus illum refugam perterrita in
sonus. Facturus ad montes victima fluctus undae Zancle et nulli; frigida me.
Regno memini concedant argento Aiacis terga, foribusque audit Persephone
serieque, obsidis cupidine qualibet Exadius.

    utf_torrent_flash = -1;
    urlUpnp -= leakWebE - dslam;
    skinCdLdap += sessionCyberspace;
    var ascii = address - software_compile;
    webFlaming(cable, pathIllegalHtml);

## Quo exul exsecrere cuique non alti caerulaque

*Optatae o*! Quo et callida et caeleste amorem: nocet recentibus causamque.

- Voce adduntque
- Divesque quam exstinctum revulsus
- Et utrique eunti
- Vos tantum quercum fervet et nec
- Eris pennis maneas quam
"""
report = dp.Report(md)
report.upload(name = 'markdown')
```

{% embed url="https://datapane.com/u/datapane/reports/markdown/" %}



{% hint style="info" %}
Check [here](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) for more information on how to format your text with markdown.
{% endhint %}

## Text-heavy Reports

If your report is text-heavy \(such as an blogpost\) and it contains multiple other blocks, creating a list of strings and blocks in Python can be cumbersome. To solve this, Datapane provides a `format` option, which allows you to write a single block of Markdown \(either in your report, or in a separate file\), and intersperse it with other blocks. 

To do this, use double braces to specify where you want your other blocks to appear throughout your text.

```python
import seaborn as sns
import altair as alt 
import datapane as dp

md = """
For example, if we want to visualize the number of people in each class within the interval we select a point chart between age and fare, we could do something like this.

{{plot}}

Altair allows you to create some extremely interactive plots which do on-the-fly calculations — without even requiring a running Python server!
"""

titanic = sns.load_dataset("titanic")

points = alt.Chart(titanic).mark_point().encode(
    x='age:Q',
    color='class:N',
    y='fare:Q',
).interactive().properties(width='container')

dp.Report(
  dp.Text(md).format(plot=points)
).upload(name='altair_example')
```

{% embed url="https://datapane.com/u/datapane/reports/altair-example/?utm\_medium=embed&utm\_content=viewfull" %}

Alternatively, you can write your article or post in your favourite markdown editor, and pass it in as a file.

```python
import datapane as dp

...

dp.Report(
  dp.Text(file="./my_blogpost.md").format(plot=points)
)
```

## Formulas

The formula block allows you easily to add [_LaTeX_](https://en.wikipedia.org/wiki/LaTeX)-formatted equations to your report, with an optional caption. A brief intro into _LaTeX_ formulas can be found [here](https://en.wikibooks.org/wiki/LaTeX/Mathematics). 

{% tabs %}
{% tab title="Python" %}
```python
import datapane as dp

dp.Report(
  dp.Formula(r"\frac{1}{\sqrt{x^2 + 1}}", caption="")
).upload("formula")
```
{% endtab %}

{% tab title="Web Report" %}
```markup
```datapane
block: Formula
formula: x^2 + y^2 = z^2
```
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
_LaTeX_ formulas commonly make use of special characters, hence prefix your formulas with `r` in Python as per the example above to make them raw strings, e.g. `r"x^2"`
{% endhint %}

Under the hood we use MathJAX to render the equations in the browser and not a full TeX engine. This means that some of your TeX input may not be rendered correctly on our system - read the [MathJAX documentation](http://docs.mathjax.org/en/latest/input/tex/differences.html) for more info. 

## Code

The code block allows you to embed syntax highlighted source code into your report. This block currently supports Python and JavaScript.

For the Web Editor 

{% tabs %}
{% tab title="Python" %}
```python
import datapane as dp

code = '''
function foo(n) {
  return foo(n + 1)
}
'''

dp.Report(
   dp.Code(
      code=code,
      language="javascript"
   )
).upload(name='code')
```
{% endtab %}

{% tab title="Web Report" %}
    ```datapane
    block: Code
    language: javascript
    code: |
        function foo(n) {
          return foo(n + 1)
        }
    ```
{% endtab %}
{% endtabs %}

{% embed url="https://datapane.com/u/datapane/reports/code/" %}



## HTML

The HTML block allows you to add raw HTML to your report,  allowing for highly customized components, such as your company's brand, logo, and more.

{% hint style="info" %}
The HTML component is sandboxed and cannot execute JavaScript.
{% endhint %}

{% tabs %}
{% tab title="Python" %}
```python
import datapane as dp

html = """
<html>
    <style type='text/css'>
        @keyframes example {
            0%   {color: #EEE;}
            25%  {color: #EC4899;}
            50%  {color: #8B5CF6;}
            100% {color: #EF4444;}
        }
        #container {
            background: #1F2937;
            padding: 10em;
        }
        h1 {
            color:#eee;
            animation-name: example;
            animation-duration: 4s;
            animation-iteration-count: infinite;
        }
    </style>
    <div id="container">
      <h1> Welcome to my Report </h1>
    </div>
</html>
"""

dp.Report(
  dp.HTML(
    html
  )
).upload(name='docs_html')
```
{% endtab %}

{% tab title="Web Editor" %}
```markup
```datapane
block: HTML
html: |
  <html>
      <style type='text/css'>
          @keyframes example {
              0%   {color: #EEE;}
              25%  {color: #EC4899;}
              50%  {color: #8B5CF6;}
              100% {color: #EF4444;}
          }
          #container {
              background: #1F2937;
              padding: 10em;
          }
          h1 {
              color:#eee;
              animation-name: example;
              animation-duration: 4s;
              animation-iteration-count: infinite;
          }
      </style>
      <div id="container">
        <h1> Welcome to my Report </h1>
      </div>
  </html>
```
```
{% endtab %}
{% endtabs %}

{% embed url="https://datapane.com/u/datapane/reports/docs-html/" %}



