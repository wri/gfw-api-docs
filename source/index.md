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

# Installation and Requirements 
```ruby
gem install 'gfw-ruby'
```
```python
pip install 'gfw-python'
```
Here we need to describe how to install the client libraries.


# Conventions

* ruby gem `gfw-ruby`
* python module `gfw-python`
* all under a module/namespace named API
* i am thinking each section should be namespaced:
  - API::ForestChange.national(...)
  - API::ForestChange.subnational(...)
  - API::ForestChange.geometry(...)
  - ...
* camelCase for method names

## Python Example

```python
import gfw.api as api
r = api.forestChange.national('forma','bra',bust=True)
r.status_code 
#-> 200
r.content
#-> '{"apis": {"national":..}}'
r.json()['value']
#-> 79491
r == api.forestChange.data.response
#-> True
api.forestChange.data.request
#->  {
      'datatype': 'iso', 
      'url': 'http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/bra', 
      'params': {'bust': True}, 
      'dataset': 'forma-alerts'
   }
api.forestChange.data.errors
#-> []
```

For reference I'm throwing in some details about the python library below.  As a reminder its super important that the libraries are as identical as possible.  If someone has been working in ruby and then wants to go to python there should only be minimal changes they need to make.

*note:  in python *\*\*params* as an argument allows me to do something like `api.forestChange.national('forma','bra',bust=True,period='2012-01-01,2013-01-01')`. I don't think you can do this in ruby so *\*\*params* should become *params={}*.  So you'd have something closer to `API::ForestChange.national('forma','bra',{bust: True,period: '2012-01-01,2013-01-01'})`


The gfw.api.forest_change module has the following methods:

* def geometry(dataset,geom,**params)
* def national(dataset,iso,**params)
* def subnational(dataset,iso,id1,**params)
* def nationalIFL(dataset,iso,**params)
* def subnationalIFL(dataset,iso,id1,**params)
* def wdpa(dataset,wdpa_id,**params)
* def use(dataset,concessions,use_id,**params)

In addition there is a `data` object on the forest_change module that contains information on the most recent request and the most recent response.  

See the python code tab for an example.

```python
#
# period/begin/end - 
#
def _clean_period(self,params):
    period = params.get('period')
    begin = params.get('begin')
    end = params.get('end')
    if (not period) and (begin or end):
      now = datetime.now()
      if not begin:
        begin = "%s-01-01" % now.year 
      if not end:
        end = now.strftime('%Y-%m-%d')  
      params['period'] = "{begin},{end}".format(begin=begin,end=end)
    if begin:
      del(params['begin'])
    if end:
      del(params['end'])
    return params
```
*note: the gfw-api takes a url parameter 'period' which is of the form 'begin-date,end-date'.  I've set up the library to accept a period or a begin or end value. period takes precedence over begin/end. if begin/end is not given it defaults to the beginning-of-the-year/current-date. see _clean_period in the python tab to clarify this logic.*

# ForestChange
```python
formaData = self._moduleData(s,{
        'name': 'forma',
        'url_id': 'forma',
        'email_name': 'FORMA',
        'summary':'Detects areas where tree cover loss is likely to have recently occurred',
        'specs':'monthly, 500m, humid tropics, WRI/CGD'
    })
terraiData = self._moduleData(s,{
        'name': 'terrai',
        'url_id': 'terrailoss',
        'email_name': 'Terra-i',
        'summary':'Detects areas in Latin American where tree cover loss is likely to have recently occurred',
        'specs':'monthly, 250m, Latin America, CIAT',
    })
imazonData = self._moduleData(s,{
        'name': 'imazon',
        'url_id': 'imazon',
        'email_name': 'SAD',
        'summary':'Detects forest cover loss and forest degradation in the Brazilian Amazon',
        'specs':'monthly, 250m, Brazilian Amazon, Imazon',
        'value_names': {
            'degrad': 'hectares degradation',
            'defor': 'hectares deforestation'
        }
    })     
quiccData = self._moduleData(s,{
        'name': 'quicc',
        'url_id': 'modis',
        'email_name': 'QUICC',
        'summary':'Identifies areas of land that have lost at least 40% of their green vegetation cover from the previous quarterly product',
        'specs':'quarterly, 5km, <37 degrees north, NASA'
    },True,force_min_date,force_max_date)
storiesData = self._storiesData(s,{
        'name': 'stories',
        'url_id': 'none/580',
        'summary':'Forest-related stories reported by GFW users',
        'specs':'',
        'email_name': 'Stories'
    })
```

The following API calls require one of the following datasets \<DATA-SET\> as a url parameter.

* forma-alerts
* umd-loss-gain
* nasa-active-fires
* quicc-alerts
* imazon-alerts
* terrai-alerts

We need to descibe these datasets somehow. The Python tab to the right temporarlly has some code pasted in containing short a short summary and specs to build off.  

Example http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/IDN?period=2010-01-01,2013-01-01

## All

if re.match(r'forest-change/%s$' % dataset, path):
    rtype = 'all'

* def geometry(dataset,geom,**params)

## National

```ruby
gem install 'gfw-ruby'

GFW::API.forestChange.national(iso,options={})
```

```python
pip install 'gfw-python'

import gfw.api as api
r = api.forestChange.national('forma','bra',bust=True)
r.status_code 
#-> 200
r.content
#-> '{"apis": {"national":..}}'
r.json()['value']
#-> 79491
r == api.forestChange.data.response
#-> True
api.forestChange.data.request
#->  {
      'datatype': 'iso', 
      'url': 'http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/bra', 
      'params': {'bust': True}, 
      'dataset': 'forma-alerts'
   }
api.forestChange.data.errors
#-> []
```

* def national(dataset,iso,**params)

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

<aside class="success">
EXAMPLE: http://staging.gfw-apis.appspot.com/forest-change/forma-alerts/admin/IDN?period=2010-01-01,2013-01-01
</aside>

## Sub-National

* def subnational(dataset,iso,id1,**params)

elif re.match(r'forest-change/%s/admin/[A-z]{3,3}/\d+$' % dataset, path):
    rtype = 'id1'

## National (Intact Forest Landscapes)


* def nationalIFL(dataset,iso,**params)

elif re.match(r'forest-change/%s/admin/ifl/[A-z]{3,3}$' % dataset, path):
    rtype = 'ifl'

## Sub-National (Intact Forest Landscapes)


* def subnationalIFL(dataset,iso,id1,**params)

elif re.match(r'forest-change/%s/admin/ifl/[A-z]{3,3}/\d$' % dataset, path):
    rtype = 'ifl_id1'        

## WPDA


* def wdpa(dataset,wdpa_id,**params)

elif re.match(r'forest-change/%s/wdpa/\d+$' % dataset, path):
    rtype = 'wdpa'

## USE


* def use(dataset,concessions,use_id,**params)

elif re.match(r'forest-change/%s/use/[A-z]+/\d+$' % dataset, path):
    rtype =  'use'

## LATEST UPDATES META

elif re.match(r'forest-change/%s/latest$' % dataset, path):
    rtype = 'latest'

# Countries
...

# Stories
...
