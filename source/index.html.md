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

# Schools API

## Search

```ruby
require 'nuonic'

api = Nuonic::APIClient.authorize!('my-api-key')
api.nuonic.get
```

```python
import requests

headers = {
    "x-api-key": "my-api-key",
    "Content-Type": "application/json",
}

url = 'https://api.nuonic.com.au/schools/v1/search?state=QLD&suburb=mermaid%20waters&name=merrimac%20high'

r = requests.get(url=url, headers=headers)

print(r.json())
```

```shell
curl "https://api.nuonic.com.au/schools/v1/search?state=QLD&suburb=mermaid%20waters&name=merrimac%20high"
  -H "Authorization: my-api-key" -H "Content-Type: application/json"
```

```javascript
const nuonic = require('nuonic');

let api = nuonic.authorize('my-api-key');
let nuonic = api.nuonic.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "government_id": 1042,
    "id_name": 2973,
    "name": "Merrimac State High School",
    "level_of_schooling": "State School",
    "year_ranges": "Year 7 - 12",
    "gender": "Coed",
    "sector": "Government",
    "teaching_staff": 71,
    "non_teaching_staff": 24,
    "total_enrolments": 1076,
    "phone_number": 6155747745,
    "fax_number": 6155747745,
    "email": "info@merrimacshs.gov.au",
    "url": "www.merrimacshs.gov.au",
    "electorate": "Moncriff",
    "federal_electorate": "federal_electorate",
    "address": "Dunlop Ct",
    "suburb": "Mermaid Waters",
    "state": "QLD",
    "postcode": 4218,
    "latitude": -28.039860999999998,
    "longitude": 153.417609999999996,
    "year_evaluated": 2016
  }
]
```

This endpoint returns a school profile based on use search terms State, Suburb and Name. If there are multiple results the API will return the top 5 (this may be changed in the future). If Name is specified the API will find the item with the closest name to the input using natural language processing to resolve the components (eg State, High, Saint) and identify the best match in our database.

Note that not all data attributes are available for all schools. Where an attribute is not available in our database it will not be returned in the results. 

### HTTP Request

`GET https://api.nuonic.com.au/schools/v1/search?state={state}&suburb={suburb}&name={name}`

<aside class="notice">Replace {state}, {suburb} and {name} with the State, Suburb and Name of the school you are looking for. Note that some of these parameters are optional</aside>

### Query Parameters

Parameter | Description | Optional
--------- | ----------- | --------
state   | Filter results for State. Must be the exact State name. | True
suburb  | Filter results for Suburb. Must be the exact Suburb name | True
name    | Filter by school Name. Can be approximate. The API will find the nearest match to your input text | True

### Response Fields

Field | Type | Description
--------- | ------- | -----------
government_id      | String  | The local government/private issued identication number
id_name            | Integer | Identifies the name of the identication provider
name               | String  | The schools name
level_of_schooling | String  | Highest level of schooling year
year_ranges        | String  | What year range a school has
gender             | String  | States the gender of each school
sector             | String  | Identifies the school sector as public or private
teaching_staff     | Integer | Total amount of teaching staff at each school
non_teaching_staff | Integer | Total amount of non-teaching staff at each school
total_enrolments   | Integer | Total amount of enrolled students at each school
phone_number       | String  | The schools phones number
fax_number         | String  | The schools fax number
email              | String  | The schools email address
url                | String  | The schools website
electorate         | String  | The local electorate 
federal_electorate | String  | The federal electorate 
address            | String  | The schools address
suburb             | String  | The schools suburb
state              | String  | The schools state
postcode           | Integer | The schools post-code
latitude           | Float   | Latitude position of the school
longitude          | Float   | Longitude position of the school
year_evaluated:    | DateTime| Government year evaluation of the schools
