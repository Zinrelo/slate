---
title: Zinrelo - API Reference
tags: [zinrelo, loyalty, award, redeem, deduct, user]
language_tabs:
  - shell: Shell
  - python: Python
  - javascript : JavaScript

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

### Purchase

```shell
curl -X POST --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20" 
--data "user_email=bob@zinrelo.com" 
--data "total=100" 
--data "subtotal=80" 
--data "order_id=75a2726d13artibb10"
--data "currency=USD" 
--data "coupon_code=CODE101" 
--data 'products=[{"category": "Mugs",
       "img_url": "https://cdn.website.com/product1/img.jpg",
       "price": "80",
       "product_id": "1234df",
       "quantity": "1",
       "tags": "Special, Coffee",
       "title": "Snoozeberry Travel Mug",
       "url": "http://www.website.com/product1.html"}]' 

"https://api.zinrelo.com/v1/loyalty/purchase"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e', 
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}
products = [{"category": "Mugs",
            "img_url": "https://cdn.website.com/product1/img.jpg",
		    "price": "80",
		    "product_id": "1234df",
		    "quantity": "1",
		    "tags": "Special, Coffee",
		    "title": "Snoozeberry Travel Mug",
		    "url": "http://www.website.com/product1.html"}] 

payload = {"user_email": "bob@zinrelo.com",
	   "total=100", 
	   "subtotal=80", 
	   "order_id=75a2726d13artibb10",
	   "currency=USD",
	   "coupon_code=CODE101",
	   "products"=json.dumps(products)}

response = requests.post(url = "https://api.zinrelo.com/v1/loyalty/purchase",
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

Award points to a user for making a  purchase. 
Points will be calculated based on the rules set in the purchase activity.

**HTTP Request**

`POST  https://api.zinrelo.com/v1/loyalty/purchase`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
user_email | string | Yes | The email of the user to whom points have to be awarded.
total | string | Yes | Total order value.
subtotal | string | Yes | Order value without shipping and taxes.
order_id | string | Yes | Order ID of the purchase transaction. This ensures that points for a particular transaction are awarded only once. 
currency | string | No | Three letter currency code for your store. Defaults to USD.
coupon_code | string | No | Coupon code used.
products | list | No | List of products bought. Product information should be passed in a specific format as shown in the sample code with the below parameters

