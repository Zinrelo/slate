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

![alt text](/images/zinrelo_settings_prod.png)

<aside class="notice">
If you don't see the API Key on your dashboard, please write to support@zinrelo.com
</aside>

* Send the keys in the HTTP header of each API request as given below :

`'api-key': '<your-api-key'`

`'partner-id' : '<your-partner-id>'`

# Zinrelo API

## Points

### Award

```shell
curl -X POST --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20" 
--data "user_email=bob@zinrelo.com" 
--data "points_passed=100" 
--data "activity_id=made_a_purchase"
--data "order_id=75a2726d13artibb10"

"https://api.zinrelo.com/v1/loyalty/award"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e', 
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = {"user_email": "bob@zinrelo.com",
           "points_passed": 100,
           "activity_id": "made_a_purchase",
           "order_id":"75a2726d13artibb10"}

response = requests.post(url = "https://api.zinrelo.com/v1/loyalty/award",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
  "data": {
          "user_email": "bob@zinrelo.com",
          "last_name": "Baker",
          "first_name": "Bob",
          "activity_id": "made_a_purchase",
          "activity_name": "Made a Purchase",
          "transaction_type":"award",
          "points": 100,
          "points_status": "auto_approved",
          "created_time": "30-Mar-16 19:20:22"
  },
  "success":true
}
```

Award points to a user for any activity. 
Points will be awarded based on the user's email address.

**HTTP Request**

`POST  https://api.zinrelo.com/v1/loyalty/award`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
user_email | string | Yes | The email of the user to whom points have to be awarded.
points_passed | integer | No (if activity type is fixed) Yes (if activity type is bonus multiplier) | If the activity is configured to award fixed number of points, the points passed will override the value set in the configuration. If the activity is configured to use a multiplier to award points, this becomes a mandatory parameter as the points cannot be awarded without the base value.
activity_id | string | Yes | The activity for which points needs to be awarded. The activity ID is available in the merchant center in the activity configuration.
order_id | string | No | When points are being awarded for purchase activity, it is recommended to pass order ID of the purchase transaction .This ensures that points for a particular transaction are awarded only once. 

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
user_email| string | The person’s email address
last_name| string | The person’s last name
first_name| string | The person’s first name
activity_id| string | The id of the activity for which the points were awarded
activity_name| string | The name of the activity for which the points were awarded
transaction_type| string | This will always be "award"
points| integer | The number of points awarded
points_status| string | The status of the award transaction
created_time| string | The time when the record was created

### Redeem

```shell
curl -X POST --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20" 
--data "user_email=bob@zinrelo.com" 
--data "redemption_id=reward_d65bc"
"https://api.zinrelo.com/v1/loyalty/redeem"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e',
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = {"user_email": "bob@zinrelo.com",
           "redemption_id": "reward_d65bc"}

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
          "redemption_id" : "reward_d65bc",
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

### Deduct

```shell
curl -X POST --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20" 
--data "user_email=bob@zinrelo.com" 
--data "reason=invalid transaction"
--data "points_passed=1000"
"https://api.zinrelo.com/v1/loyalty/users/deduct"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e', 
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = {"user_email": "bob@zinrelo.com",
           "reason": "invalid transaction",
           "points_passed": 1000}

response = requests.post(url = "https://api.zinrelo.com/v1/loyalty/users/deduct",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
  "data":{
          "first_name": "Bob",
          "last_name": "Baker",
          "points_status": "deducted",
          "transaction_type": "deduct",
          "available_points": 1500,
          "reason": "invalid transaction",
          "points": 1000,
          "created_time": "30-Mar-15 19:20:22",
          "user_email": "bob@zinrelo.com"
         },
  "success":true
}
```

Deduct points from a user’s available points. This deduction may not be related to redemption.

**HTTP Request**

`POST  https://api.zinrelo.com/v1/loyalty/users/deduct`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
user_email | string | Yes | The email of the user whose points are to be deducted.
reason | string | Yes | The reason for deduction.
points_passed | string | Yes | The number of points to be deducted.

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
first_name| string | The person’s first name
last_name| string | The person’s last name
points_status| string | This will always be "deducted"
transaction_type| string | This will always be "deduct"
available_points| integer | The points available to the user after deduction
reason| string | Reason for the deduction
points| integer | The number of points deducted
created_time| string | The time when the record was created
user_email| string | The person’s email address

