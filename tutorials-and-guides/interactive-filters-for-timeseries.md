# Plotting interactive timeseries with filters

## Introduction <a id="02b9"></a>

From identifying trends to understanding ’cause and effect’ behaviors, timeseries analysis is one of the most popular ways of understanding user behaviour. 

We often start with plotting plotting one categorical variable changing over time. But often you’ll need to show multiple categorical variables together e.g. a list of stocks for market data, or regions/locations for sales data. In such cases, you can either show all your series in the same plot or create a separate plot for each series. However, these options are hard to make sense of and take up a lot of space. 

This guide will show you how to build interactive timeseries plots using advanced visualization libraries like Plotly, Bokeh, and Altair. We'll focus on these two components: 

1. **Dropdown menus** let you toggle between different series in the same plot
2. **Date range sliders** allowing you to observe trends between specific periods

## Dropdown menu <a id="4a19"></a>

A dropdown menu is really handy if you have a lot of categories in the data, e.g. stocks or countries, and you want to observe the trends using a line plot in the same plot or figure. This saves you from creating several plots in a loop.

In this tutorial, we'll use a [Covid 19 dataset](https://www.kaggle.com/imdevskp/corona-virus-report) which shows the covid caseload across different countries and add a dropdown to change the country within the same plot.

### Altair