**Products**


Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
product_id | string | Yes | Unique product identifier
price | string | Yes  | Product price
quantity | string | Yes | Quantity bought
title | string | Yes | Title of the product
url | string | Yes | Url of the product page
img_url | string | No | Url of the product image
category | string | No | Internal Product Category
tags | string | No | Internal Product tags

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
points_status| string | The status of the award transaction.Status can be approved,auto_approved,cached or pending
created_time| string | The time when the transaction was created

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
points_passed | integer | Yes | The number of points to be deducted.

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
"https://api.zinrelo.com/v1/loyalty/transactions?from_date=01/01/2016&to_date=31/12/2017&points_status=['approved, auto_approved']&start_index=0"
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
      "points_passed": 100,
      "points_expiration_date": "2017-07-09 12:42:05.484000",
      "redemption_name": null,
      "points": 300,
      "approved_by": null,
      "user_email": "bob@zinrelo.com",
      "created_time": "09-May-2017 12:21:53",
      "activity_name": "Purchase on website",
      "transaction_type": "award",
      "additional_data": {
          "zrl_products": [
              {
                  "category": "trucks",
                  "product_id": "1235df",
                  "price": "5",
                  "product_points_per_quantity": 10,
                  "bonus_multiplier": 2,
                  "quantity": "1"
              },
              {
                  "category": "toys",
                  "product_id": "231235df",
                  "price": "5",
                  "product_points_per_quantity": 5,
                  "bonus_multiplier": 0,
                  "quantity": "1"
              }
          ]
      },
      "approved_time": "2017-05-09 12:21:53.138000",
      "returned_for_order_id": null
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
points_status | list of strings | No | The status of the transaction (pending/approved/auto_approved/<br>redeemed/rejected/<br>deducted/expired)
start_index | integer | No | The index from where data needs to be fetched (defaults to 0)

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
total | integer | total number of transactions
next_start_index | integer | starting index of next batch of transactions
transactions | list | list of transactions
first_name| string | The person’s first name
last_name| string | The person’s last name
order_id | string | The order ID in case of a purchase transaction
auto_approval_date | string | The time when transaction was auto approved
points_status| string | Status of transaction (approved/auto_approved/exhausted/<br>pending/redeemed/deducted/<br>rejected/expired)
points_used| integer | The number of points used for redeeming a reward
redemption_value | string | Currency value of the redemption
points_passed| integer | Corresponds to the order_subtotal passed during a purchase transaction
points_expiration_date| string | The time when the points for the transaction expire if expiration is enabled
redemption_name | string | Name of the redemption
points| integer | The number of points involved in transaction
approved_by | string | Name of admin who approved the transaction
user_email| string | The person’s email address
created_time| string | The time when the transaction was created
activity_name | string | Name of the activity 
transaction_type| string | Type of transaction (award,deduct,redeem)
additional_data| json | Details of the products bought in the purchase transaction
approved_time | string | The time when transaction was approved
returned_for_order_id | string | The order ID for which a return transaction has been processed




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
	"start_index" : 0,
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
            "points_passed": 100,
            "points_expiration_date": "2017-07-09 12:42:05.484000",
            "redemption_name": null,
            "points": 300,
            "approved_by": null,
            "user_email": "bob@zinrelo.com",
            "created_time": "09-May-2017 12:21:53",
            "activity_name": "Purchase on website",
            "transaction_type": "award",
            "additional_data": {
                "zrl_products": [
                    {
                        "category": "trucks",
                        "product_id": "1235df",
                        "price": "5",
                        "product_points_per_quantity": 10,
                        "bonus_multiplier": 2,
                        "quantity": "1"
                    },
                    {
                        "category": "toys",
                        "product_id": "231235df",
                        "price": "5",
                        "product_points_per_quantity": 5,
                        "bonus_multiplier": 0,
                        "quantity": "1"
                    }
                ]
            },
            "approved_time": "2017-05-09 12:21:53.138000",
            "returned_for_order_id": null
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
points_status | string | No | Status of transaction to fetch (pending/auto_approved/approved/<br>redeemed/rejected/<br>expired/deducted)
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
returned_product_id | string | No | The ID for the product to be returned.
quantity | integer | No | The number of products to be returned.

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
--data "dob=12/31/1992"
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
	"uid" : "12345",
	"dob": "12/31/1992"
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
dob | string | No | Birthdate of user. format is MM/DD/YYYY.
referral_code | string | No | Referral code of the friend who has referred the user to be created.
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
"https://api.zinrelo.com/v1/loyalty/users?from_date=01/01/2016&to_date=12/31/2016&start_index=0&count=1000"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e',
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = json.dumps({
	"from_date" : "01/01/2016",
	"to_date" : "12/31/2016",
	"start_index" : 0,
	"count" : 1000
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
       "last_name": "Baker",
       "first_name": "Bob",
       "user_email": "bob@zinrelo.com",
       "address_line1": "H1-201,Persian Drive",
       "address_line2": "Suite #101",
       "city": "Sunnyvale",
       "zipcode": "94089",
       "state": "CA",
       "country": "United States",
       "phone_country_code": "+1",
       "phone_number": "66656324213",
       "referral_code": "BOB1234",
       "uid": "user123",
       "has_opted_out": false,
       "referrer_email": "",
       "dob": "12/12/1988",
       "loyalty_enroll_time": "10/20/2017",
       "loyalty_tier_name": "Platinum",
       "loyalty_tier_id": "zrl_platinum",
       "tier_details": {
		    "period_start": "01/01/2019",
		    "points": 6100,
		    "period_end": "12/31/2019"
	     },
	    "user_id": "user123",
	    "available_points": 5900,
	    "redeemed_points": 200,
	    "awarded_points": 6100,
        "pending_points": 0,
	    "expiration_schedule": [
	      {
		    "expiration_date": "18-Jan-2020  11:17:12",
		    "points": 6100
	      }
	    ]
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
count | integer | No | Number of records to fetch (defaults to 1000)

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
   "last_name": "Baker",
   "first_name": "Bob",
   "user_email": "bob@zinrelo.com",
   "dob": "12/12/1988",
   "address_line1": "H1-201,Persian Drive",
   "address_line2": "Suite #101",
   "city": "Sunnyvale",
   "zipcode": "94089",
   "state": "CA",
   "country": "United States",
   "phone_country_code": "+21",
   "phone_number": "66656324213",
   "referral_code": "BOB1234",
   "referrer_email": "",
   "uid": "user123",
   "loyalty_enroll_time": "01/18/2019",
   "loyalty_tier_id": "zrl_platinum",
   "loyalty_tier_name": "Platinum",
   "tier_details": {
	    "period_start": "01/01/2019",
	    "points": 6100,
	    "period_end": "12/31/2019"
    },
    "available_points": 6100,
    "redeemed_points": 0,
    "awarded_points": 6100,
    "pending_points": 0,
    "expiration_schedule": [
    {
	    "expiration_date": "18-Jan-2020  11:17:12",
	    "points": 6100
    }],
    "has_opted_out": false
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
address_line1 | string | User address, if available
address_line2 | string | User address, if available
city | string | name of city, if available
state | string | state name, if available
country | string | country name, if available
phone_country_code | string | international country code for phone, if available
phone_number | string | user's phone number, if available
zipcode | string | location's zipcode, if available
user_id | string | Unique identifier for user passed by merchant
redeemed_points | integer | Points redeemed by the user
available_points | integer | Points currently available to the user
awarded_points | integer | Total number of points that have been awarded to the user
pending_points | integer | Points pending for approval
loyalty_tier_id | string | ID of the loyalty tier currently assigned to the user
loyalty_tier_name | string | Name of the loyalty tier currently assigned to the user
tier_details | array | Details regarding the period of the tier and points earned during that period
expiration_schedule | array | Array of expiration schedules
referral_code | string | Referral code of the user
has_opted_out | boolean | Indicates whether the user has opted out of the loyalty program
referrer_email | string | Email address of the friend who has referred the current user
loyalty_enroll_time | string | Date (mm/dd/yyyy) when the user enrolled into the loyalty program

### Get User Redemptions

```shell
curl -X GET --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20"
--data "is_still_valid=true",
--data "order_by=allowed_redeem_points",
--data "count=10",
--data "start_index=0",
--data "fetch_eligible_redemptions=true"
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
  "count" : 10,
  "start_index" : 0,
  "fetch_eligible_redemptions" : true
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
        "redemption_type": "Percentage Discount",
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
        "redemption_type": "Fixed Amount Discount",
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
This will return information about the redemption options available to the user based on his tier if applicable. User's email ID will be used to search and return user specific information.


**HTTP Request**

`GET  https://api.zinrelo.com/v1/loyalty/users/{user_email}/redemptions`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
is_still_valid | boolean | No | Indicates whether only active (true) or all redemptions (false) should be fetched (defaults to true)
order_by | string | No | The order in which the redemption options will be fetched (defaults to updated_time)
count | integer | No | The number of redemptions to be fetched (defaults to 12)
start_index | integer | No | The start index to fetch the data from (defaults to 0)
fetch_eligible_redemptions | boolean | No | If passed as true, only those redemptions which the user can redeem based on his available points will be fetched (defaults to true)

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
total | integer | Total number of redemptions for the user 
redemptions | array | Array of redemptions based on time range, filters and start_index
more | boolean | Indicates if more number of redemptions are available (true/false)


### Get User Next Tier

```shell
curl -X GET --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20"
"https://api.zinrelo.com/v1/loyalty/users/bob@zinrelo.com/next_tier"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e', 
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

response = requests.get(url = "https://api.zinrelo.com/v1/loyalty/users/bob@zinrelo.com/next_tier",
                        headers = headers)
```


> The above command returns JSON structured like this:

```json
{
  "data": {
    "id": "zrl_platinum",
    "name": "Platinum",
    "description": "Platinum Tier",
    "minimum_points": 5000,
    "bonus_multiplier": 1,
    "tier_rewards": [
      "reward_cb5f8",
      "reward_d537f",
      "reward_0f28c"
    ],
    "is_active": true,
    "is_default": false,
    "hide_tier_from_user": false,
    "updated_time": "15-Jan-2019 05:06:10",
    "created_time": "17-Dec-2018 10:12:34"
  },
  "success": true
}
```
This will return information about the next tier available to the user based on his tier if applicable.

**HTTP Request**

`GET  https://api.zinrelo.com/v1/loyalty/users/{user_email}/next_tier`

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
tier_rewards | array | Reward IDs of the rewards available in the tier 


## Redemptions

### Get Redemptions

```shell
curl -X GET --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20"
--data "reward_ids=reward_cb5f8,reward_d537f,reward_0f28c"
"https://api.zinrelo.com/v1/loyalty/redemptions"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e', 
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

payload = json.dumps({
  "redemption_ids" : 'reward_cb5f8,reward_d537f,reward_0f28c'
})

response = requests.get(url = "https://api.zinrelo.com/v1/loyalty/redemptions",
                        headers = headers, data = payload)
```


> The above command returns JSON structured like this:

```json
{
    "data": [
        {
            "giftcard_description": "$15 Off Coupon for 1400 points",
            "redemption_name": "$15 OFF COUPON",
            "is_active": true,
            "associated_levels": [
                "all"
            ],
            "allowed_redeem_points": 1400,
            "redemption_type": "Fixed Amount Discount",
            "redemption_id": "reward_0f28c",
            "created_time": "17-Dec-2018 10:11:42",
            "coupon_expiry": null,
            "updated_time": "17-Dec-2018 11:02:11",
            "redemption_value": "15"
        },
        {
            "giftcard_description": "$10 Off Coupon for 1000 points",
            "redemption_name": "$10 OFF COUPON",
            "is_active": true,
            "associated_levels": [
                "all"
            ],
            "allowed_redeem_points": 1000,
            "redemption_type": "Fixed Amount Discount",
            "redemption_id": "reward_d537f",
            "created_time": "17-Dec-2018 10:11:42",
            "coupon_expiry": null,
            "updated_time": "17-Dec-2018 11:01:34",
            "redemption_value": "10"
        }
    ],
    "success": true
}


```
This API will return reward details of the reward ID passed or all the rewards if no ID is passed.

**HTTP Request**

`GET  https://api.zinrelo.com/v1/loyalty/redemptions`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
redemption_ids | string | No | Redemption IDs separated by comma eg. reward_se23,reward_304f

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
associated_levels | array | Tiers on which this redemption is available. 


## Tiers

### Get all Tiers

```shell
curl -X GET --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20"
"https://api.zinrelo.com/v1/loyalty/tiers"
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e', 
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}

response = requests.get(url = "https://api.zinrelo.com/v1/loyalty/tiers",
                        headers = headers)
```


> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "id": "zrl_platinum",
      "name": "Platinum",
      "description": "Platinum Tier",
      "minimum_points": 5000,
      "bonus_multiplier": 1,
      "tier_rewards": [
        "reward_cb5f8",
        "reward_d537f",
        "reward_0f28c"
      ],
      "rules": {
        "2.0x": "Purchase on website, Write a Review"
      },
      "is_active": true,
      "is_default": false,
      "hide_tier_from_user": false,
      "updated_time": "15-Jan-2019 05:06:10",
      "created_time": "17-Dec-2018 10:12:34"
    },
    {
      "id": "zrl_gold",
      "name": "Gold",
      "description": "1000 points",
      "minimum_points": 1000,
      "bonus_multiplier": 1,
      "tier_rewards": [
        "reward_cb5f8",
        "reward_d537f",
        "reward_0f28c"
      ],
      "rules": {
        "1.5x": "Purchase on website, Write a Review"
      },
      "is_active": true,
      "is_default": false,
      "hide_tier_from_user": false,
      "updated_time": "17-Dec-2018 10:12:37",
      "created_time": "17-Dec-2018 10:12:34"
    }
  ],
  "success": true
}
```
This will return information about the next tier available to the user based on his tier if applicable.

**HTTP Request**

`GET  https://api.zinrelo.com/v1/loyalty/tiers`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
tier_ids | string | No | Tier IDs separated by comma eg. silver, gold, platinum
hide_tier_from_user | boolean | No | Are the tiers being shown to the user or not
fetch_all | boolean | No | Fetch all the tiers for the program

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
tier_rewards | array | Reward IDs of the rewards available in the tier 
rules | map | Promotions/Rules running for the Tier


