---
title: Zinrelo - API Reference
tags: [zinrelo, loyalty, award, redeem, deduct, user]
language_tabs:
  - shell: Shell
  - python: Python

toc_footers:
  - <a href='https://zinrelo.com' target="_blank">Zinrelo Home Page</a>

search: true
---

# Overview

Welcome to the Zinrelo API documentation. You can use this API to access all our API endpoints, such as the **Loyalty API**, to build a common rewards program for all your sales channels.

We have language bindings in Shell and Python! You can view code examples in the area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:


```python
import requests

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

response = requests.get(url = "https://api.zinrelo.com/v1/loyalty/transactions",
                        headers = headers)
```

```shell
curl -H "partner-id: <your-partner-id>" 
     -H "api-key: <your-api-key>" 
     "https://api.zinrelo.com/v1/loyalty/transactions"
```


The Zinrelo API exports sensitive data, so any call to the Zinrelo API needs to be authenticated. 

For authenticating an API call, you have to send your **Partner ID**, and an **API Key** in the HTTP header of each API request.

To access **Partner ID**, and the **API Key**, please follow the steps given below:

* Login to your Merchant Center Console.
* Navigate to **General > Settings**, to find your **Partner ID**, and **API Key**. 

![alt text](/images/zinrelo_settings.png)

<aside class="notice">
If you don't see the API Key on your dashboard, please write to support@zinrelo.com
</aside>

* Send the keys in the HTTP header of each API request as given below :

`'api-key': '<your-api-key'`

`'partner-id' : '<your-partner-id>'`

# Zinrelo API

## Points

### Redeem

```shell
curl -X POST --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
--data "user_email= <User’s email ID>" 
--data "redemption_id= <redemption ID>"
"https://api.zinrelo.com/v1/loyalty/redeem"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e',
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = json.dumps({"user_email": "bob@zinrelo.com",
                      "redemption_id": "$50_gift_card"})

response = requests.post(url = "https://api.zinrelo.com/v1/loyalty/redeem",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
  "data":{
          "user_email": "bob@zinrelo.com",
          "last_name": "Baker",
          "first_name": "Bob",
          "redemption_name": "$50 Gift Card",
          "points": 1000,
          "points_status": "redeemed",
          "coupon_code": "GET50OFF",
          "created_time": "03-May-17 19:20:22"
  },
  "success":true
}
```

Redeem points for a user. 
Based on the redemption option chosen, points will be deducted from the user's account and a coupon code will be returned in the response.

**HTTP Request**

`POST  https://api.zinrelo.com/v1/loyalty/redeem`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
user_email | string | Yes | The email of the user who wishes to redeem the points.
redemption_id | string | Yes | The ID of the desired redemption option. The redemption ID is available in the merchant center.

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
user_email| string | The person’s email address
last_name| string | The person’s last name
first_name| string | The person’s first name
redemption_id| string | The id of the redemption option chosen
redemption_name| string | The name of the redemption chosen option
points| integer | The number of points redeemed
points_status| string | This will always be "redeemed"
coupon_code| string | The coupon code to be used for successive transactions
created_time| string | The time when the record was created


## Returns

```shell
curl -X POST --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
--data "order_id: <order id for which the return needs to be processed>" 
--data "returned_amount: <returned_amount>" 
"https://api.zinrelo.com/v1/loyalty/transaction/return"
```

```python
import requests
import json

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

payload = json.dumps({"order_id": <order id for which the return needs to be processed>,
                      "returned_amount": <returned_amount>})

response = requests.post(url = "https://api.zinrelo.com/v1/loyalty/transaction/return",
                        headers = headers, data = payload)
```

> The above command returns JSON structured like this:

```json
{
  "data":{
          "user_email": "bob@zinrelo.com",
          "user_last_name": "Baker",
          "user_first_name": "Bob",
          "points": 1000,
          "points_status": "pending_deduction",
          "returned_for_order_id": "1254aeb7b4f",
          "reason": "Order Fully Returned",
          "created_time": "30-Mar-16 19:20:22"
	},
  "success":true
}

```
Deduct points of a user when the user returns an order.

**HTTP Request**

`POST  https://api.zinrelo.com/v1/loyalty/transaction/return`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
order_id | string | Yes | The order ID for which the return needs to be processed.
returned_amount | integer | No | If returned_amount is specified and is a non-zero positive number, it will be processed as a partial return. The amount will be subtracted from the actual value of the order and remaining points will be awarded.</br>If returned_amount is not passed, it will be considered as complete order return.


**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
user_email| string | The person’s email address
last_name| string | The person’s last name
first_name| string | The person’s first name
points| integer | The number of points awarded
points_status| string | The status of the award transaction
returned_for_order_id| string | The order ID for which the return has been processed
reason | string | The reason for returning the order
created_time| string | The time when the record was created

## Users

### Create User

