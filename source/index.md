---
title: API Reference

language_tabs:
  - ruby
  - python

toc_footers:
  - <a href='http://globalforestwatch.org'>Global Forest Watch</a>
  - <a href='http://www.wri.org/'>WRI</a>
  - <a href='https://github.com/wri/gfw-ruby'>Ruby Code</a>
  - <a href='https://github.com/wri/gfw-python'>Python Code</a>

search: true
---

# Introduction

**This documentation is currently under development**

Welcome the future home for the documentation for the GFW-API.


# Installation and Requirements 
```ruby
# future: currently not released
gem install 'gfw-ruby'
```
```python
# future: currently not released
pip install 'gfw-python'
```

Client libraries should be released in the standard way.

# API-Spec


# ForestChange

```json
{
  "value": 140256,
  "max_date": null, 
  "min_date": null,
  "apis": {
     "national": "http://staging.gfw-apis.appspot.com/forma-alerts/admin{/iso}{?period,download,bust,dev}", 
     "subnational": ..., 
     "use": ..., 
     "wdpa": ..., 
     "world": ...
  }, 
  "download_urls": {
    "csv": "http://wri-01.cartodb.com/api/v1/...&format=csv", 
    "geojson": "http://wri-01.cartodb.com/v1/...&format=geojson",
    "kml": "http://wri-01.cartodb.com/api/v1/...&format=kml", 
    "shp": "http://wri-01.cartodb.com/api/v1/...&format=svg"
  }, 
  "meta": {
    "coverage": "Humid tropical forest biome", 
    "description": "Forest disturbances alerts.", 
    "id": "forma-alerts", "name": "FORMA Alerts",
     "resolution": "500 x 500 meters", 
     "source": "MODIS", 
     "timescale": 
     "January 2006 to present", 
     "units": "Alerts", 
     "updates": "16 day"
  }, 
  "params": {
    "begin": "2010-01-01", 
    "end": "2013-01-01", 
    "iso": 
    "IDN", 
    "version": "v1"
  }
}
```

The following API calls require one of the following datasets \<DATA-SET\> as a url parameter.

* forma-alerts
* umd-loss-gain
* nasa-active-fires
* quicc-alerts
* imazon-alerts
* terrai-alerts

### Optional Query Parameters
Parameter | Description
--------- | -------- 
period | comma seperated string of the form \< begin-date \>,\< end-date \> . *both ruby and python library also accepts 'begin' and 'end' in place of period parameter
download | ...
dev | ...
bust | if true cache 

As and example, the JSON to our right is the output you receive from:

[http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/IDN?period=2010-01-01,2013-01-01](http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/IDN?period=2010-01-01,2013-01-01)

## Geometry

```ruby
GFW::API.forest_change.geometry('forma',geom,begin='2012-01-01',end='2012-06-15')
```

```python
import gfw.api as api
geom = {...} # python dict as geom - also accepts a json string
r = api.forest_change.geometry('forma',geom,begin='2012-01-01',end='2012-06-15')
r.status_code 
#-> 200
r.content
#-> '{"apis": {"national":..}}'
r.json()['value']
#-> 79491
r == api.forest_change.data.response
#-> True
api.forest_change.data.request
#->  {
      'datatype': 'iso', 
      'url': 'http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/bra', 
      'params': {'bust': True}, 
      'dataset': 'forma-alerts'
   }
api.forest_change.data.errors
#-> []
```


`GET http://staging.gfw-apis.appspot.com/forest-change/<DATA-SET>`

### Additional Required Query Parameters
Parameter | Description
--------- | -------- 
geojson | geojson for the region of interest. *python library accepts json string or dict. *ruby library accepts json string or hash 

