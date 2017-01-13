---
title: API Reference

language_tabs:
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='https://www.nuonic.com.au/contact/'>Access our APIs</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome Nuonic's API documentation. You can use our REST APIs to access Nuonic data and analytic services. Please visit our [website](https://www.nuonic.com.au) or [contact us](https://www.nuonic.com.au/contact) to learn more about our products and pricing.

You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```ruby
require 'nuonic'

api = Nuonic::APIClient.authorize!('my-api-key')
```

```python
import nuonic

api = nuonic.authorize('my-api-key')

```

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.nuonic.com.au/auth"
  -H "Authorization: my-api-key"
```

```javascript
const nuonic = require('nuonic');

let api = nuonic.authorize('my-api-key');
```

> Make sure to replace `my-api-key` with your API key.

Nuonic APIs use API keys provided to customers to allow access to the API. 

The API key must be included in all API requests to the server in a header that looks like this:

`x-api-key: my-api-key`

<aside class="notice">
Replace <code>my-api-key</code> with your Nuonic API key.
</aside>

# Food API

This API exposes analytic classification tools and data for food items driven by natural language processing and our libraries of domain-specific reference data. 

## Classifier

```python
import requests

headers = {
    "x-api-key": "my-api-key",
    "Content-Type": "application/json",
}

url = 'https://api.nuonic.com.au/food/v1/classifier?item=chobani%20YogUrt%20bluBerry%20170g%20and%20can%20of%20coca%20cola'

r = requests.get(url=url, headers=headers)

print(r.json())
```

```shell
curl "https://api.nuonic.com.au/food/v1/classifier?item=chobani%20YogUrt%20bluBerry%20170g%20and%20can%20of%20coca%20cola"
  -H "x-api-key: my-api-key" -H "Content-Type: application/json"
```

```javascript
```

> The above command returns JSON structured like this:

```json
[
  {
    "item":
    {
      "original": "chobani YogUrt bluBerry 170g and can of coca cola",
      "components": [
        {
          "text": "Chobani",
          "tag": "Brand",
          "position": 0
        },
        {
          "text": "Yogurt",
          "tag": "Text",
          "position": 1
        },
        {
          "text": "Blu berry",
          "tag": "Text",
          "position": 2
        },
        {
          "text": "170G",
          "tag": "Size",
          "extra_data": {
            "description": null,
            "grams": null,
            "pieces": null,
            "identified": true,
            "millilitres": null
          },
          "position": 3
        },
        {
          "text": "And",
          "tag": "Text",
          "position": 4
        },
        {
          "text" :"Can",
          "tag": "Size",
          "extra_data": {
            "description": "Can",
            "grams": null,
            "pieces": null,
            "identified": true,
            "millilitres": null
          },
          "position": 5
        },
        {
          "text": "Of",
          "tag": "Text",
          "position": 6
        },
        {
          "text": "Coca Cola",
          "tag": "Brand",
          "position": 7
        }
      ],
    "cleaned": "Chobani Yogurt Bluberry 170G And Can Of Coca Cola"
    }
  }
]
```

This endpoint analyses an input text string describing a food item or a comination of items to perform the following functions:
  - Identifying brand names
  - Identifying item names
  - Identifying and capturing size information
  - Correct spelling

The output is a list of classified components of the input text and a cleaned-up version of the text. This endpoint uses natural language processing methods and reference data matching to perform these tasks. It scans 1-3 gram sets.

This endpoint was originally design to classify manually entered food items (eg Menu's) to provide cleaned, identified attributes against each item. It could also be used to provide lookup functionality as part of a live system.

### HTTP Request

`GET https://api.nuonic.com.au/food/v1/classifier?item={description}`

<aside class="notice">Replace {description} with a url-encoded text string describing your food item(s)</aside>

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
item | None | A text string of the food item(s) description(s) to be analysed

### Response Fields

Field | Type | Description
--------- | ------- | -----------
original | String | The input text string
components | String | A list of components identified in the input text, listed in order of appearance, tagged with the type of component (eg "Brand") and the cleaned text. Some tags also append additional descrptive data, as an object, such as Size (see example output in the pane of the right).
cleaned | String | A cleaned-up version of the input string with spelling corrected, special characters removed and initial capital formatting

# Demographics API

This API exposes Australian population demographic data from the 2011 ABS Census that is processed and enhanced by Nuonic. The API provides access to the categories, attributes, location types and values for each geography level and unit for which data is available. 

## Categories

```python
import requests

headers = {
    "x-api-key": "my-api-key",
    "Content-Type": "application/json",
}

url = 'https://api.nuonic.com.au/demographics/v1/categories'

r = requests.get(url=url, headers=headers)

print(r.json())
```

```shell
curl "https://api.nuonic.com.au/demographics/v1/categories"
  -H "x-api-key: my-api-key" -H "Content-Type: application/json"
```

```javascript
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Selected Person Characteristics by Sex",
    "number_of_attributes": 23
  },
  {
    "id": 2,
    "name": "Selected Medians and Averages",
    "number_of_attributes": 14
  },
  {
    "id": 3,
    "name": "Place of Usual Residence on Census Night by Age",
    "number_of_attributes": 19
  }
]
```

This endpoint provides a list of categories of demographic attributes that are available through this API. Use the ID number of one of these categories to filter on the other API endpoints.

### HTTP Request

`GET https://api.nuonic.com.au/demographics/v1/categories`

### Query Parameters

None

### Response Fields

Field | Type | Description
--------- | ------- | -----------
id | Int | Our ID number for the category
name | String | The name of the category
number_of_attributes | Int | The number of demographic attributes available in this category

## Attributes

```python
import requests

headers = {
    "x-api-key": "my-api-key",
    "Content-Type": "application/json",
}

url = 'https://api.nuonic.com.au/demographics/v1/attributes'

r = requests.get(url=url, headers=headers)

print(r.json())
```

```shell
curl "https://api.nuonic.com.au/demographics/v1/attributes"
  -H "x-api-key: my-api-key" -H "Content-Type: application/json"
```

```javascript
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Total_Persons_Males"
  },
  {
    "id": 2,
    "name": "Total_Persons_Females"
  },
  {
    "id": 3,
    "name": "Total_Persons_Persons"
  }
]
```

This endpoint provides a list of demographic attributes for a specified category that are available through this API.

### HTTP Request

`GET https://api.nuonic.com.au/demographics/v1/attributes?category={id}`

<aside class="notice">Replace {id} with a category ID number</aside>

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
category_id | None | The ID number of a category listed by the categories endpoint

### Response Fields

Field | Type | Description
--------- | ------- | -----------
id | Int | Our ID number for the attribute
name | String | The name of the attribute
