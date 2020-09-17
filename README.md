## Install Streamlit

`pip install streamlit`

## Run Streamlit

`streamlit hello`


## Create a first app

Create a file called `first_app.py`

```
import streamlit as st
import numpy as np
import pandas as pd
```
### Writing in the page

Set Title 

```
st.title('My First Application in Streamlit')
```

Write in the page

```
x=4

st.write(x, 'square is', x*x)
```

Write in page with magic method

```

x=4

x, 'square is', x*x
```

### DataFrames
Write a dataframe

```
st.write("Here's our first attempt at using data to create a table:")
st.write(pd.DataFrame({
    'first column': [1, 2, 3, 4],
    'second column': [10, 20, 30, 40]
}))
```

```

"""
# Title
Here's our first attempt at using data to create a table:
"""

df = pd.DataFrame({
  'first column': [1, 2, 3, 4],
  'second column': [10, 20, 30, 40]
})

df
```

### Graphs

Create a chart

```

chart_data = pd.DataFrame(
     np.random.randn(20, 3),
     columns=['a', 'b', 'c'])

st.line_chart(chart_data)

```

Create a map data

```
map_data = pd.DataFrame(
    np.random.randn(1000, 2) / [50, 50] + [37.76, -122.4],
    columns=['lat', 'lon'])

st.map(map_data)
```

Add a checkbox to show or hide the chart

```
if st.checkbox('Show dataframe'):
```
### Widgets

Adding a Slider

```
x = st.slider('x')
st.writer(x, 'square', x*x)

```


Add an option 

```
option = st.selectbox(
    'Which number do you like best?',
     df['first column'])

'You selected: ', option
```

```
option_side = st.sidebar.selectbox(
    'Which number do you like best?',
     ["hello", "world", "!"])

'You selected:', option_side
```

Adding a slider

```
import time
'Starting a long computation...'

# Initializing the variables
latest_iteration = st.empty()
bar = st.progress(0)
```

```
for i in range(100):
  # Update the progress bar with each iteration.
  latest_iteration.text(f'Iteration {i+1}')
  bar.progress(i + 1)
  time.sleep(0.1)

'...and now we\'re done!'
```

### Sidebar
Selectbox
```
add_selectbox = st.sidebar.selectbox(
    'How would you like to be contacted?',
    ('Email', 'Home phone', 'Mobile phone')
)

```

Slider

```
add_slider = st.sidebar.slider(
    'Select a range of values',
    0.0, 100.0, (25.0, 75.0)
)
```

## Exercise Uber Pickups

create a document called `uber_pickups.py`

start

```
import streamlit as st
import pandas as pd
import numpy as np
```

Set Title

```
st.title('Uber Pickups')

```

Importing data

```
DATE_COLUMN = 'Date/Time'
DATA_URL = ('https://s3-us-west-2.amazonaws.com/streamlit-demo-data/uber-raw-data-sep14.csv.gz')
```

Create a Function to load data

```
def load_data(nrows):
    data = pd.read_csv(DATA_URL, parse_dates=[DATE_COLUMN], nrows=nrows)
    return data
```

Now we add a text to notify the data is being loaded

```
# Create a text element and let the reader know the data is loading.
data_load_state = st.text('Loading data...')
# Load 10,000 rows of data into the dataframe.
data = load_data(10000)
# Notify the reader that the data was successfully loaded.
data_load_state.text("Done!")
```

Let's displya the data

```
st.subheader('Raw data')
st.write(data)
```

Remember if you wish to hide the table you can use a checkbox

```
if st.checkbox('Show raw data'):
```

Let's create a histogram

```
st.subheader('Number of pickups by hour')

hist_values = np.histogram(
    data[DATE_COLUMN].dt.hour, bins=24, range=(0,24))[0]

st.bar_chart(hist_values)
```

Let's create a map for the data

```
st.subheader('Map of all pickups')

st.map(data)
```

Why not add a filter

```
hour_to_filter = 17
filtered_data = data[data[DATE_COLUMN].dt.hour == hour_to_filter]
st.subheader(f'Map of all pickups at {hour_to_filter}:00')
st.map(filtered_data)
```

That's boring, let's create a dynamic filter

```
hour_to_filter = st.slider('hour', 0, 23, 17)
filtered_data = data[data[DATE_COLUMN].dt.hour == hour_to_filter]
st.subheader(f'Map of all pickups at {hour_to_filter}:00')
st.map(filtered_data)
```


