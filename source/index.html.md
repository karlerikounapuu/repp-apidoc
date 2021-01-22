---

title: Estonian Internet Foundation's REST EPP

language_tabs:
   - shell

toc_footers:
   - <a href='https://internet.ee'>Estonian Internet Foundation</a>
   - <a href='https://github.com/internetee/registry'>Registry project</a>
includes:
   - errors

search: true
code_clipboard: true
---

# Introduction

Welcome to the Estonian Internet Foundation's REST EPP (REPP for short) API documentation! You can use our API endpoints, which can mimic almost the full functionality of our EPP protocol.

We have language bindings in Shell. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This API project is only available for our active registrars. You can find more information about becoming registrar at our [wiki page](https://www.internet.ee/registrars/terms-and-conditions-for-becoming-a-registrar).

# Authentication

REPP uses API keys and certificates to allow access to our API. Certificates are the same ones that are used for EPP protocol.

API key, in a nutshell, is a Basic authorization key that consists of ApiUser's username and password, separated by colon, in Base64 format.

For example if your username is `test` and password `test123`, , you have to encode `test:test123` to Base64 and put it into `Authorization` header of each request.

For example:

`Authorization: Basic dGVzdDp0ZXN0MTIz`

<aside class="notice">
You must replace <code>dGVzdDp0ZXN0MTIz</code> with your personal API key.
</aside>


# Poll messages

## Get latest unread poll message

```shell
curl --location --request GET 'https://registry.test/repp/v1/registrar/notifications' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--data-raw ''
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "id": 1399,
        "text": "Registrant rejected domain update: biz.ee",
        "attached_obj_type": "Epp::Domain",
        "attached_obj_id": 34
    }
}
```

Get the latest unread poll message

### HTTP Request

`GET /repp/v1/registrar/notifications`

## Mark poll message as read

```shell
curl --location --request PUT 'https://registry.test/repp/v1/registrar/notifications/1398' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "notification": {
        "read": true
    }
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "notification_id": 1398,
        "read": true
    }
}
```

Mark poll message as read

### HTTP Request

`PUT /repp/v1/registrar/notifications/:notification_id`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
notification_id | None | Notification ID

### Payload Parameters

Parameter | Default | Description
--------- | ------- | -----------
read | None | Set to true to mark as read

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a specific poll message

```shell
curl --location --request GET 'https://registry.test/repp/v1/registrar/notifications/1398' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--data-raw ''
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "id": 1398,
        "text": "Contact ATSAA:57C8A9CB has been updated by registrant",
        "attached_obj_type": null,
        "attached_obj_id": null
    }
}
```

Get a specific poll message

### HTTP Request

`PUT /repp/v1/registrar/notifications/:notification_id`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
notification_id | None | Notification ID

# Contacts

## Get all contacts

```shell
curl --location --request GET 'https://registry.test/repp/v1/contacts' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--data-raw ''
```

> The above command returns JSON structured like this:

```json
{
    "contacts": [
        "ATSAA:15554596",
        "ATSAA:5767A313",
        "ATSAA:634CF14B",
    ],
    "total_number_of_records": 3
}
```

Get all contacts

### HTTP Request

`GET /repp/v1/registrar/contacts`

## Get a specific contact

```shell
curl --location --request GET 'https://registry.test/repp/v1/contacts/ATSAA:KARL' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--data-raw ''
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "id": "ATSAA:KARL",
        "name": "John Doe",
        "ident": {
            "code": "12345678901",
            "type": "priv",
            "country_code": "EE"
        },
        "email": "xxx@xxx.ee",
        "phone": "+xxx",
        "fax": null,
        "auth_info": "ed33c67bedb32b1567b131",
        "statuses": [
            "ok",
            "linked"
        ],
        "disclosed_attributes": []
    }
}
```

Get a specific contact

### HTTP Request

`GET /repp/v1/contacts/:contact_id`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
contact_id | None | Contact ID





















## ***POST***

**Description:**
<p>Renew domain</p>


### HTTP Request
`***POST*** /repp/v1/domains/{domain_name}/renew`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| domain_name | path |  | Yes | number |
| body | body |  | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

# /REPP/V1/DOMAINS/{DOMAIN_NAME}/DNSSEC
## ***GET***

**Description:**
<p>View specific domain&#39;s DNSSEC keys</p>


### HTTP Request
`***GET*** /repp/v1/domains/{domain_name}/dnssec`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| domain_name | path |  | Yes | number |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

## ***POST***

**Description:**
<p>Create a new DNSSEC key(s) for domain</p>


### HTTP Request
`***POST*** /repp/v1/domains/{domain_name}/dnssec`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| domain_name | path |  | Yes | number |
| body | body |  | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

## ***DELETE***

**Description:**

### HTTP Request
`***DELETE*** /repp/v1/domains/{domain_name}/dnssec`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| domain_name | path |  | Yes | number |
| body | body |  | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

# /REPP/V1/CONTACTS
## ***GET***

**Description:**
<p>Get all existing contacts</p>


### HTTP Request
`***GET*** /repp/v1/contacts`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

## ***POST***

**Description:**
<p>Create a new contact</p>


### HTTP Request
`***POST*** /repp/v1/contacts`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

# /REPP/V1/CONTACTS/{CONTACT_CODE}
## ***DELETE***

**Description:**
<p>Delete a specific contact</p>


### HTTP Request
`***DELETE*** /repp/v1/contacts/{contact_code}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| contact_code | path |  | Yes | number |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

## ***GET***

**Description:**
<p>Get a specific contact</p>


### HTTP Request
`***GET*** /repp/v1/contacts/{contact_code}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| contact_code | path |  | Yes | number |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

## ***PUT***

**Description:**
<p>Update existing contact</p>


### HTTP Request
`***PUT*** /repp/v1/contacts/{contact_code}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| contact_code | path |  | Yes | number |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

# /REPP/V1/CONTACTS/CHECK/{CONTACT_CODE}
## ***GET***

**Description:**
<p>Check contact code availability</p>


### HTTP Request
`***GET*** /repp/v1/contacts/check/{contact_code}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| contact_code | path |  | Yes | number |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

# /REPP/V1/DOMAINS/{DOMAIN_NAME}/TRANSFER
## ***POST***

**Description:**
<p>Transfer a specific domain</p>


### HTTP Request
`***POST*** /repp/v1/domains/{domain_name}/transfer`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| domain_name | path |  | Yes | number |
| body | body |  | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

# /REPP/V1/DOMAINS/{DOMAIN_NAME}/NAMESERVERS
## ***POST***

**Description:**
<p>Create new nameserver for domain</p>


### HTTP Request
`***POST*** /repp/v1/domains/{domain_name}/nameservers`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| domain_name | path |  | Yes | number |
| body | body |  | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

# /REPP/V1/DOMAINS/{DOMAIN}/NAMESERVERS/{NAMESERVER}
## ***DELETE***

**Description:**
<p>Delete specific nameserver from domain</p>


### HTTP Request
`***DELETE*** /repp/v1/domains/{domain}/nameservers/{nameserver}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| nameserver | path |  | Yes | number |
| domain | path |  | Yes | number |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |


## ***PUT***

**Description:**
<p>Mark poll message as read</p>


### HTTP Request
`***PUT*** /repp/v1/registrar/notifications`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| body | body |  | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

# /REPP/V1/REGISTRAR/NOTIFICATIONS/{NOTIFICATION_ID}
## ***GET***

**Description:**
<p>Get a specific poll message</p>


### HTTP Request
`***GET*** /repp/v1/registrar/notifications/{notification_id}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| notification_id | path |  | Yes | number |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

# /REPP/V1/DOMAINS
## ***GET***

**Description:**
<p>Get all existing domains</p>


### HTTP Request
`***GET*** /repp/v1/domains`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

## ***POST***

**Description:**
<p>Create a new domain</p>


### HTTP Request
`***POST*** /repp/v1/domains`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| body | body |  | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Successful domain registration response |

# /REPP/V1/DOMAINS/{DOMAIN_NAME}
## ***GET***

**Description:**
<p>Get a specific domain</p>


### HTTP Request
`***GET*** /repp/v1/domains/{domain_name}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| domain_name | path |  | Yes | number |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

## ***PUT***

**Description:**
<p>Update existing domain</p>


### HTTP Request
`***PUT*** /repp/v1/domains/{domain_name}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| domain_name | path |  | Yes | number |
| body | body |  | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

# /REPP/V1/DOMAINS/{DOMAIN_NAME}/TRANSFER_INFO
## ***GET***

**Description:**
<p>Retrieve specific domain&#39;s transfer info</p>


### HTTP Request
`***GET*** /repp/v1/domains/{domain_name}/transfer_info`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| domain_name | path |  | Yes | number |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

# /REPP/V1/DOMAINS/TRANSFER
## ***POST***

**Description:**
<p>Transfer multiple domains</p>


### HTTP Request
`***POST*** /repp/v1/domains/transfer`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

# /REPP/V1/ACCOUNTS/BALANCE
## ***GET***

**Description:**
<p>Get account&#39;s balance</p>


### HTTP Request
`***GET*** /repp/v1/accounts/balance`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | ok |

<!-- Converted with the swagger-to-slate https://github.com/lavkumarv/swagger-to-slate -->
