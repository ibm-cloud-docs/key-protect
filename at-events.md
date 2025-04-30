---

copyright:
  years: 2017, 2025
lastupdated: "2025-04-30"

keywords: kp event monitoring, key actions, monitor kp events

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:term: .term}
{:important: .important}

# Activity tracking events using 
{: #at-events}

As a security officer, auditor, or manager, you can use the {{site.data.keyword.logs_full_notm}}
service to track how users and applications interact with
{{site.data.keyword.keymanagementservicefull}}. 
{: shortdesc}

{{site.data.keyword.logs_full_notm}} records service and user initiated activities
that change the state of a service in {{site.data.keyword.cloud_notm}}. You can
use this service to investigate abnormal activity and critical actions and to
comply with regulatory audit requirements. In addition, you can be alerted about
actions as they happen. The events that are collected comply with the Cloud
Auditing Data Federation (CADF) standard.

For more information regarding the {{site.data.keyword.logs_full_notm}} service, check out [About {{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-about-cl){: external}.

To determine which {{site.data.keyword.keymanagementserviceshort}} API requests
correlate to the actions below, see
[{{site.data.keyword.keymanagementserviceshort}} API reference doc](/apidocs/key-protect){: external}.

## Historical information regarding events
{: #historical-mapping-events}

| Reference                          | Current event names                   |
| ---------------------------------- | ------------------------------------- |
| `kms.governance.configread`        | `kms.governance-config.read`          |
| `kms.importtoken.create`           | `kms.import-token.create`             |
| `kms.importtoken.read`             | `kms.import-token.read`               |
| `kms.importtoken.default`          | `kms.import-token.request`            |
| `kms.instance.readallowedipport`   | `kms.instance-allowed-ip-port.read`   |
| `kms.instance.readipwhitelistport` | `kms.instance-ip-allowlist-port.read` |
| `kms.instancepolicies.write`       | `kms.instance-policies.write`         |
| `kms.instancepolicies.read`        | `kms.instance-policies.read`          |
| `kms.instancepolicies.default`     | `kms.instance-policies.request`       |
| `kms.keyrings.create`              | `kms.key-rings.create`                |
| `kms.keyrings.delete`              | `kms.key-rings.delete`                |
| `kms.keyrings.list`                | `kms.key-rings.list`                  |
| `kms.keyrings.default`             | `kms.key-rings.request`               |
| `kms.secrets.defaultalias`         | `kms.secrets-alias.request`           |
| `kms.secrets.createalias`          | `kms.secrets-alias.create`            |
| `kms.secrets.deletealias`          | `kms.secrets-alias.delete`            |
| `kms.secrets.eventack`             | `kms.secrets-event.ack`               |
| `kms.secrets.listkeyversions`      | `kms.secrets-key-versions.list`       |
| `kms.secrets.readmetadata`         | `kms.secrets-metadata.read`           |
{: caption="Table 1. Historical reference mapping of event names" caption-side="bottom"}

## Key events
{: #key-actions}

The following table lists the key actions that generate an event:

| Action                            | Description                                                  |
| --------------------------------- | ------------------------------------------------------------ |
| `kms.secrets.create`              | Create a key                                                 |
| `kms.secrets-alias.create`         | Create a key alias                                           |
| `kms.secrets.default`             | Invalid key request event                                    |
| `kms.secrets.delete`              | Delete a key                                                 |
| `kms.secrets-alias.delete`         | Delete a key alias                                           |
| `kms.secrets.disable`             | Disable operations for a key                                 |
| `kms.secrets.enable`              | Enable operations for a key                                  |
| `kms.secrets-event.ack`            | Acknowledge a lifecycle action on a key                      |
| `kms.secrets.expire`              | Expire a key                                                 |
| `kms.secrets.head`                | Retrieve key total                                           |
| `kms.secrets.list`                | List keys                                                    |
| `kms.secrets-key-versions.list`     | List all the versions of a key                               |
| `kms.secrets.wrap`                | Wrap a key                                                   |
| `kms.secrets.patch`               | Patch a key                                                  |
| `kms.secrets.purge`               | Purge a key                                                  |
| `kms.secrets.read`                | Retrieve all key information                                 |
| `kms.secrets-metadata.read`        | Retrieve key metadata (excluding key payload, if applicable) |
| `kms.secrets.restore`             | Restore a key                                                |
| `kms.secrets.rewrap`              | Rewrap a key                                                 |
| `kms.secrets.rotate`              | Rotate a key                                                 |
| `kms.secrets.setkeyfordeletion`   | Authorize deletion for a key with Dual Authorization policy  |
| `kms.secrets.unsetkeyfordeletion` | Cancel deletion for a key with Dual Authorization policy     |
| `kms.secrets.unwrap`              | Unwrap a key                                                 |
{: caption="Table 2. Lifecycle Key Actions" caption-side="bottom"}

## Key Ring events
{: #keyring-actions}

The following table lists the key ring actions that generate an event:

| Action                         | Description                   |
| ------------------------------ | ----------------------------- |
| `kms.key-rings.create`          | Create a key ring             |
| `kms.key-rings.delete`          | Delete a key ring             |
| `kms.key-rings.list`            | List key rings in an instance |
| `kms.key-rings.request`         | Invalid key ring request      |
{: caption="Table 3. Key Ring actions" caption-side="bottom"}

## Policy events
{: #policy-actions}

The following table lists the policy actions that generate an event:

| Action                         | Description                  |
| ------------------------------ | ---------------------------- |
| `kms.policies.read`            | List key policies            |
| `kms.policies.write`           | Set key policies             |
| `kms.instance-policies.read`    | List instance policies       |
| `kms.instance-policies.write`   | Set instance policies        |
| `kms.policies.default`         | Invalid policy request event |
| `kms.instance-policies.request` | Invalid policy request event |
{: caption="Table 4. Policy actions" caption-side="bottom"}

## Import token events
{: #import-token-actions}

The following table lists the import token actions that generate an event:

| Action                    | Description                        |
| ------------------------- | ---------------------------------- |
| `kms.import-token.create`  | Create an import token             |
| `kms.import-token.read`    | Retrieve an import token           |
| `kms.import-token.request` | Invalid import token request event |
{: caption="Table 5. Import token actions" caption-side="bottom"}

## Registration events
{: #registration-actions}

The corresponding actions to both list registration for any key, or list registration for a specific key generate this event.
{: note}

The following table lists the registration actions that generate an event:

| Action                                  | Description                                              |
| --------------------------------------- | -------------------------------------------------------- |
| `kms.registrations.list`                | List registrations for any key                           |
| `kms.registrations.default`             | Invalid registration request event                       |
{: caption="Table 6. Registration actions" caption-side="bottom"}



## Viewing events
{: #at-ui}

Events that are generated by an instance of
{{site.data.keyword.keymanagementserviceshort}} are automatically forwarded to
the {{site.data.keyword.logs_full_notm}} instance that is available in the
same location.

{{site.data.keyword.logs_full_notm}} can have only one instance per location. To
view events, you must access the web UI of the
{{site.data.keyword.logs_full_notm}} service in the same location where your
{{site.data.keyword.keymanagementserviceshort}} instance is available.

| Deployment Region | {{site.data.keyword.logs_full_notm}} Region |
| ----------------- | ----------------------- |
| `au-syd`          | `au-syd`                |
| `br-sao`          | `br-sao`                |
| `ca-tor`          | `ca-tor`                |
| `eu-de`           | `eu-de`                 |
| `eu-gb`           | `eu-gb`                 |
| `jp-osa`          | `jp-osa`                |
| `jp-tok`          | `jp-tok`                |
| `us-east`         | `us-east`               |
| `us-south`        | `us-south`              |
{: caption="Table 7. {{site.data.keyword.logs_full_notm}} regions" caption-side="bottom"}

## Analyzing successful events
{: #at-events-analyze}

Most successful requests have unique `requestData` and `responseData` associated
with each related event. The following sections describe the data of each
{{site.data.keyword.keymanagementserviceshort}} service action event.

Fields are not guaranteed to appear unless the request is successful.
{: note}

### Common Fields
{: #at-events-common-fields}

There are some common fields that
{{site.data.keyword.keymanagementserviceshort}} uses outside of the CADF event
model to provide more insight into your data.

|Field|Description|
|--- |--- |
|requestData.requestURI|The [URI](#x2116436){: term} of the API request that was made.|
|requestData.instanceID|The unique identifier of your {{site.data.keyword.keymanagementserviceshort}} instance.|
|correlationId|The unique identifier of the API request that generated the event.|
{: caption="Table 8. Describes the common fields in {{site.data.keyword.logs_full_notm}} events for {{site.data.keyword.keymanagementserviceshort}} service actions." caption-side="bottom"}

For more information on the event fields in the Cloud Auditing Data Federation
(CADF) event model, see
[Activity tracking events for {{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-at_events){: external}

While `initiator.host.address` is a field that is part of the Cloud Auditing
Data Federation model, the host address field will not be shown for requests
made through private networks.
{: important}

### Conditional Common Fields
{: #at-events-conditional-fields}

There are some conditional fields that
{{site.data.keyword.keymanagementserviceshort}} uses outside of the CADF event
model to provide more insight into your data when you use the key ring feature. 
These fields will appear when using key rings in specific conditions. 

| Field                  | Description                                         |
| ---------------------- | --------------------------------------------------- |
| requestData.keyRingId  | The ID of the key ring if a key ring is specified in the header (`X-Kms-Key-Ring`) of the request. |
| responseData.keyRingId | The ID of the key ring if a key ring is specified in the request or in API methods where the key ID (or alias) is in the path of the request [URI](#x2116436){: term}. |
{: caption="Table 9. Describes conditional fields for use with key rings in {{site.data.keyword.logs_full_notm}} events for {{site.data.keyword.keymanagementserviceshort}} service actions." caption-side="bottom"}

### Key action events
{: #key-action-events}

Due to the sensitivity of the information for an encryption key, when an event
is generated as a result of an API call to the
{{site.data.keyword.keymanagementserviceshort}} service, the event that is
generated does not include detailed information about the key, such as the
payload and encrypted nonce.

The `responseData.keyState` field is an integer and corresponds to the
Pre-activation = 0, Active = 1, Suspended = 2, Deactivated = 3, and Destroyed =
5 values. For more information on key states, see
[Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).
{: note}

#### Create Key
{: #create-key-success}

The following fields include extra information:

- The `requestData.keyType` field includes the type of key that was created.

- The `responseData.keyId` field includes the unique identifier associated with
    the key.

- The `responseData.keyVersionId` field includes the unique identifier of the
    current key version used to wrap input ciphertext on wrap requests.

- The `responseData.keyVersionCreationDate` field includes the date that the
    current version of the key was created.

- The `responseData.keyState` field includes the integer that correlates to the
    state of the key.

- The `responseData.expirationDate` includes the date that the key will expire
    on.

#### Delete Key
{: #delete-key-success}

The following field includes extra information:

- The `responseData.keyState` field includes the integer that correlates to the
    state of the key.

#### Expire Key
{: #expire-key-success}

The following field includes extra information:

- The `requestData.keyType` field includes the type of key that was created.

- The `responseData.keyId` field includes the unique identifier associated with
    the key.

- The `requestData.expirationDate` field includes the date that the key expired
    on.

- The `responseData.initialValue.keyState` field includes the integer that
    correlates to the previous state of the key.

- The `responseData.newValue.keyState` field includes the integer that
    correlates to the current state of the key.

#### Wrap or unwrap key
{: #wrap-unwrap-key-success}

The following field includes extra information:

- The `responseData.keyVersionId` field includes the unique identifier of the
    current key version used to wrap input ciphertext on wrap requests.

- The `responseData.expirationDate` includes the date that the key will expire
    on.

#### Rewrap key
{: #rewrap-key-success}

The following field includes extra information:

- The `responseData.keyVersionId` field includes the unique identifier of the
    current key version used to wrap input ciphertext on wrap requests.

- The `responseData.rewrappedKeyVersionId` field includes the unique identifier
    of the new key version used to wrap input ciphertext on wrap requests.

#### Restore key
{: #restore-key-success}

The following field includes extra information:

- The `responseData.keyVersionId` field includes the unique identifier of the
    current key version used to wrap input ciphertext on wrap requests.

#### Rotate key
{: #rotate-key-success}

Rotate key doesn't have any additional fields outside of those from the
[Common Fields](#at-events-common-fields)
section
{: note}

#### Patch key
{: #patch-key}

The following fields include extra information:

- The `requestData.initialValue.keyRingId` field includes the ID of the key ring that
    the key previously was a part of.

- The `requestData.newValue.keyRingId` field includes the ID of the key ring that the
    key is currently a part of.

#### Purge key
{: #purge-key}

The following fields include extra information:

- The `responseData.deletionDate` field represents the date the key was deleted.

- The `responseData.purgeAllowedFrom` field represents the date from which the purge action was allowed.

- The `responseData.purgeEligibleOn` field represents the date on which the key metadata is eligible to be permanently removed from the system by {{site.data.keyword.keymanagementserviceshort}}.

#### Get key total
{: #list-head-success}

The following field includes extra information:

- The `responseData.totalResources` field includes the total amount of keys
    within the {{site.data.keyword.keymanagementserviceshort}} instance.

#### List keys
{: #list-keys-success}

The following field includes extra information:

- The `responseData.totalResources` field includes the total amount of keys
    returned in the response.

#### Get key or key metadata
{: #get-key-success}

The following fields include extra information:

- The `requestData.keyType` field includes the type of key that was retrieved.

- The `responseData.keyState` field includes the integer that correlates to the
    state of the key.

- The `responseData.keyVersionId` field includes the unique identifier of the
    key version used to wrap input ciphertext on wrap requests.

- The `responseData.keyVersionCreationDate` field includes the date that the
    current version of the key was created.

- The `responseData.expirationDate` includes the date that the key will expire
    on.

#### List key versions
{: #list-key-versions-success}

The following field includes extra information:

- The `responseData.totalResources` field includes the total amount of key
    versions returned in the response.

#### Set or Unset key for deletion
{: #dual-auth-set-success}

The following fields include extra information:

- The `responseData.initialValue.authID` field includes the initiator ID of the
    person who set the dual authorization policy.

- The `responseData.initialValue.authExpiration` field includes the expiration
    date for the dual authorization policy.

- The `responseData.newValue.authID` field includes the initiator ID of the
    person who set the dual authorization policy.

- The `responseData.newValue.authExpiration` field includes the expiration date
    for the dual authorization policy.

`initialValue` is the initiatorID of the person who last set the dual
authorization policy and `newValue` is the new initiatorID of the person who set
the dual authorization policy.
{: note}

### Policy events
{: #policy-at-events}

#### Set instance policies
{: #set-policy-success}

##### Allowed Network Policies
{: #allowed-network-event}

The following fields include extra information:

- The `requestData.initialValue.policyAllowedNetworkEnabled` field includes if
    your allowed network policy was previously enabled or disabled.

- The `requestData.initialValue.policyAllowedNetworkAttribute` field includes if
    your allowed network policy was previously only for public networks or both
    public and private networks.

- The `requestData.newValue.policyAllowedNetworkEnabled` field includes if your
    allowed network policy is currently enabled or disabled.

- The `requestData.newValue.policyAllowedNetworkAttribute` field includes if
    your allowed network policy is currently only for public networks or both
    public and private networks.

##### Dual Auth Delete Policies
{: #dual-auth-event}

The following fields include extra information:

- The `requestData.initialValue.policyDualAuthDeleteEnabled` field includes if
    your dual auth delete policy was previously enabled or disabled.

- The `requestData.newValue.policyDualAuthDeleteEnabled` field includes if your
    dual auth delete policy is currently enabled or disabled.

##### Allowed IP Policies
{: #allowed-ip-event}

The following fields include extra information:

- The `requestData.initialValue.policyAllowedIPAttribute` field includes if
    your allowed IP policy was previously enabled or disabled.

- The `requestData.newValue.policyAllowedIPAttribute` field includes if
    your allowed IP policy is currently enabled or disabled.

##### Key Creation and Importation Access Policies
{: #allowed-key-creation-policy}

The following fields include extra information:

- The `requestData.initialValue.PolicyKCIAEnabled` field includes if
    your key creation and importation policy was previously enabled or disabled.

- The `requestData.newValue.PolicyKCIAEnabled` field includes if
    your key creation and importation policy is currently enabled or disabled.

- The `requestData.initialValue.PolicyKCIAAttrCRK` field includes if
    your key creation and importation policy previously allowed the creation of
    root keys.

- The `requestData.newValue.PolicyKCIAAttrCRK` field includes if
    your key creation and importation policy allows the creation of root keys.

- The `requestData.initialValue.PolicyKCIAAttrCSK` field includes if
    your key creation and importation policy previously allowed the creation of
    standard keys.

- The `requestData.newValue.PolicyKCIAAttrCSK` field includes if
    your key creation and importation policy allows the creation of standard keys.

- The `requestData.initialValue.PolicyKCIAAttrIRK` field includes if
    your key creation and importation policy previously allowed imported root
    keys.

- The `requestData.newValue.PolicyKCIAAttrIRK` field includes if
    your key creation and importation policy allows imported root keys.

- The `requestData.initialValue.PolicyKCIAAttrISK` field includes if
    your key creation and importation policy previously allowed imported standard
    keys.

- The `requestData.newValue.PolicyKCIAAttrISK` field includes if
    your key creation and importation policy allows imported standard keys.

- The `requestData.initialValue.PolicyKCIAAttrET` field includes if
    your key creation and importation policy previously required keys to be
    imported via import token.

- The `requestData.newValue.PolicyKCIAAttrET` field includes if
    your key creation and importation policy requires keys to be imported via
    import token.

### Import token events
{: #import-token-events}

#### Create import token
{: #create-import-token-success}

The following fields include extra information:

- The `responseData.expirationDate` field includes the expiration date of the
    import token.

- The `responseData.maxAllowedRetrievals` field includes the maximum amount of
    times the import token can be retrieved within its expiration time before it
    is no longer accessible.

#### Retrieve import token
{: #retrieve-import-token-success}

The following fields include extra information:

- The `responseData.maxAllowedRetrievals` field includes the maximum amount of
    times the import token can be retrieved within its expiration time before it
    is no longer accessible.

- The `responseData.remainingRetrievals` field includes the number of times the
    import token can be retrieved within its expiration time before it is no
    longer accessible.



#### Completed action of a key rotation
{: #rotate-key-registrations-success}

The following fields include extra information:

- The `responseData.eventAckData.eventId` field includes the unique identifier
    that is associated with the event.

- The `responseData.eventAckData.eventType` field includes the type of lifecycle
    action that is associated with the event.

- The `responseData.eventAckData.newKeyVersionId` field includes the unique
    identifier of the latest key version used to wrap input ciphertext on wrap
    requests.

- The `responseData.eventAckData.newKeyVersionCreationDate` field includes the
    date that the latest key version was created.

- The `responseData.eventAckData.oldKeyVersionId` field includes the unique
    identifier of the previous key version used to wrap input ciphertext on wrap
    requests.

- The `responseData.eventAckData.oldKeyVersionCreationDate` field includes the
    date that the previous key version was created.

#### Completed action of key restoration
{: #restore-key-registrations-success}

The following fields include extra information:

- The `responseData.eventAckData.eventId` field includes the unique identifier
    that is associated with the event.

- The `responseData.eventAckData.eventType` field includes the type of lifecycle
    action that is associated with the event.

- The `responseData.eventAckData.keyState` field includes the integer that
    correlates to the state of the key associated with the event.

- The `responseData.eventAckData.eventAckTimeStamp` field includes the date and
    time that the event was acknowledged.

#### Completed action of an enabled key
{: #enable-key-registrations-success}

The following fields include extra information:

- The `responseData.eventAckData.eventId` field includes the unique identifier
    that is associated with the event.

- The `responseData.eventAckData.eventType` field includes the type of lifecycle
    action that is associated with the event.

- The `responseData.eventAckData.keyState` field includes the integer that
    correlates to the state of the key associated with the event.

- The `responseData.eventAckData.eventAckTimeStamp` field includes the date and
    time that the event was acknowledged.

#### Completed action of a disabled key
{: #disable-key-registration-success}

The following fields include extra information:

- The `responseData.eventAckData.eventId` field includes the unique identifier
    that is associated with the event.

- The `responseData.eventAckData.eventType` field includes the type of lifecycle
    action that is associated with the event.

- The `responseData.eventAckData.keyState` field includes the integer that
    correlates to the state of the key associated with the event.

- The `responseData.eventAckData.eventAckTimeStamp` field includes the date and
    time that the event was acknowledged.

### KMIP management events
{: #at-events-kmip-management}

The following table lists the KMIP management actions that generate an event:

| Action | Description |
| - | - |
| `kms.kmip-management.create` | Create a KMIP adapter or KMIP adapter client certificate. |
| `kms.kmip-management.delete` | Delete a KMIP adapter, KMIP adapter client certificate, or KMIP object. |
| `kms.kmip-management.list` | List KMIP adapters, KMIP adapter client certificates, or KMIP objects. |
| `kms.kmip-management.read` | Get a KMIP adapter, KMIP adapter client certificate, or KMIP object. |
| `kms.kmip-management.default` | Invalid KMIP managmenet API request. |

### KMIP events
{: #at-events-kmip-events}

The following table lists the KMIP protocol operation actions that generate an event:

| Action | Description |
| - | - |
| `kms.kmip.create` | Create a KMIP symmetric key, or add a KMIP attribute to a KMIP object. |
| `kms.kmip.get` | Get a KMIP symmetric key, KMIP attributes, or supported KMIP protocol versions from the server. |
| `kms.kmip.activate` | Activate a KMIP object. |
| `kms.kmip.revoke` | Revoke a KMIP object. |
| `kms.kmip.destroy` | Destroy a KMIP object. |
| `kms.kmip.locate` | List KMIP objects. |
| `kms.kmip.default` | Invalid KMIP operation request. |

## Analyzing failed events
{: #at-events-analyze-failed}

### Unable to delete a key
{: #delete-key-failure}

If the delete key event has a `reason.reasonCode` of 409, the key could not be
deleted because it is possibly protecting one or more cloud resources that have
a retention policy. Make a GET request to `/keys/{id}/registrations` to learn
which resources this key is associated with. A registration with
`"preventKeyDeletion": true` indicates that the associated resource has a
retention policy. To enable deletion, contact an account owner to remove the
retention policy on each resource that is associated with this key.

A delete key event could also receive a `reason.reasonCode` of 409 due to a dual
auth deletion policy on the key. Make a GET request to
`/api/v2/keys/{id}/policies` to see if there is a dual authorization policy
associated with your key. If there is a policy set, contact the other authorized
user to schedule the key for deletion.

### Unable to authenticate while make a request
{: #authenticate-failure}

If the event has a `reason.reasonCode` of 401, you do not have the correct
authorization to perform {{site.data.keyword.keymanagementserviceshort}} actions
in the specified {{site.data.keyword.keymanagementserviceshort}} instance.
Verify with an administrator that you are assigned the correct platform and
service access roles in the applicable service instance. For more information
about roles, see
[Roles and permissions](/docs/key-protect?topic=key-protect-manage-access).

Check that you are using a valid token that is associated with an account
authorized to perform the service action.
{: note}

### Unable to view or list keys in a {{site.data.keyword.keymanagementserviceshort}} instance
{: #list-keys-failure}

If you make a call to `GET api/v2/keys` to list the keys that are available in
your {{site.data.keyword.keymanagementserviceshort}} instance and
`responseData.totalResources` is 0, you may need to query for keys in the
deleted state using the `state` parameter or adjust the `offset` and `limit`
parameters in your request.

### Lifecycle action on a key with registrations did not complete
{: #protected-resource-key-failure}

The `responseData.reasonForFailure` and `responseData.resourceCRN` fields
contain information on why the action wasn't able to be completed.

If the event has a `reason.reasonCode` of 409, the action could not be completed
due to the adopting service's key state conflicting with the key state that
{{site.data.keyword.keymanagementserviceshort}} has.

If the event has a `reason.reasonCode` of 408, the action could not be completed
because {{site.data.keyword.keymanagementserviceshort}} was not notified that
all appropriate actions were taken within 4 hours of the action request.

## Event Severity
{: #at-events-event-severity}

The severity for all {{site.data.keyword.logs_full_notm}} events with
{{site.data.keyword.keymanagementserviceshort}} are based on the type of request
that was made, then status code. For example, if you make a create key request
with an invalid key, but you are also unauthenticated for the
{{site.data.keyword.keymanagementserviceshort}} instance that you included in
the request, the unauthentication will take precedence and the event will be
evaluated as a `401` bad request call with a severity of `critical`.

{{site.data.keyword.keymanagementserviceshort}} returns a 401
`reason.resasonCode` for unauthorized/forbidden
{{site.data.keyword.keymanagementserviceshort}} service requests.
{: important}

The following table lists the actions associated with each severity level:

| Severity | Actions |
|--------- |-------- |
| Critical | kms.secrets.delete  \n  kms.registrations.delete|
| Warning  | kms.secrets.rotate, kms.secrets.restore  \n  kms.secrets.enable, kms.secrets.disable  \n  kms.secrets.setkeyfordeletion, kms.secrets.unsetkeyfordeletion  \n  kms.policies.write, kms.instance-policies.write|
| Normal   | kms.secrets.create, kms.secrets.read  \n  kms.secrets-metadata.read, kms.secrets.head  \n  kms.secrets.list, kms.secrets.wrap  \n  kms.secrets.unwrap, kms.secrets.rewrap  \n  kms.secrets-key-versions.list, kms.secrets-event.ack  \n  kms.policies.read, kms.instance-policies.read  \n  kms.import-token.create, kms.import-token.read  \n  kms.registrations.create, kms.registrations.write  \n  kms.registrations.merge, kms.registrations.list  \n  kms.secrets.ack-delete, kms.secrets.ack-restore  \n  kms.secrets.ack-rotate, kms.secrets.ack-enable  \n  kms.secrets.ack-disable |
{: caption="Table 10. Describes the severity level for {{site.data.keyword.keymanagementserviceshort}} service actions." caption-side="bottom"}

The following table lists the status codes associated with each severity level:

| Severity | Status Code                  |
| -------- | ---------------------------- |
| Critical | 401, 403, 503, 507           |
| Warning  | 400, 409, 424, 502, 504, 505 |
{: caption="Table 11. Describes the severity level for {{site.data.keyword.keymanagementserviceshort}} response status codes." caption-side="bottom"}