Example [http://beta.gfw-apis.appspot.com/forest-change/forma-alerts?period=2010-01-01,2013-01-01&geojson={"coordinates":[[[98.7890625,2.8991526985043006],[102.83203125,-5.703447982149503],[107.57812499999999,-2.7235830833483856],[98.7890625,2.8991526985043006]]],"type":"Polygon"}](http://beta.gfw-apis.appspot.com/forest-change/forma-alerts?period=2010-01-01,2013-01-01&geojson={"coordinates":[[[98.7890625,2.8991526985043006],[102.83203125,-5.703447982149503],[107.57812499999999,-2.7235830833483856],[98.7890625,2.8991526985043006]]],"type":"Polygon"})

## National

```ruby
GFW::API.forest_change.national('forma','bra',bust=True)
```

```python
import gfw.api as api
r = api.forest_change.national('forma','bra',bust=True)
r.status_code 
#-> 200
r.content
#-> '{"apis": {"national":..}}'
r.json()['value']
#-> 79491
r == api.forest_change.data.response
#-> True
api.forest_change.data.request
#->  {
      'datatype': 'iso', 
      'url': 'http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/bra', 
      'params': {'bust': True}, 
      'dataset': 'forma-alerts'
   }
api.forest_change.data.errors
#-> []
```

`GET http://staging.gfw-apis.appspot.com/forest-change/<DATA-SET>/admin/<ISO>`

### Required Query Parameters
Parameter | Description
--------- | -------- 
iso | 3 character ISO country code

EXAMPLE:
[http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/IDN?period=2010-01-01,2013-01-01](http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/IDN?period=2010-01-01,2013-01-01)

## Subnational

```ruby
GFW::API.forest_change.subnational('forma','bra',1,bust=True)
```

```python
import gfw.api as api
r = api.forest_change.subnational('forma','bra',1,bust=True)
r.status_code 
#-> 200
r.content
#-> '{"apis": {"national":..}}'
r.json()['value']
#-> 79491
r == api.forest_change.data.response
#-> True
api.forest_change.data.request
#->  {
      'datatype': 'iso', 
      'url': 'http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/bra', 
      'params': {'bust': True}, 
      'dataset': 'forma-alerts'
   }
api.forest_change.data.errors
#-> []
```

`GET http://staging.gfw-apis.appspot.com/forest-change/<DATA-SET>/admin/<ISO>/<ID1>`

### Additional Required Query Parameters
Parameter | Description
--------- | -------- 
iso | 3 character ISO country code
id1 | Subnational region index

EXAMPLE:
[http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/IDN/1?period=2010-01-01,2013-01-01](http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/IDN/1?period=2010-01-01,2013-01-01)

## National (Intact Forest Landscapes)

...

## Subnational (Intact Forest Landscapes)

...        

## WPDA

```ruby
GFW::API.forest_change.wdpa('forma',30,bust=True)
```

```python
import gfw.api as api
r = api.forest_change.wdpa('forma',30,bust=True)
r.status_code 
#-> 200
r.content
#-> '{"apis": {"national":..}}'
r.json()['value']
#-> 337
r == api.forest_change.data.response
#-> True
api.forest_change.data.request
#->  {
       'datatype': 'wdpa',
       'url': 'http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/wdpa/30',
       'params': {},
       'dataset': 'forma-alerts'
      }
api.forest_change.data.errors
#-> []
```

`GET http://staging.gfw-apis.appspot.com/forest-change/<DATA-SET>/wdpa/<WDPA_ID>`

### Additional Required Query Parameters
Parameter | Description
--------- | -------- 
wdpa-id | ...

EXAMPLE:
[http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/wdpa/3](http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/wdpa/3)

## USE

```ruby
GFW::API.forest_change.use('forma','oilpalm','1930',bust=True)
```

```python
import gfw.api as api
r = api.forest_change.use('forma','oilpalm','1930',bust=True)
r.status_code 
#-> 200
r.content
#-> '{"apis": {"national":..}}'
r.json()['value']
#-> 13
r == api.forest_change.data.response
#-> True
api.forest_change.data.request
#->  {
      'datatype': 'use',
      'url': 'http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/use/oilpalm/1930',
      'concession': 'oilpalm',
      'params': {},
      'dataset': 'forma-alerts'
    }
api.forest_change.data.errors
#-> []
```

`GET http://staging.gfw-apis.appspot.com/forest-change/<DATA-SET>/use/<CONCESSION>/<USE_ID>`

### Additional Required Query Parameters
Parameter | Description
--------- | -------- 
concession | mining,oilpalm,fiber,logging
use-id | ...

EXAMPLE:
[http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/use/oilpalm/1930](http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/use/oilpalm/1930)

# Countries

*a quick description of kind of data the countries api returns*

## National

```ruby
GFW::API.countries.national('forma','bra',bust=True)
```

```python
import gfw.api as api
r = api.countries.national('forma','bra')
r.status_code 
#-> 200
r.content
#-> '{"bounds": {"coordinates": [[[-74.018474690454596,..}}'
r.json()['value']
#-> 79491
r == api.countries.data.response
#-> True
api.countries.data.request
#->   {
        'datatype': 'iso',
        'url': 'http://staging.gfw-apis.appspot.com/countries/bra',
        'params': {},
        'dataset': 'forma-alerts'
      }
api.countries.data.errors
#-> []
```

`GET http://staging.gfw-apis.appspot.com/countries/<ISO>`

### Required Query Parameters
Parameter | Description
--------- | -------- 
iso | 3 character ISO country code

EXAMPLE:
[http://staging.gfw-apis.appspot.com/countries/bra](http://staging.gfw-apis.appspot.com/countries/bra)

## Subnational

```ruby
GFW::API.countries.subnational('forma','bra',1,bust=True)
```

```python
import gfw.api as api
r = api.countries.subnational('forma','bra',1,bust=True)
r.status_code 
#-> 200
r.content
#-> '{"bounds": {"coordinates": [[[-74.018474690454596,..}}'
r.json()['value']
#-> 13
r == api.countries.data.response
#-> True
api.countries.data.request
#->  {
      'datatype': 'iso', 
      'url': 'http://staging.gfw-apis.appspot.com/countries/forma-alerts/admin/bra', 
      'params': {'bust': True}, 
      'dataset': 'forma-alerts'
   }
api.countries.data.errors
#-> []
```

`GET http://staging.gfw-apis.appspot.com/countries/<ISO>/<ID1>`

### Additional Required Query Parameters
Parameter | Description
--------- | -------- 
iso | 3 character ISO country code
id1 | Subnational region index

EXAMPLE:
[http://staging.gfw-apis.appspot.com/countries/bra/2](http://staging.gfw-apis.appspot.com/countries/bra/2)


# Subscriptions
## Subscribe
...
## Unsubscribe
...
## Confirm
...

# Stories
...
## create
...
## get
...
## list
...

