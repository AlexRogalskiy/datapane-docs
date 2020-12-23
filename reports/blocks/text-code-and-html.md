---
description: >-
  Datapane allows you to add markdown text to your reports, including helpers
  for creating text-heavy Python reports.
---

# Text, Code, and HTML

## Text

**Markdown** is a lightweight markup language that allows you to include formatted text in your report, and can be access through `dp.Text`, or by passing in a string directly. 

```python
import datapane as dp

dp.Report(dp.Text("__My awesome markdown__"))

# or dp.Report("__My awesome markdown__")
```

{% embed url="https://datapane.com/u/leo/reports/docs-report-block-md/" %}

To include multi-line text and formatting the words, use triple-quoted string, e.g. `"""Some words"""`

```python
import datapane as dp

report = dp.Report(
           "## My Altair Plot",
           dp.Plot(alt_chart),
           dp.Text("""
* There is a negative relation between life expectanty and fertility
* The number of population with high life expactancy increases as time increase
           """)
report.publish(name = 'results')
```

{% hint style="info" %}
Check [here](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) for more information on how to format your text with markdown.
{% endhint %}

## Text-heavy Reports

If your report is text-heavy \(such as an blogpost\) and it contains multiple other blocks, creating a list of strings and blocks in Python can be cumbersome. To solve this, Datapane provides a `format` option, which allows you to write a single block of Markdown \(either in your report, or in a separate file\), and intersperse it with other blocks. 

To do this, use double braces to specify where you want your other blocks to appear throughout your text.

```python
import datapane as dp

...

markdown_text = """
# Introduction
Proximus qui suoque ademit occallescere Peleu, et pedes hostesque refugerit ad Pelates bella. In iam saxoque quaeque latratibus nobis nymphae gentisque equarum letifer ipse stipite hiatu. Mihi sumptaque quoque, iras origo, hanc mutare auctor eheu siste Agamemnona et vertice inhospita Achille. Hunc nec visa, mota hanc tua! Fetus et nate argenteus magno loquuntur et nisi, ore tecta; cetera membra, ab nec.

{{table}}

Mihi Laertaque annos, vim parentem aera unius cibique hoste, pugnabam satis tamen sibi! Titania credita fecit exitus dique altaria simus.

{{plot}}

# Vim parentem

{{other}}

"""


dp.Report(
  dp.Text(markdown_text).format(
    table=gen_table_df(), 
    plot=get_plot(), 
    other=other
  )
)
```

Alternatively, you can write your article or post in your favourite markdown editor, and pass it in a file.

```python
import datapane as dp

...

dp.Report(
  dp.Text(file="./my_blogpost.md").format(
    table=gen_table_df(), 
    plot=get_plot(), 
    other=other
  )
)
```

## Code

The code block allows you to embed syntax highlighted source code into your report. This block currently supports Python and JavaScript.

```python
import datapane as dp

code = '''
function foo() {
  return x + 1
}
'''

dp.Report(
   dp.Code(
      code=code
      language="javascript"
   )
)
```

## HTML

The HTML block allows you to add raw HTML to your report,  allowing for highly customized components, such as your company's brand, logo, and more.

{% hint style="info" %}
The HTML component is sandboxed and cannot execute JavaScript.
{% endhint %}

```python
import datapane as dp

html = '''
<div style='background:black;color:white'>
    <h1> This is a title </h1>
    <p> This is my paragraph </p>
</div>
'''

report = dp.Report(dp.HTML(html))
report.publish(name='sample_table')
```

