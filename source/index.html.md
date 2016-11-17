---
title: ShopSocially - API Reference

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

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

payload = json.dumps({"user_email": <Users email ID>,
                      "points_passed": <number of points to be awarded>,
                      "activity_id": <activity ID>})

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

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

payload = json.dumps({"user_email": <Users email ID>,
                      "redemption_id": <redemption ID>})

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

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

payload = json.dumps({"user_email": <Users email ID>,
                      "reason": <reason>,
                      "points_passed": <points_passed>})

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

Work in Progress

## Users

Work in Progress

## Activities

Work in Progress

## Redemptions

Work in Progress

## Levels

Work in Progress

## Returns

Work in Progress

