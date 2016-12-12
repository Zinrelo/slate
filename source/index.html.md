---
title: ShopSocially - API Reference
tags: [shopsocially, loyalty, award, redeem, deduct, user]
language_tabs:
  - shell: Shell
  - python: Python

toc_footers:
  - <a href='https://shopsocially.com' target="_blank">ShopSocially Home Page</a>

includes:
  - errors

search: true
---

# Overview

Welcome to the ShopSocially API documentation. You can use this API to access all our API endpoints, such as the **Loyalty API**, to build a common rewards program for all your sales channels.

We have language bindings in Shell and Python! You can view code examples in the area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:


```python
import requests

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

response = requests.get(url = "https://api.shopsocially.com/v2/loyalty/transactions",
                        headers = headers)
```

```shell
curl -H "partner-id: <your-partner-id>" 
     -H "api-key: <your-api-key>" 
     "https://api.shopsocially.com/v2/loyalty/transactions"
```


The ShopSocially API exports sensitive data, so any call to the ShopSocially API needs to be authenticated. 

For authenticating an API call, you have to send your **Partner ID**, and an **API Key** in the HTTP header of each API request.

To access **Partner ID**, and the **API Key**, please follow the steps given below:

* Login to your Merchant Center Console.
* Navigate to **Basic Settings > General Information**, to find your **Partner ID**, and **API Key**. 

![alt text](/images/settings.png)

<aside class="notice">
If you don't see the API Key on your dashboard, please write to support@shopsocially.com
</aside>

* Send the keys in the HTTP header of each API request as given below :

`'api-key': '<your-api-key'`

`'partner-id' : '<your-partner-id>'`

# Loyalty API

The Loyalty API enables you to extend the loyalty program beyond your website.

## Points

The Points API lets you award, redeem and deduct points from a user's account.

### Award

```shell
curl -X POST --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
--data "user_email= <User’s email ID>" 
--data "points_passed = <number of points to be awarded>" 
--data "activity_id= <activity ID>"
"https://api.shopsocially.com/v2/loyalty/award"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

payload = json.dumps({"user_email": "bob@shopsocially.com",
                      "points_passed": 100,
                      "activity_id": "made_a_purchase"})

response = requests.post(url = "https://api.shopsocially.com/v2/loyalty/award",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
  "data": {
          "user_id": "6176be2d5e",
          "user_email": "bob@shopsocially.com",
          "user_last_name": "Baker",
          "user_first_name": "Bob",
          "activity_id": "made_a_purchase",
          "activity_name": "Made a Purchase",
          "id": "54aeb7b4f283c6176be2d5ed",
          "transaction_type":"award",
          "points": 100,
          "points_status": "auto_approved",
          "partner_id": "69c62d683db96c7cabf8db0109be6bb1",
          "created_time": "30-Mar-16 19:20:22"
        },
  "success":true
}
```

Award points to a user for any activity. 
Points will be awarded based on the user's email address.

**HTTP Request**

`POST  https://api.shopsocially.com/v2/loyalty/award`

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
user_id   | string | Unique id generated for each user
user_email| string | The person’s email address
user_last_name| string | The person’s last name
user_first_name| string | The person’s first name
activity_id| string | The id of the activity for which the points were awarded
activity_name| string | The name of the activity for which the points were awarded
id| string | Internal id
transaction_type| string | This will always be "award"
points| integer | The number of points awarded
points_status| string | The status of the award transaction
partner_id| string | The id of the merchant
created_time| string | The time when the record was created

### Redeem

```shell
curl -X POST --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
--data "user_email= <User’s email ID>" 
--data "redemption_id= <redemption ID>"
"https://api.shopsocially.com/v2/loyalty/redeem"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

payload = json.dumps({"user_email": "bob@shopsocially.com",
                      "redemption_id": "$50_gift_card"})

response = requests.post(url = "https://api.shopsocially.com/v2/loyalty/redeem",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
  "data":{
          "user_id": "6176be2d5e",
          "user_email": "bob@shopsocially.com",
          "user_last_name": "Baker",
          "user_first_name": "Bob",
          "redemption_id": "$50_gift_card",
          "redemption_name": "$50 Gift Card",
          "redemption_value": "$50",
          "id": "54aeb7b4f283c6176be2d5ed",
          "transaction_type": "redeem",
          "points": 1000,
          "points_status": "redeemed",
          "coupon_code": "GET50OFF",
          "partner_id": "9c62d683db96c7cabf8db0109be6bb",
          "created_time": "30-Mar-15 19:20:22"
  },
  "success":true
}
```

Redeem points for a user. 
Based on the redemption option chosen, points will be deducted from the user's account and a coupon code will be returned in the response.

**HTTP Request**

`POST  https://api.shopsocially.com/v2/loyalty/redeem`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
user_email | string | Yes | The email of the user who wishes to redeem the points.
redemption_id | string | Yes | The ID of the desired redemption option. The redemption ID is available in the merchant center.

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
user_id   | string | Unique id generated for each user
user_email| string | The person’s email address
user_last_name| string | The person’s last name
user_first_name| string | The person’s first name
redemption_id| string | The id of the redemption option chosen
redemption_name| string | The name of the redemption chosen option
redemption_value | string | The amount of the redemption option chosen (in dollars)
id| string | Internal id
transaction_type| string | This will always be "redeem"
points| integer | The number of points redeemed
points_status| string | This will always be "redeemed"
coupon_code| string | The coupon code to be used for successive transactions
partner_id| string | The id of the merchant
created_time| string | The time when the record was created


### Deduct

```shell
curl -X POST --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
--data "user_email= <User’s email ID>" 
--data "reason= <reason>"
--data "points_passed= <points_passed>"
"https://api.shopsocially.com/v2/loyalty/deduct"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

payload = json.dumps({"user_email": "bob@shopsocially.com",
                      "reason": "invalid transaction",
                      "points_passed": 1000})

response = requests.post(url = "https://api.shopsocially.com/v2/loyalty/deduct",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
  "data":{
          "user_id": "6176be2d5e",
          "user_email": "bob@shopsocially.com",
          "user_last_name": "Baker",
          "user_first_name": "Bob",
          "id": "54aeb7b4f283c6176be2d5ed",
          "transaction_type": "deduct",
          "points": 1000,
          "points_status": "deducted",
          "available_points": 1500,
          "partner_id": "9c62d683db96c7cabf8db0109be6bb",
          "created_time": "30-Mar-15 19:20:22"
  },
  "success":true
}
```

Deduct points from a user’s available points. This deduction may not be related to redemption.

**HTTP Request**

`POST  https://api.shopsocially.com/v2/loyalty/deduct`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
user_email | string | Yes | The email of the user whose points are to be deducted.
reason | string | Yes | The reason for deduction.
points_passed | string | Yes | The number of points to be deducted.

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
user_id   | string | Unique id generated for each user
user_email| string | The person’s email address
user_last_name| string | The person’s last name
user_first_name| string | The person’s first name
id| string | Internal id
transaction_type| string | This will always be "deduct"
points| integer | The number of points deducted
points_status| string | This will always be "deducted"
available_points| integer | The points available to the user after deduction
partner_id| string | The id of the merchant
created_time| string | The time when the record was created

## Transactions

The Transactions API lets you fetch all or a single loyalty transaction.

### Get all transactions

```shell
curl -X GET --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
--data "from_date= <The start date>" 
--data "to_date = <The end date>" 
--data "points_status= <transaction status>"
--data "start_index= <index from where data needs to be fetched>"
"https://api.shopsocially.com/v2/loyalty/transactions"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

payload = json.dumps({"from_date": "01/01/2015",
				      "to_date": "11/23/2015",	
                      "points_status": '["approved","auto_approved"]',
		              "start_index": 7})

response = requests.get(url = "https://api.shopsocially.com/v2/loyalty/transactions",
                        headers = headers, params = payload)
```

> The above command returns JSON structured like this:

