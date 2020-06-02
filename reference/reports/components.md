---
description: All the components you can use to develop and layout your reports
---

# Components

## Overview

Components are used to create [Reports](./). Components are compatible with Python objects, such as Pandas DataFrames, visualisations, and Markdown. We are always adding new components, and if you have some ideas on what you would like to use in your reports, please [open an issue on GitHub](https://github.com/datapane/datapane).

## Plot

The plot component takes a supported plot object and renders it in your report.

Currently supported libraries are:

* Matplotlib and seaborn figures
* Bokeh plots
* Altair plots
* SVG images

### Altair

```text
a = alt.Chart(df).encode(x='time', y='value').mark_line()
p = dp.Plot(a)
```

### Bokeh

### Matplotlib & Seaborn

## Markdown

## Table

## Drilldown

{% hint style="info" %}
Coming soon!
{% endhint %}

