---
title: Closum API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
- shell

toc_footers:
- <a href='https://www.closum.com' target="_blank">Sign Up for Closum</a>
- <a href='mailto:hello@closum.com'>Documentation Related Issues</a>

includes:
- errors

search: true
---

# Introduction

Welcome to the Closum API! You can use our API to access Closum API endpoints, which can retrieve any data associated with your account.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To request your Bearer token, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.closum.com/api/token"
-H "Accept: application/json"
-H "Content-Type: application/json"
-X POST -d '{"email":"email@closum.com","password":"yourpassword"}'
```

> Make sure to replace `email@closum.com` with your Closum user email and `yourpassword` with the corresponding password.

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": {
            "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6Mn0.q2chPMiKRzwrO3v48fi90HyJPHDLOXtwEKr7EcU3GPk"
      }
}
```

Closum uses Bearer Tokens to allow access to the API.

Closum expects for the Bearer Token to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer your.bearer.token`

<aside class="notice">
You must replace <code>your.bearer.token</code> with your personal Bearer Token.
</aside>

# Querystring Parameters

Our API comes with support for querystring parameters that you can use to manipulate the output produced by our API.

Parameter | Default | Description
--------- | ------- | -----------
limit | 25 | If set, `limit` parameter will manipulate the number of records returned.
page | 1 | If set, `page` parameter will retrieve records from another page.
sort | `id` | If set, `sort` parameter will specify which field should be used to sort the results produced.
direction | `asc` | This parameter **in combination** with the `sort` parameter to specify the direction in which results are sorted.

# Leads

## Lead Properties

Parameter | Type | Read Only | Description
--------- | --------- | --------- | -----------
ID | integer | True | The **id** relative to lead
name | string | False | The **name** relative to lead
city_id | integer | False | The **city_id** relative to lead
custom_data | string | false | A **JSON** formated string containing extra fields
creation_date | date-time | True | The **creation_date** of the record
phone | object | False | The **phone** object associated to lead
email | object | False | The **email** object associated to lead

## List All Leads

```shell
curl -X GET \
https://api.closum.com/api/lead \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/json'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": [
            {
                  "id": 1,
                  "name": "Sylvia L. Hook",
                  "city_id": 40,
                  "custom_data": null,
                  "creation_date": "2018-01-01T10:00:00+00:00",
                  "phone": {
                        "extension" : "001",
                        "number": "517-388-1452"
                  },
                  "email": {
                        "email": "sylvialhook@closum.com"
                  }
            },
            {
                  ...
            },
      ],
      "pagination": {
            "page_count": 1001,
            "current_page": 1,
            "has_next_page": true,
            "has_prev_page": false,
            "count": 25025,
            "limit": null
      }
}
```

This endpoint retrieves all leads associated with your account.

### HTTP Request

`GET https://api.closum.com/api/lead`


## Create a New Lead

```shell
curl -X POST \
https://api.closum.com/api/lead \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/json' \
-d '{
      "name": "Sylvia L. Hook",
      "email" : {"email" : "sylvialhook@closum.com"},
      "phone" : {"extension" : "001","number" : "517-388-1452"}
}'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": {
            "id": 1
      }
}
```

This endpoint allows you to add a new Lead.

### HTTP Request

`POST https://api.closum.com/api/lead`

### Request body
Body should be formated in (absolutely) correct JSON format

Parameter | Description
--------- | -----------
name | The **name** of lead.
email | A **JSON** [email object](#email-properties)
phone | A **JSON** phone object

## Retrieve a Lead

```shell
curl -X GET \
https://api.closum.com/api/lead/1 \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/json'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": {
            "id": 1,
            "name": "Sylvia L. Hook",
            "creation_date": "2015-12-30T19:31:14+00:00",
            "city_id": 1,
            "custom_data": null,
            "email": {
                  "id" : 1,
                  "email": "sylvialhook@closum.com"
            },
            "phone": {
                  "id": 1,
                  "extension" : "001",
                  "number": "517-388-1452"
            }
      }
}
```

This endpoint retrieves a specific Lead.

### HTTP Request

`GET https://api.closum.com/api/lead/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Lead to retrieve

## Update a Lead

```shell
curl -X PUT \
https://api.closum.com/api/lead/1 \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/json' \
-d '{
      "name": "Sylvia L. Hook",
      "city_id": 1,
      "custom_data": null,
      "email": {
            "id" : 1,
            "email": "sylvialhook@closum.com"
      },
      "phone": {
            "id": 1,
            "extension" : "001",
            "number": "517-388-1452"
      }
}'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": []
}
```

This endpoint updates a specific Lead.

### HTTP Request

`PUT https://api.closum.com/api/lead/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Lead to update

### Request body
Body should be formated in (absolutely) correct JSON format

Parameter | Description
--------- | -----------
name | The **name** of lead.
email | A **JSON** email object
phone | A **JSON** phone object

# Cities

## List All Cities

