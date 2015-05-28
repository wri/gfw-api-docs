---
title: API Reference

language_tabs:
  - shell
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

Welcome the future home for the documentation for the GFW-API.  Much of what follows is example documentation from [slate](https://github.com/tripit/slate) and will be removed shortly.  **This documentation is currently under development** and for now is nothing more than a few notes to help us get started.

# Endpoints

Example: /forest-change/forma-alerts/admin/iso => (forma-alerts, iso)

As a starting point, under the python tab on the right is the python code for the forest change endpoints.

```python
dataset = _dataset_from_path(path)
path = path.strip("/")
rtype = None

if re.match(r'forest-change/%s$' % dataset, path):
    rtype = 'all'
elif re.match(r'forest-change/%s/latest$' % dataset, path):
    rtype = 'latest'
elif re.match(r'forest-change/%s/admin/ifl/[A-z]{3,3}$' % dataset, path):
    rtype = 'ifl'
elif re.match(r'forest-change/%s/admin/ifl/[A-z]{3,3}/\d$' % dataset, path):
    rtype = 'ifl_id1'        
elif re.match(r'forest-change/%s/admin/[A-z]{3,3}$' % dataset, path):
    rtype = 'iso'
elif re.match(r'forest-change/%s/admin/[A-z]{3,3}/\d+$' % dataset, path):
    rtype = 'id1'
elif re.match(r'forest-change/%s/wdpa/\d+$' % dataset, path):
    rtype = 'wdpa'
elif re.match(r'forest-change/%s/use/[A-z]+/\d+$' % dataset, path):
    rtype = 'use'
```

# Documentation Conventions

* endpoint
* required parameters it takes
* optional parameters
* method-name for client libraries
* description
* examples

# Conventions

We still need to flush this out naming convetions to should be as close to the same for all as possible for all languages. As a first go I'd say:

* all under a module/namespace named API
* camel_case for method names
* ...

```ruby
gem install 'gfw-ruby'

GFW::API.forest_change(...)
```

```python
pip install 'gfw-python'

import gfw
gfw.API.forest_change(...)

```


# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Isis",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/3"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the cat to retrieve

