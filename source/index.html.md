---
title: Closum API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
- shell

toc_footers:
- <a href='https://www.closum.com' target="_blank">Sign Up for Closum</a>
- <a href='mailto:hello@closum.com'>Documentation Related Issues</a>
- <a href='https://github.com/closum'>GitHub Repository</a>

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
curl -X POST \
  https://api.closum.com/api/token \
  -H 'Accept: application/json' \
  -H 'Cache-Control: no-cache' \
  -H 'Content-Type: application/json' \
  -d '{
	"api_username":"your-closum-api-username",
	"api_pw":"your-closum-api-pw"
   }'
```

> Make sure to replace `your-closum-api-username` with your Closum personal api_username and `your-closum-api-pw` with the corresponding personal api_pw.

> You can access and request `your-closum-api-username` and `your-closum-api-pw` inside [your personal account detail on Closum](https://www.closum.com/user/profile).

> The above command returns JSON structured like this:

```json
{
    "success": true,
    "data": {
        "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOjEsImV4cCI6MTUyNjA1OTI0M30.kwFyE1ipf78f6BJafnj0ED3m4wcYWAM70B0t065jUd0",
        "expires_in": 604800,
        "expiration_date": "2018-05-11T17:20:43+00:00"
    }
}
```

Closum uses Bearer Tokens to allow access to the API. Those are set with a lifetime as set on the field `"expires_in"` and you should refresh your Bearer Tokens within the returned `"expiration_date"`.

Closum expects for the Bearer Token to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer your.bearer.token`

<aside class="notice">
You must replace <code>your.bearer.token</code> with your personal Bearer Token.
</aside>

# Querystring Parameters

Our API comes with support for querystring parameters that you can use to manipulate the output produced by our API.

Parameter | Default | Description
--------- | ------- | -----------
limit | `25` | If set, `limit` parameter will manipulate the number of records returned.
page | `1` | If set, `page` parameter will retrieve records from another page.
sort | `id` | If set, `sort` parameter will specify which field should be used to sort the results produced.
direction | `asc` | This parameter **in combination** with the `sort` parameter to specify the direction in which results are sorted.

# Leads

## Lead Properties