# Webhooks

## Introduction

A Webhook is simply an HTTP callback, an HTTP POST that occurs when something happens. Simply put, it is a simple event-notification via HTTP POST. Webhooks allow you to build or set up integrations, which subscribe to certain events on Zinrelo Loyalty Platform. When one of those events is triggered, we'll send an HTTP POST payload to the configured URL. Webhooks are aimed at enabling our merchants to extend the functionality provided by the Zinrelo’s Loyalty program by listening to the events and adding additional functionality like CRM integrations, email notifications and integrations with wallets/account credits etc.

## Settings
Webhook settings can be configured in the Notifications section of the Zinrelo store.

Sr.No. | Event Name | Description
-------| ---------- | --------
   1   | Enable Webhook | Enable/Disable Webhook.
   2   | Webhook URL | Endpoint where events will be posted.
   3   | Events to send Webhook notifications for | Events for which you want to receive notifications for. Currently available events are User Enrollment, Points Awarded, Points Redeemed, Points Deducted

![alt text](/images/webhook_settings_prod.png)

## Webhook processing

> For example if your webhook end point is setup at below url: 
*http://example.com/loyalty_webhook/ss_webhook*
The Webhook post request can be received in the following way:

```shell
#Please refer python tab for details
```

```python
import json
@restrict('POST')
def ss_webhook(self):
  try:
    body = json.loads(request.body)
    #handling code based on the event_type provided 
    i.e. body.event_type
  except Exception as e:
    #handle exception

```

