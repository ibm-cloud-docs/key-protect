---

copyright:
  years: 2017, 2020
lastupdated: "2020-05-27"

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

As a security officer, auditor, or manager, you can use the Activity Tracker
service to track how users and applications interact with
{{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}


{{site.data.keyword.at_full_notm}} records service and user initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can 
use the Activity Tracker service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be 
alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information 
regarding the Activity Tracker service, see the
[getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started).

To see which Key Protect API requests correlate to the actions below, [check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc](/apidocs/key-protect){: external}.

## Key events
{: #key-actions}

The following table lists the key actions that generate an event:

| Action                            | Description                                                  |
| --------------------------------- | ------------------------------------------------------------ |
| `kms.secrets.create`              | Create a key                                                 |
| `kms.secrets.delete`              | Delete a key                                                 |
| `kms.secrets.read`                | Retrieve all key information                                 |
| `kms.secrets.readmetadata`        | Retrieve key metadata (excluding key payload, if applicable) |
| `kms.secrets.head`                | Retrieve key total                                           |
| `kms.secrets.list`                | List keys                                                    |
| `kms.secrets.wrap`                | Wrap a key                                                   |
| `kms.secrets.unwrap`              | Unwrap a key                                                 |
| `kms.secrets.rewrap`              | Rewrap a key                                                 |
| `kms.secrets.rotate`              | Rotate a key                                                 |
| `kms.secrets.setkeyfordeletion`   | Authorize deletion for a key with Dual Authorization policy  |
| `kms.secrets.unsetkeyfordeletion` | Cancel deletion for a key with Dual Authorization policy     |
| `kms.secrets.restore`             | Restore a key                                                |
| `kms.secrets.listkeyversions`     | List all the versions of a key                               |
| `kms.secrets.enable`              | Enable operations for a key                                  |
| `kms.secrets.disable`             | Disable operations for a key                                 |
| `kms.secrets.eventack`            | Acknowledge a lifecycle action on a key                      |
| `kms.secrets.default`             | Invalid key request event                                    |
{: caption="Table 1. Lifecycle Key Actions" caption-side="top"}

## Policy events
{: #policy-actions}

The following table lists the policy actions that generate an event:

| Action                         | Description                                  |
| ------------------------------ | -------------------------------------------- | 
| `kms.policies.read`            | List key policies                            |
| `kms.policies.write`           | Set key policies                             |
| `kms.instancepolicies.read`    | List instance policies                       |
| `kms.instancepolicies.write`   | Set instance policies                        |
| `kms.policies.default`         | Invalid policy request event                 |
| `kms.instancepolicies.default` | Invalid policy request event                 |
{: caption="Table 2. Policy actions" caption-side="top"}

## Import token events
{: #import-token-actions}

The following table lists the import token actions that generate an event:

| Action                    | Description                            |
| ------------------------- | -------------------------------------- |
| `kms.importtoken.create`  | Create an import token                 |
| `kms.importtoken.read`    | Retrieve an import token               |
| `kms.importtoken.default` | Invalid import token request event     |
{: caption="Table 3. Import token actions" caption-side="top"}

## Registration events
{: #registration-actions}

The following table lists the registration actions that generate an event:

| Action                                  | Description                                              |
| --------------------------------------- | -------------------------------------------------------- |
| `kms.registrations.list`                | List registrations for any key                           |
| `kms.registrations.default`             | Invalid registration request event                       |
{: caption="Table 4. Registration actions" caption-side="top"}


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

Most successful requests have unique `requestData` and `responseData`associated with each related event. The following sections describe the data of each {{site.data.keyword.keymanagementserviceshort}} service action event.

Fields are not guaranteed to appear unless the request is successful.
{: note}

### Common Fields
{: #at-common fields}
There are some common fields that {{site.data.keyword.keymanagementserviceshort}} uses outside of the CADF event model to provide more insight into your data.

  <table>
    <tr>
      <th>Field</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><p><varname>`requestData.requestURI`</varname></p></td>
      <td>
        <p>the URI of the API request that was made.</p>
      </td>
    </tr>
    <tr>
      <td><p><varname>`requestData.instanceID`</varname></p></td>
      <td>
        <p>the unique identifier of your {{site.data.keyword.keymanagementserviceshort}} service instance.</p>
      </td>
    </tr>
     <tr>
      <td><p><varname>`correlationId`</varname></p></td>
      <td>
        <p>the unique identifier of the API request that generated the event.</p>
      </td>
    </tr>
    <caption style="caption-side:bottom;">Table 6. Describes the common fields in Activity Tracker events for {{site.data.keyword.keymanagementserviceshort}} service 
    actions.</caption>
  </table>

For more information on the event fields in the Cloud Auditing Data Federation (CADF) event model, see [Event Fields](https://test.cloud.ibm.com/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-event){: external}

While `initiator.host.address` is a field that is part of the Cloud Auditing Data Federation model, the host address field will not be shown for requests made through 
private networks.
{: important}

### Key action events
{: #key-action-events}

Due to the sensitivity of the information for an encryption key, when an event
is generated as a result of an API call to the {{site.data.keyword.keymanagementserviceshort}}
service, the event that is generated does not include detailed information about
the key, such as the payload and encrypted nonce.

The `responseData.keyState` field is an integer and corresponds to the Pre-activation = 0, Active = 1, Suspended = 2, Deactivated = 3, and Destroyed = 5 values.
For more information on key states, see [Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).
{: note}

#### Create Key
{: #create-key-success}

The following fields include extra information:

- The `requestData.keyType` field includes the type of key that was created.
- The `responseData.keyId` field includes the unique identifier associated with the key.
- The `responseData.keyVersionId` field includes the unique identifier of the current key version used to input ciphertext on wrap requests.
- The `responseData.keyVersionCreationDate` field includes the date that the current version of the key was created.
- The `responseData.keyState` field includes the integer that correlates to the state of the key.

#### Delete Key
{: #delete-key-success}

The following field includes extra information:

- The `responseData.keyState` field includes the integer that correlates to the state of the key.

#### Wrap or unwrap key
{: #wrap-unwrap-key-success}

The following field includes extra information:

- The `responseData.keyVersionId` field includes the unique identifier of the current key version used to input ciphertext on wrap requests.

#### Rewrap key
{: #create-key-success}

The following field includes extra information:

- The `responseData.keyVersionId` field includes the unique identifier of the current key version used to input ciphertext on wrap requests.
- The `responseData.rewrappedKeyVersionId` field includes the unique identifier of the new key version used to input ciphertext on wrap requests.

#### Restore key
{: #restore-key-success}

The following field includes extra information:

- The `responseData.keyVersionId` field includes the unique identifier of the current key version used to input ciphertext on wrap requests.

#### Rotate key
{: #rotate-key-success}

Rotate key doesn't have any additional fields outside of those from the [Common Fields](#at-common-fields) section
{: note}

#### Get key total
{: #list-head-success}

The following field includes extra information:

- The `responseData.totalResources` field includes the total amount of keys within the service instance.

#### List keys
{: #list-keys-success}

The following field includes extra information:

- The `responseData.totalResources` field includes the total amount of keys returned in the response.

#### Get key or key metadata
{: #get-key-success}

The following fields include extra information:

- The `requestData.keyType` field includes the type of key that was retrieved.
- The `responseData.keyState` field includes the integer that correlates to the state of the key.
- The `responseData.keyVersionId` field includes the unique identifier of the current key version used to input ciphertext on wrap requests.
- The `responseData.keyVersionCreationDate` field includes the date that the current version of the key was created.

#### List key versions
{: #list-key-versions-success}

The following field includes extra information:

- The `responseData.totalResources` field includes the total amount of key versions returned in the response.

#### Set or Unset key for deletion
{: #dual-auth-set-success}

The following fields include extra information:

- The `responseData.initialValue.authID` field includes the initiator ID of the person who set the dual 
authorization policy.
- The `responseData.initialValue.authExpiration` field includes the expiration date for the dual 
authorization policy.
- The `responseData.newValue.authID` field includes the initiator ID of the person who set the dual 
authorization policy.
- The `responseData.newValue.authExpiration` field includes the expiration date for the dual authorization 
policy.

`initialValue` is the initiatorID of the person who last set the dual authorization policy and `newValue is the 
new initiatorID of the person who set the dual authorization policy.
{: note}

### Policy events
{: #policy-at-events}

#### Set instance policies
{: #set-policy-success}

The following fields include extra information:

- The `requestData.initialValue.policyAllowedNetworkEnabled` field includes if your allowed network policy was previously enabled or disabled.
- The `requestData.initialValue.policyAllowedNetworkAttribute` field includes if your allowed network policy was previously only for public networks or both public 
and private networks.
- The `requestData.newValue.policyAllowedNetworkEnabled` field includes if your allowed network policy is currently enabled or disabled.
- The `requestData.newValue.policyAllowedNetworkAttribute` field includes if your allowed network policy is currently only for public networks or both public and 
private networks.
- The `requestData.initialValue.policyDualAuthDeleteEnabled` field includes if your dual auth delete policy was previously enabled or disabled.
- The `requestData.newValue.policyDualAuthDeleteEnabled` field includes if your dual auth delete policy is currently enabled or disabled.

### Import token events
{: #import-token-events}

#### Create import token
{: #create-import-token-success}

The following fields include extra information: 

- The `responseData.expirationDate` field includes the expiration date of the import token.
- The `responseData.maxAllowedRetrievals` field includes the maximum amount of times the import token can be retrieved within its expiration time before it is no 
longer accessible.

#### Retrieve import token
{: #retrieve-import-token-success}

The following fields include extra information: 

- The `responseData.maxAllowedRetrievals` field includes the maximum amount of times the import token can be retrieved within its expiration time before it is no 
longer accessible.
- The `responseData.remainingRetrievals` field includes the number of times the import token can be retrieved within its expiration time before it is no longer 
accessible.


### Key with registrations events
{: #key-registration-events}

#### Rotate Key
{: #rotate-key-registrations-success}

The following fields include extra information: 

- The `responseData.eventAckData.eventId` field includes the unique identifier that is associated with the event.
- The `responseData.eventAckData.eventType` field includes the type of lifecycle action that is associated with the event.
- The `responseData.eventAckData.newKeyVersionId` field includes the unique identifier of the latest key version used to input ciphertext on wrap requests.
- The `responseData.eventAckData.newKeyVersionCreationDate` field includes the date that the latest key version was created.
- The `responseData.eventAckData.oldKeyVersionId` field includes the unique identifier of the previous key version used to input ciphertext on wrap requests.
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

#### List registrations
{: #list-registration-success}

The following field includes extra information:

- The `responseData.totalResources` field includes the total amount of registrations returned in the response.
## Analyzing failed events
{: #at-events-analyze-failed}

### Unable to delete a key
{: #delete-key-failure}

If the delete key event has a `reason.reasonCode`of 409, the key could not be deleted because it is possibly protecting one or more cloud resources that have a retention 
policy. Make a GET request to `/keys/{id}/registrations` to learn which resources this key is associated with. A registration with `"preventKeyDeletion": true` 
indicates that the associated resource has a retention policy. To enable deletion, contact an account owner to remove the retention policy on each resource 
that is associated with this key.

A delete key event could also receive a `reason.reasonCode` of 409 due to a dual auth deletion policy on the key. Make a GET request to `/api/v2/keys/{id}/policies` to see if there is a 
dual authorization policy associated with your key or make a GET request to `/api/v2/instance/policies` to see if there is a dual 
authorization associated with your service instance. If there is a policy set, contact the other authorized user to delete the key.


### Unable to authenticate while make a request
{: #authenticate-failure}

If the event has a `reason.reasonCode` of 401, you may not have the correct authorization to perform {{site.data.keyword.keymanagementserviceshort}} actions in the specified service instance. Verify with an 
administrator that you are assigned the correct platform and service access roles in the applicable service instance. For more 
information about roles, see [Roles and permissions](/docs/key-protect?topic=key-protect-manage-access).

Check that you are using a valid token that is associated with an account authorized to perform the service action.
{: note}

### Unable to view or list keys in a service instance
{: #list-keys-failure}

If you make a call to `GET api/v2/keys` to list the keys that are available in
your service instance and `responseData.totalResources` is 0, you may need to query for keys in 
the deleted state using the state parameter or adjust the `offset` and `limit` parameters in 
your request.

### Lifecycle action on a key with registrations did not complete
{: #protected-resource-key-failure}

The `responseData.reasonForFailure` and `responseData.resourceCRN` fields contain information on why the action wasn't able to 
be completed. 

If the event has a `reason.reasonCode` of 409, the action could not be completed due to the adopting service's key state 
conflicting with the key state that {{site.data.keyword.keymanagementserviceshort}} has. 

If the event has a `reason.reasonCode` of 408, the action could not be completed because 
{site.data.keyword.keymanagementserviceshort}} was not notified that all appropriate actions were taken within 4 hours of the 
action request.

## Event Severity
{: event-severity}

The severity for all Activity Tracker events with
{{site.data.keyword.keymanagementserviceshort}} are based on the type of request
that was made, then status code. For example, if you make a create key request
with an invalid key, but you are also unauthenticated for the service instance
that you included in the request, the unauthentication will take precedence and
the event will be evaluated as a `401` bad request call with a severity of
`critical`.

The following table lists the actions associated with each severity level: 

<table>
    <tr>
      <th>Severity</th>
      <th>Actions</th>
    </tr>
    <tr>
      <td><p><varname>Critical</varname></p></td>
      <td>
        <p><code>kms.secrets.delete</code></p>
        <p><code>kms.registrations.delete</code></p>
      </td>
    </tr>
    <tr>
      <td><p><varname>Warning</varname></p></td>
      <td>
        <p><code>kms.secrets.rotate</code>, <code>kms.secrets.restore</code></p>
        <p><code>kms.secrets.enable</code>, <code>kms.secrets.disable</code></p>
        <p><code>kms.secrets.setkeyfordeletion</code>, <code>kms.secrets.unsetkeyfordeletion</code></p>
        <p><code>kms.policies.write</code>, <code>kms.instancepolicies.write</code></p>
      </td>
    </tr>
     <tr>
      <td><p><varname>Normal</varname></p></td>
      <td>
        <p><code>kms.secrets.create</code>, <code>kms.secrets.read</code></p>
        <p><code>kms.secrets.readmetadata</code>, <code>kms.secrets.head</code></p>
        <p><code>kms.secrets.list</code>, <code>kms.secrets.wrap</code></p>
        <p><code>kms.secrets.unwrap</code>, <code>kms.secrets.rewrap</code></p>
        <p><code>kms.secrets.listkeyversions</code>, <code>kms.secrets.eventack</code></p>
        <p><code>kms.policies.read</code>, <code>kms.instancepolicies.read</code></p>
        <p><code>kms.importtoken.create</code>, <code>kms.importtoken.read</code></p>
        <p><code>kms.registrations.list</code></p>
      </td>
    </tr>
    <caption style="caption-side:bottom;">Table 7. Describes the severity level for 
    {{site.data.keyword.keymanagementserviceshort}} service actions.</caption>
  </table>

The following table lists the status codes associated with each severity level: 

<table>
    <tr>
      <th>Severity</th>
      <th>Status Code</th>
    </tr>
    <tr>
      <td><p><varname>Critical</varname></p></td>
      <td>
        <p><code>503</code>, <code>507</code></p>
        <p><code>401</code>, <code>403</code></p>
      </td>
    </tr>
    <tr>
      <td><p><varname>Warning</varname></p></td>
      <td>
        <p><code>502</code>, <code>502</code>, <code>504</code></p>
        <p><code>505</code>, <code>400</code>, <code>409</code></p>
        <p><code>424</code></p>
      </td>
    </tr>
    <caption style="caption-side:bottom;">Table 8. Describes the severity level for 
    {{site.data.keyword.keymanagementserviceshort}} response status codes.</caption>
  </table>
