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
The Activities API let you create an activity, update activity configurations, get an activity details and get list of all activities for a merchant.

### Get All Activities
```shell
curl -X GET --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
"https://api.shopsocially.com/v2/loyalty/activities"
```

```python
import requests
import json

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

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
				...{}
       ],
"success":true
}
```
This API can be used to view a list of all configured activities for which customers can earn loyalty points.

**HTTP Request**

`GET  https://api.shopsocially.com/v2/loyalty/activities`


**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
id| string | Internal id
activity_id| string | Unique id generated for each activity
activity_name | string | The name of activity
activity_description | string | The description about activity
activity_type | string | Whether the activity should award fixed number of points or use a multiplier(fixed/bonus_multiplier).
approval_method | string | User transaction approval method (immediate/manual/monthly).
bonus_multiplier | integer | Multiplication factor to award points instead of awarding fixed points 
created_time | string | The time when activity is created_time
is_active | string | Indicates if an activity is active or paused(True/False)
is_sale_activity | string | True, if activity is a sale activity
max_award_frequency | integer | The maximum number of times a user will be awarded points
max_award_frequency_interval | string | The time interval constraint to the max_award_frequency number(day, week, month, year, lifetime).
merchant_id | string | The id of the merchant
points_earning_url | string | Merchant's website page url where the activity will be configured.
point_per_activity | integer | Fixed number of points to be awarded for an activity
qualified_points | string | True, if activity is qualified for Tier upgrade(True/False)
show_activity_in_user_dashboard | string | True, if activity is to be shown in end user dashboard(True/False).
ss_internal | string | SS Activity(true/false)
updated_time | string | The time when activity configuration last modified


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

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

payload = json.dumps({"activity_id" : <Activity ID>,
	"activity_name" : <Activity Name>,
	"activity_description" : <Activity Description>,
	"activity_type" : <Activity type>,
	"max_award_frequency" : <Max Award Frequency>,
	"max_award_frequency_interval" : <Max Award Frequency Interval>,
	"is_active" = <Activity State>,
	"approval_method" = <Award Approval Method>,
	"points_per_activity" = <Points to be awarded>,
	"bonus_multiplier" = <Bonus Multiplier>,
	"is_sale_activity" = <Is sale activity>,
	"qualified_points" = <Is qualified for Tier change>,
	"show_activity_in_user_dashboard" = <Show in dashboard>
	})

response = requests.post(url = "https://api.shopsocially.com/v2/loyalty/activities",
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
				"ss_internal":true,
				"updated_time":"31-Aug-2016 05:10:21"
				},
"success":true
}
```
This API can be used to create a new activity which will earn loyalty points for customers.


**HTTP Request**

`POST  https://api.shopsocially.com/v2/loyalty/activities`

**Query Parameters**

Parameter | Type | Mandatory | Description
--------- | ---- | -------- | -----------
activity_id | string | Yes | The unique id of an activity.
activity_name | string | Yes | The name of the activity
activity_description | string | Yes | Description of the activity
activity_type | string | Yes | Whether the activity should award fixed number of points or use a multiplier.(fixed/bonus_multiplier)
max_award_frequency | integer | Yes| The maximum number of times a user will be awarded points
max_award_frequency_interval | string | Yes | The time interval constraint to the max_award_frequency number(day, week, month, year, lifetime).
bonus_multiplier | integer | No | Multiplication factor to award points instead of awarding fixed points(defaults to 1).
points_per_activity | integer | No | Fixed number of points to be awarded for an activity(defaults to 1).
points_earning_url | string | No | Merchant's website page url where the activity will be configured.
approval_method | string | Yes | User transaction approval method(immediate/manual/monthly).
is_active | string | Yes | Whether the activity should be active or paused(defaults to 1).
is_sale_activity | string | No | Whether the activity should be a sale activity(defaults to false).
qualified_points | string | No | Whether the activity should be qualified for Tier upgrade(defaults to true).
show_activity_in_user_dashboard | string | no | Whether the activity should be shown in user dashboard(defaults to true).