## Transactions

### Get all transactions

```shell
curl -X GET --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20" 
--data "from_date=01/01/2016" 
--data "to_date =31/12/2017" 
--data "points_status=['approved, auto_approved']"
--data "start_index=7"
"https://api.zinrelo.com/v1/loyalty/transactions"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e', 
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = { "from_date": "01/01/2016",
            "to_date": "05/23/2017",	
            "points_status": '["approved","auto_approved"]',
            "start_index": 7
          }

response = requests.get(url = "https://api.zinrelo.com/v1/loyalty/transactions",
                        headers = headers, params = payload)
```

> The above command returns JSON structured like this:

```json
{
  "data":{
    "total": 250,
    "next_start_index":100,
    "transactions":[{
      "first_name": "bob",
      "last_name": "baker",
      "order_id": "12345ddfd67",
      "auto_approval_date": null,
      "points_status": "approved",
      "points_used": 0,
      "redemption_value": null,
      "points_expiration_date": "2017-07-09 12:42:05.484000",
      "redemption_name": null,
      "points": 300,
      "approved_by": null,
      "user_email": "enterprise",
      "created_time": "09-May-2017 12:21:53",
      "activity_name": "Purchase on website",
      "transaction_type": "award",
      "points_passed": 100,
      "approved_time": "2017-05-09 12:21:53.138000"
      }, 
    {...}],
    "more":true
  },
  "success":true
}
```

Fetch list of all transactions in the given period

**HTTP Request**

`GET  https://api.zinrelo.com/v1/loyalty/transactions`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
from_date | string | Yes | The start date (format is MM/DD/YYYY)
to_date | string | Yes | The end date (format is MM/DD/YYYY)
points_status | list of strings | No | The status of the transaction (pending/approved/auto_approved/<br>auto_rejected/rejected/<br>pending/exhausted/expired)
start_index | integer | No | The index from where data needs to be fetched (defaults to 0)

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
total | integer | total number of transactions
next_start_index | integer | starting index of next batch of transactions
transactions | list | list of transactions
activity_name | string | Name of the activity
first_name| string | The person’s first name
last_name| string | The person’s last name
user_email| string | The person’s email address
redemption_name | string | Name of the redemption   
approved_time | string | The time when transaction was approved
auto_approval_date | string | The time when transaction was auto approved 
points_status| string | Status of transaction (approved,auto_approved,exhausted,<br>pending,rejected,auto_rejected,expired)
transaction_type| string | Type of transaction (award,deduct,redeem)
points| integer | The number of points involved in transaction
approved_by | string | Name of admin who approved the transaction
created_time| string | The time when the transaction was created




### Get User Transactions

```shell
curl -X GET --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20"
--data "from_date= 01/01/2016",
--data "to_date= 12/31/2016",
--data "points_status= ['approved']",
--data "start_index= 0",
"https://api.zinrelo.com/v1/loyalty/users/bob@zinrelo.com/transactions"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e', 
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = {
	"from_date" : "01/01/2016",
	"to_date" : "12/31/2016",
	"points_status" : ['approved'],
	"start_index" : "0",
}

response = requests.get(url = "https://api.zinrelo.com/v1/loyalty/users/bob@zinrelo.com/transactions",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
    "data": {
  		"total": 1,
  		"next_start_index": 2,
  		"transactions": [{
				    "first_name": "bob",
				    "last_name": "baker",
				    "order_id": "12345ddfd67",
				    "auto_approval_date": null,
				    "points_status": "approved",
				    "points_used": 0,
				    "redemption_value": null,
				    "points_expiration_date": "2017-07-09 12:42:05.484000",
				    "redemption_name": null,
				    "points": 300,
				    "approved_by": null,
				    "user_email": "enterprise",
				    "created_time": "09-May-2017 12:21:53",
				    "activity_name": "Purchase on website",
				    "transaction_type": "award",
				    "points_passed": 100,
				    "approved_time": "2017-05-09 12:21:53.138000"
  		}],
  		"more": false
    },
    "success":true
}
```
This will return information about the transactions done (points earned / redeemed) by any user. User's email ID will be used to search and return user specific information.


**HTTP Request**

