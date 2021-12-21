---
title: "Part 3: Origin-Destination Analysis"
date: 2021-12-18
published: true
tags: [dataviz, hvplot, MovingPandas]
excerpt: "Visualization using LODES Origin-destination data"

hv-loader:
  3-emp_eff: ["charts/3_emp_eff.html","800"]
  3-datashade: ["charts/3_datashade.html","800"]
  3-orig-dest: ["charts/3_orig_dest.html","800"]
  3-single-traj: ["charts/3_single_traj.html","800"]
  3-od-hist: ["charts/3_od_hist.html","800']
  3_emp_eff_pct:["charts/3_emp_eff_pct.html","800"]

toc: true
toc_sticky: true
---

## Part 3: Origin-Destination Analysis

The LODES origin-destination dataset can be described as the heart of the entire database structure. Each row has a pair of Census blocks defining the home and work location of one worker, along with other variables denoting age, income, and general category of the worker. We are able to build a few insightful visualizations on top of this.

As usual, we take in OD data directly from the Census FTP server. With all 11 years, the dataset is over 25 million rows long, almost unmanageable. For the dotmap and network map, we thus will only consider 2019.

```python
years = ['2009','2010','2011','2012','2013','2014','2015','2016','2017','2018','2019']

url = "https://lehd.ces.census.gov/data/lodes/LODES7/pa/od/pa_od_main_JT00_"
od_all = dd.concat([pd.read_csv(gzip.open(BytesIO(urlopen(url+year+".csv.gz").read()))).assign(year=int(year)) for year in years])
```

```python
od = od_all[od_all['year'] == 2019].compute()
od
```

<div>
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
      <th>w_geocode</th>
      <th>h_geocode</th>
      <th>S000</th>
      <th>SA01</th>
      <th>SA02</th>
      <th>SA03</th>
      <th>SE01</th>
      <th>SE02</th>
      <th>SE03</th>
      <th>SI01</th>
      <th>SI02</th>
      <th>SI03</th>
      <th>createdate</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>420010301011003</td>
      <td>420010303002134</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>20211018</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>1</th>
      <td>420010301011003</td>
      <td>420210137001010</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>20211018</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>2</th>
      <td>420010301011012</td>
      <td>420010303002079</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>20211018</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>3</th>
      <td>420010301011012</td>
      <td>420550120003013</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>20211018</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>4</th>
      <td>420010301011012</td>
      <td>420550120006025</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>20211018</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5128502</th>
      <td>421330240022035</td>
      <td>421330240013021</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>5128503</th>
      <td>421330240022035</td>
      <td>421330240021000</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>20211018</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>5128504</th>
      <td>421330240022035</td>
      <td>421330240021004</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>20211018</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>5128505</th>
      <td>421330240022035</td>
      <td>421330240021072</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>5128506</th>
      <td>421330240022035</td>
      <td>421330240022034</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>2019</td>
    </tr>
  </tbody>
</table>
<p>5128507 rows × 14 columns</p>
</div>

Aggregation to block group and merge with shapefiles.

```python
od['w_geocode'] = od['w_geocode'].astype('str').str[:12]
od['h_geocode'] = od['h_geocode'].astype('str').str[:12]
od["id"] = od.index + 1
```

```python
phl_bg = gpd.read_file("bg/tl_2019_42_bg/tl_2019_42_bg.shp").query("COUNTYFP in ['017','029','045','091','101']").to_crs(2272)
phl_bg = phl_bg[['GEOID','geometry']].assign(geometry=phl_bg.geometry.centroid)
phl_bg = phl_bg.to_crs(4326)
```

```python
od_geo = od.merge(phl_bg.rename(columns={"geometry":"geo_w"}), left_on='w_geocode', right_on='GEOID', how='inner')
od_geo = od_geo.merge(phl_bg.rename(columns={"geometry":"geo_h"}), left_on='h_geocode', right_on='GEOID', how='inner')
```

### Viz 1: Dot map of each worker residence and job

Firstly we can produce a datashaded dot map of each worker residence and job, similar to the hexbin in part 1. To do so, it is best to convert the data to a SpatialPandas `GeoDataFrame`, which is developed by Holoviews to work well with hvPlot and Datashader.  Here, the data seems to be more complete and discernable than the hexmap.

```python
import spatialpandas as spd
od_ds_work = spd.GeoDataFrame(od_geo.drop(columns='geo_h'))
od_ds_home = spd.GeoDataFrame(od_geo.drop(columns='geo_w'))
```

```python
combine = (
    od_ds_work.hvplot(c='S000', tiles='StamenTonerRetina', datashade=True, project=True, frame_height=600, frame_width=600, xaxis=None,yaxis=None,cmap='hot')
    +
    od_ds_home.hvplot(c='S000', tiles='StamenTonerRetina', datashade=True, project=True, frame_height=600, frame_width=600, xaxis=None,yaxis=None)
) 
hv.save(combine,"./charts/3_datashade.html", backend='bokeh')
```

<div id="3-datashade"></div>

### Viz 2: Employment efficiency and growing spatial mismatch over time

Adding the temporal dimension to the O-D analysis can really aid in understand how people's general commute distances have changed over time. Because analysis with this many rows is unfeasible, we will sample the O-D data to a tenth of the total.

Next we categorize each O-D pair on four categories: looking at if the home lies in Philadelphia or the surrounding counties, and if the workplace lies in Philadelphia or the surrounding counties. This produces an in-city commute, in-suburb commute, suburb-to-city commute, and city-to-suburb commute. In Census parlance, this rate of cross-border commuting is called "employment efficiency". We produce two plots below: the first is calculating the percent change of the count of each type of commute, while the second is a stacked bar showing the absolute percent.

The last commute (city-to-suburb) is also commonly known as the reverse commute, and is a product of the ever-growing job sprawl and suburbanization trends of the last fifty years. In Philadelphia's case, the region has one of the highest rates of reverse commuting, nearing almost forty percent. Nonetheless, the barplot shows that suburb-suburb jobs still have a clear majority over any commute involving the city. And the strong percentage growth of the reverse commute seen in the first chart is a significant trend that also reflects continued suburban domiance of job workplaces.

The consequences of this trend are severe. The regional transit network, Regional Rail in particular, has not been optimized well for the reverse commute as opposed to the traditional peak-direction; offices have also not been keen to locate close to rail stations, opting for farther business parks along highways. Numerous policies could be modified or put into place to incentivize offices to relocate closer to stations, or better yet, move back into the city.

```python
od_all_samp = od_all[['w_geocode','h_geocode','year','S000']].sample(frac=0.1).compute()
```

```python
cond = [np.logical_and(od_all_samp['h_geocode'].astype('str').str[2:5] == '101',od_all_samp['w_geocode'].astype('str').str[2:5] == '101'),
        np.logical_and(od_all_samp['h_geocode'].astype('str').str[2:5] == '101',od_all_samp['w_geocode'].astype('str').str[2:5] != '101'),
        np.logical_and(od_all_samp['h_geocode'].astype('str').str[2:5] != '101',od_all_samp['w_geocode'].astype('str').str[2:5] == '101'),
        np.logical_and(od_all_samp['h_geocode'].astype('str').str[2:5] != '101',od_all_samp['w_geocode'].astype('str').str[2:5] != '101')]

choice = ['in Phila','city-to-suburb commute','suburb-to-city commute','in suburb']

od_all_samp['emp_eff'] = np.select(cond,choice)
```

```python
od_eff = od_all_samp.groupby(['year','emp_eff'])['S000'].sum().reset_index()
od_eff['pctchg'] = od_eff.sort_values(['emp_eff','year']).groupby(['emp_eff'])['S000'].apply(lambda x: x.div(x.iloc[0]).subtract(1).mul(100))

hv.save(hv.HLine(0).opts(hv.opts.HLine(line_dash='dotted')) * od_eff.hvplot.line(x='year',y='pctchg',by='emp_eff').opts(legend_position='top'),
"./charts/3_emp_eff.html",backend='bokeh')

```

<div id="3-emp-eff"></div>

```python
hv.save(od_all_samp.groupby(['year','emp_eff'])['S000'].sum().hvplot.bar(stacked=True, legend='top_left'),
"./charts/3_emp_eff_pct.html",backend='bokeh')
```

<div id="3-emp-eff-pct"></div>

### Viz 3: Origin-destination network flow graph

Now, we get to the meat and bones of O-D analysis: the origin-destination network flow graph. This graph will aggregate the lines of origin (home) to destination (work) into nodes and edges (or flows). The library used here is MovingPandas, a new library that deals with all sorts of temporal-spatial trajectories such as flightpaths and bird migrations. However, as there is no time information associated with the O-D data unfortunately, we will stick with the temporal dimension.

First we must melt the data so that each origin and destination block group is on a separate row. Then we set a mock time for each O and D just so the data can be processed.

```python
valuevar = ['geo_w','geo_h']
od_long = od_geo.melt(value_vars=valuevar,id_vars=od.columns.difference(valuevar),var_name='location',value_name="geometry")
od_long['t'] = np.where(od_long['location'] == 'geo_h',pd.to_datetime('2019-01-01 06:00:00'),pd.to_datetime('2019-01-01 08:00:00'))
od_long = od_long.set_index('t')
od_long.sort_values('id')
```

<div>
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
      <th>S000</th>
      <th>SA01</th>
      <th>SA02</th>
      <th>SA03</th>
      <th>SE01</th>
      <th>SE02</th>
      <th>SE03</th>
      <th>SI01</th>
      <th>SI02</th>
      <th>SI03</th>
      <th>createdate</th>
      <th>h_geocode</th>
      <th>id</th>
      <th>w_geocode</th>
      <th>year</th>
      <th>location</th>
      <th>geometry</th>
    </tr>
    <tr>
      <th>t</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01 08:00:00</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>420171001022</td>
      <td>989830</td>
      <td>420171001021</td>
      <td>2019</td>
      <td>geo_w</td>
      <td>POINT (-74.92836437904931 40.07687511288584)</td>
    </tr>
    <tr>
      <th>2019-01-01 06:00:00</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>420171001022</td>
      <td>989830</td>
      <td>420171001021</td>
      <td>2019</td>
      <td>geo_h</td>
      <td>POINT (-74.96474358791602 40.0606801695214)</td>
    </tr>
    <tr>
      <th>2019-01-01 08:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>420171001022</td>
      <td>989831</td>
      <td>420171001021</td>
      <td>2019</td>
      <td>geo_w</td>
      <td>POINT (-74.92836437904931 40.07687511288584)</td>
    </tr>
    <tr>
      <th>2019-01-01 06:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>420171001022</td>
      <td>989831</td>
      <td>420171001021</td>
      <td>2019</td>
      <td>geo_h</td>
      <td>POINT (-74.96474358791602 40.0606801695214)</td>
    </tr>
    <tr>
      <th>2019-01-01 08:00:00</th>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>20211018</td>
      <td>420171001022</td>
      <td>989832</td>
      <td>420171001021</td>
      <td>2019</td>
      <td>geo_w</td>
      <td>POINT (-74.92836437904931 40.07687511288584)</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2019-01-01 08:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>421010390008</td>
      <td>4579656</td>
      <td>421019891001</td>
      <td>2019</td>
      <td>geo_w</td>
      <td>POINT (-75.0057899620279 40.03434987047037)</td>
    </tr>
    <tr>
      <th>2019-01-01 06:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>421019802001</td>
      <td>4579657</td>
      <td>421019891001</td>
      <td>2019</td>
      <td>geo_h</td>
      <td>POINT (-75.04900990952665 40.06974013053726)</td>
    </tr>
    <tr>
      <th>2019-01-01 08:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>421019802001</td>
      <td>4579657</td>
      <td>421019891001</td>
      <td>2019</td>
      <td>geo_w</td>
      <td>POINT (-75.0057899620279 40.03434987047037)</td>
    </tr>
    <tr>
      <th>2019-01-01 08:00:00</th>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>20211018</td>
      <td>421019802001</td>
      <td>4579658</td>
      <td>421019891001</td>
      <td>2019</td>
      <td>geo_w</td>
      <td>POINT (-75.0057899620279 40.03434987047037)</td>
    </tr>
    <tr>
      <th>2019-01-01 06:00:00</th>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>20211018</td>
      <td>421019802001</td>
      <td>4579658</td>
      <td>421019891001</td>
      <td>2019</td>
      <td>geo_h</td>
      <td>POINT (-75.04900990952665 40.06974013053726)</td>
    </tr>
  </tbody>
</table>
<p>3012858 rows × 17 columns</p>
</div>

We sample the data for 2000 O-D pairs to save time; this is fine as we are only concerned with relative flows, and take in point geometry.

```python
# The trajectory aggregation function is very slow, so the OD data will be sampled for 2000 pairs
sample_od = np.random.choice(od_long['id'].unique(),2000)
od_geo_sample = gpd.GeoDataFrame(od_long[od_long['id'].isin(sample_od)],geometry='geometry',crs='EPSG:4326')
od_geo_sample.sort_values(['id','location'])
```

<div>
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
      <th>S000</th>
      <th>SA01</th>
      <th>SA02</th>
      <th>SA03</th>
      <th>SE01</th>
      <th>SE02</th>
      <th>SE03</th>
      <th>SI01</th>
      <th>SI02</th>
      <th>SI03</th>
      <th>createdate</th>
      <th>h_geocode</th>
      <th>id</th>
      <th>w_geocode</th>
      <th>year</th>
      <th>location</th>
      <th>geometry</th>
    </tr>
    <tr>
      <th>t</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01 06:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>20211018</td>
      <td>420454041012</td>
      <td>990434</td>
      <td>420171001021</td>
      <td>2019</td>
      <td>geo_h</td>
      <td>POINT (-75.34330 39.88576)</td>
    </tr>
    <tr>
      <th>2019-01-01 08:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>20211018</td>
      <td>420454041012</td>
      <td>990434</td>
      <td>420171001021</td>
      <td>2019</td>
      <td>geo_w</td>
      <td>POINT (-74.92836 40.07688)</td>
    </tr>
    <tr>
      <th>2019-01-01 06:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>20211018</td>
      <td>420912001052</td>
      <td>990507</td>
      <td>420171001021</td>
      <td>2019</td>
      <td>geo_h</td>
      <td>POINT (-75.06218 40.11149)</td>
    </tr>
    <tr>
      <th>2019-01-01 08:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>20211018</td>
      <td>420912001052</td>
      <td>990507</td>
      <td>420171001021</td>
      <td>2019</td>
      <td>geo_w</td>
      <td>POINT (-74.92836 40.07688)</td>
    </tr>
    <tr>
      <th>2019-01-01 06:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>20211018</td>
      <td>421010348012</td>
      <td>992342</td>
      <td>420171001021</td>
      <td>2019</td>
      <td>geo_h</td>
      <td>POINT (-75.02046 40.05865)</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2019-01-01 08:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>20211018</td>
      <td>420171014011</td>
      <td>4576670</td>
      <td>421019891001</td>
      <td>2019</td>
      <td>geo_w</td>
      <td>POINT (-75.00579 40.03435)</td>
    </tr>
    <tr>
      <th>2019-01-01 06:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>421010333003</td>
      <td>4579116</td>
      <td>421019891001</td>
      <td>2019</td>
      <td>geo_h</td>
      <td>POINT (-75.04795 40.05165)</td>
    </tr>
    <tr>
      <th>2019-01-01 08:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>421010333003</td>
      <td>4579116</td>
      <td>421019891001</td>
      <td>2019</td>
      <td>geo_w</td>
      <td>POINT (-75.00579 40.03435)</td>
    </tr>
    <tr>
      <th>2019-01-01 06:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>421010356021</td>
      <td>4579415</td>
      <td>421019891001</td>
      <td>2019</td>
      <td>geo_h</td>
      <td>POINT (-75.04733 40.10327)</td>
    </tr>
    <tr>
      <th>2019-01-01 08:00:00</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>20211018</td>
      <td>421010356021</td>
      <td>4579415</td>
      <td>421019891001</td>
      <td>2019</td>
      <td>geo_w</td>
      <td>POINT (-75.00579 40.03435)</td>
    </tr>
  </tbody>
</table>
<p>4000 rows × 17 columns</p>
</div>

Next we call MovingPandas to build the initial `TrajectoryCollection`, an object holding all trajectories and their connected line segments (each O-D pair in this case only has one).

```python
traj_collection = mpd.TrajectoryCollection(od_geo_sample, 'id', min_length=1000)     
print("Finished creating {} trajectories".format(len(traj_collection)))
```

    Finished creating 1946 trajectories

Next we use `TrajectoryCollectionAggregator`, which implements an algorithm developed by [Andrienko and Andrienko (2011)](http://geoanalytics.net/and/papers/tvcg11.pdf) to aggregate the trajectories into a weighted network. The crucial parameter here is setting `max_distance` to a reasonable maximum commute length for the region, as setting it too high will simply aggregate everything into a single giant O-D pair.

```python
aggregator = mpd.TrajectoryCollectionAggregator(traj_collection, max_distance=15000, min_distance=1000, min_stop_duration=dt.timedelta(minutes=30))

flows = aggregator.get_flows_gdf()
clusters = aggregator.get_clusters_gdf()
print("Finished creating {} flows".format(len(flows)) + " and {} clusters".format(len(clusters)))
```

    Finished creating 342 flows and 29 clusters

Here you can see what a single O-D pair looks like.

```python
hv.save(traj_collection.trajectories[64].hvplot(title="A single O-D trajectory",line_width=2, tiles='StamenTonerBackground', width=600, height=500),
"./charts/3_single_traj.html",backend='bokeh')
```

<div id="3-single-traj"></div>

The results of the graph are striking. Firstly we can see the sheer concentrated density of the commute flows to Center City, strongest from other neighborhoods in Philadelphia. But accompanying this is also several significant suburban notes, the majority of which correspond to major job centers like Chesterbrook and Horsham. While each suburb-suburb commute flow ranges from small to moderate, their combined sum as part of an everywhere-to-everywhere network, compared to Philadelphia's largely-radial network, is the reason why there are so many more suburb-suburb commutes in the region than any other type.

```python
flows = flows[flows['weight'] > 1].to_crs('EPSG:3857')
clusters = clusters.to_crs('EPSG:3857')

odnet = (flows.hvplot(title='Origin-destination network graph, Philadelphia and 5 PA counties',
            geo=True,
            crs='EPSG:3857',
            hover_cols=['weight'],
            line_width='weight',
            alpha=0.5, color='#1f77b3', tiles='StamenTonerRetina',
            frame_height=600, frame_width=800) *
 clusters.hvplot(geo=True, crs='EPSG:3857', color='red', size='n')
)

hv.save(odnet,"./charts/3_orig_dest.html",backend="bokeh")

```

<div id="3-orig-dest"></div>

Lastly here are two histograms showing the distribution of flows by weight and nodes by size; this gives a general picture of how much certain job centers still dominate the region despite the trend towards more distributed job sprawl.

```python
hv.Layout(flows.hvplot.hist(title='Distribution of flows by weight',y='weight') +
clusters.hvplot.hist(title='Distribution of nodes by size',y='n')).cols(1)
```

<div id="3-od-hist"></div>
