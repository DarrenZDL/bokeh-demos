# Intro to data visualization with Bokeh

## PyData at Strata

- Sarah Bird [@birdsarah](https://twitter.com/birdsarah)
- Bryan Van de Ven [@bryvdv](https://twitter.com/bryvdv)

http://github.com/bokeh/bokeh-demos/

<small>March 29th, 2016</small>


---

## Overview

- 11:30 - 12:30 Intro to Bokeh ecosystem (code-along)
- 12:30 - 13:30 LUNCH
- 13:30 - 14:30 Hands-on examples (tutorials or bring your own data)

---


## Bokeh

<img class="slide_image" src="images/gallery-screenshot.png" style="position: absolute; top: 0; right: 0;">

- data visualization
- web-based & interactive
- no javascript required
- large data
- streaming data


---

## Core concepts for today

- Overview of Bokeh's functionality
- Navigating Bokeh's interfaces 
 - charts, plotting, and probably not models!
- Bokeh server 
 - what it is, what it isn't, do I need it
- Sharing your work 
 - notebook, standalone files, server 
- Awareness of the Bokeh ecosystem

---
<img class="slide_image" src="images/box_chart.png" style="position: absolute; top: -20px; right: 0; width: 200px; height: 200px">
### Demo 1 - Charts in the notebook
quick & easy data exploration

* work in the notebok
```python
from bokeh.io import output_notebook, show
output_notebook()
```
* make charts - simple one-liners accept tabular data
```python
from bokeh.charts import Bar, BoxPlot, Histogram, Scatter....
Scatter(df, x='mpg', y='hp', color='cyl')
BoxPlot(df, values='mpg', label='cyl', marker='square')
```

<small>
[bokeh.pydata.org/en/latest/docs/user_guide/charts.html](http://bokeh.pydata.org/en/latest/docs/user_guide/charts.html)
</small>

---

<img class="slide_image" src="images/clustering.png" style="position: absolute; top: -50px; right: -50px; width: 400px; height: 200px">
### Demo 2 - Server - clustering
interact in the browser, run python code
```python
def update_samples_or_dataset(attrname, old, new):
    dataset = dataset_select.value
    algorithm = algorithm_select.value
    X, y = get_dataset(dataset, n_samples)
    X, y_pred = clustering(X, algorithm, n_clusters)
    new_data = {'x': X[:, 0], 'y': X[:, 1]}
    source.data = new_data
    
algorithm_select = Select(value='MiniBatchKMeans', options=opts)
algorithm_select.on_change('value', update_algorithm_or_clusters)
```

<small>
[github.com/bokeh/bokeh/tree/master/examples/app/clustering](https://github.com/bokeh/bokeh/tree/master/examples/app/clustering)
</small>

---

### Demo 3 - Server - streaming
<img class="slide_image" src="images/ohlc.png" style="position: absolute; top: 0px; right: 0px; width: 400px; height: 250px">

connect your plot to a streaming <br />
data source

bokeh will take care of the rest


```python
def update():
    new_data = get_new_data()
    source.stream(new_data, 300)
    
doc.add_periodic_callback(update, 50)
```
<small>
[github.com/bokeh/bokeh/tree/master/examples/app/ohlc](https://github.com/bokeh/bokeh/tree/master/examples/app/ohlc)
</small>

---

<img class="slide_image" src="images/datashader.png" style="position: absolute; top: 0px; right: 0px; width: 300px; height: 3 00px">
### Demo 4 - Datashader
Plotting **very** large datasets meaningfully

Uses bokeh's interactivity, hooks into <br />
powerful python libraries

[datashader.readthedocs.org](http://datashader.readthedocs.org)

---

## Core concepts for today

- Overview of Bokeh's functionality
- Navigating Bokeh's interfaces 
 - charts, plotting, and probably not models!
- Bokeh server 
 - what it is, what it isn't, do I need it
- Sharing your work 
 - notebook, standalone files, server 
- Awareness of the Bokeh ecosystem

---

## Bokeh - Core

- Python-based - http://bokeh.pydata.org
- Charts (Bar chart, Histogram...)
- Custom plots
- User interactions & styling
- Server interactions

---

## Bokeh - Ecosystem

- Other language bindings: rbokeh (actively maintained), scala (activeley maintained), julia (not maintained), typescript (actively maintained)
- Datashader - interact with BIG<sup>**</sup> data
- Write your own custom models

---

## Interfaces

There are a number of ways to use Bokeh - pick the one that's right for you:

* Charts (high speed)
* Plotting (sensible defaults)
* Models (high customization)

We'll come back to this multiple times today.

---

<h5 style="padding-top: 1em; margin-bottom: -0.2em"> `bokeh.charts` (high speed)</h5>

- One-line charts
- Processes your data & spits out a chart


<h5 style="padding-top: 1em; margin-bottom: -0.2em"> `bokeh.plotting` (sensible defaults)</h5>

- Tries to pick sensible defaults
- You organize your data, it organizes your plot

<h5 style="padding-top: 1em; margin-bottom: -0.2em"> `bokeh.models` (high customization)</h5>

- The lowest level
- Offers you the most control
- Do all the work yourself

---

## Interfaces

* Charts (high speed)
* Plotting (sensible defaults)
* Models (high customization)

We'll come back to this multiple times today.


---

# Charts

---

### Charts
* Area, Bar, Box, Donut, Dot, Heatmap, Histogram, Horizon, Line, Scatter,
Step, Timeseries
* Wherever possible, the interface is designed to be simple to use with [pandas](http://pandas.pydata.org), by accepting a DataFrame and names of columns directly to specify data.

[reference](http://bokeh.pydata.org/en/latest/docs/reference/charts.html#module-bokeh.charts)

Note: 
would be nice to make a little visual grid of all of these
dots vs scatter?
what is blend
box plot whiskers are: the lowest datum still within 1.5 IQR of the lower quartile, and the highest datum still within 1.5 IQR

---




Tall data is preferred  

Common inputs:

* values / label (Bar, Histogram, Area)
* x / y (Scatter, Line)

Defaults:

* plot_width, plot_height, tools, legend, xgrid, ygrid, xlabel, ylabel, xscale, yscale, title_text_font_size, responsive.....

Legend:

* can pe positioned around the polot
* sort_legend: List of tuples to use for sorting the legend - can be different than sorting used for the rest of the chart

---

### Becoming a charts power user

* Understand:
 * Attribute Specification - `label=['col1','col2']`
 * blend - `blend('col1', 'col2', name='temp', labels_name='sensor')`
 * color - `color=color('a', palette=['red', 'green', 'blue'])`
* Aggregations (sum, mean, count, nunique, median, min, max)
* Chart operations - blend, stack
* color, dodge
* Set-up your defaults

---

### Charts resources

* [bokeh.pydata.org/en/latest/docs/user_guide/charts.html](http://bokeh.pydata.org/en/latest/docs/user_guide/charts.html)
* [bokeh.pydata.org/en/latest/docs/reference/charts.html](http://bokeh.pydata.org/en/latest/docs/reference/charts.html)
* [examples/howto/charts/](https://github.com/bokeh/bokeh/tree/master/examples/howto/charts)
* [examples/charts/](https://github.com/bokeh/bokeh/tree/master/examples/charts)

---

At the heart of Bokeh is the `ColumnDataSource`

| 'column of xs' | 'column of ys'
|---|---
|0|1 
|1|2
|3|4

[notebooks/ColumnDataSource.ipynb](http://localhost:8888/notebooks/notebooks/ColumnDataSource.ipynb)


---

# Plotting

---

#### Plotting


---

### Output

* output_notebook
* output_file
* embedding - file_html, components

---

### Resources

* `INLINE` - in the file
* `CDN` - load from web
* There are other options

---

# Server

---

## Bokeh Server

* easily connect visualization to python code e.g.
 * drive scikit-learn from data point selection in 10 lines of code
 * perform complex downsampling on pan/zoom
* streaming data

---

How the server works

---

Some code that shows the core principles of bokeh server

---

Embedding

---

## Server gotchas 1 - Data updating

* update your ColumnDataSource in one go

<br />
#### GOOD
```python
new_data = {'x': [...], 'y': [....], ....}
source.data = new_data
```

#### BAD
```python
source.data['x'] = [...]
source.data['y'] = [...]
```

---

## Server gotchas 2 - Session cleanup

Periodic callback
--unused-session-lifetime
--check-unused-sessions

---

## Server gotchas 3 - Embedding

--allow-websocket-origin

---

## Server

Whitelisting your host

---

# Datashader


---
## Getting set-up

- In order of simplicity:
 - Anaconda https://www.continuum.io/downloads
 - **Miniconda http://conda.pydata.org/miniconda.html (my personal preference)**
   
    `conda install bokeh`

 - Pip (can take a long time due to dependencies)

      `pip install bokeh`

***You don't need to do this now, we will be available to help duing lunch, so you're ready for the tutorials***

---

# Appendix

* compat
* user & reference guide
* search
* examples

---

## Hacker-slides how-to

- Separate slides using '`---`' on a blank line
- Write github flavored markdown
- Click 'Present' (top right) when you're ready to talk
- There is also a speaker view, with notes - press '`s`'
- Press '`?`' with focus on the presentation for shortcuts
- <em>You can use html when necessary</em>
- Share the 'Present' URL with anyone you like!

Learn more:
- [RevealJS Demo/Manual](http://lab.hakim.se/reveal-js)
- [RevealJS Project/README](https://github.com/hakimel/reveal.js)
- [GitHub Flavored Markdown](https://help.github.com/articles/github-flavored-markdown)

Note:
- Anything after `Note:` will only appear here