```shell
curl -X GET \
https://api.closum.com/api/city \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/json'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": [
            {
                  "id": 213,
                  "city_name": "Abrantes"
            },
            {
                  ...
            }
      ],
      "pagination": {
            "page_count": 13,
            "current_page": 1,
            "has_next_page": true,
            "has_prev_page": false,
            "count": 308,
            "limit": null
      }
}
```

This endpoint retrieves all leads associated with your account.

### HTTP Request

`GET https://api.closum.com/api/lead`

# Emails

## Email Properties

Parameter | Type | Read Only | Description
--------- | --------- | --------- | -----------
ID | integer | True | The **id** relative to email
email | string | False | The **email** address to insert
status | integer | Yes | The activation **status** relative to email
register_date | date-time | Yes | The **register_date** of the record

## List All Emails

```shell
curl -X GET \
https://api.closum.com/api/email \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/json'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": [
            {
                  "id": 1,
                  "email": "sylvialhook@closum.com",
                  "status": 1,
                  "register_date": "2018-01-01T01:01:01+00:00"
            },
            {
                  ...
            }
      ],
      "pagination": {
            "page_count": 100,
            "current_page": 1,
            "has_next_page": true,
            "has_prev_page": false,
            "count": 2500,
            "limit": null
      }
}
```

This endpoint retrieves all emails associated with your account.

### HTTP Request

`GET https://api.closum.com/api/email`


## Create a New Email

```shell
curl -X POST \
https://api.closum.com/api/email \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/json' \
-d '{
      "email" : "sylvialhook@closum.com"
}'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": {
            "id": 1
      }
}
```

This endpoint allows you to add a new Email.

### HTTP Request

`POST https://api.closum.com/api/email`

### Request body
Body should be formated in (absolutely) correct JSON format

Parameter | Description
--------- | -----------
email | The **email** address to insert

## Retrieve an Email

```shell
curl -X GET \
https://api.closum.com/api/email/1 \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/json'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": {
            "id": 1,
            "email": "sylvialhook@closum.com",
            "status": 1,
            "register_date": "2018-01-01T01:01:01+00:00"
      }
}
```

This endpoint retrieves a specific Email.

### HTTP Request

`GET https://api.closum.com/api/email/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Email to retrieve

## Update an Email

```shell
curl -X PUT \
https://api.closum.com/api/email/1 \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/json' \
-d '{
      "id" : 1,
      "email": "update.sylvialhook@closum.com"
}'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": []
}
```

This endpoint updates a specific Email.

### HTTP Request

`PUT https://api.closum.com/api/email/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Email to update

### Request body
Body should be formated in (absolutely) correct JSON format

Parameter | Description
--------- | -----------
email | The new **email** address

# Phones

## Phone Properties

Parameter | Type | Read Only | Description
--------- | --------- | --------- | -----------
ID | integer | True | The **id** relative to phone
extension | string | False | The phone **extension** associated with country
number | string | False | The **phone** number
is_valid | boolean | Yes | The phone validation **status**
date_add | date-time | Yes | The **register_date** of the record

## List All Phones

```shell
curl -X GET \
https://api.closum.com/api/phone \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-type: application/json'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": [
            {
                  "id": 1,
                  "extension": "001",
                  "number": "517-388-1452",
                  "is_valid": 1,
                  "date_add": "2018-01-01T10:00:00+00:00"
            },
            {
                  ...
            }
      ],
      "pagination": {
            "page_count": 10,
            "current_page": 1,
            "has_next_page": true,
            "has_prev_page": false,
            "count": 250,
            "limit": null
      }
}
```

This endpoint retrieves all phones associated with your account.

### HTTP Request

`GET https://api.closum.com/api/phone`


## Create a New Phone

```shell
curl -X POST \
curl -X POST \
https://api.closum.com/api/phone \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-type: application/json' \
-d '{
      "extension": "001",
      "number": "517-388-1452"
}'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": {
            "id": 1
      }
}
```

This endpoint allows you to add a new Phone.

### HTTP Request

`POST https://api.closum.com/api/phone`

### Request body
Body should be formated in (absolutely) correct JSON format

Parameter | Description
--------- | -----------
extension | The phone **extension** associated with country
number | The **phone** number

## Retrieve a Phone

```shell
curl -X GET \
https://api.closum.com/api/phone/1 \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-type: application/json'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": {
            "id": 1,
            "extension": "001",
            "number": "517-388-1452",
            "is_valid": 1,
            "date_add": "2018-01-01T10:00:00+00:00"
      }
}
```

This endpoint retrieves a specific Phone.

### HTTP Request

`GET https://api.closum.com/api/phone/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of Phone to retrieve

## Update a Phone

```shell
curl -X PUT \
https://api.closum.com/api/phone/1 \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-type: application/json' \
-d '{
      "id" : 1,
      "extension": "001",
      "number": "517-388-1452"
}'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": []
}
```

This endpoint updates a specific Phone.

### HTTP Request

`PUT https://api.closum.com/api/phone/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of Phone to update

### Request body
Body should be formated in (absolutely) correct JSON format

Parameter | Description
--------- | -----------
extension | The phone **extension** associated with country
number | The **phone** number
