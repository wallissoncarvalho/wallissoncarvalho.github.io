---
layout: page
title: HydroBr
subtitle: HydroBr is an open-source package to work with Brazilian hydrometeorological time series.
---

<h3 style='text-align: center'>
  <a href="https://pypi.org/project/hydrobr/">PyPi Page</a>
  <br>
  <a href="https://zenodo.org/badge/latestdoi/246373223">
  <img src="https://zenodo.org/badge/246373223.svg" alt="DOI"></a>
  <a href="https://img.shields.io/badge/python-3.6%20%7C%203.7%20%7C%203.8-blue">
  <img src="https://img.shields.io/badge/python-3.6%20%7C%203.7%20%7C%203.8-blue" alt="PyV"></a>
</h3>


Here I'll show you how to use some of HydroData's functions. If you have any doubts about the functions/methods
available, you use the python's help function or read the
[docs page](https://github.com/wallissoncarvalho/hydrobr/blob/master/README.md#modules---documentation).

## ANA database
Here is an example of usage of the functions that provides data from the Brazilian National Water Agency
(Agência Nacional de Águas - ANA) database.

#### Importing the package

<br>

```python
import hydrobr
```
<br>

### Getting data from ANA
#### List of stations

HydroBr has two functions for list of stations from ANA:

- `hydrobr.get_data.ANA.list_prec_stations` - To get the list of precipitation stations
- `hydrobr.get_data.ANA.list_flow_stations` - To get the list of flow/stage stations

When you use one of these functions, you will be able to get the list of stations directly from the ANA database (source='ANA'), where you will get a list with many stations that have no one registered data, leading you to lost time in your search for data.

I highly recommend you to use the functions standard source, ['ANAF'](https://zenodo.org/record/3755065), where you will get a filtered list of stations, which already contains some description about each of that. The description includes:
- date of first measured data (FirstDate)
- date of last measured data (EndDate)
- number of years with data (NYD)
- missing data percentage between the start and the last date (MD)
- number of years without any missing data (N_YWOMD)
- percentage of years with missing data (YWMD).


```python
# To get the list of prec stations - source='ANAF' is the standart
list_stations = hydrobr.get_data.ANA.list_prec_stations() 
# To show the first five rows of the data
list_stations.head() 
```
<br>
<div class="table-responsive">
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Code</th>
      <th>Type</th>
      <th>SubBasin</th>
      <th>City</th>
      <th>State</th>
      <th>Responsible</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>StartDate</th>
      <th>EndDate</th>
      <th>NYD</th>
      <th>MD</th>
      <th>N_YWOMD</th>
      <th>YWMD</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>SALINÓPOLIS</td>
      <td>47000</td>
      <td>2</td>
      <td>32</td>
      <td>SALINÓPOLIS</td>
      <td>PARÁ</td>
      <td>INMET</td>
      <td>-0.6500</td>
      <td>-47.5500</td>
      <td>1958/01/01</td>
      <td>1964/12/31</td>
      <td>7</td>
      <td>25.0</td>
      <td>0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>SALINÓPOLIS</td>
      <td>47002</td>
      <td>2</td>
      <td>32</td>
      <td>SALINÓPOLIS</td>
      <td>PARÁ</td>
      <td>ANA</td>
      <td>-0.6231</td>
      <td>-47.3536</td>
      <td>1977/12/09</td>
      <td>2019/08/31</td>
      <td>43</td>
      <td>3.5</td>
      <td>35</td>
      <td>18.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CURUÇA</td>
      <td>47003</td>
      <td>2</td>
      <td>32</td>
      <td>CURUÇA</td>
      <td>PARÁ</td>
      <td>ANA</td>
      <td>-0.7375</td>
      <td>-47.8536</td>
      <td>1981/07/01</td>
      <td>2019/07/31</td>
      <td>39</td>
      <td>2.4</td>
      <td>29</td>
      <td>25.6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>PRIMAVERA</td>
      <td>47004</td>
      <td>2</td>
      <td>32</td>
      <td>PRIMAVERA</td>
      <td>PARÁ</td>
      <td>ANA</td>
      <td>-0.9294</td>
      <td>-47.0994</td>
      <td>1982/02/18</td>
      <td>2019/08/31</td>
      <td>38</td>
      <td>0.0</td>
      <td>35</td>
      <td>7.9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>MARUDA</td>
      <td>47005</td>
      <td>2</td>
      <td>32</td>
      <td>MARAPANIM</td>
      <td>PARÁ</td>
      <td>ANA</td>
      <td>-0.6336</td>
      <td>-47.6583</td>
      <td>1989/08/21</td>
      <td>2019/07/31</td>
      <td>31</td>
      <td>5.0</td>
      <td>20</td>
      <td>35.5</td>
    </tr>
  </tbody>
</table>
</div>

<br>

```python
# Getting the first five stations code as a list
stations_code = list_stations.Code.to_list()[:5] 
```


If we need to know about the spatial distribution from a list of stations, we can use the
`hydrobr.Plot.spatial_stations` function.

- To use this function you'll need a mapbox access token, that you can get
[here](https://account.mapbox.com/access-tokens/).
- The functions from `hydrobr.Plot` provides `plotly` figure, to show that we need to import the  `plotly` library.
Thus it is possible to configure the layout of the plot.

In the example below, I will plot the flow station distribution in the state of Rio de Janeiro.

```python
from plotly.offline import plot
map_box_access_token = 'your-mapbox-token-access-here'
flow_stations_list = hydrobr.get_data.ANA.list_flow_stations(
                                        state='RIO DE JANEIRO')
```
<br>

```python
#Get the spatial stations plot fig
spatial_fig=hydrobr.Plot.spatial_stations(flow_stations_list,map_box_access_token)
# If you want tu update the plot zoom! 
spatial_fig.update_layout(mapbox=dict(zoom=7))
plot(spatial_fig,filename='spatial' + '.html')
```
The `.html` figure output:

<div class="row">
      <div class="embed-responsive embed-responsive-4by3">
      <iframe class="embed-responsive-item"  src="https://wallissoncarvalho.github.io/assets/html_posts/spatial_stations"></iframe>
      </div>
</div>

<br>

#### Stations' Data

HydroBr has three functions to gate data from the ANA database:

- `hydrobr.get_data.ANA.prec_data` - To get the precipitation data from a list of precipitation stations
- `hydrobr.get_data.ANA.flow_data` - To get the flow data from a list of flow/stage stations
- `hydrobr.get_data.ANA.stage_data` - To get the stage data from a list of flow/stage stations



```python
#Gettin the data
data_stations = hydrobr.get_data.ANA.prec_data(stations_code) 
```

    100%|██████████████████████████████████| 5/5 [00:13<00:00,  2.62s/it]
    



<br>


```python
data_stations.info()
```

    <class 'pandas.core.frame.DataFrame'>
    DatetimeIndex: 22523 entries, 1958-01-01 to 2019-08-31
    Freq: D
    Data columns (total 5 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   00047000  1918 non-null   float64
     1   00047002  14516 non-null  float64
     2   00047003  13397 non-null  float64
     3   00047004  13524 non-null  float64
     4   00047005  10212 non-null  float64
    dtypes: float64(5)
    memory usage: 1.0 MB
   
    

In the last command, we used the pandas' `info` function, and it returns basic information about the data that we got.
For instance, for the station with code 00047003, we could see 13397 registered daily data (Freq: D) between
1958-01-01 and 2019-08-31.

But we can plot the temporal data availability using the Gantt Plot.

The functions from `hydrobr.Plot` provides `plotly` figure, to show that we need to import the  `plotly` library.
Thus it is possible to configure the layout of the plot.


```python
from plotly.offline import plot
```

<br>

```python
gantt_fig = hydrobr.Plot.gantt(data_stations) #Get the Gantt Fig

#Updating the layout
gantt_fig.update_layout(
    autosize=False,
    width=1000,
    height=500,
    xaxis_title = 'Year',
    yaxis_title = 'Station Code',
    font=dict(family="Courier New, monospace", size=12))

#To plot and save the gantt plot as html
plot(gantt_fig,filename='gantt' + '.html') 
```
The `.html` figure output:

<div class="row">
      <div class="embed-responsive embed-responsive-16by9">
      <iframe class="embed-responsive-item"  src="https://wallissoncarvalho.github.io/assets/html_posts/gantt_ana"></iframe>
      </div>
</div>