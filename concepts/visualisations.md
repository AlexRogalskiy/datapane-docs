---
description: >-
  Boards can display results from your analysis, such as visualisations and
  files
---

# Assets: Visualisations and Files

## Importing Assets

Assets can be imported using the API, CLI, or can be manually uploaded using the Web UI.

## Viewing Assets

Supported assets are automatically detected and rendered in your boards. If they are interactive \(such as some visualisations\), Datapane will render them interactively.

![An interactive Bokeh Visualisation](../.gitbook/assets/image%20%2830%29.png)

## Supported Formats

Datapane has native support for both uploading and rendering many common visualisation libraries and file formats. The following assets be added to your boards using the web interface or API.

| Format | Comments |
| :--- | :--- |
| Altair |  Altair provides a way to generate vega plots from Python |
| Vega/Vega-lite | Vega is a common format for describing interactive visualisations |
| Bokeh | Bokeh is a popular interactive Python visualisation library.  |
| Static Images | JPEG, PNG, and SVG images |
| Plain Text | Plain text is rendered pre-formatted |
| JSON Documents | JSON documents are rendered as collapsible trees |

All other files \(such as PDFs, blobs\) are not rendered, but can be downloaded from your Datapane.