```json
{
  "data":{
          "total": 250,
          "next_start_index":100,
          "transactions":[
            {
            "activity_id":"Made_a_Purchase",
            "activity_name":"Made a Purchase",
            "user_id":"6176be2d5e",
            "user_first_name":"Bob",
            "user_last_name":"Baker",
            "user_email":"bob@shopsocially.com",
            "redemption_id":null,
            "redemption_name":null,
            "id":"550ff099fa0eaa0767086968",
            "approved_time":"20-Mar-15 19:20:22",
            "auto_approval_date":null,
            "points_status":"approved",
            "transaction_type":"award",
            "coupon_code":null,
            "points": 100,
            "approved_by":"Jai",
            "created_time":"20-Mar-15 19:20:22",
            "updated_time":"30-Mar-15 19:20:22",
            "merchant_id":"9c62d683db96c7cabf8db0109be6bb"
            }, 
	        {...}
          ],
          "more":true
         },
  "success":true
}
```

Fetch list of all transactions in the given period

**HTTP Request**

`GET  https://api.shopsocially.com/v2/loyalty/transactions`

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
activity_id | string | Unique id for an activity
activity_name | string | Name of the activity
user_id   | string | Unique id generated for each user
user_first_name| string | The person’s first name
user_last_name| string | The person’s last name
user_email| string | The person’s email address
redemption_id | string | Unique id for an redemption
redemption_name | string | Name of the redemption   
id| string | Internal id
approved_time | string | The time when transaction was approved
auto_approval_date | string | The time when transaction was auto approved 
points_status| string | Status of transaction (approved,auto_approved,exhausted,<br>pending,rejected,auto_rejected,expired)
transaction_type| string | Type of transaction (award,deduct,redeem)
coupon_code | string | Coupon code used in the transaction
points| integer | The number of points involved in transaction
approved_by | string | Name of admin who approved the transaction
created_time| string | The time when the transaction was created
updated_time| string | The time when the transaction was updated
merchant_id| string | The id of the merchant


### Get Transaction
```shell
curl -X GET --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
"https://api.shopsocially.com/v2/loyalty/transactions/{transaction_id}"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

response = requests.get(url = "https://api.shopsocially.com/v2/loyalty/transactions/550ff099fa0eaa0767086968",
                        headers = headers)
```


> The above command returns JSON structured like this:

```json
{
"data":{
        "activity_id":"Made_a_Purchase",
        "activity_name":"Made a Purchase",
        "user_id":"6176be2d5e",
        "user_first_name":"Bob",
        "user_last_name":"Baker",
        "user_email":"bob@shopsocially.com",
        "redemption_id":null,
        "redemption_name":null,
        "id":"550ff099fa0eaa0767086968",
        "approved_time":"20-Mar-15 19:20:22",
        "auto_approval_date":null,
        "points_status":"approved",
        "transaction_type":"award",
        "coupon_code":null,
        "points": 100,
        "approved_by":"Jai",
        "created_time":"20-Mar-15 19:20:22",
        "updated_time":"30-Mar-15 19:20:22",
        "partner_id":"9c62d683db96c7cabf8db0109be6bb"
        },
"success":true
}
```

View details about any transaction. The transaction ID will be used to fetch this data.


**HTTP Request**

`GET  https://api.shopsocially.com/v2/loyalty/transactions/{transaction_id}`


**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
activity_id | string | Unique id for an activity
activity_name | string | Name of the activity
user_id   | string | Unique id generated for each user
user_first_name| string | The person’s first name
user_last_name| string | The person’s last name
user_email| string | The person’s email address
redemption_id | string | Unique id for an redemption
redemption_name | string | Name of the redemption   
id| string | Internal id
approved_time | string | The time when transaction was approved
auto_approval_date | string | The time when transaction was auto approved 
points_status| string | Status of transaction (approved,auto_approved,exhausted,<br>pending,rejected,auto_rejected,expired)
transaction_type| string | Type of transaction (award,deduct,redeem)
coupon_code | string | Coupon code used in the transaction
points| integer | The number of points involved in transaction
approved_by | string | Name of admin who approved the transaction
created_time| string | The time when the transaction was created
updated_time| string | The time when the transaction was updated
partner_id| string | The id of the merchant

## Users
The Users API lets you create an user, update user information, get details of a particular user and get the list of all users for a merchant.


### Create User

```shell
curl -X POST --header "partner-id: 9c62d683db96c7cabf8db0109be6bb" 
--header "api-key: 9a4cf9131sasaw21dsb53a29e1b13" 
--data "first_name= bob" 
--data "last_name= Baker"
--data "email= bob@shopsocially.com"
--data "uid= merc"
--data "gender= Male"
--data "profile_image_url= https://some_link_to_ss_logo"
--data "loyalty_level_id= initial_level"
"https://api.shopsocially.com/v2/loyalty/users"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

payload = json.dumps({
	"first_name" : "Bob",
	"last_name" : "Baker",
	"email" : "bob@shopsocially.com",
	"uid" : "merc",
	"gender" : "male",
	"profile_image_url" : "https://some_link_to_ss_logo"
})

response = requests.post(url = "https://api.shopsocially.com/v2/loyalty/users",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
    "data":
	    {
		    "user_id":"6176be2d5e",
		    "user_email":"bob@shopsocially.com",
		    "first_name":"Bob",
		    "last_name":"Baker",
		    "profile_image_url":"https://some_link_to_ss_logo",
		    "gender":"Male",
		    "redeemed_points":0,
		    "available_points":0,
		    "loyalty_level_id":"",
		    "loyalty_lifetime_level_id":"",
		    "merchant_uid":""
	    },
    "success":true
}
```
Create a new user for merchant.


**HTTP Request**

`POST  https://api.shopsocially.com/v2/loyalty/users`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
first_name | string | Yes | First name of the user
last_name | string | Yes | Last name of the user
email | string | Yes | Email of the user
uid | string | Yes | User ID of the user
gender | string | No | Gender of the user (male/female)
profile_image_url | string | No | Profile Image URL of the user
loyalty_level_id | string | No | The loyalty level in which user will be moved

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
user_id | string | Internal id
user_email | string | Email of the user
first_name | string | First name of the user
last_name | string | Last name of the user
profile_image_url | string | Profile Image Url of the user
gender | string | Gender of the user
redeemed_points | integer | Points redeemed by the user
available_points | integer | Points currently available for the user
loyalty_level_id | string | ID of loyalty level currently assigned to user
loyalty_lifetime_level_id | string | ID of lifetime loyalty level currently assigned to the user
merchant_uid | string | UID of the merchant

### Update User

```shell
curl -X PUT --header "partner-id: 9c62d683db96c7cabf8db0109be6bb" 
--header "api-key: 9a4cf9131sasaw21dsb53a29e1b13" 
--data "loyalty_level_id= new_level_id"
"https://api.shopsocially.com/v2/loyalty/users/{uid}"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

payload = json.dumps({
	"loyalty_level_id" : "new_level_id"
})

response = requests.put(url = "https://api.shopsocially.com/v2/loyalty/users/{uid}",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
    "data":
	    {
		    "user_id":"6176be2d5e",
		    "user_email":"bob@shopsocially.com",
		    "first_name":"Bob",
		    "last_name":"Baker",
		    "profile_image_url":"https://some_link_to_ss_logo",
		    "gender":"Male",
		    "redeemed_points":0,
		    "available_points":0,
		    "loyalty_level_id":"",
		    "loyalty_lifetime_level_id":"",
		    "merchant_uid":""
	    },
    "success":true
}
```
This function is only applicable to the users created through the merchant’s authentication function. The details of the user, for user ID passed by merchant, will be updated. The users created through the ShopSocially authentication cannot be updated.


**HTTP Request**

