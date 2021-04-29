---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-28"

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

# Managing dual authorization
{: #manage-dual-auth}

After you set up your {{site.data.keyword.keymanagementservicelong}} service
instance, you can manage dual authorization by using the service API.
{: shortdesc}

## Managing dual authorization settings
{: #manage-dual-auth-instance-policy}

Dual authorization for {{site.data.keyword.keymanagementserviceshort}} service
instances is an extra policy that helps to prevent accidental or malicious
deletion of keys. When you enable this policy at the instance level,
{{site.data.keyword.keymanagementserviceshort}} requires an authorization from
two users to delete any keys that are created after the policy is enabled.

The dual authorization capability is available only through the
{{site.data.keyword.keymanagementserviceshort}} API. To find out more about
accessing the {{site.data.keyword.keymanagementserviceshort}} APIs, check out
[Setting up the API](/docs/key-protect?topic=key-protect-set-up-api).
{: preview}

Before you enable dual authorization for your
{{site.data.keyword.keymanagementserviceshort}} instance, keep in mind the
following considerations:

- **When you enable dual authorization for your {{site.data.keyword.keymanagementserviceshort}} instance, the policy is applicable only for new keys.**
  By enabling dual authorization at the instance level, any new keys that you
  add to the instance will automatically inherit a dual authorization policy.
  Your existing keys are not affected by the policy change and will still
  require a single authorization for deletion.

- **You can always disable a dual authorization policy for your {{site.data.keyword.keymanagementserviceshort}} instance.**
  If you want to
  [disable an existing dual authorization policy](#disable-dual-auth-instance-policy-ui)
  to allow for single authorization, keep in mind that the change is applicable
  only for future keys that you add to the instance. Any existing keys that were
  created under a dual authorization policy will continue to require actions
  from two users before the keys can be deleted. After a key inherits a dual
  authorization policy, the policy cannot be reverted.

### Enabling dual authorization for your {{site.data.keyword.keymanagementserviceshort}} instance with the console
{: #enable-dual-auth-instance-policy-ui}

If you prefer to disable a dual authorization policy on your instance by using a
graphical interface, you can use the IBM Cloud console.

After creating a {{site.data.keyword.keymanagementserviceshort}} instance,
complete the following steps to create a dual authorization policy:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
   provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. Click the **Instance policies** link on the left side of the page.

   - Find the `Dual authorization delete` panel (on the top-left side of the
     page).

   - Toggle `Dual authorization deletion` to enable or disable the policy.

   - Click `Save` or `Cancel` (whichever is appropriate).

### Enabling dual authorization for your {{site.data.keyword.keymanagementserviceshort}} instance with the API
{: #enable-dual-auth-instance-policy-api}

As an instance manager, enable a dual authorization policy for a
{{site.data.keyword.keymanagementserviceshort}} instance by making a
`PUT` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable and disable dual authorization policies, you must be assigned a
    _Manager_ access policy for your
    {{site.data.keyword.keymanagementserviceshort}} instance. To learn how IAM
    roles map to {{site.data.keyword.keymanagementserviceshort}} service
    actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Enable a dual authorization policy for your
   {{site.data.keyword.keymanagementserviceshort}} instance by running the
   following `curl` command.

    ```sh
    $ curl -X PUT \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "x-kms-key-ring: <key_ring_ID>" \
        -H "content-type: application/vnd.ibm.kms.policy+json" \
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

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the key ring that the key is a part of.<br> If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is recommended to specify the key ring ID for a more optimized request.<br><br>Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: default.<br><br>For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
{: caption="Table 1. Describes the variables that are needed to enable dual authorization at the instance level." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which
indicates that your {{site.data.keyword.keymanagementserviceshort}} instance
is now enabled for dual authorization. Keys that you create or import to the
service now require two authorizations before they can be deleted. For more
information, see
[Deleting keys](/docs/key-protect?topic=key-protect-delete-keys).

This new policy does not affect existing keys in your instance. If you need
to enable dual authorization for an existing key, see
[Creating a dual authorization policy for a key](/docs/key-protect?topic=key-protect-set-dual-auth-key-policy#create-dual-auth-key-policy-api).
{: note}

### Optional: Verify dual auth policy enablement
{: #dual-auth-api-verify}

You can verify that a dual auth policy key has been enabled by issuing a list policies request:

 ```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete" \
    -H "accept: application/vnd.ibm.kms.policy+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Where the `<instance_ID>` is the name of your instance and your `<IAM_token>` is your IAM token.

### Disabling dual authorization for your {{site.data.keyword.keymanagementserviceshort}} instance with the console
{: #disable-dual-auth-instance-policy-ui}

If you prefer to disable a dual authorization policy on your instance by using a
graphical interface, you can use the IBM Cloud console.

After creating a {{site.data.keyword.keymanagementserviceshort}} instance,
complete the following steps to create a dual authorization policy:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
   provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the **Instance policies** page, use the **Policies** table to
   browse the policies in your {{site.data.keyword.keymanagementserviceshort}}
   instance.

5. Click the ⋯ icon to open a list of options for the policy that you want to
   disable.

6. From the options menu, click **Disable policy** and confirm the policy was
   disabled in the updated **Policies** table.

### Disabling dual authorization for your {{site.data.keyword.keymanagementserviceshort}} instance with the API
{: #disable-dual-auth-instance-policy-api}

As an instance manager, disable an existing dual authorization policy for a
{{site.data.keyword.keymanagementserviceshort}} instance by making a
`PUT` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable and disable dual authorization policies, you must be assigned a
    _Manager_ access policy for your
    {{site.data.keyword.keymanagementserviceshort}} instance. To learn how IAM
    roles map to {{site.data.keyword.keymanagementserviceshort}} service
    actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Disable an existing dual authorization policy for your
   {{site.data.keyword.keymanagementserviceshort}} instance by running the
   following `curl` command.

    ```sh
    $ curl -X PUT \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "x-kms-key-ring: <key_ring_ID>" \
        -H "content-type: application/vnd.ibm.kms.policy+json" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.policy+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                        "type": "application/vnd.ibm.kms.policy+json",
                        "dualAuthDelete": {
                            "enabled": false
                        }
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the key ring that the key is a part of.<br>If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is recommended to specify the key ring ID for a more optimized request.<br><br>Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: default.<br><br>For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
{: caption="Table 1. Describes the variables that are needed to enable dual authorization at the instance level." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which
indicates that the dual authorization policy was updated for your service
instance. Keys that you create or import to the service now require only one
authorization before they can be deleted. For more information, see
[Deleting keys](/docs/key-protect?topic=key-protect-delete-keys).

### Optional: Verify dual auth policy disablement
{: #dual-auth-disable-api-verify}

You can verify that a dual auth policy key has been disabled by issuing a list policies request:

 ```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete" \
    -H "accept: application/vnd.ibm.kms.policy+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Where the `<instance_ID>` is the name of your instance and your `<IAM_token>` is your IAM token.
