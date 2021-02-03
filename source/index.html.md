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

This production API project is only available for our active registrars. You can find more information about becoming registrar at our [wiki page](https://www.internet.ee/registrars/terms-and-conditions-for-becoming-a-registrar).

<aside class="success">
Remember — This is a sneak-peek of our REPP service, which isn't in production yet. For now, please rely on standard EPP implementation.
</aside>

# Environments

Production endpoint: https://registrar.internet.ee/repp/v1/

Sandbox endpoint: https://testrar.internet.ee/repp/v1/

Production environment is only available for our active registrars.

Access to our sandbox environment is not publicly available. We can grant access to third parties upon request.

# Authentication

REPP uses API keys and certificates to allow access to our API. Certificates are the same ones that are used for EPP protocol.

API key, in a nutshell, is a Basic authorization key that consists of ApiUser's username and password, separated by colon, in Base64 format.

For example if your username is `test` and password `test123`, , you have to encode `test:test123` to Base64 and put it into `Authorization` header of each request.

For example:

`Authorization: Basic dGVzdDp0ZXN0MTIz`

<aside class="notice">
You must replace <code>dGVzdDp0ZXN0MTIz</code> with your personal API key.
</aside>

# Billing

## Get account balance

```shell
curl --location --request GET 'https://testrar.internet.ee/repp/v1/accounts/balance' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--data-raw ''
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "balance": "420.0",
        "currency": "EUR"
    }
}
```

Get account balance

### HTTP Request

`GET /repp/v1/accounts/balance`

# Poll messages

## Get latest unread poll message

```shell
curl --location --request GET 'https://testrar.internet.ee/repp/v1/registrar/notifications' \
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
        "text": "Registrant rejected domain update: koer.ee",
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
curl --location --request PUT 'https://testrar.internet.ee/repp/v1/registrar/notifications/1398' \
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

Marks unread poll message as read

### HTTP Request

`PUT /repp/v1/registrar/notifications/:unread_notification_id`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
notification_id | Yes | Unread notification ID

### Payload Parameters

Parameter | Required | Description
--------- | ------- | -----------
read | Yes | Allows only value 'true'

## Get a specific poll message

```shell
curl --location --request GET 'https://testrar.internet.ee/repp/v1/registrar/notifications/1398' \
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

Parameter | Required | Description
--------- | ------- | -----------
notification_id | Yes | Notification ID

# Contacts

## Get all contacts

```shell
curl --location --request GET 'https://testrar.internet.ee/repp/v1/contacts' \
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

`GET /repp/v1/contacts`

## Get a specific contact

```shell
curl --location --request GET 'https://testrar.internet.ee/repp/v1/contacts/ATSAA:KARL' \
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

Parameter | Required | Description
--------- | ------- | -----------
contact_id | Yes | Contact ID

## Delete a specific contact

```shell
curl --location --request DELETE 'https://testrar.internet.ee/repp/v1/contacts/ATSAA:KARL' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--data-raw ''
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {}
}
```

> In case contact can not be destroyed, it returns a JSON structured like this:

```json
{
    "code": 2305,
    "message": "Object association prohibits operation [domains]",
    "data": {}
}
```

Delete a specific contact

### HTTP Request

`DELETE /repp/v1/contacts/:contact_id`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
contact_id | Yes | Contact ID

## Create a new contact

```shell
curl --location --request POST 'https://testrar.internet.ee/repp/v1/contacts' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "contact": {
        "name": "John Doe",
        "email": "john@doe.com",
        "phone": "+372.xxx",
        "ident": {
            "ident": "xxx",
            "ident_type": "priv",
            "ident_country_code": "EE"
        }
    }
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "contact": {
            "id": "ATSAA:84F23FD1"
        }
    }
}
```

Create a new contact

### HTTP Request

`POST /repp/v1/contacts`

### Payload Parameters

Parameter | Required | Description
--------- | ------- | -----------
id | No | Custom contact id for contact
name | Yes | Full name of contact
email | Yes | Email address of contact
phone | Yes | phone number of contact, with '.' as CC separator
ident[ident] | Yes | Identity / Registry code of contact
ident[ident_type] | Yes | Type of ident number (priv/org/birthday)
ident[ident_country_code] | Yes | Nationality of contact in alpha-2 format

<aside class="notice">
Note — Wrap your payload attributes into "contact" object, as shown in example.
</aside>

## Update an existing contact

```shell
curl --location --request PUT 'https://testrar.internet.ee/repp/v1/contacts/ATSAA:84F23FD1' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "contact": {
        "name": "John Doee",
        "email": "john@doee.com",
        "phone": "+372.590146111"
    }
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "contact": {
            "id": "ATSAA:84F23FD1"
        }
    }
}
```

Update an existing contact

### HTTP Request

`PUT /repp/v1/contacts/:contact_id`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
contact_id | Yes | Contact ID

### Payload Parameters

Parameter | Required | Description
--------- | ------- | -----------
name | No | Full name of contact
email | No | Email address of contact
phone | No | phone number of contact, with '.' as CC separator

<aside class="notice">
Note — Wrap your payload attributes into "contact" object, as shown in example.
</aside>

## Check contact code availability

```shell
curl --location --request GET 'https://testrar.internet.ee/repp/v1/contacts/check/PUDERJAKAPSAS' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--data-raw ''
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "contact": {
            "id": "PUDERJAKAPSAS",
            "available": true
        }
    }
}
```

Checks whether the contact code is available for custom use

### HTTP Request

`GET /repp/v1/contacts/check/:contact_id`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
contact_id | Yes | Contact ID to check

# Domains

## Get all existing domains

```shell
curl --location --request GET 'https://testrar.internet.ee/repp/v1/domains' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--data-raw ''
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domains": [
            "kass.ee",
            "koer.ee",
        ],
        "total_number_of_records": 69
    }
}
```

> When ?details=true is appended, it returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domains": [
            {
                "id": 7,
                "name": "kass.ee",
                "registrar_id": 2,
                "valid_to": "2022-09-23T00:00:00.000+03:00",
                "registrant_id": 12,
                "created_at": "2020-09-22T14:16:47.420+03:00",
                "updated_at": "2020-11-18T10:59:06.344+02:00",
                "name_dirty": "kass.ee",
                "name_puny": "kass.ee",
                "period": 1,
                "period_unit": "y",
                "creator_str": "2-ApiUser: test",
                "updator_str": "job - DomainUpdateConfirmJob - confirmed by email link, user not authenticated",
                "outzone_at": null,
                "delete_date": null,
                "registrant_verification_asked_at": "2020-11-18T10:59:05.240+02:00",
                "registrant_verification_token": "6edd80c248db682a0c2b2e02877cdfc528243a59942fe05fae164848a5df9c5e4aa2a9875b1cf42c64b4",
                "pending_json": null,
                "force_delete_date": null,
                "statuses": [
                    "serverRenewProhibited",
                    "pendingUpdate"
                ],
                "status_notes": {
                    "ok": "",
                    "pendingDelete": "",
                    "pendingUpdate": "",
                    "serverRenewProhibited": ""
                },
                "upid": 2,
                "up_date": "2020-11-13T14:41:58.613+02:00",
                "uuid": "6b6affa7-1449-4bd8-acf5-8b4752406705",
                "locked_by_registrant_at": null,
                "force_delete_start": null,
                "force_delete_data": null,
                "auth_info": "367b1e6d1f0d9aa190971ad8f571cd4d",
                "valid_from": "2020-09-22T14:16:47.420+03:00"
            },
            {
                "id": 19,
                "name": "koer.ee",
                "registrar_id": 2,
                "valid_to": "2025-12-08T00:00:00.000+02:00",
                "registrant_id": 12,
                "created_at": "2020-12-07T12:29:04.099+02:00",
                "updated_at": "2021-01-05T15:36:35.342+02:00",
                "name_dirty": "koer.ee",
                "name_puny": "koer.ee",
                "period": 1,
                "period_unit": "y",
                "creator_str": "2-ApiUser: test",
                "updator_str": "2-ApiUser: test",
                "outzone_at": null,
                "delete_date": null,
                "registrant_verification_asked_at": null,
                "registrant_verification_token": null,
                "pending_json": {},
                "force_delete_date": null,
                "statuses": [
                    "ok"
                ],
                "status_notes": {},
                "upid": 2,
                "up_date": "2021-01-05T15:36:35.080+02:00",
                "uuid": "b43635ca-46c6-4af6-a852-fdb13dd69b7f",
                "locked_by_registrant_at": null,
                "force_delete_start": null,
                "force_delete_data": null,
                "auth_info": "8b8f673ed4ac1fbc0147c75eb0e6aaa4",
                "valid_from": "2020-12-07T12:29:04.099+02:00"
            }
        ],
        "total_number_of_records": 69
    }
}
```

Gets all existing domains under current registrar account.

### HTTP Request

`GET /repp/v1/domains`

### URL Parameters

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
limit | No | 200 | How many objects to return
offset | No | 0 | Object query offset
details | No | | false | Show full object each domain

## Get a specific domain

```shell
curl --location --request GET 'https://testrar.internet.ee/repp/v1/domains/biz.ee' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--data-raw ''
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domain": {
            "id": "cd1be00a-58bb-4bee-9b5c-279e90bb5e24",
            "name": "biz.ee",
            "registrar": {
                "name": "20 ALLA OU",
                "website": "https://20alla.ee"
            },
            "registered_at": "2021-01-15T12:07:20.079+02:00",
            "valid_to": "2023-01-16T00:00:00.000+02:00",
            "created_at": "2021-01-15T12:07:20.079+02:00",
            "updated_at": "2021-01-21T16:41:59.707+02:00",
            "registrant": {
                "name": "xxx",
                "id": "ca79c932-41ff-4d2e-8479-f0e58275e077"
            },
            "tech_contacts": [
                {
                    "name": "xxx",
                    "id": "020417df-cf4c-43ea-a454-9e300c504c61",
                    "email": "xxx"
                }
            ],
            "admin_contacts": [
                {
                    "name": "xxx",
                    "id": "20417232-59a0-4dd3-b5db-243414724c0d",
                    "email": "xxx"
                }
            ],
            "transfer_code": "2f81ec671b69a2aa5d6375631e259ae7",
            "name_dirty": "biz.ee",
            "name_puny": "biz.ee",
            "period": 1,
            "period_unit": "y",
            "creator_str": "2-ApiUser: test",
            "updator_str": "test",
            "outzone_at": null,
            "delete_date": null,
            "registrant_verification_asked_at": null,
            "registrant_verification_token": null,
            "pending_json": {},
            "force_delete_date": null,
            "statuses": [
                "inactive"
            ],
            "locked_by_registrant_at": null,
            "status_notes": {
                "pendingDelete": "",
                "pendingUpdate": ""
            },
            "nameservers": [],
            "dnssec_keys": [],
            "dnssec_changed_at": null
        }
    }
}
```

Gets a specific domain object

### HTTP Request

`GET /repp/v1/domains/:domain_name`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
domain_name | No | Domain name

## Register a new domain

```shell
curl --location --request POST 'https://testrar.internet.ee/repp/v1/domains' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "domain": {
        "name": "kasskoer.ee",
        "registrant_id": "ATSAA:KARL",
        "period": 1,
        "period_unit": "y",
        "admin_domain_contacts_attributes": [
            "ATSAA:KARL"
        ],
        "tech_domain_contacts_attributes": [
            "ATSAA:KARL"
        ],
        "nameservers_attributes": [
            {
                "hostname": "ns1.kreative.ee"
            }
        ]
    }
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domain": {
            "name": "kasskoer.ee"
        }
    }
}
```

Register a new domain

### HTTP Request

`POST /repp/v1/domains`

### Payload Parameters

Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
name | Yes | String | Domain name
registrant_id | Yes | String | Registrant contact code
period_unit | Yes | String | Period unit. Can be year (y) or month (m)
period | Yes | Integer | For how many period units to register domain
admin_domain_contact_attributes | No | Array | Array of admin domain contact codes
tech_domain_contact_attributes | No | Array | Array of tech domain contact codes
nameservers_attributes | No | Array | Array of nameserver objects
nameservers_attributes[hostname] | Yes | String | Hostname of nameserver
nameservers_attributes[ipv4] | Yes | Array | Array of IPv4 attributes
nameservers_attributes[ipv6] | Yes | Array | Array of IPv6 attributes
dnskeys_attributes | No | Array | Array of DNSSEC key attributes
dnskeys_attributes[flags] | Yes | String | 256 (KSK) or 257 (ZSK)
dnskeys_attributes[protocol] | Yes | String | Key protocol (3)
dnskeys_attributes[alg] | Yes | String | DNSSEC key algorithm (3,5,6,7,8,10,13,14)
dnskeys_attributes[public_key] | Yes | String | DNSSEC public key

<aside class="notice">
Note — Wrap your payload attributes into "domain" object, as shown in example.
</aside>

## Change domain registrant / Auth code

```shell
curl --location --request PUT 'https://testrar.internet.ee/repp/v1/domains/kasskoer.ee' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "domain": {
        "registrant": {
            "code": "ATSAA:LIZ"
        },
        "auth_info": "123"
    }
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domain": {
            "name": "kasskoer.ee"
        }
    }
}
```

Changes domain registrant or auth code

### HTTP Request

`PUT /repp/v1/domains/:domain_name`

### URL Parameters
Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
domain_name | Yes | String | Domain


### Payload Parameters

Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
registrant | Yes | Object | New registrant object
registrant[code] | Yes | String | Contact ID of new registrant
registrant[verified] | No | Boolean | Defaults to false. Include it only if set to true.
auth_info | No | String | New EPP authorization code for domain

<aside class="notice">
Note — Wrap your payload attributes into "domain" object, as shown in example.
</aside>

## Renew a specific domain

```shell
curl --location --request POST 'https://testrar.internet.ee/repp/v1/domains/kasskoer.ee/renew' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "renew": {
        "period": 1,
        "period_unit": "y"
    }
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domain": {
            "name": "kasskoer.ee"
        }
    }
}
```

Renews current domain for desired period.

### HTTP Request

`POST /repp/v1/domains/:domain_name/renew`

### URL Parameters
Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
domain_name | Yes | String | Domain name


### Payload Parameters

Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
period_unit | Yes | String | Period unit. Can be year (y) or month (m)
period | Yes | Integer | For how many period units to register domain

<aside class="notice">
Note — Wrap your payload attributes into "renew" object, as shown in example.
</aside>

## Delete a specific domain

```shell
curl --location --request DELETE 'https://registry.test/repp/v1/domains/kanakotlet.ee' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "delete": {
        "verified": false
    }
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domain": {
            "name": "kanakotlet.ee"
        }
    }
}
```

Deletes a specific domain

### HTTP Request

`DELETE /repp/v1/contacts/:contact_id`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
contact_id | Yes | Contact ID

### Payload Parameters

Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
delete | Yes | Hash | Hash holding verified flag
verified | Yes | Boolean | Whether to ask registrant confirmation or not

# Nameservers

## Add new nameserver

```shell
curl --location --request POST 'https://testrar.internet.ee/repp/v1/domains/kotlet.ee/nameservers' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "nameservers": [
        {
            "hostname": "ns1.domainer.ee",
            "ipv4": ["192.168.1.1"],
            "ipv6": ["2620:119:35::35"]
        }
    ]
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domain": {
            "name": "kotlet.ee"
        }
    }
}
```

Add new nameserver(s) to specific domain

### HTTP Request

`POST /repp/v1/domains/:domain_name/nameservers`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
domain_name | Yes | Domain name

### Payload Parameters

Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
nameservers | Yes | Array | Array of new nameserver objects
nameservers[hostname] | Yes | Hostname of nameserver
nameservers[ipv4] | No | Array | Array of IPv4 addresses
nameservers[ipv6] | No | Array | Array of IPv6 addresses

## Delete existing nameserver

```shell
curl --location --request DELETE 'https://testrar.internet.ee/repp/v1/domains/kotlet.ee/nameservers/ns3.domainer.ee' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--data-raw ''
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domain": {
            "name": "666.ee"
        }
    }
}
```

Delete existing nameserver from domain.

### HTTP Request

`DELETE /repp/v1/domains/:domain_name/nameservers/:nameserver_hostname`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
domain_name | Yes | Domain name
nameserver_hostname | Yes | nameserver hostname to delete

# DNSSEC

## Get domain's DNSSEC keys

```shell
curl --location --request GET 'https://testrar.internet.ee/repp/v1/domains/666.ee/dnssec' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--data-raw ''
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "dns_keys": [
            {
                "flags": 257,
                "protocol": 3,
                "alg": 13,
                "public_key": "pubkeyhere"
            }
        ]
    }
}
```

Gets a specific domain's DNSSEC keys

### HTTP Request

`GET /repp/v1/domains/:domain_name/dnssec`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
domain_name | Yes | Domain name

## Add new DNSSEC key

```shell
curl --location --request POST 'https://testrar.internet.ee/repp/v1/domains/666.ee/dnssec' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "dns_keys": [
        {
            "flags": "257",
            "protocol": "3",
            "alg": "8",
            "public_key": "AwEAAddt2AkLfYGKgiEZB5SmIF8EvrjxNMH6HtxWEA4RJ9Ao6LCWheg8"
        }
    ]
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domain": {
            "name": "666.ee"
        }
    }
}
```

Add new DNSSEC key(s) to specific domain

### HTTP Request

`POST /repp/v1/domains/:domain_name/dnssec`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
domain_name | Yes | Domain name

### Payload Parameters

Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
dns_keys | Yes | Array | Array of DNSSEC key objects
dns_keys[flags] | Yes | String | 256 (KSK) or 257 (ZSK)
dns_keys[protocol] | Yes | String | Key protocol (3)
dns_keys[alg] | Yes | String | DNSSEC key algorithm (3,5,6,7,8,10,13,14)
dns_keys[public_key] | Yes | String | DNSSEC public key

## Delete DNSSEC key

```shell
curl --location --request DELETE 'https://testrar.internet.ee/repp/v1/domains/666.ee/dnssec' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "dns_keys": [
        {
            "flags": "257",
            "protocol": "3",
            "alg": "8",
            "public_key": "AwEAAddt2AkLfYGKgiEZB5SmIF8EvrjxNMH6HtxWEA4RJ9Ao6LCWheg8"
        }
    ]
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domain": {
            "name": "666.ee"
        }
    }
}
```

Deletes one or many DNSSEC key(s) for specific domain.

### HTTP Request

`DELETE /repp/v1/domains/:domain_name/dnssec`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
domain_name | Yes | Domain name

### Payload Parameters

Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
dns_keys | Yes | Array | Array of DNSSEC key objects
dns_keys[flags] | Yes | String | 256 (KSK) or 257 (ZSK) of existing DNSSEC key
dns_keys[protocol] | Yes | String | Key protocol of existing DNSSEC key
dns_keys[alg] | Yes | String | DNSSEC key algorithm of existing DNSSEC key
dns_keys[public_key] | Yes | String | Public key of existing DNSSEC key

# Transfers

## Get transfer info

```shell
curl --location --request GET 'https://testrar.internet.ee/repp/v1/domains/kotlet.ee/transfer_info' \
--header 'Auth-Code: 6699fb661ee3a2006a3d1e5833d7c5abv' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--data-raw ''
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domain": "kotlet.ee",
        "registrant": {
            "code": "ATSAA:E36957D7",
            "phone": "+372.xxx",
            "email": "xxx",
            "ident": "xxx",
            "ident_type": "priv",
            "name": "xxx",
            "ident_country_code": "EE",
            "city": null,
            "street": null,
            "zip": null,
            "country_code": null,
            "statuses": [
                "ok",
                "linked"
            ]
        },
        "admin_contacts": [
            {
              "code": "ATSAA:E36957D7",
              "phone": "+372.xxx",
              "email": "xxx",
              "ident": "xxx",
              "ident_type": "priv",
              "name": "xxx",
              "ident_country_code": "EE",
              "city": null,
              "street": null,
              "zip": null,
              "country_code": null,
              "statuses": [
                "ok",
                "linked"
              ]
            }
        ],
        "tech_contacts": [
            {
                "code": "ATSAA:E36957D7",
                "phone": "+372.xxx",
                "email": "xxx",
                "ident": "xxx",
                "ident_type": "priv",
                "name": "xxx",
                "ident_country_code": "EE",
                "city": null,
                "street": null,
                "zip": null,
                "country_code": null,
                "statuses": [
                    "ok",
                    "linked"
                ]
            }
        ]
    }
}
```

Gets a specific domain's transfer info

### HTTP Request

`GET /repp/v1/domains/:domain_name/transfer_info`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
domain_name | Yes | Domain name

### Required Headers

Submit domain's EPP authorization code if domain doesn't belong to you (yet)

Parameter | Required | Description
--------- | ------- | -----------
Auth-Code | No | Queried domain's EPP authorization code

## Transfer domain

```shell
curl --location --request POST 'https://testrar.internet.ee/repp/v1/domains/666.ee/transfer' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "transfer": {
        "transfer_code": "d79d865fdf589a1cd52ae95fde4e5d30"
    }
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "domain": {
            "name": "666.ee",
            "type": "domain_transfer"
        }
    }
}
```

Transfers domain from another registrar to your account.

### HTTP Request

`POST /repp/v1/domains/:domain_name/transfer`

### URL Parameters

Parameter | Required | Description
--------- | ------- | -----------
domain_name | Yes | Domain name

### Payload Parameters

Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
transfer | Yes | Object | Object holding transfer elements
transfer[transfer_code] | Yes | String | EPP authorization code

# Bulk actions

## Bulk domain transfer

```shell
curl --location --request POST 'https://testrar.internet.ee/repp/v1/domains/transfer' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "data": {
        "domain_transfers": [
            {
                "domain_name": "domain1.ee",
                "transfer_code": "a45e543ddabee202f95eebae5a7c917c"
            },
            {
                "domain_name": "domain2.ee",
                "transfer_code": "cce67ca63adc8c2938f199f10108881e"
            }
        ]
    }
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "success": [
            {
                "type": "domain_transfer",
                "domain_name": "domain1.ee"
            },
            {
                "type": "domain_transfer",
                "domain_name": "domain2.ee"
            }
        ],
        "failed": []
    }
}
```

Transfers multiple domains from another registrar to your account.

### HTTP Request

`POST /repp/v1/domains/transfer`

### Payload Parameters

Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
data | Yes | Object | Object holding domain_transfers array
data[domain_transfers] | Yes | Array | Array of domain transfer objects
data[domain_transfers][domain_name] | Yes | String | Domain name
data[domain_transfers][transfer_code] | Yes | String | EPP authorization code

## Bulk tech contact replace

```shell
curl --location --request PATCH 'https://testrar.internet.ee/repp/v1/domains/contacts' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "current_contact_id": "ATSAA:B839862D",
    "new_contact_id": "ATSAA:KARL"
}'
```

> The above command returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "success": [
            {
                "type": "domain_transfer",
                "domain_name": "kotlet.ee"
            },
            {
                "type": "domain_transfer",
                "domain_name": "kanakotlet.ee"
            }
        ],
        "failed": []
    }
}
```

