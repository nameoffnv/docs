---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors
  - connect

search: true
---

# Introduction

Welcome to the Endpass API! You can use our API to access Endpass API endpoints, which can get information about Endpass users.

We have language bindings in Shell and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Constants

## Document types

|Key            |Description   |
|---------------|--------------|
|`Passport`     |Passport      |
|`DriverLicense`|Driver license|

## Status

|Key          |Description                                             |
|-------------|--------------------------------------------------------|
|`New`        |Default status, not submitted to verification service   |
|`Pending`    |Submitted to verification service, waiting result       |
|`NotReadable`|Document not readable, need update document file (image)|
|`NotVerified`|Received failed result from the verification service    |
|`Verified`   |Document verified                                       |

# Wallet Endpoints

## Users

### User resource

Generic user information. By default, only public information is shared without any scopes. Verified phone numbers or email can be requested with additional scopes.

> User’s public information (default)

```json
{
  "id": "0d8c5fa3-c8a5-4c5f-8435-f35aef353f30"
}
```

> Authenticated user with their verified phone numbers (user:phone:read permission)

```json
{
  ...
  "email": "user@endpass.com"
}
```

> Authenticated user with their email (user:email:read permission)

```json
{
  ...
  "phones": [
    {
      "id": "c4d4ef1c-0d73-4a6f-aad9-7600a8fc79b8",
      "createdAt": 1557220652,
      "status": "Verified",
      "country": "7",
      "number": "7771112233"
    }
  ]
}
```

|Fields            |Description                  |
|------------------|-----------------------------|
|`id` (*string*)   |Resource ID                  |
|`email` (*string*)|User’s email                 |
|`phones` ([*Phone*](#phone))|User’s verified phone numbers|

### Phone

|Fields                                      |Description        |
|--------------------------------------------|-------------------|
|`id` (*string*)                             |Resource ID        |
|`createdAt` (*timestamp*)                   |Creation timestamp |
|`status` (*string*, [*enumerable*](#status))|Verification status|
|`country` (*string*)                        |Country code       |
|`number` (*string*)                         |Phone number       |

### Show current user

Returns user information.

### HTTP Request

`GET https://api.endpass.com/v1/user`

### Scopes

- `user:email:read`
- `user:phone:read`

```ruby

```

```python

```

```shell
curl "https://api.endpass.com/v1/user"
  -H "Authorization: Bearer <access_token>"
```

```javascript

```

## Accounts

Account resource represents all of a user’s accounts.

> List of account

```json
[
  "0x123...",
  "0x456..."
]
```

> Account resource

```json
{
  "address": "0x123..."
}
```

### Account resource

|Fields              |Description        |
|--------------------|-------------------|
|`address` (*string*)|Address hash       |

### Show user accounts

Returns a list of user accounts that have a keystore.

### HTTP Request

`GET https://api.endpass.com/v1/accounts`

### Scopes

- `wallet:accounts:read`

```ruby

```

```python

```

```shell
curl "https://api.endpass.com/v1/accounts"
  -H "Authorization: Bearer <access_token>"
```

```javascript

```

### Last active account

Returns user last active account.

### HTTP Request

`GET https://api.endpass.com/v1/accounts/active`

### Scopes

- `wallet:address:read`

```ruby

```

```python

```

```shell
curl "https://api.endpass.com/v1/accounts/active"
  -H "Authorization: Bearer <access_token>"
```

```javascript

```

## Documents

Document resource represents a user’s document.

> List of documents

```json
[
  {
    "id": "f1f80c7a-57c7-4259-8e80-d1bb09932e82",
    "createdAt": 1543336121,
    "status": "New",
    "documentType": "Passport",
    "description": "Custom document description",
    "firstName": "Test",
    "lastName": "User",
    "number": "47313892501",
    "dateOfBirth": 1543323121,
    "dateOfIssue": 1543336122,
    "dateOfExpiry": 1543336123,
    "issuingCountry": "Country",
    "issuingAuthority": "Authority",
    "issuingPlace": "Place",
    "address": "Address"
  }
]
```

> Document's public information (default)

```json
{
  "id": "f1f80c7a-57c7-4259-8e80-d1bb09932e82",
  "createdAt": 1543336121
}
```

> Document's status (documents:status:read)

```json
{
  ...
  "status": "New"
}
```

> Document's data (documents:data:read)

```json
{
  ...
  "documentType": "Passport",
  "description": "Custom document description",
  "firstName": "Test",
  "lastName": "User",
  "number": "47313892501",
  "dateOfBirth": 1543323121,
  "dateOfIssue": 1543336122,
  "dateOfExpiry": 1543336123,
  "issuingCountry": "Country",
  "issuingAuthority": "Authority",
  "issuingPlace": "Place",
  "address": "Address"
}
```

### Document resource

|Fields                                                    |Description               |
|----------------------------------------------------------|--------------------------|
|`id` (*string*)                                           |Resource ID               |
|`createdAt` (*timestamp*)                                 |Creation timestamp        |
|`status` (*string*, [*enumerable*](#status))              |Document current status   |
|`documentType` (*string*, [*enumerable*](#document-types))|Document type             |
|`description` (*string*)                                  |Description               |
|`firstName` (*string*)                                    |First name                |
|`lastName` (*string*)                                     |Last name                 |
|`number` (*string*)                                       |Document number           |
|`dateOfBirth` (*timestamp*)                               |Date of birth             |
|`dateOfIssue` (*timestamp*)                               |Document issue date       |
|`dateOfExpiry` (*timestamp*)                              |Document expiration date  |
|`issuingCountry` (*string*)                               |Document issuing country  |
|`issuingAuthority` (*string*)                             |Document issuing authority|
|`issuingPlace` (*string*)                                 |Document issuing place    |
|`address` (*string*)                                      |Address                   |

### Show user documents

Returns all user documents.

### HTTP Request

`GET https://api.endpass.com/v1/documents`

### Scopes

- `documents:status:read`
- `documents:data:read`

```ruby

```

```python

```

```shell
curl "https://api.endpass.com/v1/documents"
  -H "Authorization: Bearer <access_token>"
```

```javascript

```

### Show document

Returns document by ID.

### HTTP Request

`GET https://api.endpass.com/v1/documents/{ID}`

### Scopes

- `documents:status:read`
- `documents:data:read`

```ruby

```

```python

```

```shell
curl "https://api.endpass.com/v1/documents/{ID}"
  -H "Authorization: Bearer <access_token>"
```

```javascript

```

### Show document's file

Returns the document file (jpeg, pdf) into `binary` stream.

### HTTP Request

`GET https://api.endpass.com/v1/documents/{ID}/file`

### Scopes

- `documents:image:read`

```ruby

```

```python

```

```shell
curl "https://api.endpass.com/v1/documents/{ID}/file"
  -H "Authorization: Bearer <access_token>"
```

```javascript

```
