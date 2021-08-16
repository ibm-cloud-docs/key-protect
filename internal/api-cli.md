---

copyright:
  years: 2020
lastupdated: "2020-11-24"

keywords: Key Protect integration, application programming interface, API, command line interface, CLI

subcollection: key-protect

---

{:external: target="_blank" .external}
{:important: .important}
{:screen: .screen}

# API and CLI differences
{: #api-cli}

This document summarizes the differences between the application programming
interface (API) and the command line interface (CLI).

These are links to the API and CLI documentation.

- [API](/apidocs/key-protect){: external}
- [CLI](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference)

The documentation contains these 5 sections:

- Import tokens
- Key events
- Keys
- Policies
- Registrations

## Summary
{: #api-cli-summary}

| Name                                                                | API                                                            | CLI                    |
| ------------------------------------------------------------------- | -------------------------------------------------------------- | ---------------------- |
| [Import token create](#api-cli-import-token-create)                 | POST /api/v2/import_token                                      | kp import-token create |
| [Import token key encrypt](#api-cli-import-token-key-encrypt)       |                                                                | kp import-token key-encrypt |
| [Import token nonce encrypt](#api-cli-import-token-nonce-encrypt)   |                                                                | kp import-token nonce-encrypt |
| [Import token retrieve](#api-cli-import-token-retrieve)             | GET /api/v2/import_token                                       | kp import-token show |
| [Acknowledge key event](#api-cli-key-events-acknowledge)            | POST /api/v2/event_ack                                         | |
| [Key create](#api-cli-key-create)                                   | POST /api/v2/keys                                              | kp key create |
| [List keys](#api-cli-list-keys)                                     | GET /api/v2/keys                                               | kp keys |
| [Key total](#api-cli-key-total)                                     | HEAD /api/v2/keys                                              | |
| [Key list](#api-cli-key-list)                                       | GET /api/v2/keys/{id}                                          | kp key show |
| [Key cancel delete](#api-cli-key-cancel)                            | POST /api/v2/keys/{id}/actions/unsetKeyDeletion                | kp key cancel-delete |
| [Key disable](#api-cli-key-disable)                                 | POST /api/v2/keys/{id}/actions/disable                         | kp key disable |
| [Key enable](#api-cli-key-enable)                                   | POST /api/v2/keys/{id}/actions/enable                          | kp key enable |
| [Key restore](#api-cli-key-restore)                                 | POST /api/v2/keys/{id}/restore                                 | kp key restore |
| [Key rewrap](#api-cli-key-rewrap)                                   | POST /api/v2/keys/{id}/actions/rewrap                          | |
| [Key rotate](#api-cli-key-rotate)                                   | POST /api/v2/keys/{id}/actions/rotate                          | kp key rotate |
| [Key schedule delete](#api-cli-key-schedule)                        | POST /api/v2/keys/{id}/actions/setKeyDeletion                  | kp key schedule-delete |
| [Key unwrap](#api-cli-key-unwrap)                                   | POST /api/v2/keys/{id}/actions/unwrap                          | kp key unwrap |
| [Key wrap](#api-cli-key-wrap)                                       | POST /api/v2/keys/{id}/actions/wrap                            | kp key wrap |
| [Key delete](#api-cli-key-delete)                                   | DELETE /api/v2/keys/{id}                                       | kp key delete |
| [Key metadata](#api-cli-key-metadata)                               | GET /api/v2/keys/{id}/metadata                                 | |
| [Key versions](#api-cli-key-versions)                               | GET /api/v2/keys/{id}/versions                                 | |
| [Key policy dual-auth-delete](#api-cli-key-policy-dual)             | PUT /api/v2/keys/{id}/policies                                 | kp key policy-update dual-auth-delete |
| [Key policy rotation](#api-cli-key-policy-rotation)                 | PUT /api/v2/keys/{id}/policies                                 | kp key policy-update rotation |
| [Key policy list](#api-cli-key-policy-list)                         | GET /api/v2/keys/{id}/policies                                 | kp key policies |
| [Instance policy allowed-network](#api-cli-instance-policy-allowed) | PUT /api/v2/instance/policies                                  | kp instance policy-update allowed-network |
| [Instance policy dual-auth-delete](#api-cli-instance-policy-dual)   | PUT /api/v2/instance/policies                                  | kp instance policy-update dual-auth-delete |
| [Instance policy list](#api-cli-instance-policy-list)               | GET /api/v2/instance/policies                                  | kp instance policies |
| [Registration create](#api-cli-registration-create)                 | POST /api/v2/keys/{id}/registrations/{urlEncodedResourceCRN}   | |
| [Registration update](#api-cli-registration-update)                 | PATCH /api/v2/keys/{id}/registrations/{urlEncodedResourceCRN}  | |
| [Registration replace](#api-cli-registration-replace)               | PUT /api/v2/keys/{id}/registrations/{urlEncodedResourceCRN}    | |
| [Registration delete](#api-cli-registration-delete)                 | DELETE /api/v2/keys/{id}/registrations/{urlEncodedResourceCRN} | |
| [Registration list key](#api-cli-registration-key-list)             | GET /api/v2/keys/{id}/registrations                            | kp registrations |
| [Registration list](#api-cli-registration-list)                     | GET /api/v2/keys/registrations                                 | kp registrations |

## Column headers
{: #api-cli-column-headers}

Each API/CLI has two sections, a request and a response. These are the headers
for each secion.

- Request

    - **API parameter:** where the parameter is specified (path, query parameter,
        head, or body) and the parameter name
    - **API required:** `req`, if the API requires the parameter
    - **CLI parameter:** the CLI parameter or the optional parameter
    - **CLI required:** `req`, if the CLI requires the parameter

- Response

    - **API JSON:** the key name in the API JSON response
    - **API opt:** `opt`, if the element is optional, or maybe, if the state is
        unknown
    - **CLI text:** an identifiable label in the text response
    - **CLI JSON:** the key name in the CLI JSON response
    - **CLI opt:** `opt`, if the element is optional

## Import tokens
{: #api-cli-import-tokens}

### Create an import token
{: #api-cli-import-token-create}

An import token which consists of a `nonce` and a `public key`.

API: `POST /api/v2/import_token`

CLI: `kp import-token create`

#### Request
{: #api-cli-import-token-create-request}

| API parameter              | API req | CLI parameter        | CLI req |
| -------------------------- | ------- | -------------------- | ------- |
| head: bluemix-instance     | Req     | -i, --instance-id    | Req     |
| head: correlation-id       |         |                      |         |
| body: expiration           |         | -e, --expiration     |         |
| body: maxAllowedRetrievals |         | -m, --max-retrievals |         |

#### Response
{: #api-cli-import-token-create-response}

| API JSON             | API opt | CLI text             | CLI JSON | CLI opt |
| -------------------- | ------- | -------------------- | -------- | ------- |
| creationDate         |         | Created              |          |         |
| expiration           | Maybe   |                      |          |         |
| expirationDate       |         | Expires              |          |         |
| maxAllowedRetrievals |         | Max Retrievals       |          |         |
| remainingRetrievals  |         | Remaining Retrievals |          |         |

#### Notes
{: #api-cli-import-token-create-notes}

The CLI does **not** return JSON output.

### Encrypt a key
{: #api-cli-import-token-key-encrypt}

Encrypt the public key using the key material.

This is required to import a key using an import token.

The public key is the `payload` element in the example JSON structure.

```json
{
    "nonce": "8rf2ldP/zWm1Tjrb",
    "payload": "LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0t ...<redacted>... QyBLRVktLS0tLQo="
}
```
{: screen}

API: does not exist because a plain text key material would be exposed as it
traverses between the customer and the cloud.

CLI: `kp import-token key-encrypt`

#### Request
{: #api-cli-import-token-key-encrypt-request}

| API parameter | API req | CLI parameter     | CLI req |
| ------------- | ------- | ----------------- | ------- |
|               |         | -i, --instance-id | Req     |
|               |         | -k, --key         | Req     |
|               |         | -p, --pubkey      | Req     |
|               |         | -a, --hash        |         |

#### Response
{: #api-cli-import-token-key-encrypt-response}

| API JSON | API opt | CLI text      | CLI JSON | CLI opt |
| -------- | ------- | ------------- | -------- | ------- |
|          |         | Encrypted Key |          |         |

#### Notes
{: #api-cli-import-token-key-encrypt-notes}

You must manually encrypt the public key using the key material if you use the
API. Either use the CLI or create a program to perform the encryption. Python or
Go have supporting encryption libraries.

The CLI command runs locally and does not connect to any external resources.

The CLI does **not** return JSON output.

### Encrypt a nonce
{: #api-cli-import-token-nonce-encrypt}

Encrypt the nonce using the key material.

This is required to import a key using an import token.

The nonce is the `nonce` element in the example JSON structure.

```json
{
    "nonce": "8rf2ldP/zWm1Tjrb",
    "payload": "LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0t ...<redacted>... QyBLRVktLS0tLQo="
}
```
{: screen}

API: does not exist because a plain text key material would be exposed as it
traverses between the customer and the cloud.

CLI: `kp import-token nonce-encrypt`

#### Request
{: #api-cli-import-token-nonce-encrypt-request}

| API parameter | API req | CLI parameter     | CLI req |
| ------------- | ------- | ----------------- | ------- |
|               |         | -i, --instance-id | Req     |
|               |         | -k, --key         | Req     |
|               |         | -n, --nonce       | Req     |
|               |         | -c, --cbc         |         |

#### Response
{: #api-cli-import-token-nonce-encrypt-response}

| API JSON | API opt | CLI text        | CLI JSON | CLI opt |
| -------- | ------- | --------------- | -------- | ------- |
|          |         | Encrypted Nonce |          |         |
|          |         | IV              |          |         |

#### Notes
{: #api-cli-import-token-nonce-encrypt-notes}

You must manually encrypt the nonce using the key material if you use the API.
Either use the CLI or create a program to perform the encryption. Python or Go
have supporting encryption libraries.

The CLI command runs locally and does not connect to any external resources.

The CLI does **not** return JSON output.

### Retrieve an import token
{: #api-cli-import-token-retrieve}

Retrieve an import token. There is one, and only one, import token associated
with a {{site.data.keyword.keymanagementserviceshort}} instance.

API: `GET /api/v2/import_token`

CLI: `kp import-token show`

#### Request
{: #api-cli-import-token-retrieve-request}

| API parameter          | API req | CLI parameter     | CLI req |
| ---------------------- | ------- | ----------------- | ------- |
| head: bluemix-instance | Req     | -i, --instance-id | Req     |
| head: correlation-id   |         |                   |         |

#### Response
{: #api-cli-import-token-retrieve-response}

| API JSON             | API opt | CLI text | CLI JSON | CLI opt |
| -------------------- | ------- | -------- | -------- | ------- |
| creationDate         |         |          |          |         |
| expiration           | Maybe   |          |          |         |
| expirationDate       |         |          |          |         |
| maxAllowedRetrievals |         |          |          |         |
| nonce                |         |          | nonce    |         |
| payload              |         |          | payload  |         |
| remainingRetrievals  |         |          |          |         |

#### Notes
{: #api-cli-import-token-retrieve-notes}

The CLI **only** returns JSON output.

## Key events
{: #api-cli-key-events}

### Acknowledge key events
{: #api-cli-key-events-acknowledge}

This applies to service-to-service calls only.

Acknowledges a key lifecycle event.

When a customer performs an action on a root key,
{{site.data.keyword.keymanagementserviceshort}} uses Hyperwarp to notify the
cloud services that are registered with the key.

API: `POST /api/v2/event_ack`

CLI: **not implemented**

#### Request
{: #api-cli-key-events-acknowledge-request}

| API parameter                 | API req | CLI parameter | CLI req |
| ----------------------------- | ------- | ------------- | ------- |
| head: bluemix-instance        | Req     |               |         |
| head: correlation-id          |         |               |         |
| body: eventId                 | Req     |               |         |
| body: adopterKeyState         | Req     |               |         |
| body: timestamp               | Req     |               |         |
| body: keyStateData.keyVersion | Req     |               |         |
| body: deleteRegistration      |         |               |         |

#### Response
{: #api-cli-key-events-acknowledge-response}

The response is an HTTP status code.

## Keys
{: #api-cli-keys}

### Create a key
{: #api-cli-key-create}

Create a new key with specified key material.

{{site.data.keyword.keymanagementserviceshort}} designates the resource as
either a root key or a standard key based on the `extractable` value that is
specified.

Extractable determines whether the key material can leave the service.

- If set to false, {{site.data.keyword.keymanagementserviceshort}} designates
    the key as a nonextractable root key used for wrap and unwrap actions.

- If set to true, {{site.data.keyword.keymanagementserviceshort}} designates the
    key as a standard key that you can store in your apps and services. Once set
    to false it cannot be changed to true.

A successful POST /keys operation adds the key to the service and returns the
details of the request in the response entity-body, if the `prefer` header is
set to `return=representation`.

**What is the key material**

A `key material` is a base64-encoded value. It must be 16, 24, or 32 bytes long;
corresponding to 128, 192, or 256 bits.

API: `POST /api/v2/keys`

CLI: `kp key create`

There are three variations for creating a key.

1. Allow {{site.data.keyword.keymanagementserviceshort}} to create a key
    material on your behalf
2. Specify a base64-encoded key material
3. Specify a base64-encoded encrypted key material; this uses an `import token`

#### Request - allow {{site.data.keyword.keymanagementserviceshort}} to create a key material
{: #api-cli-key-create-request-1}

| API parameter          | API req | CLI parameter      | CLI req |
| ---------------------- | ------- | ------------------ | ------- |
| head: bluemix-instance | Req     | -i, --instance-id  | Req     |
| head: correlation-id   |         |                    |         |
| head: prefer           |         |                    |         |
| body: description      |         |                    |         |
| body: expirationDate   |         |                    |         |
| body: extractable      |         | -s, --standard-key |         |
| body: name             | Req     | name               | Req     |
| body: tags             |         |                    |         |
| body: type             | Req     |                    |         |
|                        |         | -o, --output       |         |

#### Response - allow {{site.data.keyword.keymanagementserviceshort}} to create a key material
{: #api-cli-key-create-response-1}

| API JSON                         | API opt | CLI text | CLI JSON    | CLI opt |
| -------------------------------- | ------- | -------- | ----------- | ------- |
| algorithmBitSize                 |         |          |             |         |
| algorithmMetadata.bitLength      |         |          |             |         |
| algorithmMetadata.mode           |         |          |             |         |
| algorithmMode                    |         |          |             |         |
| algorithmType                    |         |          |             |         |
| createdBy                        |         |          |             |         |
| creationDate                     |         |          |             |         |
| crn                              |         |          | crn         |         |
| deleted                          |         |          |             |         |
| deletedBy                        | Opt     |          |             |         |
| deletionDate                     | Opt     |          |             |         |
| description                      | Opt     |          |             |         |
| dualAuthDelete.authExpiration    | Opt     |          |             |         |
| dualAuthDelete.enabled           | Opt     |          |             |         |
| dualAuthDelete.keySetForDeletion | Opt     |          |             |         |
| extractable                      |         |          | extractable |         |
| id                               |         | Key ID   | id          |         |
| imported                         |         |          |             |         |
| keyVersion.creationDate          |         |          |             |         |
| keyVersion.id                    |         |          |             |         |
| lastRotateDate                   | Opt     |          |             |         |
| lastUpdateDate                   |         |          |             |         |
| name                             |         | Key Name | name        |         |
| nonactiveStateReason             | Opt     |          |             |         |
| payload                          | Opt     |          |             |         |
| state                            |         |          | state       |         |
| tags                             | Opt     |          |             |         |
| type                             |         |          | type        |         |

#### Request - specify a key material
{: #api-cli-key-create-request-2}

| API parameter          | API req | CLI parameter      | CLI req |
| ---------------------- | ------- | ------------------ | ------- |
| head: bluemix-instance | Req     | -i, --instance-id  | Req     |
| head: correlation-id   |         |                    |         |
| head: prefer           |         |                    |         |
| body: description      |         |                    |         |
| body: expirationDate   |         |                    |         |
| body: extractable      |         | -s, --standard-key |         |
| body: name             | Req     | name               | Req     |
| body: payload          | Req     | -k, --key-material | Req     |
| body: tags             |         |                    |         |
| body: type             | Req     |                    |         |
|                        |         | -o, --output       |         |

#### Response - specify a key material
{: #api-cli-key-create-response-2}

The response is the same as the
[key create response](#api-cli-key-create-response-1).

#### Request - specify an encrypted key material using an import token
{: #api-cli-key-create-request-3}

| API parameter             | API req | CLI parameter         | CLI req |
| ------------------------- | ------- | --------------------- | ------- |
| head: bluemix-instance    | Req     | -i, --instance-id     | Req     |
| head: correlation-id      |         |                       |         |
| head: prefer              |         |                       |         |
| body: description         |         |                       |         |
| body: encryptedNonce      | Req     | -n, --encrypted-nonce | Req     |
| body: encryptionAlgorithm |         |                       |         |
| body: expirationDate      |         |                       |         |
| body: extractable         |         | -s, --standard-key    |         |
| body: iv                  | Req     | -v, --iv              | Req     |
| body: name                | Req     | name                  | Req     |
| body: payload             | Req     | -k, --key-material    | Req     |
| body: tags                |         |                       |         |
| body: type                | Req     |                       |         |
|                           |         | -o, --output          |         |

#### Response - specify an encrypted key material using an import token
{: #api-cli-key-create-response-3}

The response is the same as the
[key create response](#api-cli-key-create-response-1).

### List keys
{: #api-cli-list-keys}

Retrieves a list of keys that are stored in your
{{site.data.keyword.keymanagementserviceshort}} instance.

The key material is not shown. You can retrieve the key material for a standard
key with a subsequent GET /keys/{id} request.

API: `GET /api/v2/keys`

CLI: `kp keys`

#### Request
{: #api-cli-list-keys-request}

| API parameter          | API req | CLI parameter         | CLI req |
| ---------------------- | ------- | --------------------- | ------- |
| query: limit           |         | -n, --number-of-keys  |         |
| query: offset          |         | -s, --starting-offset |         |
| query: state           |         |                       |         |
| head: bluemix-instance | Req     | -i, --instance-id     | Req     |
| head: correlation-id   |         |                       |         |
|                        |         | -c, --crn             |         |
|                        |         | -o, --output          |         |

#### Response
{: #api-cli-list-keys-response}

| API JSON                         | API opt | CLI text | CLI JSON                | CLI opt |
| -------------------------------- | ------- | -------- | ----------------------- | ------- |
| algorithmBitSize                 |         |          |                         |         |
| algorithmMetadata.bitLength      |         |          |                         |         |
| algorithmMetadata.mode           |         |          |                         |         |
| algorithmMode                    |         |          |                         |         |
| algorithmType                    |         |          | algorithmType           |         |
| createdBy                        |         |          | createdBy               |         |
| creationDate                     |         |          | creationDate            |         |
| crn                              |         |          | crn                     |         |
| deleted                          |         |          |                         |         |
| deletedBy                        | Opt     |          |                         |         |
| deletionDate                     | Opt     |          |                         |         |
| description                      | Opt     |          |                         |         |
| dualAuthDelete.authExpiration    | Opt     |          |                         |         |
| dualAuthDelete.enabled           | Opt     |          |                         |         |
| dualAuthDelete.keySetForDeletion | Opt     |          |                         |         |
| extractable                      |         |          | extractable             |         |
| id                               |         | Key ID   | id                      |         |
| imported                         |         |          |                         |         |
| keyVersion.creationDate          |         |          | keyVersion.creationDate |         |
| keyVersion.id                    |         |          | keyVersion.id           |         |
| lastRotateDate                   | Opt     |          |                         |         |
| lastUpdateDate                   |         |          | lastUpdateDate          |         |
| name                             |         | Key Name | name                    |         |
| nonactiveStateReason             | Opt     |          |                         |         |
| payload                          | Opt     |          |                         |         |
| state                            |         |          | state                   |         |
| tags                             | Opt     |          |                         |         |
| type                             |         |          | type                    |         |

### Retrieve key total
{: #api-cli-key-total}

Returns the same HTTP headers as a GET request without returning the
entity-body.

This operation returns the number of keys in your instance in a header called
`Key-Total`.

API: `HEAD /api/v2/keys`

CLI: **not implemented**

#### Request
{: #api-cli-key-total-request}

| API parameter          | API req | CLI parameter | CLI req |
| ---------------------- | ------- | ------------- | ------- |
| query: state           |         |               |         |
| head: bluemix-instance | Req     |               |         |
| head: correlation-id   |         |               |         |

#### Response - no body
{: #api-cli-key-total-response}

The `Key-Total` is returned in the header. There is no body response.

### Retrieve a key
{: #api-cli-key-list}

Description.

API: `GET /api/v2/keys/{id}`

CLI: `kp key show`

#### Request
{: #api-cli-key-list-request}

| API parameter          | API req | CLI parameter     | CLI req |
| ---------------------- | ------- | ----------------- | ------- |
| path: id               | Req     | id                | Req     |
| head: bluemix-instance | Req     | -i, --instance-id | Req     |
| head: correlation-id   |         |                   |         |
|                        |         | -o, --output      |         |

#### Response
{: #api-cli-key-list-response}

| API JSON                         | API opt | CLI text | CLI JSON                | CLI opt |
| -------------------------------- | ------- | -------- | ----------------------- | ------- |
| algorithmBitSize                 |         |          |                         |         |
| algorithmMetadata.bitLength      |         |          |                         |         |
| algorithmMetadata.mode           |         |          |                         |         |
| algorithmMode                    |         |          |                         |         |
| algorithmType                    |         |          | algorithmType           |         |
| createdBy                        |         |          | createdBy               |         |
| creationDate                     |         |          | creationDate            |         |
| crn                              |         |          | crn                     |         |
| deleted                          |         |          |                         |         |
| deletedBy                        | Opt     |          |                         |         |
| deletionDate                     | Opt     |          |                         |         |
| description                      | Opt     |          |                         |         |
| dualAuthDelete.authExpiration    | Opt     |          |                         |         |
| dualAuthDelete.enabled           | Opt     |          |                         |         |
| dualAuthDelete.keySetForDeletion | Opt     |          |                         |         |
| extractable                      |         |          | extractable             |         |
| id                               |         | Key ID   | id                      |         |
| imported                         |         |          |                         |         |
| keyVersion.creationDate          |         |          | keyVersion.creationDate |         |
| keyVersion.id                    |         |          | keyVersion.id           |         |
| lastRotateDate                   | Opt     |          |                         |         |
| lastUpdateDate                   |         |          | lastUpdateDate          |         |
| name                             |         | Key Name | name                    |         |
| nonactiveStateReason             | Opt     |          |                         |         |
| payload                          | Opt     |          | payload                 | Opt     |
| state                            |         |          | state                   |         |
| tags                             | Opt     |          |                         |         |
| type                             |         |          | type                    |         |

### Invoke an action on a key
{: #api-cli-key-action}

### Action - remove an authorization for a key with a dual authorization policy
{: #api-cli-key-cancel}

A key with a `dual-auth-delete` policy requires authorization from two
administrative users to delete the key. This cancels, or removes, a prior
authorization.

API: `POST /api/v2/keys/{id}/actions/unsetKeyDeletion`

CLI: `kp key cancel-delete`

#### Request
{: #api-cli-key-cancel-request}

| API parameter          | API req | CLI parameter     | CLI req |
| ---------------------- | ------- | ----------------- | ------- |
| path: id               | Req     | id                | Req     |
| query: action          | Req     |                   |         |
| head: bluemix-instance | Req     | -i, --instance-id | Req     |
| head: correlation-id   |         |                   |         |
| head: prefer           |         |                   |         |

#### Response - none
{: #api-cli-key-cancel-respone}

There is no body response, check the HTTP status code.

### Action - disable a key
{: #api-cli-key-disable}

Disable a root key and temporarily revokes access to the key's associated data
in the cloud.

API: `POST /api/v2/keys/{id}/actions/disable`

CLI: `kp key disable`

#### Request
{: #api-cli-key-disable-request}

| API parameter          | API req | CLI parameter     | CLI req |
| ---------------------- | ------- | ----------------- | ------- |
| path: id               | Req     | id                | Req     |
| query: action          | Req     |                   |         |
| head: bluemix-instance | Req     | -i, --instance-id | Req     |
| head: correlation-id   |         |                   |         |
| head: prefer           |         |                   |         |

#### Response - none
{: #api-cli-key-disable-response}

There is no body response, check the HTTP status code.

### Action - enable a key
{: #api-cli-key-enable}

When you enable a root key that was previously disabled, the key transitions
from the _Suspended_ (value is 2) to the _Active_ (value is 1) key state.

This action restores the key's encrypt and decrypt operations.

API: `POST /api/v2/keys/{id}/actions/enable`

CLI: `kp key enable`

#### Request
{: #api-cli-key-enable-request}

| API parameter          | API req | CLI parameter     | CLI req |
| ---------------------- | ------- | ----------------- | ------- |
| path: id               | Req     | id                | Req     |
| query: action          | Req     |                   |         |
| head: bluemix-instance | Req     | -i, --instance-id | Req     |
| head: correlation-id   |         |                   |         |
| head: prefer           |         |                   |         |

#### Response - none
{: #api-cli-key-enable-response}

There is no body response, check the HTTP status code.

### Action - restore a key
{: #api-cli-key-restore}

Restoring a previously deleted root key restores access to its associated data
in the cloud.

There are two forms of restore:

1. Use the key material that was used to create the key

2. Use an import token for keys that were imported into
    {{site.data.keyword.keymanagementserviceshort}}

API: `POST /api/v2/keys/{id}/restore`

CLI: `kp key enable`

#### Request - key was created using a key material
{: #api-cli-key-restore-request-1}

| API parameter          | API req | CLI parameter      | CLI req |
| ---------------------- | ------- | ------------------ | ------- |
| path: id               | Req     | id                 | Req     |
| query: action          | Req     |                    |         |
| head: bluemix-instance | Req     | -i, --instance-id  | Req     |
| head: correlation-id   |         |                    |         |
| head: prefer           |         |                    |         |
| body: payload          | Req     | -k, --key-material | Req     |

#### Response
{: #api-cli-key-restore-response-1}

The API documentation does not show a response for `restore`.
{: important}

| API JSON | API opt | CLI text | CLI JSON    | CLI opt |
| -------- | ------- | -------- | ----------- | ------- |
|          |         |          | crn         |         |
|          |         |          | extractable |         |
|          |         | Key ID   | id          |         |
|          |         | Key Name | name        |         |
|          |         |          | state       |         |
|          |         |          | type        |         |

#### Request - key was created using an import token
{: #api-cli-key-restore-request-2}

| API parameter             | API req | CLI parameter         | CLI req |
| ------------------------- | ------- | --------------------- | ------- |
| path: id                  | Req     | id                    | Req     |
| query: action             | Req     |                       |         |
| head: bluemix-instance    | Req     | -i, --instance-id     | Req     |
| head: correlation-id      |         |                       |         |
| head: prefer              |         |                       |         |
| body: encryptedNonce      | Req     | -n, --encrypted-nonce | Req     |
| body: encryptionAlgorithm | Maybe   |                       |         |
| body: iv                  | Req     | -v, --iv              | Req     |
| body: payload             | Req     | -k, --key-material    | Req     |

#### Response
{: #api-cli-key-restore-response-2}

The API documentation does not show a response for `restore`.
{: important}

| API JSON | API opt | CLI text | CLI JSON    | CLI opt |
| -------- | ------- | -------- | ----------- | ------- |
|          |         |          | crn         |         |
|          |         |          | extractable |         |
|          |         | Key ID   | id          |         |
|          |         | Key Name | name        |         |
|          |         |          | state       |         |
|          |         |          | type        |         |

### Action - rewrap a key
{: #api-cli-key-rewrap}

When you rotate a root key in {{site.data.keyword.keymanagementserviceshort}},
new cryptographic key material becomes available for protecting the data
encryption keys (DEKs) that are associated with the root key.

With the rewrap API, you can re-encrypt or rewrap your DEKs without exposing the
keys in their plaintext form.

API: `POST /api/v2/keys/{id}/actions/rewrap`

CLI: **not implemented**

#### Request
{: #api-cli-key-rewrap-request}

| API parameter          | API req | CLI parameter | CLI req |
| ---------------------- | ------- | ------------- | ------- |
| path: id               | Req     |               |         |
| query: action          | Req     |               |         |
| head: bluemix-instance | Req     |               |         |
| head: correlation-id   |         |               |         |
| head: prefer           |         |               |         |
| body: aad              |         |               |         |
| body: ciphertext       | Req     |               |         |

#### Response
{: #api-cli-key-rewrap-response}

| API JSON                         | API opt | CLI text | CLI JSON | CLI opt |
| -------------------------------- | ------- | -------- | -------- | ------- |
| ciphertext                       |         |          |          |         |
| keyVersion.creationDate          |         |          |          |         |
| keyVersion.id                    |         |          |          |         |
| rewrappedKeyVersion.creationDate |         |          |          |         |
| rewrappedKeyVersion.id           |         |          |          |         |

### Action - rotate a key
{: #api-cli-key-rotate}

When you rotate your root key, you replace the key with a new key material.

API: `POST /api/v2/keys/{id}/actions/rotate`

CLI: `kp key rotate`

#### Request
{: #api-cli-key-rotate-request}

| API parameter          | API req | CLI parameter      | CLI req |
| ---------------------- | ------- | ------------------ | ------- |
| path: id               | Req     |  id                | Req     |
| query: action          | Req     |                    |         |
| head: bluemix-instance | Req     | -i, --instance-id  | Req     |
| head: correlation-id   |         |                    |         |
| head: prefer           |         |                    |         |
| body: payload          | Req     | -k, --key-material |         |

#### Response
{: #api-cli-key-rotate-response}

The API documentation does not show a response for `actions/rotate`.
{: important}

The CLI **only** returns text output, indicating `SUCCESS` or `FAILED`.

### Action - schedule an authorization for a key with a dual authorization policy
{: #api-cli-key-schedule}

A key with a `dual-auth-delete` policy requires authorization from two
administrative users to delete the key. This authorizes a key to be deleted.

API: `POST /api/v2/keys/{id}/actions/setKeyDeletion`

CLI: `kp key schedule-delete`

#### Request
{: #api-cli-key-schedule-request}

| API parameter          | API req | CLI parameter     | CLI req |
| ---------------------- | ------- | ----------------- | ------- |
| path: id               | Req     | id                | Req     |
| query: action          | Req     |                   |         |
| head: bluemix-instance | Req     | -i, --instance-id | Req     |
| head: correlation-id   |         |                   |         |
| head: prefer           |         |                   |         |

#### Response - none
{: #api-cli-key-schedule-response}

There is no body response, check the HTTP status code.

The CLI **only** returns text output, indicating `SUCCESS` or `FAILED`.

### Action - unwrap a key
{: #api-cli-key-unwrap}

Unwrap a data encryption key (DEK) using a root key.

API: `POST /api/v2/keys/{id}/actions/unwrap`

CLI: `kp key unwrap`

#### Request
{: #api-cli-key-unwrap-request}

| API parameter          | API req | CLI parameter     | CLI req |
| ---------------------- | ------- | ----------------- | ------- |
| path: id               | Req     | id                | Req     |
| query: action          | Req     |                   |         |
| head: bluemix-instance | Req     | -i, --instance-id | Req     |
| head: correlation-id   |         |                   |         |
| head: prefer           |         |                   |         |
| body: aad              |         | -a, --aad         |         |
| body: ciphertext       | Req     | ciphertext        | Req     |
|                        |         | -o, --output      |         |

#### Response
{: #api-cli-key-unwrap-response}

| API JSON                         | API opt | CLI text            | CLI JSON            | CLI opt |
| -------------------------------- | ------- | ------------------- | ------------------- | ------- |
| ciphertext                       |         |                     |                     |         |
| keyVersion.creationDate          |         |                     |                     |         |
| keyVersion.id                    |         |                     |                     |         |
| plaintext                        |         | Plaintext           | Plaintext           |         |
| rewrappedKeyVersion.creationDate |         |                     |                     |         |
| rewrappedKeyVersion.id           |         |                     |                     |         |
|                                  |         | Rewrapped Plaintext | Rewrapped Plaintext |         |

It's not obvious how the API response is mapped to the `Rewrapped Plaintext`
field.
{: important}

### Action - wrap a key
{: #api-cli-key-wrap}

Wrap a data encryption key (DEK) using a root key.

API: `POST /api/v2/keys/{id}/actions/wrap`

CLI: `kp key wrap`

#### Request
{: #api-cli-key-wrap-request}

| API parameter          | API req | CLI parameter     | CLI req |
| ---------------------- | ------- | ----------------- | ------- |
| path: id               | Req     | id                | Req     |
| query: action          | Req     |                   |         |
| head: bluemix-instance | Req     | -i, --instance-id | Req     |
| head: correlation-id   |         |                   |         |
| head: prefer           |         |                   |         |
| body: aad              |         | -a, --aad         |         |
| body: plaintext        | Req     | -p, --plaintext   | Req     |
|                        |         | -o, --output      |         |

#### Response
{: #api-cli-key-wrap-response}

| API JSON                         | API opt | CLI text   | CLI JSON   | CLI opt |
| -------------------------------- | ------- | ---------- | ---------- | ------- |
| ciphertext                       |         | Ciphertext | Ciphertext |         |
| keyVersion.creationDate          |         |            |            |         |
| keyVersion.id                    |         |            |            |         |
| plaintext                        |         |            |            |         |
| rewrappedKeyVersion.creationDate |         |            |            |         |
| rewrappedKeyVersion.id           |         |            |            |         |

### Delete a key
{: #api-cli-key-delete}

Delete a key.

API: `DELETE /api/v2/keys/{id}`

CL: `kp key delete`

#### Request
{: #api-cli-key-delete-request}

| API parameter          | API req | CLI parameter     | CLI req |
| ---------------------- | ------- | ----------------- | ------- |
| path: id               | Req     | id                | Req     |
| query: force           |         | -f, --force       |         |
| head: bluemix-instance | Req     | -i, --instance-id | Req     |
| head: correlation-id   |         |                   |         |
| head: prefer           |         |                   |         |
|                        |         | -o, --output      |         |

#### Response
{: #api-cli-key-delete-response}

| API JSON                         | API opt | CLI text    | CLI JSON | CLI opt |
| -------------------------------- | ------- | ----------- | -------- | ------- |
| algorithmBitSize                 |         |             |          |         |
| algorithmMetadata.bitLength      |         |             |          |         |
| algorithmMetadata.mode           |         |             |          |         |
| algorithmMode                    |         |             |          |         |
| algorithmType                    |         |             |          |         |
| createdBy                        |         |             |          |         |
| creationDate                     |         |             |          |         |
| crn                              |         |             |          |         |
| deleted                          |         |             |          |         |
| deletedBy                        |         |             |          |         |
| deletionDate                     |         |             |          |         |
| description                      |         |             |          |         |
| dualAuthDelete.authExpiration    |         |             |          |         |
| dualAuthDelete.enabled           |         |             |          |         |
| dualAuthDelete.keySetForDeletion |         |             |          |         |
| extractable                      |         |             |          |         |
| id                               |         | Deleted Key | id       |         |
| imported                         |         |             |          |         |
| keyVersion.creationDate          |         |             |          |         |
| keyVersion.id                    |         |             |          |         |
| lastRotateDate                   |         |             |          |         |
| lastUpdateDate                   |         |             |          |         |
| name                             |         |             |          |         |
| nonactiveStateReason             |         |             |          |         |
| payload                          |         |             |          |         |
| state                            |         |             |          |         |
| tags                             |         |             |          |         |
| type                             |         |             |          |         |

### Retrieve key metadata
{: #api-cli-key-metadata}

Retrieve the details of a key.

API: `GET /api/v2/keys/{id}/metadata`

CLI: **not implemented**

#### Request
{: #api-cli-key-metadata-request}

| API parameter          | API req | CLI parameter | CLI req |
| ---------------------- | ------- | ------------- | ------- |
| path: id               | Req     |               |         |
| head: bluemix-instance | Req     |               |         |
| head: correlation-id   |         |               |         |

#### Response
{: #api-cli-key-metadata-response}

| API JSON                         | API opt | CLI text | CLI JSON | CLI opt |
| -------------------------------- | ------- | -------- | -------- | ------- |
| algorithmBitSize                 |         |          |          |         |
| algorithmMetadata.bitLength      |         |          |          |         |
| algorithmMetadata.mode           |         |          |          |         |
| algorithmMode                    |         |          |          |         |
| algorithmType                    |         |          |          |         |
| createdBy                        |         |          |          |         |
| creationDate                     |         |          |          |         |
| crn                              |         |          |          |         |
| deleted                          |         |          |          |         |
| deletedBy                        |         |          |          |         |
| deletionDate                     |         |          |          |         |
| description                      |         |          |          |         |
| dualAuthDelete.authExpiration    |         |          |          |         |
| dualAuthDelete.enabled           |         |          |          |         |
| dualAuthDelete.keySetForDeletion |         |          |          |         |
| extractable                      |         |          |          |         |
| id                               |         |          |          |         |
| imported                         |         |          |          |         |
| keyVersion.creationDate          |         |          |          |         |
| keyVersion.id                    |         |          |          |         |
| lastRotateDate                   |         |          |          |         |
| lastUpdateDate                   |         |          |          |         |
| name                             |         |          |          |         |
| nonactiveStateReason             |         |          |          |         |
| payload                          |         |          |          |         |
| state                            |         |          |          |         |
| tags                             |         |          |          |         |
| type                             |         |          |          |         |

### List key versions
{: #api-cli-key-versions}

Retrieve all versions of a root key.

When you rotate a root key, you generate a new version of the key. If you're
using the root key to protect resources across IBM Cloud, the registered cloud
services that you associate with the key use the latest key version to wrap your
data.

API: `GET /api/v2/keys/{id}/versions`

CLI: **not implemented**

#### Request
{: #api-cli-key-versions-request}

| API parameter          | API req | CLI parameter | CLI req |
| ---------------------- | ------- | ------------- | ------- |
| path: id               | Req     |               |         |
| query: limit           |         |               |         |
| query: offset          |         |               |         |
| head: bluemix-instance | Req     |               |         |
| head: correlation-id   |         |               |         |

#### Response
{: #api-cli-key-versions-response}

| API JSON     | API opt | CLI text | CLI JSON | CLI opt |
| ------------ | ------- | -------- | -------- | ------- |
| creationDate |         |          |          |         |
| id           |         |          |          |         |

## Policies
{: #api-cli-policy}

### Set key policy - dual authorization delete
{: #api-cli-key-policy-dual}

Creates or updates one or more policies

API: `PUT /api/v2/keys/{id}/policies`

CLI: `kp key policy-update dual-auth-delete`

#### Request
{: #api-cli-key-policy-dual-request}

| API parameter                | API req | CLI parameter     | CLI req |
| ---------------------------- | ------- | ----------------- | ------- |
| path: id                     | Req     | id                | Req     |
| query: policy                | Req     |                   |         |
| head: bluemix-instance       | Req     | -i, --instance-id | Req     |
| head: correlation-id         |         |                   |         |
| body: dualAuthDelete.enabled | Req     | -e, --enable      | Req     |
| body: type                   | Req     |                   |         |
|                              |         | -o, --output      |         |

#### Response
{: #api-cli-key-policy-dual-response}

| API JSON               | API opt | CLI text | CLI JSON               | CLI opt |
| ---------------------- | ------- | -------- | ---------------------- | ------- |
| createdBy              |         |          | createdBy              |         |
| creationDate           |         |          | creationDate           |         |
| crn                    |         |          | crn                    |         |
| dualAuthDelete.enabled |         |          | dualAuthDelete.enabled |         |
| id                     |         |          |                        |         |
| lastUpdateDate         |         |          | lastUpdateDate         |         |
| type                   |         |          |                        |         |
| updatedBy              |         |          | updatedBy              |         |

### Set key policy - rotation
{: #api-cli-key-policy-rotation}

API: `PUT /api/v2/keys/{id}/policies`

CLI: `kp key policy-update rotation`

#### Request
{: #api-cli-key-policy-rotation-request}

| API parameter                 | API req | CLI parameter          | CLI req |
| ----------------------------- | ------- | ---------------------- | ------- |
| path: id                      | Req     | id                     | Req     |
| query: policy                 | Req     |                        |         |
| head: bluemix-instance        | Req     | -i, --instance-id      | Req     |
| head: correlation-id          |         |                        |         |
| body: rotation.interval_month | Req     | -m, --monthly-interval | Req     |
| body: type                    | Req     |                        |         |
|                               |         | -o, --output           |         |

#### Response
{: #api-cli-key-policy-rotation-response}

| API JSON                | API opt | CLI text | CLI JSON                | CLI opt |
| ----------------------- | ------- | -------- | ----------------------- | ------- |
| createdBy               |         |          | createdBy               |         |
| creationDate            |         |          | creationDate            |         |
| crn                     |         |          | crn                     |         |
| id                      |         |          |                         |         |
| lastUpdateDate          |         |          | lastUpdateDate          |         |
| rotation.interval_month |         |          | rotation.interval_month |         |
| type                    |         |          |                         |         |
| updatedBy               |         |          | updatedBy               |         |

Why is `interval_month` the only parameter with underscore instead of camelCase?
{: important}

### List key policies
{: #api-cli-key-policy-list}

Retrieve details about a key policy, such as the key's automatic rotation
interval.

API: `GET /api/v2/keys/{id}/policies`

CLI: `kp key policies`

#### Request
{: #api-cli-key-policy-list-request}

| API parameter          | API req | CLI parameter     | CLI req |
| ---------------------- | ------- | ----------------- | ------- |
| path: id               | Req     | id                | Req     |
| query: policy          | Req     |                   |         |
| head: bluemix-instance | Req     | -i, --instance-id | Req     |
| head: correlation-id   |         |                   |         |
|                        |         | -o, --output      |         |

#### Response
{: #api-cli-key-policy-list-response}

| API JSON                | API opt | CLI text | CLI JSON                | CLI opt |
| ----------------------- | ------- | -------- | ----------------------- | ------- |
| createdBy               |         |          | createdBy               |         |
| creationDate            |         |          | creationDate            |         |
| crn                     |         |          | crn                     |         |
| dualAuthDelete.enabled  | Req     |          | dualAuthDelete.enabled  | Req     |
| id                      |         |          |                         |         |
| lastUpdateDate          |         |          | lastUpdateDate          |         |
| rotation.interval_month | Req     |          | rotation.interval_month | Req     |
| type                    |         |          |                         |         |
| updatedBy               |         |          | updatedBy               |         |

### Set instance policies - allowed network
{: #api-cli-instance-policy-allowed}

API: `PUT /api/v2/instance/policies`

CLI: `kp instance policy-update allowed-network`

#### Request
{: #api-cli-instance-policy-allowed-request}

| API parameter                                | API req | CLI parameter             | CLI req |
| -------------------------------------------- | ------- | ------------------------- | ------- |
| query: policy                                | Req     |                           |         |
| head: bluemix-instance                       | Req     | -i, --instance-id         | Req     |
| head: correlation-id                         |         |                           |         |
| body: policy_data.attributes.allowed_network | Req     | -t, --network-type        | Req     |
| body: policy_data.enabled                    | Req     | -d/-e, --disable/--enable | Req     |
| body: policy_type                            | Req     |                           |         |
|                                              |         | -o, --output              |         |

#### Response
{: #api-cli-instance-policy-allowed-response}

The response is an HTTP status code.

### Set instance policies - dual authorization delete
{: #api-cli-instance-policy-dual}

API: `PUT /api/v2/instance/policies`

CLI: `kp instance policy-update dual-auth-delete`

#### Request
{: #api-cli-instance-policy-dual-request}

| API parameter             | API req | CLI parameter             | CLI req |
| ------------------------- | ------- | ------------------------- | ------- |
| query: policy             | Req     |                           |         |
| head: bluemix-instance    | Req     | -i, --instance-id         | Req     |
| head: correlation-id      |         |                           |         |
| body: policy_data.enabled | Req     | -d/-e, --disable/--enable | Req     |
| body: policy_type         | Req     |                           |         |
|                           |         | -o, --output              |         |

#### Response
{: #api-cli-instance-policy-dual-response}

The response is an HTTP status code.

### List instance policies
{: #api-cli-instance-policy-list}

Retrieve details about instance policies, such as allowed networks
(public-and-private or private-only) and dual authorization delete (deleting a
key requires an authorization from two users).

API: `GET /api/v2/instance/policies`

CLI: `kp instance policies`

#### Request
{: #api-cli-instance-policy-list-request}

| API parameter          | API req | CLI parameter                               | CLI req |
| ---------------------- | ------- | ------------------------------------------- | ------- |
| query: policy          | Req     | -a/-d, --allowed-network/--dual-auth-delete |         |
| head: bluemix-instance | Req     | -i, --instance-id                           | Req     |
| head: correlation-id   |         |                                             |         |
|                        |         | -o, --output                                |         |

#### Response
{: #api-cli-instance-policy-list-response}

| API JSON                               | API opt | CLI text        | CLI JSON                               | CLI opt |
| -------------------------------------- | ------- | --------------- | -------------------------------------- | ------- |
| policy_type                            |         | Policy Type     | policy_type                            |         |
| policy_data.enabled                    |         | Enabled         | policy_data.enabled                    |         |
| policy_data.attributes.allowed_network | Req     | Network Allowed | policy_data.attributes.allowed_network |         |
| creationDate                           |         | Creation Date   | creationDate                           |         |
| createdBy                              |         | Created By      | createdBy                              |         |
| updatedBy                              |         | Updated By      | updatedBy                              |         |
| lastUpdated                            |         | Last Updated    | lastUpdated                            |         |

## Registrations
{: #api-cli-registration}

### Create a registration
{: #api-cli-registration-create}

Creates a registration between a root key and a cloud resource, such as a
Cloud Object Storage bucket.

The key is identified by its ID, and the cloud resource is identified by its
Cloud Resource Name (CRN).

API: `POST /api/v2/keys/{id}/registrations/{urlEncodedResourceCRN}`

CLI: **not implemented**

#### Request
{: #api-cli-registration-create-request}

| API parameter               | API req | CLI parameter | CLI req |
| --------------------------- | ------- | ------------- | ------- |
| path: id                    | Req     |               |         |
| path: urlEncodedResourceCRN | Req     |               |         |
| head: bluemix-instance      | Req     |               |         |
| head: correlation-id        |         |               |         |
| body: description           |         |               |         |
| body: preventKeyDeletion    | Req     |               |         |
| body: registrationMetadata  |         |               |         |

#### Response
{: #api-cli-registration-create-response}

| API JSON                | API opt | CLI text | CLI JSON | CLI opt |
| ----------------------- | ------- | -------- | -------- | ------- |
| createdBy               |         |          |          |         |
| creationDate            |         |          |          |         |
| description             |         |          |          |         |
| keyId                   |         |          |          |         |
| keyVersion.creationDate |         |          |          |         |
| keyVersion.id           |         |          |          |         |
| lastUpdated             |         |          |          |         |
| preventKeyDeletion      |         |          |          |         |
| registrationMetadata    |         |          |          |         |
| resourceCrn             |         |          |          |         |
| updatedBy               |         |          |          |         |

### Update a registration
{: #api-cli-registration-update}

This applies to service-to-service calls only.

Update an existing registration based on properties specified.

When you call PATCH /registrations,
{{site.data.keyword.keymanagementserviceshort}} updates only the properties that
you specify in the request entity-body.

To replace the registration, use PUT /registrations.

API: `PATCH /api/v2/keys/{id}/registrations/{urlEncodedResourceCRN}`

CLI: **not implemented**

#### Request
{: #api-cli-registration-update-request}

| API parameter               | API req | CLI parameter | CLI req |
| --------------------------- | ------- | ------------- | ------- |
| path: id                    | Req     |               |         |
| path: urlEncodedResourceCRN | Req     |               |         |
| head: bluemix-instance      | Req     |               |         |
| head: correlation-id        |         |               |         |
| body: description           |         |               |         |
| body: keyVersionId          |         |               |         |
| body: preventKeyDeletion    |         |               |         |
| body: registrationMetadata  |         |               |         |

#### Response
{: #api-cli-registration-update-response}

| API JSON                | API opt | CLI text | CLI JSON | CLI opt |
| ----------------------- | ------- | -------- | -------- | ------- |
| createdBy               |         |          |          |         |
| creationDate            |         |          |          |         |
| description             |         |          |          |         |
| keyId                   |         |          |          |         |
| keyVersion.creationDate |         |          |          |         |
| keyVersion.id           |         |          |          |         |
| lastUpdated             |         |          |          |         |
| preventKeyDeletion      |         |          |          |         |
| registrationMetadata    |         |          |          |         |
| resourceCrn             |         |          |          |         |
| updatedBy               |         |          |          |         |

### Replace a registration
{: #api-cli-registration-replace}

This applies to service-to-service calls only.

Replace an existing registration between a root key and a cloud resource.

The key is identified by the ID, and the cloud resource is identified by the
cloud resource name (CRN).

When you call PUT /registrations,
{{site.data.keyword.keymanagementserviceshort}} replaces the existing
registration with the new values from all required properties that you provide
in the request entity-body.

API: `PUT /api/v2/keys/{id}/registrations/{urlEncodedResourceCRN}`

CLI: **not implemented**

#### Request
{: #api-cli-registration-replace-request}

| API parameter               | API req | CLI parameter | CLI req |
| --------------------------- | ------- | ------------- | ------- |
| path: id                    | Req     |               |         |
| path: urlEncodedResourceCRN | Req     |               |         |
| head: bluemix-instance      | Req     |               |         |
| head: correlation-id        |         |               |         |
| body: description           |         |               |         |
| body: keyVersionId          |         |               |         |
| body: preventKeyDeletion    |         |               |         |
| body: registrationMetadata  |         |               |         |

#### Response
{: #api-cli-registration-replace-response}

| API JSON                | API opt | CLI text | CLI JSON | CLI opt |
| ----------------------- | ------- | -------- | -------- | ------- |
| createdBy               |         |          |          |         |
| creationDate            |         |          |          |         |
| description             |         |          |          |         |
| keyId                   |         |          |          |         |
| keyVersion.creationDate |         |          |          |         |
| keyVersion.id           |         |          |          |         |
| lastUpdated             |         |          |          |         |
| preventKeyDeletion      |         |          |          |         |
| registrationMetadata    |         |          |          |         |
| resourceCrn             |         |          |          |         |
| updatedBy               |         |          |          |         |

### Delete a registration
{: #api-cli-registration-delete}

This applies to service-to-service calls only.

Delete an existing registration between a root key and a cloud resource.

This action permanently removes the registration from the
{{site.data.keyword.keymanagementserviceshort}} database.

API: `DELETE /api/v2/keys/{id}/registrations/{urlEncodedResourceCRN}`

CLI: **not implemented**

#### Request
{: #api-cli-registration-delete-request}

| API parameter               | API req | CLI parameter | CLI req |
| --------------------------- | ------- | ------------- | ------- |
| path: id                    | Req     |               |         |
| path: urlEncodedResourceCRN | Req     |               |         |
| head: bluemix-instance      | Req     |               |         |
| head: correlation-id        |         |               |         |
| head: prefer                |         |               |         |

#### Response
{: #api-cli-registration-delete-response}

| API JSON                | API opt | CLI text | CLI JSON | CLI opt |
| ----------------------- | ------- | -------- | -------- | ------- |
| createdBy               |         |          |          |         |
| creationDate            |         |          |          |         |
| description             |         |          |          |         |
| keyId                   |         |          |          |         |
| keyVersion.creationDate |         |          |          |         |
| keyVersion.id           |         |          |          |         |
| lastUpdated             |         |          |          |         |
| preventKeyDeletion      |         |          |          |         |
| registrationMetadata    |         |          |          |         |
| resourceCrn             |         |          |          |         |
| updatedBy               |         |          |          |         |

### List registrations for a key
{: #api-cli-registration-key-list}

Retrieves a list of registrations that are associated with a specified root key.

When you use a root key to protect an IBM Cloud resource, such as a Cloud Object
Storage bucket, {{site.data.keyword.keymanagementserviceshort}} creates a
registration between the resource and root key.

You can use GET /keys/{id}/registrations to understand which cloud resources are
protected by the key that you specify.

API: `GET /api/v2/keys/{id}/registrations`

CLI: `kp registrations`

#### Request
{: #api-cli-registration-key-list-request}

| API parameter                | API req | CLI parameter     | CLI req |
| ---------------------------- | ------- | ----------------- | ------- |
| path: id                     | Req     | -k, --key-id      | Req     |
| query: limit                 |         |                   |         |
| query: offset                |         |                   |         |
| query: preventKeyDeletion    |         |                   |         |
| query: totalCount            |         |                   |         |
| query: urlEncodedResourceCRN |         | -c, --crn-query   |         |
| head: bluemix-instance       | Req     | -i, --instance-id | Req     |
| head: correlation-id         |         |                   |         |
|                              |         | -o, --output      |         |

#### Response
{: #api-cli-registration-key-list-response}

| API JSON                | API opt | CLI text | CLI JSON                | CLI opt |
| ----------------------- | ------- | -------- | ----------------------- | ------- |
| createdBy               |         |          | createdBy               |         |
| creationDate            |         |          | creationDate            |         |
| description             |         |          |                         |         |
| keyId                   |         |          | keyId                   |         |
| keyVersion.creationDate |         |          | keyVersion.creationDate |         |
| keyVersion.id           |         |          | keyVersion.id           |         |
| lastUpdated             |         |          | lastUpdated             |         |
| preventKeyDeletion      |         |          |                         |         |
| registrationMetadata    |         |          |                         |         |
| resourceCrn             |         |          | resourceCrn             |         |
| updatedBy               |         |          |                         |         |

### List registrations for any key
{: #api-cli-registration-list}

Retrieves a list of registrations that match the Cloud Resource Name (CRN) query
that you specify.

When you use a root key to protect an IBM Cloud resource, such as a Cloud Object
Storage bucket, {{site.data.keyword.keymanagementserviceshort}} creates a
registration between the resource and root key.

You can use GET /keys/registrations to understand which cloud resources are
protected by keys in your {{site.data.keyword.keymanagementserviceshort}}
instance.

API: `GET /api/v2/keys/registrations`

CLI: `kp registrations`

#### Request
{: #api-cli-registration-list-request}

| API parameter                | API req | CLI parameter     | CLI req |
| ---------------------------- | ------- | ----------------- | ------- |
| query: limit                 |         |                   |         |
| query: offset                |         |                   |         |
| query: preventKeyDeletion    |         |                   |         |
| query: totalCount            |         |                   |         |
| query: urlEncodedResourceCRN |         | -c, --crn-query   |         |
| head: bluemix-instance       | Req     | -i, --instance-id | Req     |
| head: correlation-id         |         |                   |         |
|                              |         | -o, --output      |         |

#### Response
{: #api-cli-registration-list-response}

| API JSON                | API opt | CLI text | CLI JSON                | CLI opt |
| ----------------------- | ------- | -------- | ----------------------- | ------- |
| createdBy               |         |          | createdBy               |         |
| creationDate            |         |          | creationDate            |         |
| description             |         |          |                         |         |
| keyId                   |         |          | keyId                   |         |
| keyVersion.creationDate |         |          | keyVersion.creationDate |         |
| keyVersion.id           |         |          | keyVersion.id           |         |
| lastUpdated             |         |          | lastUpdated             |         |
| preventKeyDeletion      |         |          |                         |         |
| registrationMetadata    |         |          |                         |         |
| resourceCrn             |         |          | resourceCrn             |         |
| updatedBy               |         |          |                         |         |