`PUT  https://api.shopsocially.com/v2/loyalty/users/{uid}`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
loyalty_level_id | string | No | The loyalty level in which user will be moved

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
user_id | string | Internal id
user_email | string | Email of the user
first_name | string | First name of the user
last_name | string | Last name of the user
profile_image_url | string | Profile Image Url of the user
gender | string | Gender of the user
redeemed_points | integer | Points redeemed by the user
available_points | integer | Points currently available for the user
loyalty_level_id | string | ID of loyalty level currently assigned to user
loyalty_lifetime_level_id | string | ID of lifetime loyalty level currently assigned to the user
merchant_uid | string | UID of the merchant

### Get All Users

```shell
curl -X GET --header "partner-id: 9c62d683db96c7cabf8db0109be6bb" 
--header "api-key: 9a4cf9131sasaw21dsb53a29e1b13"
--data "from_date= 01-01-2016",
--data "to_date= 31-12-2016",
--data "start_index= 0",
--data "count= 10"
"https://api.shopsocially.com/v2/loyalty/users"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

payload = json.dumps({
	"from_date" : "01-01-2016",
	"to_date" : "31-12-2016",
	"start_index" : "0",
	"count" : "10"
})

response = requests.get(url = "https://api.shopsocially.com/v2/loyalty/users",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
    "data":
	    {
		    "next_start_index":2,
		    "total_loyalty_users":1,
		    "loyalty_users":
					[{
					     "loyalty_life_time_level_name": "",
					     "loyalty_lifetime_level_id": "",
					     "expiration_schedule": [],
					     "last_name": "Baker",
					     "redeemed_points": 0,
					     "last_award_transaction": {},
					     "gender": "male",
					     "dashboard_url": "https://shopsocially.com/loyalty/test_site/user/56d62cb9735f833785f95ef5/dashboard",
					     "first_name": "Bob",
					     "profile_image_url": "https://some_link_to_ss_logo", 
					     "available_points": 0,
					     "loyalty_level_id": "",
					     "loyalty_level_name": "",
					     "user_id": "",
					     "user_email": "bob@shopsocially.com"
					}],
		    "more":false
	    },
    "success":true
}
```
This will get all the users enrolled in the loyalty program.


**HTTP Request**

`GET  https://api.shopsocially.com/v2/loyalty/users`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
from_date | string | Yes | The start date
to_date | string | Yes | The end date
filter_by | string | No | Filters to be applied along with from_date and to_date. Accepted values enrollment_date' , 'last_activity_date
start_index | integer | No | The start index to fetch the data from. If not specified, the start index will be 0
count | integer | No | Number of records to fetch. If not specified, the count will be 100

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
next_start_index | integer | Next index of record to fetch 
total_loyalty_users | integer | Total number of Loyalty users for this merchant
loyalty_users | array | Array of users based on time range, filters and start_index
more | boolean | Indicates if more number of users are available (True/False)

### Get User

```shell
curl -X GET --header "partner-id: 9c62d683db96c7cabf8db0109be6bb" 
--header "api-key: 9a4cf9131sasaw21dsb53a29e1b13" 
"https://api.shopsocially.com/v2/loyalty/users/bob@shopsocially.com"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

payload = json.dumps({
})

response = requests.get(url = "https://api.shopsocially.com/v2/loyalty/users/bob@shopsocially.com",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
    "data":
	    {
		    "user_id":"6176be2d5e",
		    "user_email":"bob@shopsocially.com",
		    "first_name":"Bob",
		    "last_name":"Baker",
		    "profile_image_url":"https://some_link_to_ss_logo",
		    "gender":"Male",
		    "redeemed_points":0,
		    "available_points":0,
		    "loyalty_level_id":None,
		    "loyalty_lifetime_level_id":None,
		    "user_language":"English",
		    "user_status":"Active?",
		    "expiration_schedule": [{"expiration_date": "07-Jan-2016 23:59:59","points": 34226}],
		    "last_award_transaction":{"date": "07-Dec-2015 13:23:35","points": 300,"name": "Became a Twitter Follower"}
	    },
    "success":true
}
```
This will get the details of the user along with available, and redeemed points .


**HTTP Request**

`GET  https://api.shopsocially.com/v2/loyalty/users/user_email`

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
user_id | string | Internal id
user_email | string | Email of the user
first_name | string | First name of the user
last_name | string | Last name of the user
profile_image_url | string | Profile Image Url of the user
gender | string | Gender of the user
redeemed_points | integer | Points redeemed by the user
available_points | integer | Points currently available for the user
loyalty_level_id | string | ID of loyalty level currently assigned to user
loyalty_lifetime_level_id | string | ID of lifetime loyalty level currently assigned to the user
user_language | string | Default language associated to user's profile
user_status | string | User status
expiration_schedule | array | Array of expiration schedules
last_award_transaction | string | record of last awarded transaction

### Get User Transactions

```shell
curl -X GET --header "partner-id: 9c62d683db96c7cabf8db0109be6bb" 
--header "api-key: 9a4cf9131sasaw21dsb53a29e1b13"
--data "from_date= 01-01-2016",
--data "to_date= 31-12-2016",
--data "points_status= ['approved']"
--data "start_index= 0",
"https://api.shopsocially.com/v2/loyalty/users/bob@shopsocially.com/transactions"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

payload = json.dumps({
	"from_date" : "01-01-2016",
	"to_date" : "31-12-2016",
	"points_status" : ['approved']
	"start_index" : "0",
})

response = requests.get(url = "https://api.shopsocially.com/v2/loyalty/users/bob@shopsocially.com/transactions",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
    "data":
	    {
		"total": 1,
		"next_start_index": 2,
		"transactions":
			[{
			    "points_status": "approved",
			    "points_used": 0,
			    "available_points": 22,
			    "points_expired": 0,
			    "created_time": "14-Jan-2016 06:33:22",
			    "updated_time": "14-Jan-2016 06:33:22",
			    "id": "569741320daac146edac90c0",
			    "user_id": "54b3b13e0daac1321e07ce1f",
			    "points_expiration_date": "14-Mar-2016 06:33:22",
			    "approved_by": null,
			    "qualified_points": true,
			    "redemption_value": null,
			    "activity_id": "shared_photo_via_desktop_or_mobile",
			    "user_first_name": "Bob",
			    "order_id": "1452749387649368175",
			    "reason": null,
			    "points_deducted": 0,
			    "merchant_id": "9c62d683db96c7cabf8db0109be6bb",
			    "redemptions_list": null,
			    "approved_time": "14-Jan-2016 06:33:22",
			    "redemption_name": null,
			    "auto_approval_date": "",
			    "transaction_type": "award",
			    "user_last_name": "Baker",
			    "coupon_code": null,
			    "points": 22,
			    "redemption_id": null,
			    "activity_name": "Shared Photo via Mobile or Desktop",
			    "user_email": "bob@shopsocially.com"
			}],
		    "more": false
	    },
    "success":true
}
```
This will return information about the transactions done (points earned / redeemed ) by any user. User's email ID will be used to search and return user specific information.


**HTTP Request**

`GET  https://api.shopsocially.com/v2/loyalty/users/user_email/transactions`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
from_date | string | Yes | The start date
to_date | string | Yes | The end date
points_status | string | No | Status of transaction to fetch viz. pending, auto_approved, approved, auto_rejected, rejected, expired, exhausted
start_index | integer | No | The start index to fetch the data from. If not specified, the start index will be 0

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
total | integer | Total number of transactions for the user 
next_start_index | integer | Next index of record to fetch 
transactions | array | Array of users based on time range, filters and start_index
more | boolean | Indicates if more number of transactions are available (True/False)

Work in Progress USERS END


---------------------------------------------------------------------------------------------------------------------

## Activities
The Activities API lets you create an activity, update activity configurations, get details of a particular activity and get the list of all activities for a merchant.


### Create Activity

