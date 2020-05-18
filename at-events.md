---

copyright:
  years: 2017, 2020
lastupdated: "2020-05-18"

keywords: IBM, activity tracker, LogDNA, event, security, KMS API calls, monitor KMS events

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
{:important: .important}

# Activity Tracker events
{: #at-events}

As a security officer, auditor, or manager, you can use the Activity Tracker service to track how users and applications interact with {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

**This content is currently being developed and reviewed.**

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. 

For more information, see the [getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started).


## Key events
{: #key-actions}

The following table lists the key actions that generate an event:

| Action                            | Description                             |
| --------------------------------- | --------------------------------------- |
| `kms.secrets.create`              | Create a key                            |
| `kms.secrets.read`                | Retrieve a key                          |
| `kms.secrets.readmetadata`        | Retrieve details about a key            |
| `kms.secrets.head`                | Retrieve key total                      |
| `kms.secrets.list`                | List keys                               |
| `kms.secrets.wrap`                | Wrap a key                              |
| `kms.secrets.unwrap`              | Unwrap a key                            |
| `kms.secrets.rewrap`              | Rewrap a key                            |
| `kms.secrets.rotate`              | Rotate a key                            |
| `kms.secrets.setkeyfordeletion`   | Authorize deletion for a key            |
| `kms.secrets.unsetkeyfordeletion` | Cancel deletion for a key               |
| `kms.secrets.restore`             | Restore a key                           |
| `kms.secrets.listkeyversions`     | List all the versions of a key          |
| `kms.secrets.enable`              | Enable operations for a key             |
| `kms.secrets.disable`             | Disable operations for a key            |
| `kms.secrets.eventack`            | Acknowledge a lifecycle action on a key |
| `kms.secrets.default`             | Failed key event
{: caption="Table 1. Lifecycle Key Actions" caption-side="top"}

## Policy events
{: #policy-actions}

The following table lists the key actions that generate an event:

| Action                         | Description            |
| ------------------------------ | ---------------------- |
| `kms.policies.read`            | List key policies      |
| `kms.policies.write`           | Set key policies       |
| `kms.instancepolicies.read`    | List instance policies |
| `kms.instancepolicies.write`   | Set an instance policy |
| `kms.policies.default`         | Failed policy event    |
| `kms.instancepolicies.default` | Failed policy event    |
{: caption="Table 2. Policy actions" caption-side="top"}

## Import token events
{: #import-token-actions}

The following table lists the key actions that generate an event:

| Action                    | Description               |
| ------------------------- | ------------------------- |
| `kms.importtoken.create`  | Create an import token    |
| `kms.importtoken.read`    | Retrieve an import token  |
| `kms.importtoken.deaults` | Failed import token event |
{: caption="Table 3. Import token actions" caption-side="top"}

## Key with registration events
{: #protected-resource-key-actions}

The following table lists the key actions that generate an event:

| Action                    | Description                                     |
| ------------------------- | ----------------------------------------------- |
| `kms.secrets.ack-delete`  | Delete a key with registrations                 |
| `kms.secrets.ack-restore` | Restore a key with registrations                |
| `kms.secrets.ack-rotate`  | Rotate an import token                          |
| `kms.secrets.ack-enable`  | Enable operations for a key with registrations  |
| `kms.secrets.ack-disable` | Disable operations for a key with registrations |
{: caption="Table 4. Lifecycle actions that involve keys that protect Cloud Resources" caption-side="top"}

## Registration events
{: #registration-actions}

The following table lists the key actions that generate an event:

| Action                                  | Description                                              |
| --------------------------------------- | -------------------------------------------------------- |
| `kms.registrations.create`[^services-1] | Create a registration between a key and a cloud resource |
| `kms.registrations.list`                | List registrations for any key                           |
| `kms.registrations.merge`[^services-2]  | Update a registration between a key and a cloud resource |
| `kms.registrations.write`[^services-3]  | Replace registration between a key and a cloud resource  |
| `kms.registrations.delete`[^services-4] | Delete registration between a key and a cloud resource   |
| `kms.registrations.default`             | Failed registration event                                |
{: caption="Table 5. Registration actions" caption-side="top"}

[^services-1]: This action is performed on your behalf by an [integrated service](/docs/key-protect?topic=key-protect-integrate-services) that has enabled support for key registration. [Learn more](/docs/key-protect?topic=key-protect-view-protected-resources)

[^services-2]: This action is performed on your behalf by an [integrated service](/docs/key-protect?topic=key-protect-integrate-services) that has enabled support for key registration. [Learn more](/docs/key-protect?topic=key-protect-view-protected-resources)

[^services-3]: This action is performed on your behalf by an [integrated service](/docs/key-protect?topic=key-protect-integrate-services) that has enabled support for key registration. [Learn more](/docs/key-protect?topic=key-protect-view-protected-resources)

[^services-4]: This action is performed on your behalf by an [integrated service](/docs/key-protect?topic=key-protect-integrate-services) that has enabled support for key registration. [Learn more](/docs/key-protect?topic=key-protect-view-protected-resources)


## Viewing events
{: #at-ui}

Events that are generated by an instance of {{site.data.keyword.keymanagementserviceshort}} are automatically forwarded to the 
{{site.data.keyword.at_full_notm}} service instance that is available in the same location. 

{{site.data.keyword.at_full_notm}} can have only one instance per location. To view events, you must access the web UI of the 
{{site.data.keyword.at_full_notm}} service in the same location where your service instance is available. For more information, 
see [Launching the web UI through the IBM Cloud UI](/docs/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2).

| Deployment Region         | Activity Tracker Region                         |
| ------------------------- | ----------------------------------------------- |
| `us-south`                | `us-south`                                      |
| `us-east`                 | `us-east`                                       |
| `eu-gb`                   | `eu-gb`                                         |
| `eu-de`                   | `eu-de`                                         |
| `au-syd`                  | `au-syd`                                        |
| `jp-tok`                  | `jp-tok`                                        |
{: caption="Table 5. Activity Tracker regions" caption-side="top"}

## Analyzing successful events
{: #at-events-analyze}

Due to the sensitivity of the information for an encryption key, when an event
is generated as a result of an API call to the {{site.data.keyword.keymanagementserviceshort}}
service, the event that is generated does not include detailed information about
the key.

The event includes a correlation ID that you can use to identify the
key internally in your cloud environment. The correlation ID is a field that is
returned as part of the `correlationId` field.

You can use this
information to correlate the {{site.data.keyword.keymanagementserviceshort}} key
with the information of the action that is reported through the
{{site.data.keyword.cloudaccesstrailshort}} event.

Requests made through private networks do not show the host address in Activity Tracker events.
{: note}

### Key action events
{: #key-action-events}

The `responseData.keyState` field is an integer and corresponds to the Pre-activation = 0, Active = 1, Suspended = 2, Deactivated = 3, and Destroyed = 5 values.
For more information on key states, see [Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).
{: note}

#### Create Key
{: #create-key-success}

The following fields include extra information:
- The `requestData.keyType` field includes the type of key that was created.
- The `responseData.keyId` field includes the unique identifier associated with the key.
- The `responseData.keyVersionId` field includes the unqiue idenitifier of the current key version.
- The `responseData.keyVersionCreationDate` field includes the date that the current version of the key was created.
- The `responseData.keyState` field includes the integer that correlates to the state of the key.

#### Delete Key
{: #delete-key-success}

The following field includes extra information:
- The `responseData.keyState` field includes the integer that correlates to the state of the key.

#### Wrap or unwrap key
{: #wrap-unwrap-key-success}

The following field includes extra information:
- The `responseData.keyVersionId` field includes the unqiue idenitifier of the current key version.

#### Rewrap key
{: #create-key-success}

The following field includes extra information:
- The `responseData.keyVersionId` field includes the unqiue idenitifier of the current key version.
- The `responseData.rewrappedKeyVersionId` field includes the unique identifier of the new key version associated with the ciphertext from the response.

#### Restore key
{: #restore-key-success}

The following field includes extra information:
- The `responseData.keyVersionId` field includes the unqiue idenitifier of the current key version.

#### List keys or get key total
{: #list-keys-success}

The following field includes extra information:
- The `responseData.totalResources` field includes the total amount of keys within the service instance.

#### Get key or key metadata
{: #get-key-success}

The following fields include extra information:
- The `requestData.keyType` field includes the type of key that was retrieved.
- The `responseData.keyState` field includes the integer that correlates to the state of the key.

#### List key versions
{: #list-key-versions-success}

The following field includes extra information:
- The `responseData.totalResources` field includes the total amount of key versions associated with the key.

#### Set or unset key for deletion
{: #dual-auth-success}

The following fields include extra information:
- The `responseData.authID` field includes the initiator ID of the person who set the dual authorization policy.
- The `responseData.authExpiration` field includes the expiration date for the dual authorization policy.

### Policy events
{: #policy-at-events}

#### Set instance policies
{: #set-policy-success}

The following fields include extra information:
- The `requestData.initialValue.policyAllowedNetworkEnabled` field includes if your allowed network policy was previously enabled or disabled.
- The `requestData.initialValue.policyAllowedNetworkAttribute` field includes if your allowed network policy was previously only for public networks or both public and private networks.
- The `requestData.newValue.policyAllowedNetworkEnabled` field includes if your allowed network policy is currently enabled or disabled.
- The `requestData.newValue.policyAllowedNetworkAttribute` field includes if your allowed netowrk policy is currently only for public networks or both public and private networks.
- The `requestData.initalValue.policyDualAuthDeleteEnabled` field includes if your dual auth delete policy was previously enabled or disabled.
- The `requestData.newValue.policyDualAuthDeleteEnabled` field includes if your dual auth delete policy is currently enabled or disabled.

### Import token events
{: #import-token-events}

#### Create import token
{: #create-import-token-success}

The following fields include extra information: 
- The `responseData.expirationDate` field includes the expiration date of the import token.
- The `responseData.maxAllowedRetrievals` field includes the maximum amount of times the import token can be retrieved within its expiration time before it is no longer accessible.

#### Retrieve import token
{: #retrieve-import-token-success}

The following fields include extra information: 
- The `responseData.maxAllowedRetrievals` field includes the maximum amount of times the import token can be retrieved within its expiration time before it is no longer accessible
- The `responseData.remainingRetrievals` field includes the number of times the import token can be retrieved within its expiration time before it is no longer accessible.


### Key with registrations events
{: #key-registration-events}

#### Rotate Key
{: #rotate-key-registrations-success}

The following fields include extra information: 
- The `responseData.eventAckData.newKeyVersionId` field includes the unique identifier of the latest key version.
- The `responseData.eventAckData.newKeyVersionCreationDate` field includes the date that the latest key version was created.
- The `responseData.eventAckData.oldKeyVersionId` field includes the unique identifier of the previous key version.
- The `responseData.eventAckData.oldKeyVersionCreationDate` field includes the date that the previous key version was created.

#### Restore Key
{: #restore-key-registrations-success}

The following fields include extra information: 
- The `responseData.eventAckData.eventId` field includes the unique identifier that is associated with the event.
- The `responseData.eventAckData.eventType` field includes the type of lifecycle action that is associated with the event.
- The `responseData.eventAckData.keyState` field includes the integer that correlates to the state of the key associated with the event.
- The `responseData.eventAckData.eventAckTimeStamp` field includes the date and time that the event was acknowledged.

#### Enable Key
{: #enable-key-registrations-success}

The following fields include extra information: 
- The `responseData.eventAckData.eventId` field includes the unique identifier that is associated with the event.
- The `responseData.eventAckData.eventType` field includes the type of lifecycle action that is associated with the event.
- The `responseData.eventAckData.keyState` field includes the integer that correlates to the state of the key associated with the event.
- The `responseData.eventAckData.eventAckTimeStamp` field includes the date and time that the event was acknowledged.

#### Disable key
{: #disable-key-registration-success}

The following fields include extra information: 
- The `responseData.eventAckData.eventId` field includes the unique identifier that is associated with the event.
- The `responseData.eventAckData.eventType` field includes the type of lifecycle action that is associated with the event.
- The `responseData.eventAckData.keyState` field includes the integer that correlates to the state of the key associated with the event.
- The `responseData.eventAckData.eventAckTimeStamp` field includes the date and time that the event was acknowledged.

### Registration events
{: #registration-events}

#### Create registration
{: #create-registration-success}

The following fields include extra information: 
- The `requestData.preventKeyDeletion` field is set to true if the registration prevents the associated key from being deleted or false if the registration doesn't prevent the associated key from being deleted.
- The `responseData.keyVersionId` field includes the unqiue idenitifier of the key version.
- The `responseData.keyVersionCreationDate` field includes the date that the version of the key was created.

#### Replace Registration
{: #replace-registration-success}

The following fields include extra information: 
- The `requestData.initialValue.preventKeyDeletion` field is set to true if the registration previously prevented the associated key from being deleted or false if the registration didn't previously prevent the associated key from being deleted.
- The `requestData.keyVersionId` field includes the unqiue idenitifier of the previous key version.
- The `requestData.newValue.preventKeyDeletion` field is set to true if the registration prevents the associated key from being deleted or false if the registration doesn't prevent the associated key from being deleted.
- The `requestData.newValue.keyVersionId` field includes the unqiue idenitifier of the current key version.

#### Update Registration
{: #update-registration-success}

The following fields include extra information: 
- The `responseData.keyVersionId` field includes the unqiue idenitifier of the current key version.
- The `responseData.keyVersionCreationDate` field includes the date that the current version of the key was created.

#### Delete Registration
{: #delete-registration-success}

The following fields include extra information: 
- The `requestData.preventKeyDeletion` field is set to true if the registration prevents the associated key from being deleted or false if the registration doesn't prevent the associated key from being deleted.
- The `responseData.keyVersionId` field includes the unqiue idenitifier of the current key version.
- The `responseData.keyVersionCreationDate` field includes the date that the current version of the key was created.

#### List registration
{: #list-registration-success}

The following field includes extra information:
- The `responseData.totalResources` field includes the total amount of key versions associated with the key.

## Analyzing failed events
{: #at-events-analyze-failed}

### Unable to delete a key
{: #delete-key-failure}

If the delete key event has a `reason.reasonCode`of 409, the key could not be deleted because it is protecting one or more cloud resources that have a retention policy. Make a GET request to `/keys/{id}/registrations` to learn which resources this key is associated with. A registration with `"preventKeyDeletion": true` indicates 
that the associated resource has a retention policy. To enable deletion, contact an account owner to remove the retention policy on each resource that is associated with 
this key.

### Unable to authenticate while make a request
{: #authenticate-failure}

If the event has a `reason.reasonCode` of 401, you do not have the correct authorization to perform Key Protect actions in the specified service instance. Verify with an 
administrator that you are assigned the correct platform and service access roles in the applicable service instance. For more information about roles, see [Roles and permissions](/docs/key-protect?topic=key-protect-manage-access).

### Unable to view or list keys in a service instance
{: #list-keys-failure}

If you make a call to `GET api/v2/keys` to list the keys that are available in your service instance and `responseData.totalResources` is 0, you may not have the correct authorization to view the requested range of keys. Contact an administrator to check your permissions. If the service instance contains keys that you're unable to view, verify that you're assigned the applicable [level of access](/docs/key-protect?topic=key-protect-grant-access-keys) to keys in the service instance. If the service instance contains more than 200 keys, you need to use the offset and limit parameters to list another subset of keys.

### Severity is at critical level for an event

The severity for all Activity Tracker events with Key Protect are based on the type of request that was made, then status code. For example, if you make a delete key request with an invalid key, but you are also unathenticated for the service instance that you included in the request, the unathentication will take precedence and the event will be evaluated as a `401` bad request call with a severity of `critical`.