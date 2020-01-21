---

copyright:
  years: 2020
lastupdated: "2020-01-15"

keywords: instance settings, service settings, dual authorization

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
{:preview: .preview}

# Managing your service settings
{: #manage-settings}

After you set up your {{site.data.keyword.keymanagementservicelong}} service instance, you can manage advanced preferences by using the service API.
{: shortdesc}

## Managing dual authorization settings
{: #manage-dual-auth-instance-policies}

Dual authorization for {{site.data.keyword.keymanagementserviceshort}} service instances is an extra policy that helps to prevent accidental or malicious deletion of keys. When you enable this policy at the instance level, {{site.data.keyword.keymanagementserviceshort}} requires an authorization from two users to delete any keys that are created after the policy is enabled.

The dual authorization capability is available only through the {{site.data.keyword.keymanagementserviceshort}} API. To find out more about accessing the {{site.data.keyword.keymanagementserviceshort}} APIs, check out [Setting up the API](/docs/key-protect?topic=key-protect-set-up-api).
{: preview}

Before you enable dual authorization for your service instance, keep in mind the following considerations:

- **When you enable dual authorization for your service instance, the policy is applicable only for new keys.** By enabling dual authorization at the instance level, any new keys that you add to the instance will automatically inherit a dual authorization policy. Your existing keys are not affected by the policy change and will still require a single authorization for deletion.
- **You can always disable a dual authorization policy for your service instance.** If you want to [disable an existing dual authorization policy](#disable-dual-auth-instance-policy) to allow for single authorization, keep in mind that the change is applicable only for future keys that you add to the instance. Any existing keys that were created under a dual authorization policy will continue to require actions from two users before the keys can be deleted. After a key inherits a dual authorization policy, the policy cannot be reverted.

### Enabling dual authorization for your service instance
{: #enable-dual-auth-instance-policy}

As an instance manager, enable a dual authorization policy for a {{site.data.keyword.keymanagementserviceshort}} service instance by making a `PUT` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable and disable dual authorization policies, you must be assigned a _Manager_ access policy for your service instance. To learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}} service actions, check out [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Enable a dual authorization policy for your service instance by running the following cURL command.

    ```cURL
    curl -X PUT \
      'https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete' \
      -H 'accept: application/vnd.ibm.kms.policy+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
        "metadata": {
          "collectionType": "application/vnd.ibm.kms.policy+json",
          "collectionTotal": 1
        },
        "resources": [
          {
            "policy_type": "dualAuthDelete",
            "policy_data": {
              "enabled": true
            }
          }
        ]
      }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to enable dual authorization at the instance level.</caption>
    </table>

    A successful request returns an HTTP `204 No Content` response, which indicates that your service instance is now enabled for dual authorization. Keys that you create or import to the service now require two authorizations before they can be deleted. For more information, see [Deleting keys](/docs/key-protect/key-protect?topic=key-protect-delete-keys).

    This new policy does not affect existing keys in your instance. If you need to enable dual authorization for an existing key, see [Creating a dual authorization policy for a key](/docs/key-protect?topic=key-protect-set-dual-auth-key-policy#create-dual-auth-key-policy-api).
    {: note}

3. Optional: Verify that the dual authorization policy was created by browsing the policies that are available for your {{site.data.keyword.keymanagementserviceshort}} service instance.

    ```cURL
    curl -X GET \
    https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'accept: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}
  
### Disabling dual authorization for your service instance
{: #disable-dual-auth-instance-policy}

As an instance manager, disable an existing dual authorization policy for a {{site.data.keyword.keymanagementserviceshort}} service instance by making a `PUT` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable and disable dual authorization policies, you must be assigned a _Manager_ access policy for your service instance. To learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}} service actions, check out [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Disable an existing dual authorization policy for your service instance by running the following cURL command.

    ```cURL
    curl -X PUT \
      'https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete' \
      -H 'accept: application/vnd.ibm.kms.policy+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
        "metadata": {
          "collectionType": "application/vnd.ibm.kms.policy+json",
          "collectionTotal": 1
        },
        "resources": [
        {
          "type": "application/vnd.ibm.kms.policy+json",
           dualAuthDelete: {
            "enabled": false
            }
          }
        ]
      }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to enable dual authorization at the instance level.</caption>
    </table>

    A successful request returns an HTTP `204 No Content` response, which indicates that the dual authorization policy was updated for your service instance. Keys that you create or import to the service now require only one authorization before they can be deleted. For more information, see [Deleting keys](/docs/key-protect/key-protect?topic=key-protect-delete-keys).

3. Optional: Verify that the dual authorization policy was updated by browsing the policies that are available for your {{site.data.keyword.keymanagementserviceshort}} service instance.

    ```cURL
    curl -X GET \
    https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'accept: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}