```shell
curl -X POST --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
--data "activity_id= <Activity ID>" 
--data "activity_name= <Activity Name>"
--data "activity_description= <Activity Description>"
--data "activity_type = <Activity type>"
--data "max_award_frequency = <Max Award Frequency>"
--data "max_award_frequency_interval = <Max Award Frequency Interval>"
--data "is_active = <Activity State>"
--data "approval_method = <Award Approval Method>"
--data "points_per_activity = <Points to be awarded>"
--data "bonus_multiplier = <Bonus Multiplier>"
--data "is_sale_activity = <Is sale activity>"
--data "qualified_points = <Is qualified for Tier change>"
--data "show_activity_in_user_dashboard = <Show in dashboard>"
"https://api.shopsocially.com/v2/loyalty/activities"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

payload = json.dumps({
    "activity_id" : "shared_via_email",
	"activity_name" : "Share via Email",
	"activity_description" : "Earn points when you share our website 
          with your friends via email.",
	"activity_type" : "fixed",
	"max_award_frequency" : 100,
	"max_award_frequency_interval" : "day",
	"is_active": true,
	"approval_method": "immediate",
	"points_per_activity": 100,
	"bonus_multiplier": 1,
	"is_sale_activity": false,
	"qualified_points": true,
	"show_activity_in_user_dashboard": true
	})

response = requests.post(url = "https://api.shopsocially.com/v2/loyalty/activities",
                        headers = headers, data = payload)
```



> The above command returns JSON structured like this:

```json
{
  "data":{
		"id":"56655832418a580281c2c41c",
        "merchant_id":"c75d9a44221ac904ddadb81766786b34",
		"activity_id":"shared_via_email",
		"activity_name":"Share via Email",
		"activity_description":"Earn points when you share our website 
          with your friends via email.",
		"activity_type":"fixed",
		"approval_method":"immediate",
		"bonus_multiplier":1,
		"created_time":"07-Dec-2015 09:58:10",
		"fixed_duration_period":30,
		"is_active":true,
		"is_sale_activity":false,
		"loyalty_activity_img":"https://d1qbqkkh49kht1.cloudfront.net/dc79fd08611160b9176d0d4b5c3f506f.png",
		"max_award_frequency":100,
		"max_award_frequency_interval":"day",
		"points_earning_url":"",
		"points_per_activity":100,
		"qualified_points":true,
		"show_activity_in_user_dashboard":true,
		"ss_internal":true,
		"updated_time":"31-Aug-2016 05:10:21"
  },
  "success":true
}
```
Create a new activity which will earn loyalty points for customers.


**HTTP Request**

`POST  https://api.shopsocially.com/v2/loyalty/activities`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
activity_id | string | Yes | The unique id of an <br>activity
activity_name | string | Yes | The name of the activity
activity_description | string | Yes | Description of the activity
activity_type | string | Yes | Whether the activity <br> should award fixed <br>number of points or <br>use a multiplier.<br>(fixed/bonus_multiplier)
max_award_frequency | integer | Yes| The maximum number <br>of times a user will <br>be awarded points
max_award_frequency_interval | string | Yes | The time interval <br>constraint to the max_award_frequency <br>number (day, week, month, year, lifetime)
bonus_multiplier | integer | No | Multiplication factor to <br>award points instead of <br>awarding fixed points<br>(defaults to 1)
points_per_activity | integer | No | Fixed number of points to be awarded for an activity<br>(defaults to 1)
points_earning_url | string | No | Merchant's website page url where the activity will be configured
approval_method | string | Yes | User transaction approval method (immediate/<br>manual/monthly)
is_active | boolean | Yes | Whether the activity should be active or paused<br>(defaults to true)
is_sale_activity | boolean | No | Whether the activity should be a sale activity (defaults to false)
qualified_points | boolean | No | Whether the activity should be qualified for tier upgrade (defaults to true)
show_activity_in_user_dashboard | boolean | No | Whether the activity should be shown in user dashboard (defaults to true)

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
id| string | Internal id
merchant_id | string | The id of the merchant
activity_id| string | Unique id generated for each activity
activity_name | string | The name of the activity
activity_description | string | The description of the activity
activity_type | string | Whether the activity should award fixed number of points or use a multiplier (fixed/bonus_multiplier)
approval_method | string | User transaction approval method (immediate/manual/monthly).
bonus_multiplier | integer | Multiplication factor to award points instead of awarding fixed points 
created_time | string | The time when activity is created_time
is_active | boolean | Indicates whether the activity is active or paused (true/false)
is_sale_activity | boolean | Indicates if the activity is a purchase activity (true/false)
loyalty_activity_img | string | The image url of an activity
max_award_frequency | integer | The maximum number of times a user will be awarded points
max_award_frequency_interval | string | The time interval constraint to the max_award_frequency number (day, week, month, year, lifetime)
points_earning_url | string | Merchant's website page url where the activity will be configured
point_per_activity | integer | Fixed number of points to be awarded for an activity
qualified_points | boolean | Indicates if the activity is qualified for tier upgrade (true/false)
show_activity_in_user_dashboard | boolean | Indicates if the activity is to be shown on the end user dashboard (true/false)
ss_internal | boolean | Indicates whether an activity is an internal activity (true/false)
updated_time | string | The time when activity configuration was last modified

### Update Activity
```shell
curl -X PUT --header "partner-id: <your-partner-id>"
--header "api-key: <your-api-key>" 
--data "activity_name= <Activity Name>"
--data "activity_description= <Activity Description>"
--data "activity_type = <Activity type>"
--data "max_award_frequency = <Max Award Frequency>"
--data "max_award_frequency_interval = <Max Award Frequency Interval>"
--data "bonus_multiplier = <Bonus Multiplier>"
--data "points_per_activity = <Points to be awarded>"
--data "approval_method = <Award Approval Method>"
--data "is_active = <Activity State>"
"https://api.shopsocially.com/v2/loyalty/activities/{activity_id}"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

payload = json.dumps({
  "activity_name" : "Share via Email",
  "activity_description" : "Earn points when you share our website 
          with your friends via email.",
  "activity_type" : "fixed",
  "max_award_frequency" : 100,
  "max_award_frequency_interval" : "day",
  "bonus_multiplier": 1,
  "points_per_activity": 100,
  "approval_method": "immediate",
  "is_active": true
})

response = requests.put(url = "https://api.shopsocially.com/v2/loyalty/activities/{activity_id}",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
  "data":{
    "id":"56655832418a580281c2c41c",
    "activity_id":"shared_via_email",
    "activity_name":"Share via Email",
    "activity_description":"Earn points when you share our website with your friends via email.",
    "activity_type":"fixed",
    "approval_method":"immediate",
    "bonus_multiplier":1,
    "created_time":"07-Dec-2015 09:58:10",
    "fixed_duration_period":30,
    "is_active":true,
    "is_sale_activity":false,
    "loyalty_activity_img":"https://d1qbqkkh49kht1.cloudfront.net/dc79fd08611160b9176d0d4b5c3f506f.png",
    "max_award_frequency":100,
    "max_award_frequency_interval":"day",
    "merchant_id":"c75d9a44221ac904ddadb81766786b34",
    "points_earning_url":"",
    "points_per_activity":100,
    "qualified_points":true,
    "show_activity_in_user_dashboard":true,
    "show_on_merchant_center":true,
    "ss_internal":true,
    "updated_time":"31-Aug-2016 05:10:21"
  },
  "success":true
}
```
Update an activity configuration by passing the activity ID.
Any or all of the below listed fields (query parameters table) can be updated by passing in the adequate parameter.

**HTTP Request**

`PUT  https://api.shopsocially.com/v2/loyalty/activities/{activity_id}`

**Query Parameters**