Cycles through every domain and replaces specific tech contact with another one.

### HTTP Request

`PATCH /repp/v1/domains/contacts`

### Payload Parameters

Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
current_contact_id | Yes | String | Tech contact ID you wish to replace
new_contact_id | Yes | String | New contact ID to assign as tech contact

## Bulk nameserver change

```shell
curl --location --request PUT 'https://testrar.internet.ee/repp/v1/registrar/nameservers' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "data": {
        "type": "nameserver",
        "id": "ns1.domainer.ee",
        "attributes": {
            "hostname": "ns1.ams1.domainer.ee",
            "ipv4": ["192.168.1.1"],
            "ipv6": ["2620:119:35::36"]
        }
    }
}'
```
> If you want to replace nameserver only for specific domains, submit scoped "domains" array like that:

```shell
curl --location --request PUT 'https://testrar.internet.ee/repp/v1/registrar/nameservers' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "data": {
        "type": "nameserver",
        "id": "ns1.ams1.domainer.ee",
        "domains": ["only.ee", "these.ee", "domains.ee"],
        "attributes": {
            "hostname": "ns2.ams1.domainer.ee",
            "ipv4": ["192.168.1.1"],
            "ipv6": ["2620:119:35::36"]
        }
    }
}'
```

> Both commands above return JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "type": "nameserver",
        "id": "ns1.ams1.domainer.ee",
        "attributes": {
            "hostname": "ns1.ams1.domainer.ee",
            "ipv4": [
                "192.168.1.1"
            ],
            "ipv6": [
                "2620:119:35::36"
            ]
        },
        "affected_domains": [
            "only.ee",
            "these.ee",
            "domains.ee"
        ],
        "skipped_domains": []
    }
}
```

Cycles through every domain and replaces their specific nameserver with new data.

### HTTP Request

`PATCH /repp/v1/domains/contacts`

### Payload Parameters

Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
data | Yes | Object | Object holding nameserver changes
data[type] | Yes | String | Always set it as "nameserver"
data[id] | Yes | String | Hostname of replacable nameserver
data[domains] | No | Array | Array of domain names qualified for nameserver replacement.
data[attributes] Yes | Object | Object holding new nameserver values
data[attributes][hostname] | Yes | String | New hostname of nameserver
data[attributes][ipv4] | No | Array | Array of nameserver's fixed IPv4 addresses
data[attributes][ipv6] | No | Array | Array of nameserver's fixed IPv6 addresses

<aside class="warning">
Note - It's safer to use "domains" scope, although it's not required. If you don't submit "domains" array, we're going to take every domain in registrar's account to cycle, which can, from experience, deal pretty large damage.
</aside>

## Bulk domain renew

```shell
curl --location --request POST 'https://testrar.internet.ee/repp/v1/domains/renew/bulk' \
--header 'Authorization: Basic dGVzdDp0ZXN0MTIz' \
--header 'Content-Type: application/json' \
--data-raw '{
    "domains": ["kotlet.ee", "kasskoer.ee"],
    "renew_period": "1y"
}'
```

> Command above returns JSON structured like this:

```json
{
    "code": 1000,
    "message": "Command completed successfully",
    "data": {
        "updated_domains": [
            "kotlet.ee",
            "kasskoer.ee"
        ]
    }
}
```

Renews multiple domanins at once, for a fixed period.

### HTTP Request

`POST /repp/v1/domains/renew/bulk`

### Payload Parameters

Parameter | Required | Type | Description
--------- | ------- | ----- | -----------
domains | Yes | Array | Array of domain names for renewal
renew_period | Yes | String | Period for domain renew (3m, 6m, 9m, 1y ... 10y)