`GET  https://api.zinrelo.com/v1/loyalty/users/{user_email}/transactions`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
from_date | string | Yes | The start date
to_date | string | Yes | The end date
points_status | string | No | Status of transaction to fetch (pending/auto_approved/approved/<br>auto_rejected/rejected/expired/exhausted)
start_index | integer | No | The start index to fetch the data from (defaults to 0)

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
total | integer | Total number of transactions for the user 
next_start_index | integer | Next index of record to fetch 
transactions | array | Array of transactions based on time range, filters and start_index
more | boolean | Indicates if more number of transactions are available (true/false)

## Returns

```shell
curl -X POST --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20" 
--data "order_id=ORDER101" 
"https://api.zinrelo.com/v1/loyalty/transaction/return"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e',
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = {"order_id": 'ORDER101'}

response = requests.post(url = "https://api.zinrelo.com/v1/loyalty/transaction/return",
                        headers = headers, data = payload)
```

> The above command returns JSON structured like this:

```json
{
  "data":{
          "user_email": "bob@zinrelo.com",
          "last_name": "Baker",
          "first_name": "Bob",
          "points": 1000,
          "points_status": "pending_deduction",
          "returned_for_order_id": "ORDER101",
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
--data "first_name=bob"
--data "last_name=Baker"
--data "email=bob@zinrelo.com"
--data "uid=12345"
"https://api.zinrelo.com/v1/loyalty/users"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e',
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = {
	"first_name" : "Bob",
	"last_name" : "Baker",
	"email" : "bob@zinrelo.com",
	"uid" : "12345"
	}

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
--data "from_date=01/01/2016",
--data "to_date=12/31/2016",
--data "start_index=0",
--data "count=10"
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

### Get User Redemptions

```shell
curl -X GET --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20"
--data "is_still_valid=true",
--data "order_by=allowed_redeem_points",
--data "count=10",
--data "start_index=0",
"https://api.zinrelo.com/v1/loyalty/users/bob@zinrelo.com/redemptions"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e', 
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = json.dumps({
  "is_still_valid" : true,
  "order_by" : "allowed_redeem_points",
  "count" : "10",
  "start_index" : "0",
})

response = requests.get(url = "https://api.zinrelo.com/v1/loyalty/users/bob@zinrelo.com/redemptions",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
   "data": {
   "total": 2,
   "redemptions": [
       {
        "giftcard_description": "$25 Off Coupon for 2100 points",
        "redemption_name": "$25 OFF COUPON",
        "is_active": false,
        "associated_levels": [
                              "all"
                              ],
        "allowed_redeem_points": 2100,
        "updated_time": "18-May-2017 13:03:05",
        "paypal_cashback_enabled": false,
        "redemption_id": "reward_3063f",
        "created_time": "18-May-2017 13:03:05",
        "coupon_expiry": null,
        "redemption_value": "25",
        "coupon_status": {
                          "uploaded_coupons": 0,
                          "remaining_coupons": 0
                         }
       },

       {
        "giftcard_description": "$20 Off Coupon for 1800 points",
        "redemption_name": "$20 OFF COUPON",
        "is_active": false,
        "associated_levels": [
                              "all"
                             ],
        "allowed_redeem_points": 1800,
        "updated_time": "18-May-2017 13:03:05",
        "paypal_cashback_enabled": false,
        "redemption_id": "reward_eb549",
        "created_time": "18-May-2017 13:03:05",
        "coupon_expiry": null,
        "redemption_value": "20",
        "coupon_status": {
                          "uploaded_coupons": 0,
                          "remaining_coupons": 0
                         }
       },

       {...}
    ],

   "more": false
   },
 "success": true
}

```
This will return information about the redemption options available to the user. User's email ID will be used to search and return user specific information.


**HTTP Request**

`GET  https://api.zinrelo.com/v1/loyalty/users/{user_email}/redemptions`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
is_still_valid | boolean | No | Indicates whether only valid or all redemptions should be fetched (defaults to false)
order_by | string | No | The order in which the redemption options will be fetched (defaults to updated_time)
count | string | No | The number of redemptions to be fetched (defaults to 12)
start_index | integer | No | The start index to fetch the data from (defaults to 0)

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
total | integer | Total number of redemptions for the user 
redemptions | array | Array of redemptions based on time range, filters and start_index
more | boolean | Indicates if more number of redemptions are available (true/false)

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
    