**Response Body**

Attribute | Type | Description
--------- | ---- | -----------
id| string | Internal id
merchant_id | string | The id of the merchant
activity_id| string | Unique id generated for each activity
activity_name | string | The name of activity
activity_description | string | The description of the activity
activity_type | string | Whether the activity should award fixed number of points or use a multiplier(fixed/bonus_multiplier).
approval_method | string | User transaction approval method (immediate/manual/monthly).
bonus_multiplier | integer | Multiplication factor to award points instead of awarding fixed points 
created_time | string | The time when activity is created_time
is_active | string | Indicates whether the activity is active or paused(True/False)
is_sale_activity | string | True if activity is a purchase activity
loyalty_activity_img | string | The image url of an activity
max_award_frequency | integer | The maximum number of times a user will be awarded points
max_award_frequency_interval | string | The time interval constraint to the max_award_frequency number(day, week, month, year, lifetime).
points_earning_url | string | Merchant's website page url where the activity will be configured.
point_per_activity | integer | Fixed number of points to be awarded for an activity
qualified_points | string | True, if activity is qualified for Tier upgrade(True/False)
show_activity_in_user_dashboard | string | True, if activity is to be shown in end user dashboard.
ss_internal | string | SS Activity(true/false)
updated_time | string | The time when activity configuration last modified


### Get Activity
```shell
curl -X GET --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
"https://api.shopsocially.com/v2/loyalty/activities/{activity_id}"
```

```python
import requests
import json

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

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
This API can be used to view details about any activity. The activity ID will be used to fetch this data.

**HTTP Request**

`GET  https://api.shopsocially.com/v2/loyalty/activities/{activity_id}`


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
is_active | string | Indicates if activity is active or paused(True/False)
is_sale_activity | string | True if activity is a purchase activity
loyalty_activity_img | string | The image url of an activity
max_award_frequency | integer | The maximum number of times a user will be awarded points
max_award_frequency_interval | string | The time interval constraint to the max_award_frequency number(day, week, month, year, lifetime).
merchant_id | string | The id of the merchant
points_earning_url | string | Merchant's website page url where the activity will be configured.
point_per_activity | integer | Fixed number of points to be awarded for an activity
qualified_points | string | True, if activity is qualified for Tier upgrade(True/False)
show_activity_in_user_dashboard | string | True, if activity is to be shown in end user dashboard.
ss_internal | string | SS Activity(true/false)
updated_time | string | The time when activity configuration last modified


### Update Activity
```shell
curl -X PUT --header "partner-id: <your-partner-id>" 
--header "api-key: <your-api-key>" 
"https://api.shopsocially.com/v2/loyalty/activities/{activity_id}"
```

```python
import requests
import json

headers = {'partner-id': <your-partner-id>, 
           'api-key': <your-api-key>}

response = requests.put(url = "https://api.shopsocially.com/v2/loyalty/activities/{activity_id}",
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
An activity configuration can be updated using this API by passing Activity ID

**HTTP Request**

`PUT  https://api.shopsocially.com/v2/loyalty/activities{activity_id}`


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
is_active | string | Indicates whether the activity is active or paused(True/False)
is_sale_activity | string | True if activity is a purchase activity
loyalty_activity_img | string | The image url of an activity
max_award_frequency | integer | The maximum number of times a user will be awarded points
max_award_frequency_interval | string | The time interval constraint to the max_award_frequency number(day, week, month, year, lifetime).
merchant_id | string | The id of the merchant
points_earning_url | string | Merchant's website page url where the activity will be configured.
point_per_activity | integer | Fixed number of points to be awarded for an activity
qualified_points | string | True, if activity is qualified for Tier upgrade(True/False)
show_activity_in_user_dashboard | string | True, if activity is to be shown in end user dashboard.
ss_internal | string | SS Activity(true/false)
updated_time | string | The time when activity configuration last modified


## Redemptions

Work in Progress

## Levels

Work in Progress

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
The Returns API can be used to deduct points of a user when the user returns an order.

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
