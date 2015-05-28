---
title: API Reference

language_tabs:
  - ruby
  - python

toc_footers:
  - <a href='http://globalforestwatch.org'>Global Forest Watch</a>
  - <a href='http://www.wri.org/'>WRI</a>

includes:
  - errors

search: true
---

# Introduction

**This documentation is currently under development**

Welcome the future home for the documentation for the GFW-API. For now this is nothing more than a few notes to help us get started.

# Documentation Conventions

* endpoint
* required parameters it takes
* optional parameters
* method-name for client libraries
* description
* examples

# Conventions

* all under a module/namespace named API
* i am thinking each section should be namespaced:
  - GFW::API.forestChange.national(...)
  - GFW::API.forestChange.subnational(...)
  - GFW::API.forestChange.geometry(...)
  - ...
* camelCase for method names
* ...

# ForestChange

The following API calls require one of the following datasets as a url parameter (<DATA-SET>).

* forma-alerts
* umd-loss-gain
* nasa-active-fires
* quicc-alerts
* imazon-alerts
* terrai-alerts

Example http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/IDN?period=2010-01-01,2013-01-01

## All

if re.match(r'forest-change/%s$' % dataset, path):
    rtype = 'all'

## National

elif re.match(r'forest-change/%s/admin/[A-z]{3,3}$' % dataset, path):
    rtype = 'iso'

`GET http://http://staging.gfw-apis.appspot.com/forest-change/<DATA-SET>/admin/<ISO>`

### Query Parameters
        'all': ['period', 'download', 'geojson', 'dev', 'bust'],

Parameter | Default | Required | Description
--------- | ------- | -------- | -----------
period | ... | False | time period to search for alerts
download | ... | False | ...
geojson | ... | -- | ...
dev | ... | False | ...
bust | ... | False | ...

```ruby
gem install 'gfw-ruby'

GFW::API.forestChange.national(...)
```

```python
pip install 'gfw-python'

import gfw
gfw.API.forestChange.national(...)

```

<aside class="success">
EXAMPLE: http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/IDN?period=2010-01-01,2013-01-01
</aside>

> The above command returns JSON structured like this:

```json
{
  "value": 140256,
  "max_date": null, 
  "min_date": null,
  "apis": {
     "national": "http://staging.gfw-apis.appspot.com/forma-alerts/admin{/iso}{?period,download,bust,dev}", 
     "subnational": "http://staging.gfw-apis.appspot.com/forma-alerts/admin{/iso}{/id1}{?period,download,bust,dev}", 
     "use": "http://staging.gfw-apis.appspot.com/forma-alerts/use/{/name}{/id}{?period,download,bust,dev}", 
     "wdpa": "http://staging.gfw-apis.appspot.com/forma-alerts/wdpa/{/id}{?period,download,bust,dev}", 
     "world": "http://staging.gfw-apis.appspot.com/forma-alerts{?period,geojson,download,bust,dev}"
  }, 
  "download_urls": {
    "csv": "http://wri-01.cartodb.com/api/v1/sql?q=SELECT+f.%2A+FROM+forma_api+f+WHERE+f.date+%3E%3D+%272010-01-01%27%3A%3Adate+AND+f.date+%3C%3D+%272013-01-01%27%3A%3Adate+AND+f.iso+%3D+UPPER%28%27IDN%27%29&version=v1&format=csv", 
    "geojson": "http://wri-01.cartodb.com/api/v1/sql?q=SELECT+f.%2A+FROM+forma_api+f+WHERE+f.date+%3E%3D+%272010-01-01%27%3A%3Adate+AND+f.date+%3C%3D+%272013-01-01%27%3A%3Adate+AND+f.iso+%3D+UPPER%28%27IDN%27%29&version=v1&format=geojson",
    "kml": "http://wri-01.cartodb.com/api/v1/sql?q=SELECT+f.%2A+FROM+forma_api+f+WHERE+f.date+%3E%3D+%272010-01-01%27%3A%3Adate+AND+f.date+%3C%3D+%272013-01-01%27%3A%3Adate+AND+f.iso+%3D+UPPER%28%27IDN%27%29&version=v1&format=kml", 
    "shp": "http://wri-01.cartodb.com/api/v1/sql?q=SELECT+f.%2A+FROM+forma_api+f+WHERE+f.date+%3E%3D+%272010-01-01%27%3A%3Adate+AND+f.date+%3C%3D+%272013-01-01%27%3A%3Adate+AND+f.iso+%3D+UPPER%28%27IDN%27%29&version=v1&format=shp", "svg": "http://wri-01.cartodb.com/api/v1/sql?q=SELECT+f.%2A+FROM+forma_api+f+WHERE+f.date+%3E%3D+%272010-01-01%27%3A%3Adate+AND+f.date+%3C%3D+%272013-01-01%27%3A%3Adate+AND+f.iso+%3D+UPPER%28%27IDN%27%29&version=v1&format=svg"
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


## Sub-National

elif re.match(r'forest-change/%s/admin/[A-z]{3,3}/\d+$' % dataset, path):
    rtype = 'id1'

## National (Intact Forest Landscapes)

elif re.match(r'forest-change/%s/admin/ifl/[A-z]{3,3}$' % dataset, path):
    rtype = 'ifl'

## Sub-National (Intact Forest Landscapes)

elif re.match(r'forest-change/%s/admin/ifl/[A-z]{3,3}/\d$' % dataset, path):
    rtype = 'ifl_id1'        

## WPDA

elif re.match(r'forest-change/%s/wdpa/\d+$' % dataset, path):
    rtype = 'wdpa'

## USE

elif re.match(r'forest-change/%s/use/[A-z]+/\d+$' % dataset, path):
    rtype =  'use'

## LATEST UPDATES META

elif re.match(r'forest-change/%s/latest$' % dataset, path):
    rtype = 'latest'