Parameter | Type | Description
--------- | ---- | -----------
activity_name | string |  The name of the activity
activity_description | string |  Description of the activity
activity_type | string | Whether the activity should award fixed number of points or use a multiplier (fixed/bonus_multiplier)
max_award_frequency | integer |  The maximum number of times a user will be awarded points
max_award_frequency_interval | string | The time interval constraint to the max_award_frequency number (day, week, month, year, lifetime)
bonus_multiplier | integer | Multiplication factor to award points instead of awarding fixed points (defaults to 1)
points_per_activity | integer | Fixed number of points to be awarded for an activity (defaults to 1)
approval_method | string | User transaction approval method (immediate/manual/monthly)
is_active | boolean | Whether the activity should be active or paused (defaults to true)

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
id| string | Internal id
activity_id| string | Unique id generated for each activity
activity_name | string | The name of activity
activity_description | string | The description of the activity
activity_type | string | Whether the activity should award fixed number of points or use a multiplier(fixed/bonus_multiplier).
approval_method | string | User transaction approval method (immediate/manual/monthly).
bonus_multiplier | integer | Multiplication factor to award points instead of awarding fixed points 
created_time | string | The time when activity is created_time
is_active | boolean | Indicates whether the activity is active or paused (true/false)
is_sale_activity | boolean | Indicates if the activity is a purchase activity (true/false)
loyalty_activity_img | string | The image url of an activity
max_award_frequency | integer | The maximum number of times a user will be awarded points
max_award_frequency_interval | string | The time interval constraint to the max_award_frequency number (day, week, month, year, lifetime).
merchant_id | string | The id of the merchant
points_earning_url | string | Merchant's website page url where the activity will be configured
point_per_activity | integer | Fixed number of points to be awarded for an activity
qualified_points | boolean | Indicates if the activity is qualified for tier upgrade (true/false)
show_activity_in_user_dashboard | boolean | Indicates if the activity is to be shown in the end user dashboard (true/false)
ss_internal | boolean | Indicates whether an activity is an internal activity (true/false)
updated_time | string | The time when the activity configuration was last modified


### Get Activity
```shell
curl -X GET --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
"https://api.shopsocially.com/v2/loyalty/activities/{activity_id}"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

response = requests.get(url = "https://api.shopsocially.com/v2/loyalty/activities/{activity_id}",
                        headers = headers)
```


> The above command returns JSON structured like this:

```json
{
  "data":{
		"id":"56655832418a580281c2c41c",
		"activity_id":"shared_via_email",
		"activity_name":"Share via Email",
		"activity_description":"Earn points when you share our website with your friends via email.",
		"activity_type":"fixed",
		"approval_method":"immediate",
		"bonus_multiplier":1,
		"created_time":"07-Dec-2015 09:58:10",
		"is_active":true,
		"is_sale_activity":false,
		"loyalty_activity_img":"https://d1qbqkkh49kht1.cloudfront.net/dc79fd08611160b9176d0d4b5c3f506f.png",
		"max_award_frequency":100,
		"max_award_frequency_interval":"day",
		"merchant_id":"c75d9a44221ac904ddadb81766786b34",
		"points_earning_url":"",
		"points_per_activity":100,
		"qualified_points":true,
		"show_activity_in_user_dashboard":true,
		"show_on_merchant_center":true,
		"ss_internal":true,
		"updated_time":"31-Aug-2016 05:10:21"
  },
  "success":true
}
```
View details about any activity. The activity ID will be used to fetch this data.

**HTTP Request**

`GET  https://api.shopsocially.com/v2/loyalty/activities/{activity_id}`


**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
id| string | Internal id
activity_id| string | Unique id generated for each activity
activity_name | string | The name of activity
activity_description | string | The description of the activity
activity_type | string | Whether the activity should award fixed number of points or use a multiplier (fixed/bonus_multiplier)
approval_method | string | User transaction approval method (immediate/manual/monthly).
bonus_multiplier | integer | Multiplication factor to award points instead of awarding fixed points 
created_time | string | The time when activity is created_time
is_active | boolean | Indicates if activity is active or paused (true/false)
is_sale_activity | boolean | Indicates if the activity is a purchase activity (true/false)
loyalty_activity_img | string | The image url of an activity
max_award_frequency | integer | The maximum number of times a user will be awarded points
max_award_frequency_interval | string | The time interval constraint to the max_award_frequency number (day, week, month, year, lifetime)
merchant_id | string | The id of the merchant
points_earning_url | string | Merchant's website page url where the activity will be configured.
points_per_activity | integer | Fixed number of points to be awarded for an activity
qualified_points | boolean | Indicates if the activity is qualified for tier upgrade (true/false)
show_activity_in_user_dashboard | boolean | Indicates if the activity is to be shown on the end user dashboard (true/false)
ss_internal | boolean | Indicates whether an activity is an internal activity (true/false)
updated_time | string | The time when the activity configuration was last modified

### Get All Activities
```shell
curl -X GET --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
"https://api.shopsocially.com/v2/loyalty/activities"
```

```python
import requests
import json

headers = {'partner-id': '9c62d683db96c7cabf8db0109be6bb', 
           'api-key': '9a4cf9131sasaw21dsb53a29e1b13'}

response = requests.get(url = "https://api.shopsocially.com/v2/loyalty/activities",
                        headers = headers)
```


> The above command returns JSON structured like this:

```json
{
  "data":[
      {
      "id":"56655832418a580281c2c41c",
      "activity_id":"shared_via_email",
      "activity_name":"Share via Email",
      "activity_description":"Earn points when you share our website with your friends via email.",
      "activity_type":"fixed",
      "approval_method":"immediate",
      "bonus_multiplier":1,
      "created_time":"07-Dec-2015 09:58:10",
      "is_active":true,
      "is_sale_activity":false,
      "max_award_frequency":100,
      "max_award_frequency_interval":"day",
      "merchant_id":"c75d9a44221ac904ddadb81766786b34",
      "points_earning_url":"",
      "points_per_activity":100,
      "qualified_points":true,
      "show_activity_in_user_dashboard":true,
      "ss_internal":true,
      "updated_time":"31-Aug-2016 05:10:21"
      },
      {...}
  ],
  "success":true
}
```
View a list of all configured activities for which customers can earn loyalty points.

**HTTP Request**

`GET  https://api.shopsocially.com/v2/loyalty/activities`


**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
id| string | Internal id
activity_id| string | Unique id generated for each activity
activity_name | string | The name of the activity
activity_description | string | The description about activity
activity_type | string | Whether the activity should award fixed number of points or use a multiplier (fixed/bonus_multiplier)
approval_method | string | User transaction approval method (immediate/manual/monthly).
bonus_multiplier | integer | Multiplication factor to award points instead of awarding fixed points
created_time | string | The time when the activity was created
is_active | boolean | Indicates if an activity is active or paused (true/false)
is_sale_activity | boolean | Indicates if an activity is a sale activity (true/false)
max_award_frequency | integer | The maximum number of times a user will be awarded points
max_award_frequency_interval | string | The time interval constraint to the max_award_frequency number (day, week, month, year, lifetime)
merchant_id | string | The id of the merchant
points_earning_url | string | Merchant's website page url where the activity will be configured.
point_per_activity | integer | Fixed number of points to be awarded for an activity
qualified_points | boolean | Indicates if the activity is qualified for tier upgrade (true/false)
show_activity_in_user_dashboard | boolean | Indicates if the activity is to be shown on the end user dashboard (true/false).
ss_internal | boolean | Indicates whether an activity is an internal activity (true/false)
updated_time | string | The time when activity configuration was last modified



## Redemptions
The Redemptions API lets you create a redemption, update redemption configurations, get details of a particular redemption and get the list of all redemptions for a merchant.

### Create Redemption

```shell
curl -X POST --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
--data "redemption_id: <redemption_id>" 
--data "redemption_name: <redemption_name>" 
--data "is_active: <redemption_status>" 
--data "allowed_redeem_points: <allowed_redeem_points>" 
--data "redemption_value: <redemption_value>" 
--data "giftcard_description: <giftcard_description>" 
--data "terms_and_conds_text: <terms_and_conds_text>" 
--data "redemption_instructions: <redemption_instructions>" 
--data "giftcard_coupon_codes: <giftcard_coupon_codes>" 
--data "coupon_expiry: <coupon_expiry>" 
"https://api.shopsocially.com/v2/loyalty/redemptions"
```

