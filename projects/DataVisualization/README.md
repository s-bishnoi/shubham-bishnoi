[< Back to the portfolio](https://s-bishnoi.github.io/shubham-bishnoi/)

# Data Visualization

- 5 seconds of running bar graph visualization
- Some useful libraries
- Reading and editing the data
- Getting different colours for provinces
- Getting the bar chats for each date
- Getting the running bar graph
- References

A running graph for COVID-19 total number of cases in provinces of Canada from 29th February to 18th September in 2020.

### The visualization looks something like this

<iframe src='//gifs.com/embed/the-spread-of-covid-19-across-the-canada-in-a-running-bar-graph-JyBw1J' frameborder='0' scrolling='no' width='900px' height='600px' style='-webkit-backface-visibility: hidden;-webkit-transform: scale(1);' ></iframe>

### Some useful libraries

```
import pandas as pd
import datetime as dt
import matplotlib.pyplot as plt
import matplotlib.ticker as ticker
import matplotlib.animation as animation
from IPython.display import HTML
import numpy as np
import matplotlib
import matplotlib.colors as mc
import colorsys
from random import randint
import re
```

### Reading and editing the data

```
data = pd.read_csv('./data.csv',keep_default_na=False)
df = data[['date','prname','numtotal']]
df = df[df['prname'] != 'Repatriated travellers']
df = df[df['prname'] != 'Canada']
df.date = pd.to_datetime(df.date, format='%d%m%Y', errors='ignore')
df.head()
dateList = df.date.unique()
```

### Getting different colours for provinces

```
def transform_color(color, amount = 0.5):

    try:
        c = mc.cnames[color]
    except:
        c = color
        c = colorsys.rgb_to_hls(*mc.to_rgb(c))
    return colorsys.hls_to_rgb(c[0], 1 - amount * (1 - c[1]), c[2])

all_names = df['prname'].unique().tolist()
random_hex_colors = []
for i in range(len(all_names)):
    random_hex_colors.append('#' + '%06X' % randint(0, 0xFFFFFF))

rgb_colors = [transform_color(i, 1) for i in random_hex_colors]
rgb_colors_opacity = [rgb_colors[x] + (0.825,) for x in range(len(rgb_colors))]
rgb_colors_dark = [transform_color(i, 1.12) for i in random_hex_colors]
```

### Getting the bar chats for each date

```
def draw_barchart(myDate):
    dff = (df[df['date'].eq(myDate)]
       .sort_values(by='numtotal', ascending=True)
       .tail(10))
    ax.clear()
    
    normal_colors = dict(zip(df['prname'].unique(), rgb_colors_opacity))
    dark_colors = dict(zip(df['prname'].unique(), rgb_colors_dark))
    
    y_pos = list(range(0, len(dff)))
    ax.barh(y_pos,dff['numtotal'],color = [normal_colors[x] for x in dff['prname']],
            edgecolor =([dark_colors[x] for x in dff['prname']]))
    ax.set_yticks(y_pos)
    ax.set_yticklabels(' ')
    for i, (numtotal, prname) in enumerate(zip(dff['numtotal'], dff['prname'])):
        ax.text(numtotal, i,numtotal,size=14, weight=600, ha='left', va='center')   # 38194.2: value
        ax.text(numtotal, i,prname,size=14, ha='right',  va='center')  # Tokyo: name

    ax.text(1, 0.4, myDate, transform=ax.transAxes, size=46, ha='right')
    ax.grid(which='major', axis='x', linestyle='-')
    ax.set_axisbelow(True)
    ax.text(0, 1.12, 'The Spread of COVID-19 across the Canada',
            transform=ax.transAxes, size=20, weight=400, ha='left')
    ax.margins(0, 0.01)
    ax.xaxis.set_ticks_position('top')
    ax.tick_params(axis='x', colors='#777777', labelsize=12)
    ax.text(0, 1.06, 'Number of Confirmed Cases', transform=ax.transAxes, size=12, color='#777777')
```

### Getting the running bar graph

```
fig, ax = plt.subplots(figsize=(15, 8))
animator = animation.FuncAnimation(fig, draw_barchart, frames=dateList)
HTML(animator.to_jshtml())
```

### References

Canada, P. (2020, September 13). Government of Canada. Retrieved September 19, 2020, from https://www.canada.ca/en/public-health/services/diseases/2019-novel-coronavirus-infection.html