> The structure of the webhook post request body is as follows:

```json
{
"id":"570265a77fae7c14c97a2b63",
"data": {..},
"event_type":"evt_points_awarded",
"created":"04-Apr-2016 13:01:27"
}
```
**Receiving a Webhook Notification**

Creating a Webhook on your server is simple. Webhook data is sent as JSON in the POST request body. All the details regarding the event are included in the JSON. You are good to go once you parse the JSON.

**Responding a webhook**

Based on the event type, the payload will contain the user data or the transaction data. In the case of events - User Enrollment the payload contains the user object. Whereas in case of events - Points Awarded, Points Redeemed, Points Deducted the payload will contain the loyalty transaction details.

Your endpoint should return 200 HTTP status code to acknowledge receipt of the event. Any code other than the 200 HTTP status code is considered as a failure, and the event will be fired again with 5 minute intervals until the 200 HTTP status code is received or the maximum number of retry attempts has reached (The default retry attempts limit is 5. Get in touch with your Customer Success Manager to configure the settings).

**Best Practices**

If your Webhook script performs complex logic, or makes network calls, it is possible that the request would timeout before Zinrelo sees the script’s complete execution. For this reason, you may want to have your Webhook endpoint immediately acknowledge receipt by returning a 200 HTTP status code and then perform the rest of its duties.