```python
import requests
import json

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

payload = json.dumps({
"redemption_id": "<redemption_id>",
"redemption_name": "<redemption_name>",
"is_active": "<redemption_status>",
"allowed_redeem_points": "<allowed_redeem_points>",
"redemption_value": "<redemption_value>",
"giftcard_description": "<giftcard_description>",
"terms_and_conds_text": "<terms_and_conds_text>",
"redemption_instructions": "<redemption_instructions>",
"giftcard_coupon_codes": "<giftcard_coupon_codes>",
"coupon_expiry": "<coupon_expiry>"})

response = requests.post(url = "https://api.shopsocially.com/v2/loyalty/transaction/return",
                        headers = headers, data = payload)
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "redemption_email_html": "<div style=\"border:solid 1px rgba(0,0,0,0.2);font-family:'Source Sans Pro', 'Helvetica Neue', 'Lucida Sans', sans-serif;\"><img src=\"https://d1qbqkkh49kht1.cloudfront.net/c66c2dc1d96e2ac46b9722b1a2524c10.png\" style=\"width:100%\"><div style=\"text-align:left;margin-top:10px;font-size: 15px;padding:0px 30px 0px 30px\">Hi,<br><div style=\"margin-top: 10px;\">You have redeemed _POINTS_REDEEMED_ loyalty points from your account at <a href=\"_MERCHANT_WEB_URL_\" target=\"_blank\">_MERCHANT_NAME_</a>.<br><br><a href=\"_USER_DASHBOARD_URL_\" target=\"_blank\">Click here</a> to log in and check your loyalty points status and to redeem more points.</div><div style=\"text-align:left;margin-top: 15px;font-size:15px;\">Thank you.<br><br>Thanks,<br>_MERCHANT_NAME_ Team<br><br></div></div>",
    "loyalty_redemption_card_img": "https://d1qbqkkh49kht1.cloudfront.net/b32b7803e9bbcf6b8e9b8d8ce1be7224.png",
    "created_time": "07-Dec-2016 12:17:02",
    "giftcard_coupon_codes": "a,b,c,d,e,f,g,h,i",
    "id": "5847fdbeb0207a60ce2facf1",
    "giftcard_description": "giftcard_description",
    "terms_and_conds_text": "terms_and_conds_text",
    "after_redemption_html": "<div class=\"coupon_code_div\">_COUPON_CODE_</div><div style=\"margin-top: 15px;\"> A copy of the code has been emailed to _USER_EMAIL_.</div>",
    "associated_levels": [
      "all"
    ],
    "send_redemption_email": true,
    "redemption_value": "10",
    "coupon_expiry_trigger": 0,
    "is_active": false,
    "enable_coupon_based_incentive": true,
    "redemption_instructions": "redemption_instructions",
    "redemption_email_subject": "Redemption Confirmation Receipt - _MERCHANT_NAME_ Loyalty Program",
    "coupon_expiry": "01-Jan-2018",
    "merchant_id": "c75d9a44221ac904ddadb81766786b34",
    "show_redemption_in_user_dashboard": true,
    "show_on_merchant_center": true,
    "redemption_name": "redemption_name",
    "coupon_expiry_delta": 7,
    "allowed_redeem_points": 100,
    "updated_time": "07-Dec-2016 12:17:03",
    "redemption_id": "test_redemption"
  },
  "success": true
}

```
Create a redemption option.

**HTTP Request**

`POST  https://api.shopsocially.com/v2/loyalty/redemptions`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
redemption_id | string | Yes | Redemption ID.
redemption_name | integer | Yes | The name of the redemption which will be displayed to the user
is_active | boolean | No | Should the redemption option be active or paused<br>(defaults to false)
allowed_redeem_points | number | Yes | The number of points to be deducted when user redeems
redemption_value | number | Yes | The monetary value associated with the redemption
giftcard_description | string | Yes | The description of the giftcard/ coupon which will be shown to the user
terms_and_conds_text | string | Yes | The terms and conditions text users have to agree to at the time of redemption
redemption_instructions | string | No | The redemption instructions displayed to the users
giftcard_coupon_codes | string | Yes | One time coupon codes to be issued when user redeems<br>(comma separated)
coupon_expiry | string | Yes | The expiry date of the coupons<br>(DD-MMM-YYYY)


**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
redemption_email_html| string | Redemption Email HTML
loyalty_redemption_card_img| string | Loyalty Redemption Card Image
created_time| string | Redemption creation time
giftcard_coupon_codes| string | Giftcard Coupons entered
id| string | Internal ID
giftcard_description| string | Giftcard Description
terms_and_conds_text| string | Terms and Conditions
after_redemption_html| string | After redemption HTML
associated_levels| array | Levels Associated with the Redemption
send_redemption_email| boolean | Indicates whether the redemption email was sent or not (true/false)
redemption_value| number | Points to be deducted after redemption
coupon_expiry_trigger| string | Coupon Expiry Trigger
is_active| boolean | Indicates whether the Redemption Option is active or not (true/false)
enable_coupon_based_incentive| boolean | Indicates whether coupon based incentive is enabled or not (true/false)
redemption_instructions| string | Redemption instructions
redemption_email_subject| string | Redemption Email Subject
coupon_expiry| string | The expiry date of the coupons
merchant_id| string | Merchant ID
show_redemption_in_user_dashboard| boolean | Indicates whether the redemption option is shown on the user dashboard (true/false)
show_on_merchant_center| boolean | Indicates whether the redemption option is shown in the merchant center (true/false)
redemption_name| string | The name of the redemption
coupon_expiry_delta| number | Number of days before coupon expires
allowed_redeem_points| string | Allowed Redeem points
updated_time| string | Redemption option updation time
redemption_id| string | Redemption ID


### Update Redemption

```shell
curl -X PUT --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
--data "redemption_name: <redemption_name>" 
--data "is_active: <redemption_status>" 
--data "allowed_redeem_points: <allowed_redeem_points>" 
--data "redemption_value: <redemption_value>" 
--data "giftcard_description: <giftcard_description>" 
--data "terms_and_conds_text: <terms_and_conds_text>" 
--data "redemption_instructions: <redemption_instructions>" 
--data "giftcard_coupon_codes: <giftcard_coupon_codes>" 
--data "coupon_expiry: <coupon_expiry>" 
"https://api.shopsocially.com/v2/loyalty/redemptions/{redemption_id}"
```