Parameter | Type | Read Only | Description
--------- | --------- | --------- | -----------
ID | integer | True | The **id** relative to lead
name | string | False | The **name** relative to lead
city_id | integer | False | The **city_id** relative to lead
custom_data | string | False | A **JSON** formated string containing extra fields
creation_date | date-time | True | The **creation_date** of the record
phone | object | False | The **phone** object associated to lead
email | object | False | The **email** object associated to lead
lead_opt_in | object | False | The **lead_opt_in** object associated to lead

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
    "phone" : {"extension" : "001","number" : "517-388-1452"},
    "lead_opt_in" : {
    	"sms" : true,
    	"email" : true,
    	"enviroment" : {
    		"CONTENT_LENGTH" : "222",
    		"HTTP_USER_AGENT" : "Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:47.0) Gecko/20100101 Firefox/47.0",
    		"HTTP_REFERER" : "https://www.google.com",
    		"HTTP_ACCEPT_LANGUAGE" : "pt-PT,pt;q=0.8,en;q=0.5,en-US;q=0.3",
    		"REMOTE_ADDR" : "192.168.1.1",
    		"REMOTE_PORT" : "1364",
    		"REQUEST_URI" : "/",
    		"REQUEST_TIME_FLOAT" : "1547548827.898"
    	}
    }
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
phone | A **JSON** [phone object](#phones).
lead_opt_in | A **JSON** [lead_opt_in object](#lead-opt-in-properties)
city_id | integer | False | The **city_id** relative to lead
custom_data | string | False | A **JSON** formated string containing extra fields

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

<!-- ## Update a Lead

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
phone | A **JSON** phone object -->

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

# Contact Types

## Contact Types Properties

Parameter | Type | Read Only | Description
--------- | --------- | --------- | -----------
ID | integer | True | The **id** relative to phone
label | string | False | The **label** which the contacts will be associated with
creation_date | date-time | Yes | The **creation date** of the record

## List All Contact Types

```shell
curl -X GET \
https://api.closum.com/api/contact-type \
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
                  "label": "My Sample Contact Type",
                  "creation_date": "2018-01-01T10:00:00+00:00"
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

This endpoint retrieves all contact types created by your company.

### HTTP Request

`GET https://api.closum.com/api/contact-type`


## Create a Contact Type

```shell
curl -X POST \
curl -X POST \
https://api.closum.com/api/contact-type \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-type: application/json' \
-d '{
      "label": "My Sample Contact Type"
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

This endpoint allows you to add a new Contact Type.

### HTTP Request

`POST https://api.closum.com/api/contact-type`

### Request body
Body should be formated in (absolutely) correct JSON format

Parameter | Description
--------- | -----------
label | The **label** which the contacts will be associated with

## Retrieve a Contact Type

```shell
curl -X GET \
https://api.closum.com/api/contact-type/1 \
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
            "label": "My Sample Contact Type",
            "date_add": "2018-01-01T10:00:00+00:00"
      }
}
```

This endpoint retrieves a specific Contact Type.

### HTTP Request

`GET https://api.closum.com/api/contact-type/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of Contact Type to retrieve

## Update a Contact Type

```shell
curl -X PUT \
https://api.closum.com/api/contact-type/1 \
-H 'Accept: application/json' \
-H 'Authorization: Bearer your.bearer.token' \
-H 'Cache-Control: no-cache' \
-H 'Content-type: application/json' \
-d '{
      "label": "My Sample Contact Type (Updated)"
}'
```

> The above command returns JSON structured like this:

```json
{
      "success": true,
      "data": []
}
```

This endpoint updates a specific Contact Type.

### HTTP Request

`PUT https://api.closum.com/api/contact-type/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of Contact Type to update

### Request body
Body should be formated in (absolutely) correct JSON format

Parameter | Description
--------- | -----------
label | The **label** which the contacts will be associated with

# Lead Opt In

## Lead Opt In Properties

Parameter | Type | Read Only | Description
--------- | --------- | --------- | -----------
ID | integer | True | The **id** relative to lead opt in
lead_id | integer | True | The **lead_id** which the lead opt in belong to
sms | boolean | False | Consent **status** for **SMS** marketing
email | boolean | False | Consent **status** for **EMAIL** marketing
enviroment | string | False | A **JSON** [enviroment object](#enviroment-properties)
creation_date | date-time | True | The **creation_date** of the record

# Unqualified Lead Opt In

## Unqualified Lead Opt In Properties

Parameter | Type | Read Only | Description
--------- | --------- | --------- | -----------
ID | integer | True | The **id** relative to unqualified lead opt in
unqualified_lead_id | integer | True | The **lead_id** which the unqualified lead opt in belong to
sms | boolean | False | Consent **status** for **SMS** marketing
email | boolean | False | Consent **status** for **EMAIL** marketing
enviroment | string | False | A **JSON** [enviroment object](#enviroment-properties)
creation_date | date-time | True | The **creation_date** of the record


# Enviroment Properties

Parameter | Type | Read Only | Description
--------- | --------- | --------- | -----------
CONTENT_LENGTH | string | False | The **Content-Length** entity header is indicating the size of the entity-body, in bytes, sent to the recipient.
HTTP_USER_AGENT | string | False | The **User-Agent** containing network protocol peers to identify the application type, operating system, software vendor or software version of the requesting software user agent.
HTTP_REFERER | string | False | The **Referer** request header contains the address of the previous web page from which a link to the currently requested page was followed.
HTTP_ACCEPT_LANGUAGE | string | False | The **Accept-Language** request HTTP header advertises which languages the client is able to understand, and which locale variant is preferred.
REMOTE_ADDR | string | False | The **IP address** of the remote host.
REMOTE_PORT | string | False | The **Port** being used on the user's machine to communicate with the web server.
REQUEST_URI | string | False | The **URI** which was given in order to access this page.
REQUEST_TIME_FLOAT | string | False | The **timestamp** of the start of the request, with microsecond precision.