You can read more about interactive components with Altair [here](https://altair-viz.github.io/gallery/multiple_interactions.html). Creating the plot with Altair involves binding a country list to a selection component: 

```python
import pandas as pd
import datapane as dp
import altair as alt
alt.data_transformers.disable_max_rows()

df = pd.read_csv("./covid_19_clean_complete.csv")

df['Date'] = pd.to_datetime(df['Date'])
country_list = list(df['Country/Region'].unique())

input_dropdown = alt.binding_select(options=country_list)
selection = alt.selection_single(fields=['Country/Region'], bind=input_dropdown, name='Country')

alt_plot = alt.Chart(df).mark_line().encode(
    x='Date',
    y='Confirmed',
    tooltip='Confirmed'
).add_selection(
    selection
).transform_filter(
    selection
)

alt_plot
```

Running that code gives us:  

{% embed url="https://datapane.com/u/shoumik/reports/dkjVxgk/dropdown-with-altair/embed/" %}

### Plotly

Plotly offers a range of interactive options which are called [Custom Controls](https://plotly.com/python/#controls). The best part about these controls is that they can be added to the plots purely in pythonic code. Let's visualize the same plot in Plotly: 

```python
import pandas as pd
import plotly.graph_objects as go
import plotly.express as px


df = pd.read_csv("./covid_19_clean_complete.csv")

fig = go.Figure()

country_list = list(df['Country/Region'].unique())

for country in country_list:
    fig.add_trace(
        go.Scatter(
            x = df['Date'][df['Country/Region']==country],
            y = df['Confirmed'][df['Country/Region']==country],
            name = country, visible = True
        )
    )
    
buttons = []

for i, country in enumerate(country_list):
    args = [False] * len(country_list)
    args[i] = True
    
    button = dict(label = country,
                  method = "update",
                  args=[{"visible": args}])
    
    buttons.append(button)
    
fig.update_layout(
    updatemenus=[dict(
                    active=0,
                    type="dropdown",
                    buttons=buttons,
                    x = 0,
                    y = 1.1,
                    xanchor = 'left',
                    yanchor = 'bottom'
                )], 
    autosize=False,
    width=1000,
    height=800
)
```

And you will have a nice-looking dropdown added to the time-series plot: 

{% embed url="https://datapane.com/u/shoumik/reports/0kz9Za7/dropdown-with-plotly/embed/" %}

### Bokeh

Bokeh has components called widgets which can be used to add several interactive components to your plots. Widgets are primarily aimed at creating dashboard components hosted on the Bokeh server. You can read more about widgets [here](https://docs.bokeh.org/en/latest/docs/user_guide/interaction/widgets.html).

Keep in mind that in order to create widgets for standalone HTML files or even while working with Jupyter notebook, you will need to use **CustomJS** callbacks. This requires a bit of JavaScript knowledge to get the dropdown working properly. If you want to do it the pure pythonic way, you have to use the Bokeh server to make the widgets work.

We will replicate the same use-case as above using Bokeh dropdowns.

```python
import pandas as pd
import datapane as dp
from bokeh.io import output_file, show, output_notebook, save
from bokeh.models import ColumnDataSource, Select, DateRangeSlider
from bokeh.plotting import figure, show
from bokeh.models import CustomJS 
from bokeh.layouts import row,column

df = pd.read_csv("/Users/johnreid/PycharmProjects/pythonProject/internal-reports/Docs/covid_19_clean_complete.csv")
country_list = list(df['Country/Region'].unique())

df['Date'] = pd.to_datetime(df['Date'])

cols1 = df[['Country/Region','Date', 'Confirmed']]
cols2 = cols1[cols1['Country/Region'] == 'Afghanistan']

Overall = ColumnDataSource(data=cols1)
Curr = ColumnDataSource(data=cols2)

#plot and the menu is linked with each other by this callback function
callback = CustomJS(args=dict(source=Overall, sc=Curr), code="""
var f = cb_obj.value
sc.data['Date']=[]
sc.data['Confirmed']=[]
for(var i = 0; i <= source.get_length(); i++){
	if (source.data['Country/Region'][i] == f){
		sc.data['Date'].push(source.data['Date'][i])
		sc.data['Confirmed'].push(source.data['Confirmed'][i])
	 }
}   
   
sc.change.emit();
""")

menu = Select(options=country_list,value='Afghanistan', title = 'Country')  # drop down menu

bokeh_p=figure(x_axis_label ='Date', y_axis_label = 'Confirmed', y_axis_type="linear",x_axis_type="datetime") #creating figure object 
bokeh_p.line(x='Date', y='Confirmed', color='green', source=Curr) # plotting the data using glyph circle

menu.js_on_change('value', callback) # calling the function on change of selection
layout=column(menu, bokeh_p) # creating the layout
show(layout)
```

This is how the plot will look like: 

![](https://miro.medium.com/max/1400/1*Y_Jgo484IKhjIbxVoIiDbA.png)

### Matplotlib/Seaborn

If you want to use a non-interactive library like Matplotlib or Seaborn, you can use the `dp.Select`  block to mimic the interactive filter ability, like this:

```python
import pandas as pd
import matplotlib.pyplot as plt
import datapane as dp

df = pd.read_csv("/Users/johnreid/PycharmProjects/pythonProject/internal-reports/Docs/covid_19_clean_complete.csv")
country_list = list(df['Country/Region'].unique())[:10]

plt.figure(figsize=(10, 5), dpi=300)

plot_list = ([dp.Plot(df[df['Country/Region']==country].plot.line(x='Date', y='Confirmed'), label=country)
              for country in country_list])
             
report = dp.Report(
    dp.Select(blocks = plot_list)
).upload(name="Matplotlib example")
```

This is how it will look like: 

{% embed url="https://datapane.com/u/shoumik/reports/R70pzg7/dropdown-with-datapane/embed/" %}

## Date range slider <a id="63d5"></a>

Another interactive component that comes really handy while working with timeseries plots is a date range slider or a slider in general.

Since most of the timeseries plots have a date range in the X-axis, a slider allows you to dynamically change the period and view only a section of the plot to understand the trends for that particular period.

### Altair

With Altair, similar to Plotly, you can use the generic slider to use as a Date Range Slider. However, keep in mind that Vega considers the timeseries data in milliseconds and it is very difficult to show the date information in the slider. It works if you have yearly data but if the data is broken into days and months, it is tricky to make it work.

```python
import altair as alt
import pandas as pd
alt.data_transformers.disable_max_rows()

df = pd.read_csv("./covid_19_clean_complete.csv")
df['Date'] = pd.to_datetime(df['Date'])
country_list = list(df['Country/Region'].unique())

input_dropdown = alt.binding_select(options=country_list)
selection = alt.selection_single(fields=['Country/Region'], bind=input_dropdown, name='Country')

def timestamp(t):
    return pd.to_datetime(t).timestamp() * 1000
  
slider = alt.binding_range(
    step = 30 * 24 * 60 * 60 * 1000, # 30 days in milliseconds
    min=timestamp(min(df['Date'])),
    max=timestamp(max(df['Date']))
)

select_date = alt.selection_single(
    fields=['Date'],
    bind=slider,
    init={'Date': timestamp(min(df['Date']))},
    name='slider'
)
    
alt_plot = alt.Chart(df).mark_line().encode(
    x='Date',
    y='Confirmed',
    tooltip='Confirmed'
).add_selection(
    selection
).transform_filter(
    selection
).add_selection(select_date).transform_filter(
    "(year(datum.date) == year(slider.Date[0])) && "
    "(month(datum.date) == month(slider.Date[0]))"
)

alt_plot
```

This is how it will look like: 

{% embed url="https://datapane.com/u/shoumik/reports/VkGyaV3/dropdown-and-slider-with-altair/embed/" %}

### Plotly

Plotly has a generic slider component that can be used to change the data corresponding to any axis. While it does not have a specific slider for timeseries data, the generic slider can be used to create a date range slider. You can read more about sliders [here](https://plotly.com/python/sliders/).

To create a slider, we will take the same timeseries plot created previously with the dropdown menu and add a slider component below the plot - just a single extra parameter `xaxis1_rangeslider_visible`! 

```python
import pandas as pd
import plotly.graph_objects as go
import plotly.express as px

df = pd.read_csv("/Users/johnreid/PycharmProjects/pythonProject/internal-reports/Docs/covid_19_clean_complete.csv")

fig = go.Figure()

country_list = list(df['Country/Region'].unique())

for country in country_list:
    fig.add_trace(
        go.Scatter(
            x = df['Date'][df['Country/Region']==country],
            y = df['Confirmed'][df['Country/Region']==country],
            name = country, visible = True
        )
    )
    
buttons = []

for i, country in enumerate(country_list):
    args = [False] * len(country_list)
    args[i] = True
    
    #create a button object for the country we are on
    button = dict(label = country,
                  method = "update",
                  args=[{"visible": args}])
    
    #add the button to our list of buttons
    buttons.append(button)
    
fig.update_layout(
    updatemenus=[dict(
                    active=0,
                    type="dropdown",
                    buttons=buttons,
                    x = 0,
                    y = 1.1,
                    xanchor = 'left',
                    yanchor = 'bottom'
                )], 
    autosize=False,
    width=1000,
    height=800,
    xaxis1_rangeslider_visible = True
)

fig
```

This will give you something like this: 

{% embed url="https://datapane.com/u/shoumik/reports/43gPbv7/dropdown-and-slider-with-plotly/embed/" %}

### Bokeh

Similar to the Dropdown widget, Bokeh has a Date Range Slider widget specifically to work with timeseries data. This widget is different from the generic Range Slider widget. In order to make this widget work, a CustomJS callback is required.

```python
cols1=df.loc[:, ['country','date', 'confirmed']]
cols2 = cols1[cols1['country'] == 'Afghanistan' ]

Overall = ColumnDataSource(data=cols1)
Curr=ColumnDataSource(data=cols2)#plot and the menu is linked with each other by this callback function

callback = CustomJS(
	args=dict(source=Overall, sc=Curr), 
	code="""
		var f = cb_obj.value
		sc.data['date']=[]
		sc.data['confirmed']=[]
		for(var i = 0; i <= source.get_length(); i++){
			if (source.data['country'][i] == f){
				sc.data['date'].push(source.data['date'][i])
				sc.data['confirmed'].push(source.data['confirmed'][i])
			 }
		}   
		   
		sc.change.emit();
		"""
)

menu = Select(options=country_list,value='Afghanistan', title = 'Country')  # drop down menu
bokeh_p=figure(x_axis_label ='date', y_axis_label = 'confirmed', y_axis_type="linear",x_axis_type="datetime") #creating figure object 
bokeh_p.line(x='date', y='confirmed', color='green', source=Curr) # plotting the data using glyph circle

menu.js_on_change('value', callback) # calling the function on change of selection

date_range_slider = DateRangeSlider(value=(min(df['date']), max(df['date'])),
                                    start=min(df['date']), end=max(df['date']))

date_range_slider.js_link("value", bokeh_p.x_range, "start", attr_selector=0)
date_range_slider.js_link("value", bokeh_p.x_range, "end", attr_selector=1)
layout = column(menu, date_range_slider, bokeh_p)
show(layout) # displaying the layout
```

This is how it will look like: 

![](https://miro.medium.com/max/1400/1*oss5eDaeEaiZHWzEMaFU-g.png)

### 