```python
import requests
import json

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

payload = json.dumps({
"redemption_name": "<redemption_name>",
"is_active": "<redemption_status>",
"allowed_redeem_points": "<allowed_redeem_points>",
"redemption_value": "<redemption_value>",
"giftcard_description": "<giftcard_description>",
"terms_and_conds_text": "<terms_and_conds_text>",
"redemption_instructions": "<redemption_instructions>",
"giftcard_coupon_codes": "<giftcard_coupon_codes>",
"coupon_expiry": "<coupon_expiry>"})

response = requests.put(url = "https://api.shopsocially.com/v2/loyalty/redemptions/{redemption_id}",
                        headers = headers, data = payload)
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "redemption_email_html": "<div style=\"border:solid 1px rgba(0,0,0,0.2);font-family:'Source Sans Pro', 'Helvetica Neue', 'Lucida Sans', sans-serif;\"><img src=\"https://d1qbqkkh49kht1.cloudfront.net/c66c2dc1d96e2ac46b9722b1a2524c10.png\" style=\"width:100%\"><div style=\"text-align:left;margin-top:10px;font-size: 15px;padding:0px 30px 0px 30px\">Hi,<br><div style=\"margin-top: 10px;\">You have redeemed _POINTS_REDEEMED_ loyalty points from your account at <a href=\"_MERCHANT_WEB_URL_\" target=\"_blank\">_MERCHANT_NAME_</a>.<br><br><a href=\"_USER_DASHBOARD_URL_\" target=\"_blank\">Click here</a> to log in and check your loyalty points status and to redeem more points.</div><div style=\"text-align:left;margin-top: 15px;font-size:15px;\">Thank you.<br><br>Thanks,<br>_MERCHANT_NAME_ Team<br><br></div></div>",
    "loyalty_redemption_card_img": "https://d1qbqkkh49kht1.cloudfront.net/b32b7803e9bbcf6b8e9b8d8ce1be7224.png",
    "created_time": "07-Dec-2016 12:17:02",
    "giftcard_coupon_codes": "a,b,c,d,e,f,g,h,i",
    "id": "5847fdbeb0207a60ce2facf1",
    "giftcard_description": "giftcard_description",
    "terms_and_conds_text": "terms_and_conds_text",
    "after_redemption_html": "<div class=\"coupon_code_div\">_COUPON_CODE_</div><div style=\"margin-top: 15px;\"> A copy of the code has been emailed to _USER_EMAIL_.</div>",
    "associated_levels": [
      "all"
    ],
    "send_redemption_email": true,
    "redemption_value": "10",
    "coupon_expiry_trigger": 0,
    "is_active": false,
    "enable_coupon_based_incentive": true,
    "redemption_instructions": "redemption_instructions",
    "redemption_email_subject": "Redemption Confirmation Receipt - _MERCHANT_NAME_ Loyalty Program",
    "coupon_expiry": "01-Jan-2018",
    "merchant_id": "c75d9a44221ac904ddadb81766786b34",
    "show_redemption_in_user_dashboard": true,
    "show_on_merchant_center": true,
    "redemption_name": "redemption_name",
    "coupon_expiry_delta": 7,
    "allowed_redeem_points": 100,
    "updated_time": "07-Dec-2016 12:17:03",
    "redemption_id": "test_redemption"
  },
  "success": true
}

```
Update any of the configured redemption options using the Redemption option ID. Any or all of the below listed fields (query parameters table) can be updated by passing in the adequate parameter.

**HTTP Request**

`PUT  https://api.shopsocially.com/v2/loyalty/redemptions/{redemption_id}`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
redemption_name | integer | No | The name of the redemption which will be displayed to the user
is_active | boolean | No | Should the redemption option be active or paused<br>(defaults to false)
allowed_redeem_points | number | No | The number of points to be deducted when user redeems
redemption_value | number | No | The monetary value associated with the redemption
giftcard_description | string | No | The description of the giftcard/ coupon which will be shown to the user
terms_and_conds_text | string | No | The terms and conditions text users have to agree to at the time of redemption
redemption_instructions | string | No | The redemption instructions displayed to the users
giftcard_coupon_codes | string | No | One time coupon codes to be issued when user redeems<br>(comma separated)
coupon_expiry | string | No | The expiry date of the coupons<br>(DD-MMM-YYYY)


**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
redemption_email_html| string | Redemption Email HTML
loyalty_redemption_card_img| string | Loyalty Redemption Card Image
created_time| string | Redemption creation time
giftcard_coupon_codes| string | Giftcard Coupons entered
id| string | Internal ID
giftcard_description| string | Giftcard Description
terms_and_conds_text| string | Terms and Conditions
after_redemption_html| string | After redemption HTML
associated_levels| array | Levels Associated with the Redemption
send_redemption_email| boolean | Indicates whether the redemption email is to be sent or not (true/false)
redemption_value| number | Points to be deducted after redemption
coupon_expiry_trigger| string | Coupon Expiry Trigger
is_active| boolean | Indicates whether the redemption option is active or not (true/false)
enable_coupon_based_incentive| boolean | Indicates whether coupon based incentive is enabled or not (true/false)
redemption_instructions| string | Redemption instructions
redemption_email_subject| string | Redemption Email Subject
coupon_expiry| string | Coupon Expiry
merchant_id| string | Merchant ID
show_redemption_in_user_dashboard| boolean | Indicates whether the redemption option is to be shown on the user dashboard (true/false)
show_on_merchant_center| boolean | Indicates whether the redemption option is to be shown on the merchant center (true/false)
redemption_name| string | Redemption Name
coupon_expiry_delta| number | Number of days before coupon expires
allowed_redeem_points| string | Allowed Redeem points
updated_time| string | Redemption option updation time
redemption_id| string | Redemption ID


### Get Redemption

```shell
curl -X GET --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>"
"https://api.shopsocially.com/v2/loyalty/redemptions/{redemption_id}"
```

```python
import requests
import json

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}


response = requests.get(url = "https://api.shopsocially.com/v2/loyalty/redemptions/{redemption_id}",
                        headers = headers)
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "redemption_email_html": "<div style=\"border:solid 1px rgba(0,0,0,0.2);font-family:'Source Sans Pro', 'Helvetica Neue', 'Lucida Sans', sans-serif;\"><img src=\"https://d1qbqkkh49kht1.cloudfront.net/c66c2dc1d96e2ac46b9722b1a2524c10.png\" style=\"width:100%\"><div style=\"text-align:left;margin-top:10px;font-size: 15px;padding:0px 30px 0px 30px\">Hi,<br><div style=\"margin-top: 10px;\">You have redeemed _POINTS_REDEEMED_ loyalty points from your account at <a href=\"_MERCHANT_WEB_URL_\" target=\"_blank\">_MERCHANT_NAME_</a>.<br><br><a href=\"_USER_DASHBOARD_URL_\" target=\"_blank\">Click here</a> to log in and check your loyalty points status and to redeem more points.</div><div style=\"text-align:left;margin-top: 15px;font-size:15px;\">Thank you.<br><br>Thanks,<br>_MERCHANT_NAME_ Team<br><br></div></div>",
    "loyalty_redemption_card_img": "https://d1qbqkkh49kht1.cloudfront.net/b32b7803e9bbcf6b8e9b8d8ce1be7224.png",
    "created_time": "07-Dec-2016 12:17:02",
    "giftcard_coupon_codes": "a,b,c,d,e,f,g,h,i",
    "id": "5847fdbeb0207a60ce2facf1",
    "giftcard_description": "giftcard_description",
    "terms_and_conds_text": "terms_and_conds_text",
    "after_redemption_html": "<div class=\"coupon_code_div\">_COUPON_CODE_</div><div style=\"margin-top: 15px;\"> A copy of the code has been emailed to _USER_EMAIL_.</div>",
    "associated_levels": [
      "all"
    ],
    "send_redemption_email": true,
    "redemption_value": "10",
    "coupon_expiry_trigger": 0,
    "is_active": false,
    "enable_coupon_based_incentive": true,
    "redemption_instructions": "redemption_instructions",
    "redemption_email_subject": "Redemption Confirmation Receipt - _MERCHANT_NAME_ Loyalty Program",
    "coupon_expiry": "01-Jan-2018",
    "merchant_id": "c75d9a44221ac904ddadb81766786b34",
    "show_redemption_in_user_dashboard": true,
    "show_on_merchant_center": true,
    "redemption_name": "redemption_name",
    "coupon_expiry_delta": 7,
    "allowed_redeem_points": 100,
    "updated_time": "07-Dec-2016 12:17:03",
    "redemption_id": "test_redemption"
  },
  "success": true
}

```
View details about a particular redemption option using the Redemption Option ID.

**HTTP Request**

`GET  https://api.shopsocially.com/v2/loyalty/redemptions/{redemption_id}`


