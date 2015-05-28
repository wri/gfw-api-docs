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

The following API calls require one of the following datasets \<DATA-SET\> as a url parameter.

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

### Optional Query Parameters
Parameter | Default | Description
--------- | ------- | -------- 
period | ... | time period to search for alerts
download | ... | ...
geojson | ... | *required for ...  
dev | ... | ...
bust | ... | cache or no

```ruby
gem install 'gfw-ruby'

GFW::API.forestChange.national(iso,options={})
```

```python
pip install 'gfw-python'

import gfw
gfw.API.forestChange.national(iso,options={})

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

