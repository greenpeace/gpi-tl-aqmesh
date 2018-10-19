# AQMesh

Two scripts to pipe airmonitor API's data into BigQuery. 

### File description
- `get_history.py`: requests all historic data (up to today) of the airmonitor API, reformats it and pipes it into BigQuery. If a new table/dataset needs to be created in the process (as specified in the file in the top section), the currently used table schema is read from `schema.py`. Logs are written to a file, per default `airmonitorHistory.log` and to stdout. 
- `query.py`: contains a class, `Query` that is used to organise and build a string that can be used to query BigQuery. (helper class)
- `scraper.py`: is in principal almost identical to `get_history.py`; this script should be run by e.g. a __cronjob__, to scrape the latest data off the API. It checks the timestamp of the latest entry in BigQuery for every available station and starts scraping from there. Has logging to `Stackdriver.Logging` enabled, so all logging messages are available in GCP. Also logs to stdout, but not to a file (can still be enabled if wanted though).
- `tools.py`: contains two functions that are needed for the visualisations to unclutter the code. The first one (`read_ts`) makes reading data from the BigQuery table easier, the second one (`bounded_graph`) helps to draw a bounded graph with `plotly`. Both are used in the visualisations, described below.

#### ---

```
visu 
|  BigQueryInlineQuery.ipynb
|  BigQueryPandasPlotly.ipynb
|  global_air_quality.ipynb
  
```
- `BigQueryInlineQuery.ipynb`: example of how to use jupyter magic commands to query BigQuery and use the data (here with `matplotlib`).
- `BigQueryPandasPlotly.ipynb`: example of how to use `pandas.io.gbq` to query BigQuery. Visualising time series using `pandas` and `plotly`. Also trying to forecast timeseries (temperature and carbon monoxide time series in this example) using the package `fbprophet`. 
- `global_air_quality.ipynb`: uses the historical open data of EPA to do the same as in `BigQueryPandasPlotly.ipynb`, but with a longer historical record to train the model from, resulting in  better forecasts. In this example, a site in St. Louis, Missouri, was used with a hourly temperature record going back to 2013. 