**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
redemption_email_html| string | Redemption Email HTML
loyalty_redemption_card_img| string | Loyalty Redemption Card Image
created_time| string | Redemption creation time
giftcard_coupon_codes| string | Giftcard Coupons entered
id| string | Internal ID
giftcard_description| string | Giftcard Description
terms_and_conds_text| string | Terms and Conditions
after_redemption_html| string | After redemption HTML
associated_levels| array | Levels Associated with the Redemption
send_redemption_email| boolean | Indicates whether the redemption email is to be sent or not (true/false)
redemption_value| number | Points to be deducted after redemption
coupon_expiry_trigger| string | Coupon Expiry Trigger
is_active| boolean | Indicates whether the redemption option is active or not (true/false)
enable_coupon_based_incentive| boolean | Indicates whether coupon based incentive is enabled or not (true/false)
redemption_instructions| string | Redemption instructions
redemption_email_subject| string | Redemption Email Subject
coupon_expiry| string | Coupon Expiry
merchant_id| string | Merchant ID
show_redemption_in_user_dashboard| boolean | Indicates whether the redemption option is to be shown on the user dashboard (true/false)
show_on_merchant_center| boolean | Indicates whether the redemption option is to be shown on the merchant center (true/false)
coupon_expiry_delta| number | Number of days before coupon expires
allowed_redeem_points| string | Allowed Redeem points
updated_time| string | Redemption option updation time
redemption_id| string | Redemption ID


### Get All Redemptions

```shell
curl -X GET --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
"https://api.shopsocially.com/v2/loyalty/redemptions"
```

```python
import requests
import json

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

response = requests.get(url = "https://api.shopsocially.com/v2/loyalty/redemptions", headers = headers)
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "redemption_email_html": "<div style=\"border:solid 1px rgba(0,0,0,0.2);font-family:'Source Sans Pro', 'Helvetica Neue', 'Lucida Sans', sans-serif;\"><img src=\"https://d1qbqkkh49kht1.cloudfront.net/c66c2dc1d96e2ac46b9722b1a2524c10.png\" style=\"width:100%\"><div style=\"text-align:left;margin-top:10px;font-size: 15px;padding:0px 30px 0px 30px\">Hi,<br><div style=\"margin-top: 10px;\">You have redeemed _POINTS_REDEEMED_ loyalty points from your account at <a href=\"_MERCHANT_WEB_URL_\" target=\"_blank\">_MERCHANT_NAME_</a>.<br><br><a href=\"_USER_DASHBOARD_URL_\" target=\"_blank\">Click here</a> to log in and check your loyalty points status and to redeem more points.</div><div style=\"text-align:left;margin-top: 15px;font-size:15px;\">Thank you.<br><br>Thanks,<br>_MERCHANT_NAME_ Team<br><br></div></div>",
    "loyalty_redemption_card_img": "https://d1qbqkkh49kht1.cloudfront.net/b32b7803e9bbcf6b8e9b8d8ce1be7224.png",
    "created_time": "07-Dec-2016 12:17:02",
    "giftcard_coupon_codes": "a,b,c,d,e,f,g,h,i",
    "id": "5847fdbeb0207a60ce2facf1",
    "giftcard_description": "giftcard_description",
    "terms_and_conds_text": "terms_and_conds_text",
    "after_redemption_html": "<div class=\"coupon_code_div\">_COUPON_CODE_</div><div style=\"margin-top: 15px;\"> A copy of the code has been emailed to _USER_EMAIL_.</div>",
    "associated_levels": [
      "all"
    ],
    "send_redemption_email": true,
    "redemption_value": "10",
    "coupon_expiry_trigger": 0,
    "is_active": false,
    "enable_coupon_based_incentive": true,
    "redemption_instructions": "redemption_instructions",
    "redemption_email_subject": "Redemption Confirmation Receipt - _MERCHANT_NAME_ Loyalty Program",
    "coupon_expiry": "01-Jan-2018",
    "merchant_id": "c75d9a44221ac904ddadb81766786b34",
    "show_redemption_in_user_dashboard": true,
    "show_on_merchant_center": true,
    "redemption_name": "redemption_name",
    "coupon_expiry_delta": 7,
    "allowed_redeem_points": 100,
    "updated_time": "07-Dec-2016 12:17:03",
    "redemption_id": "test_redemption"
  }, ...
  "success": true
}

```
View a list of all the redemption options that are currently configured.

**HTTP Request**

`GET  https://api.shopsocially.com/v2/loyalty/redemptions`

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
redemption_email_html| string | Redemption Email HTML
loyalty_redemption_card_img| string | Loyalty Redemption Card Image
created_time| string | Redemption creation time
giftcard_coupon_codes| string | Giftcard Coupons entered
id| string | Internal ID
giftcard_description| string | Giftcard Description
terms_and_conds_text| string | Terms and Conditions
after_redemption_html| string | After redemption HTML
associated_levels| array | Levels Associated with the Redemption
send_redemption_email| boolean | Indicates whether the redemption email is to be sent or not (true/false)
redemption_value| number | Points to be deducted after redemption
coupon_expiry_trigger| string | Coupon Expiry Trigger
is_active| boolean | Indicates whether the redemption option is active or not (true/false)
enable_coupon_based_incentive| boolean | Indicates whether coupon based incentive is enabled or not (true/false)
redemption_instructions| string | Redemption instructions
redemption_email_subject| string | Redemption Email Subject
coupon_expiry| string | Coupon Expiry
merchant_id| string | Merchant ID
show_redemption_in_user_dashboard| boolean | Indicates whether the redemption option is to be shown on the user dashboard (true/false)
show_on_merchant_center| boolean | Indicates whether the redemption option is to be shown on the merchant center (true/false)
coupon_expiry_delta| number | Number of days before coupon expires
allowed_redeem_points| string | Allowed Redeem points
updated_time| string | Redemption option updation time
redemption_id| string | Redemption ID

## Returns

```shell
curl -X POST --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
--data "order_id: <order id for which the return needs to be processed>" 
--data "returned_amount: <returned_amount>" 
"https://api.shopsocially.com/v2/loyalty/transaction/return"
```

```python
import requests
import json

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

payload = json.dumps({"order_id": <order id for which the return needs to be processed>,
                      "returned_amount": <returned_amount>})

response = requests.post(url = "https://api.shopsocially.com/v2/loyalty/transaction/return",
                        headers = headers, data = payload)
```

> The above command returns JSON structured like this:

```json
{
  "data":{
          "user_id": "6176be2d5e",
          "user_email": "bob@shopsocially.com",
          "user_last_name": "Baker",
          "user_first_name": "Bob",
          "id": "54aeb7b4f283c6176be2d5ed",
          "transaction_type": "deduct",
          "points": 1000,
          "points_status": "pending_deduction",
          "returned_for_order_id": "1254aeb7b4f",
          "reason": "Order Partially Returned",
          "partner_id": "9c62d683db96c7cabf8db0109be6bb",
          "created_time": "30-Mar-16 19:20:22",
          "approved_time": "15-Apr-16 19:20:22",
          "updated_time": "15-Apr-16 19:20:22",
          "auto_approval_date": "15-May-16 19:20:22"
  },
  "success":true
}

```
Deduct points of a user when the user returns an order.

**HTTP Request**

`POST  https://api.shopsocially.com/v2/loyalty/transaction/return`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
order_id | string | Yes | The order ID for which the return needs to be processed.
returned_amount | integer | No | If returned_amount is specified and is a non-zero positive number, it will be processed as a partial return. The amount will be subtracted from the actual value of the order and remaining points will be awarded.</br>If returned_amount is not passed, it will be considered as complete order return.


**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
user_id   | string | Unique id generated for each user
user_email| string | The person’s email address
user_last_name| string | The person’s last name
user_first_name| string | The person’s first name
id| string | Internal id
transaction_type| string | This will always be "deduct"
points| integer | The number of points awarded
points_status| string | The status of the award transaction
returned_for_order_id| string | The order ID for which the return has been processed
reason | string | The reason for returning the order
partner_id| string | The id of the merchant
created_time| string | The time when the record was created
approved_time| string | The time when the record was approved
updated_time| string | The time when the record was updated
auto_approval_date| string | The date when the transaction is scheduled for auto approval
