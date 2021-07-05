---
description: This section describes how to add custom colours and fonts to your report.
---

# Styling

Datapane Teams allows you to define a global style block which will apply to all of your reports. In this block you can \(re\)define the default font, background colour and more. 

{% hint style="info" %}
 This feature is a part of our Datapane [Growth and Enterprise](https://datapane.com/pricing/) plans - you may need to upgrade in order to use it.
{% endhint %}

To use this feature, you'll need to have Admin Access. Head over to Settings -&gt; Visual Settings, and scroll to the Styling section, where you'll see a block of HTML/CSS which looks like this: 

```markup
<style type="text/css">
    :root {
      --dp-accent-color: #fff;
      --dp-bg-color: #1F2937;
      --dp-text-align: justify;
      --dp-font-family: Inter var, ui-sans-serif, system-ui;
    }
</style>
```

You can edit this block to add your brand colours, load in a custom font or even add a logo. Any assets that you add will be displayed in a bar at the top of your report, and these changes will apply retroactively to your old reports. 

{% hint style="info" %}
If you want to change the font colour, it's best to use the 'Light text mode' checkbox in the Visual Settings page. 
{% endhint %}

### **Adding a custom font**

If you have a brand font, you can load it in as follows: 

```markup
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Sofia">

<style type="text/css">
    :root {
      --dp-accent-color: #fff;
      --dp-bg-color: #1F2937;
      --dp-text-align: justify;
      --dp-font-family: Sofia;
    }
</style>
```

Note that this will only apply to the headings and prose - UI text in DataTables, Select blocks etc will stay as the system default. 

### **Adding a custom header**

Anything you add under the `<style>` block will be added to the top of your report as a default header. You can use this to load in a custom image, text or analytics script as follows:

```markup
<style type="text/css">    
:root {
  --dp-accent-color: #4E46E5;
  --dp-bg-color: #FFF;
  --dp-text-align: justify;
  --dp-font-family: Inter var, ui-sans-serif, system-ui;
}</style>

<img src="my-image.png"/>
```