```shell
curl -X POST --header "partner-id: cad458dc4e"
--header "api-key: c921e097e6679d21c0cad26a45bfec20"
--data "first_name= bob"
--data "last_name= Baker"
--data "email= bob@zinrelo.com"
--data "uid= 12345"
"https://api.zinrelo.com/v1/loyalty/users"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e',
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = json.dumps({
	"first_name" : "Bob",
	"last_name" : "Baker",
	"email" : "bob@zinrelo.com",
	"uid" : "12345"
})

response = requests.post(url = "https://api.zinrelo.com/v1/loyalty/users",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
  "data": {
    "email":"bob@zinrelo.com",
    "first_name":"Bob",
    "last_name":"Baker"
  },
  "success":true
}
```
Create a new user for merchant.


**HTTP Request**

`POST  https://api.zinrelo.com/v1/loyalty/users`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
first_name | string | Yes | First name of the user
last_name | string | Yes | Last name of the user
email | string | Yes | Email of the user
uid | string | Yes | User ID of the user

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
email | string | Email of the user
first_name | string | First name of the user
last_name | string | Last name of the user

### Get All Users

```shell
curl -X GET --header "partner-id: cad458dc4e"
--header "api-key: c921e097e6679d21c0cad26a45bfec20"
--data "from_date= 01/01/2016",
--data "to_date= 12/31/2016",
--data "start_index= 0",
--data "count= 10"
"https://api.zinrelo.com/v1/loyalty/users"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e',
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = json.dumps({
	"from_date" : "01/01/2016",
	"to_date" : "12/31/2016",
	"start_index" : "0",
	"count" : "10"
})

response = requests.get(url = "https://api.zinrelo.com/v1/loyalty/users",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
  "data": {
    "total_loyalty_users":1,
    "loyalty_users": [{
       "loyalty_tier_name": "",
       "loyalty_tier_id": "",
       "expiration_schedule": [],
       "last_name": "Baker",
       "redeemed_points": 0,
       "first_name": "Bob",
       "available_points": 0,
       "pending_points": 0,
       "awarded_points": 0,
       "user_email": "bob@zinrelo.com",
       "dob" : ""
    }],
    "more":false
  },
  "success":true
}
```
This will get all the users enrolled in the loyalty program.


**HTTP Request**

`GET  https://api.zinrelo.com/v1/loyalty/users`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
from_date | string | No | The start date
to_date | string | No | The end date
filter_by | string | No | Filters to be applied along with from_date and to_date. Accepted values: 'enrollment_date' , 'last_activity_date
start_index | integer | No | The start index to fetch the data from (defaults to 0)
count | integer | No | Number of records to fetch (defaults to 100)

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
next_start_index | integer | Next index of record to fetch 
total_loyalty_users | integer | Total number of loyalty users for this merchant
loyalty_users | array | Array of users based on time range, filters and start_index
more | boolean | Indicates if more number of users are available (true/false)

### Get User

```shell
curl -X GET --header "partner-id: cad458dc4e"
--header "api-key: c921e097e6679d21c0cad26a45bfec20"
"https://api.zinrelo.com/v1/loyalty/users/bob@zinrelo.com"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e',
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}


response = requests.get(url = "https://api.zinrelo.com/v1/loyalty/users/bob@zinrelo.com",
                        headers = headers)
```


> The above command returns JSON structured like this:

```json
{
  "data": {
   "loyalty_tier_name": "",
   "loyalty_tier_id": "",
   "expiration_schedule": [],
   "last_name": "Baker",
   "redeemed_points": 0,
   "first_name": "Bob",
   "available_points": 0,
   "pending_points": 0,
   "awarded_points": 0,
   "user_email": "bob@zinrelo.com",
   "dob" : ""
  },
  "success":true
}
```
This will get the details of the user along with available and redeemed points.


**HTTP Request**

`GET  https://api.zinrelo.com/v1/loyalty/users/{user_email}`

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
user_email | string | Email of the user
first_name | string | First name of the user
last_name | string | Last name of the user
dob | string | Date of birth of user, if available
redeemed_points | integer | Points redeemed by the user
available_points | integer | Points currently available for the user
awarded_points | integer | Points currently available for the user
pending_points | integer | Points pending for approval
loyalty_tier_id | string | ID of loyalty level currently assigned to user
loyalty_tier_name | string | ID of loyalty level currently assigned to user
expiration_schedule | array | Array of expiration schedules

# Status Codes

Our API returns standard HTTP success or failure status codes. For failures, we will also include extra information about what went wrong encoded in the response as JSON. The various HTTP and API status codes we might return are listed below.

## HTTP Status codes


Code | Title | Description
-----| ------- | --------
200 | OK | The request was successful.
400 | Bad Request | Bad request.
401 | Unauthorized | Your API key is invalid.
404 | Not Found | The resource does not exist.
50X | Internal Server Error | An error occured with our API.

## API Status Codes

Code | Description
-----| --------
601 | One or more request parameters do not have values in the expected format
602 | User does not exist
603 | Redeemption operation failed
604 | Invalid Redemption ID
605 | Inactive Redemption ID
606 | Redemption coupon expired
607 | Insufficient redeemable points
608 | Redemption coupons exhausted
609 | Duplicate Order ID
610 | Activity is in paused state
611 | Invalid Activity ID
612 | end_date cannot be less than start_date
613 | Invalid Order ID
614 | Points to deduct is greater than points available to deduct for the Order ID
615 | missing parameters
    
