---
title: API Reference

language_tabs:
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Nuonic account</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Kids Health API! You can use our API to access Kids Health API endpoints which provide the latest data on a range of consumption and health metrics for primary school age children across Australia.

We have language bindings in Shell, Ruby, C#, Java, Javascript and Python. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This API documentation page was created with [Slate](https://github.com/tripit/slate). Thanks to Robert Lord for creating this awesome tool.

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

Nuonic APIs use JWT API keys to allow access to the API. You can obtain a Nuonic API key by [signing up ](http://www.nuonic.com.au/signup) for a Nuonic account.

Nuonic expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: my-api-key`

<aside class="notice">
Replace <code>my-api-key</code> with your Nuonic API key.
</aside>

# Nutrient Profile API

## Summary by location

```ruby
require 'nuonic'

api = Nuonic::APIClient.authorize!('my-api-key')
api.nuonic.get
```

```python
import nuonic

api = nuonic.authorize('my-api-key')
api.nuonic.get()
```

```shell
curl "http://api.nuonic.com.au/kids_health/summary_by_location"
  -H "Authorization: my-api-key"
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
    "number_schools": 1103,
    "number_students": 23847,
    "number_orders": 25738,
    "number_items": 104753,
    "protein_g": 19.23,
    "carbohydrates_g": 26.53,
    "sugar_g": 18.43,
    "sodium_g": 4.54,
    "saturated_fat_g": 23.26,
    "unsaturated_fat_g": 16.34
  }
]
```

This endpoint returns a high-level nutrient consumption profile for a specified time period and location. The profile is an average across all students who transacted (purchased food items) in the period.

### HTTP Request

`GET http://api.nuonic.com.au/kids_health/summary_by_location`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
location_type | country| The type of location to aggregate to. One of ('country', 'school', 'postcode', 'region', 'state').
location_name | Australia | The location name as a string. For example one of ('The Southport School', '4215', 'South East Queensland', 'Queensland').
time_period | day | The time period to aggregate over. One of ('day', 'week', 'month', 'year').
end_date | yesterday's date | The end date of the time period in the form 'yyyy-mm-dd'.
number_periods | 1 | The number of time periods to return results for, up to a maximum of 12.

### Response Fields

Field | Type | Description
--------- | ------- | -----------
number_schools | Int | The number of schools within the location that data is sourced from.
number_students | Int | The number of students at the schools who transacted in the requested period.
number_orders | Int | The number of meal orders by the students at the schools who transacted in the requested period.
number_items | Int | The number of food items ordered by the students at the schools who transacted in the requested period.
protein_g | Float | The average amount of protein consumed by all students in the specified subset in g.
carbohydrates_g | Float | The average amount of carbohydrates consumed by all students in the specified subset in g.
sugar_g | Float | The average amount of sugar consumed by all students in the specified subset in g.
sodium_g | Float | The average amount of sodium (salt) consumed by all students in the specified subset in g.
saturated_fat_g | Float | The average amount of saturated fat consumed by all students in the specified subset in g.
unsaturated_fat_g | Float | The average amount of unsaturated fat consumed by all students in the specified subset in g.







# National School Register API

## Summary by location

```ruby
require 'nuonic'

api = Nuonic::APIClient.authorize!('my-api-key')
api.nuonic.get
```

```python
import nuonic

api = nuonic.authorize('my-api-key')
api.nuonic.get()
```

```shell
curl "http://api.nuonic.com.au/kids_health/summary_by_location"
  -H "Authorization: my-api-key"
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
    "government_id" : 1042,
    "id_name" : 2973,
    "name" : "Merrimac State High School",
    "level_of_schooling" : "State School",
    "year_ranges" : "Year 7 - 12",
    "gender" : "Coed",
    "sector" : "Government",
    "teaching_staff" : 71,
    "non_teaching_staff" : 24,
    "total_enrolments" : 1076,
    "phone_number" : 6155747745,
    "fax_number" : 6155747745,
    "email" : "info@merrimacshs.gov.au",
    "url" : "www.merrimacshs.gov.au",
    "electorate" : "Moncriff",
    "federal_electorate" : "federal_electorate",
    "address" : "Dunlop Ct",
    "suburb" : "Mermaid Waters",
    "state" : "QLD",
    "postcode" : 4218,
    "latitude" : -28.039860999999998,
    "longitude" : 153.417609999999996,
    "year_evaluated" : 2016
  }
]
```

This endpoint returns a school profile based on the user input, with the capability of finding all schools listed in Australia. 

### HTTP Request

`GET http://api.nuonic.com.au/kids_health/summary_by_location`

### Query Parameters

Parameter | Description
--------- | -----------
state   | Find all schools in a given Australian state.
suburb  | Find all schools in a given Australian suburb.
name    | Find a particular school based on a speciifc given name.

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