Webhook endpoints may occasionally receive the same event more than once. We advise you to guard against duplicated event receipts by logging the events you have processed against the Event ID and then not processing already-logged events

**Securities**

You may want to confirm that the data received at your Webhook URL is actually the data sent by Zinrelo before acting upon it. To do so, you can use the events API endpoint as described below:
Upon receiving a Webhook event follow the below steps:

  1. Parse the JSON data in request body and get the Event ID value.
  2. Use the Event ID value to get the data from Zinrelo using the [Events API endpoint](#events-api-endpoint).

## Events API Endpoint

```shell
curl -X GET --header "partner-id: cad458dc4e" 
--header "api-key: c921e097e6679d21c0cad26a45bfec20" 
"https://api.zinrelo.com/v1/events/570265a77fae7c14c97a2b63”
```

```python
import requests
import json

headers = {'partner-id': 'cad458dc4e', 
           'api-key': 'c921e097e6679d21c0cad26a45bfec20'}
response = requests.get(
        url = "https://api.zinrelo.com/v1/events/570265a77fae7c14c97a2b63",
        headers = headers)

```

> The response of the above command is:

```json
{
"data": {
        "id": "570265a77fae7c14c97a2b63",
        "data": {
              "user_last_name": "Pingle",
              "activity_id": "became_an_email_subscriber",
              "user_first_name": "Aditya",
              "user_id": "56263eac2d6f2a7715b3d2d1",
              "order_id": null,
              "points_status": "approved",
              "transaction_type": "award",
              "points_expiration_date": null,
              "qualified_points": true,
              "points": 6,
              "points_expired": 0,
              "created_time": "04-Apr-2016 13:01:27",
              "user_email": "aditya@shopsocially.com",
              "activity_name": "Became an Email Subscriber",
              "merchant_id": "9935740ac1fe23a1962de5cf4250c036",
              "id": "570265a7735f835a2ce12a15",
              "approved_time": "04-Apr-2016 13:01:27" 
              },
        "event_type": "evt_points_awarded", 
        "created": "04-Apr-2016 13:01:27"
 	      },
"success": true
}
```

This will get the Event data given the Event ID. The ‘data’ returned in the response of the API is identical to the Event data received via Webhook.

**HTTP Request**

`GET https://api.zinrelo.com/v1/loyalty/events/{id}`

`Response Body`

Parameter   | Type   |Description
------------|--------|------------
id          | String |Event ID
data        | Dictionary | Transaction Details\User deatils
event_type  | String | Event Type
created     | String | Event Created Time


## Event Data Format
When configuring a Webhook, you can choose the events you would like to receive payloads for. Subscribing only to the events, which you implement, will help in limiting the HTTP requests to your server. 

The events supported by Zinrelo Loyalty program are:

Sr.No.|   Event Name   |Description
------|----------------|----------------
1     |User Enrollment |When a new user enrolls into the loyalty program.
2     |Points Awarded  |When points are awarded (approved) to a user.
3     |Points Redeemed |When a user redeems points.
4     |Points Deducted |When points are deducted from a user’s loyalty points balance
5     |Tier Upgrade    |When a customer upgrades to a higher tier
6     |Tier Downgrade    |When a customer downgrades to a lower tier


> Example request body for a User Enrollment event:

```json
{"created": "28-Sep-2017 06:49:54",
  "data": 
      {"first_name": "Daena",
      "last_name":"Wille",
      "user_id": "59cc9b91418a580a512b450d",
      "uid": "daen1234",
      "dob": "",
      "loyalty_tier_id": "zrl_silver",
      "available_points": 100,
      "loyalty_tier_name": "Silver", 
      "referral_url": "https://arti55.zinrelo.com/ref/DAE4429", 
      "referral_code": "DAE4429", 
      "redeemed_points": 0,
      "awarded_points": 100,
      "user_email": "Daena1234@shopsocially.com",
      "pending_points": 0
      },
    "id": "59cc9b93418a58027716b78d", 
    "event_type": "evt_user_enrollment"
}

```

**User Enrollment**

User Enrollment event occurs whenever a new user enrolls into Zinrelo loyalty program.
On successfull user enrollment we make a call to your webhook URL with following request body: 


Parameter   | Type   |Description
------------|--------|------------
id          | String |Event ID
data        | Dictionary |User Details
event_type  | String | Event Type
created     | String | Event Created Time

> Example request body for a Points Awarded event:

```json
{  
  "id": "57026ce2c22f9f7b50bbdf6e",
  "data": {
          "activity_id": "became_an_email_subscriber",
          "user_first_name": "Aditya", 
          "order_id": null,
          "points_status": "approved",
          "points_expired": 0,
          "created_time": "04-Apr-2016 13:32:18",
          "merchant_id": "9935740ac1fe23a1962de5cf4250c036",
          "id": "57026ce2735f835a2ce12a33",
          "approved_time": "04-Apr-2016 13:32:18",
          "points_expiration_date": null,
          "user_id": "56263eac2d6f2a7715b3d2d1",
          "transaction_type": "award",
          "user_last_name": "Pingle",
          "points": 300,
          "qualified_points": true, 
          "activity_name": "Became an Email Subscriber",
          "user_email": "aditya@shopsocially.com"
          },
  "event_type": "evt_points_awarded",
  "created": "04-Apr-2016 13:32:18"
}
```



**Points Awarded**

Points Awarded event occurs whenever any user has been awarded loyalty points. In the case of the activities where moderation is required, the event occurs once the points have been approved.
On successfull points award event, we make a call to your webhook URL with following  request body: 


Parameter   | Type   |Description
------------|--------|------------
id          | String |Event ID
data        | Dictionary |Transaction Details
event_type  | String | Event Type
created     | String | Event Created Time


> Example request body for a Points Redeemed event:

```json
{  
  "id": "570265487fae7c14c77a2b63",
  "data": {
        "points_status": "redeemed", 
        "user_id": "56263eac2d6f2a7715b3d2d1",
        "redemption_name": "new_redemption 50",
        "user_first_name": "Aditya",
        "transaction_type": "redeem",
        "user_last_name": "Pingle",
        "coupon_code": "5",
        "points": 1,
        "redemption_id": "new_redemption_50",
        "created_time": "04-Apr-2016 12:59:52",
        "merchant_id": "9935740ac1fe23a1962de5cf4250c036",
        "id": "57026548735f835a2d16b01c",
        "user_email": "aditya@shopsocially.com"
        },
  "event_type": "evt_points_redeemed",
  "created": "04-Apr-2016 12:59:52"
}
```




**Points Redeemed**

Points Redeemed event occurs whenever a user redeems his loyalty points.
On successfull points redeemed event, we make a call to your webhook URL with following request body: 


Parameter   | Type   |Description
------------|--------|------------
id          | String |Event ID
data        | Dictionary |Transaction Details
event_type  | String | Event Type
created     | String | Event Created Time


> Example request body for a Points Deducted event:

```json
{
  "id": "57026857c22f9f7b50bbdf6c",
    "data": {
      "user_first_name": "Aditya",
      "user_id": "56263eac2d6f2a7715b3d2d1",
      "points_status": "deducted", 
      "transaction_type": "deduct",
      "user_last_name": "Pingle",
      "reason": "testing",
      "points": 30000,
      "points_deducted": 0,
      "created_time": "04-Apr-2016 13:12:55",
      "merchant_id": "9935740ac1fe23a1962de5cf4250c036",
      "id": "57026857735f835a2ce12a27",
      "user_email": "aditya@shopsocially.com"
    },
    "event_type": "evt_points_deducted",
    "created": "04-Apr-2016 13:12:55"
}
```



**Points Deducted**

Points Deducted event occurs whenever points are deducted from a user’s loyalty points balance. The points can be deducted from Zinrelo store or using API calls.
On successful points deduct event, we make a call to your webhook URL with following request body: 


Parameter   | Type   |Description
------------|--------|------------
id          | String |Event ID
data        | Dictionary |Transaction Details
event_type  | String | Event Type
created     | String | Event Created Time


> Example request body for a Tier Upgrade event:

```json
{
  "id": "57026857c22f9f7b50bbdf6c",
  "data": {
      "first_name": "Bob",
      "last_name": "Baker",
      "loyalty_tier_name": "Pearl",
      "uid": "76183",
      "user_id": "5982d90663fda91f6fb37564",
      "dob": "",
      "loyalty_tier_id": "zrl_pearl",
      "available_points": 1738,
      "referral_url": "https://storeforpreviewdahsboard.zinrelo.com/ref/BOB7091",
      "referral_code": "BOB7091",
      "redeemed_points": 0,
      "awarded_points": 1738,
      "user_email": "bob@xyz.com",
      "pending_points": 0
  },
  "event_type": "evt_level_upgrade",
  "created": "01-Apr-2016 12:26:08"
}
```



**Tier Upgrade**

The Tier upgrade event occurs whenever a customer earns enough points to move up to a higher tier. The tier upgrade is processed immediately and the webhook is triggered at the time of upgrade. On successful tier upgrade event, we make a call to your webhook URL with following request body: 


Parameter   | Type   |Description
------------|--------|------------
id          | String |Event ID
data        | Dictionary |Transaction Details
event_type  | String | Event Type
created     | String | Event Created Time


> Example request body for a Tier Downgrade event:

```json
{
  "id": "57026857c22f9f7b50bbdf6c",
  "data": {
      "first_name": "Bob",
      "last_name": "Baker",
      "loyalty_tier_name": "Pearl",
      "uid": "76183",
      "user_id": "5982d90663fda91f6fb37564",
      "dob": "",
      "loyalty_tier_id": "zrl_pearl",
      "available_points": 1738,
      "referral_url": "https://storeforpreviewdahsboard.zinrelo.com/ref/BOB7091",
      "referral_code": "BOB7091",
      "redeemed_points": 0,
      "awarded_points": 1738,
      "user_email": "bob@xyz.com",
      "pending_points": 0
  },
  "event_type": "evt_level_degrade",
  "created": "01-Apr-2016 12:26:08"
}
```



**Tier Downgrade**

The Tier downgrade event occurs whenever enough points are deducted from a customer's account resulting in downgrading to a lower tier. The tier downgrade is processed immediately and the webhook is triggered at the time of downgrade. On successful tier downgrade event, we make a call to your webhook URL with following request body: 


Parameter   | Type   |Description
------------|--------|------------
id          | String |Event ID
data        | Dictionary |Transaction Details
event_type  | String | Event Type
created     | String | Event Created Time


#JavaScript API


## Get User Info
This javascript function will get the details of the user from Zinrelo
Platform.

```javascript
zrl_mi.get_loyalty_user_info()

window.zrlmi_loyalty_user_info_success_handler = function(data){
     console.log(data)
     /***code to handle success response***/
}

window.zrlmi_loyalty_user_info_failure_handler = function(data){
     console.log(data)
     /***code to handle failure response***/
}
```

**Request**

`zrl_mi.get_loyalty_user_info()`

<aside class="notice">
Implement the JavaScript success and failure handler functions mentioned in JavaScript tab to get response from above function.
</aside>


>The above command returns JSON structured like this:


```json

{"available_points":19862,
"awarded_points":20200,
"dob":"07/22/1990",
"first_name":"RT",
"last_name":"406",
"loyalty_tier_id":"zrl_level_platinum",
"loyalty_tier_name":"Platinum",
"pending_points":0,
"redeemed_points":338,
"referral_code":"ART7993",
"referrer_email":"",
"ss_uid":"b986ff5cdf5eb5498ec492a9939b6628",
"success":true,
"uid":"arti 406",
"user_email":"arti@shopsocially.com"}
```

**Response Body**

Parameter        | Type   |Description
-----------------|--------|------------
awarded_points   | Integer| Total Points awarded to the user
dob              | String | Date of birth of the user
first_name       | String | First name of the user
last_name        | String | Last name of the user
loyalty_tier_id  | String | ID of loyalty tier currently assigned to user
loyalty_tier_name| String | Name of loyalty tier currently assigned to user
pending_points   | Integer| Points pending for approval
redeemed_points  | Integer| Points redeemed by the user
referral_code    | String | Current user's referral code
referrer_email   | String | Referrer's email address If the current user is referred by someone 
ss_uid           | String | Zinrelo internal user ID
uid              | String | Unique ID of user at merchant side
user_email       | String | Email of the user


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
616 | Invalid event ID
617 | Maximum award limit exceeded
618 | Points could not be awarded because exclusion rule was applied
619 | Multiple accounts found
620 | Invalid Product ID
621 | Maximum product quantity exceeded
622 | User record has been deleted
623 | User has been blocked
    
