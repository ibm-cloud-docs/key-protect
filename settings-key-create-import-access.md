---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-01"

keywords: instance settings, service settings, key creation/import, key create policy, key creation/import, key policy

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

# Managing a key create and import access policy
{: #manage-keyCreateImportAccess}

After you set up your {{site.data.keyword.keymanagementservicelong}} service
instance, you can manage keyCreateImportAccess policies by using the
{{site.data.keyword.keymanagementservicelong}} service API.
{: shortdesc}

## Managing keyCreateImportAccess settings
{: #manage-keyCreateImportAccess-instance-policy}

A keyCreateImportAccess policy for
{{site.data.keyword.keymanagementserviceshort}} instances is an access policy
that you can use to restrict how keys are created and imported into your
{{site.data.keyword.keymanagementserviceshort}} instance.

When you enable this policy, {{site.data.keyword.keymanagementserviceshort}}
only permits the creation or importation of keys in your
{{site.data.keyword.keymanagementserviceshort}} instance that follow the key
creation and importation settings listed on the keyCreateImportAccess policy.

Setting and retrieving a keyCreateImportAccess policy is supported through the
application programming interface (API). KeyCreateImportAccess policy support
will be added to the user interface (UI), command line interface (CLI), and
software development kit (SDK) in the future. To find out more about accessing
the {{site.data.keyword.keymanagementserviceshort}} APIs, check out
[Setting up the API](/docs/key-protect?topic=key-protect-set-up-api).

Before you enable a keyCreateImportAccess policy for your
{{site.data.keyword.keymanagementserviceshort}} instance, keep in mind the
following considerations:

- **KeyCreateImportAccess policies do not affect keys that existed prior to policy creation.**
    KeyCreateImportAccess policies only affect
    {{site.data.keyword.keymanagementserviceshort}} requests that are sent after
    the policy is set. You will still have access to all keys that existed in your
    {{site.data.keyword.keymanagementserviceshort}} instance prior to policy
    creation.

- **KeyCreateImportAccess policies can affect your keys across various key actions.**
    The `enforce_token` attribute will affect imported keys during creation,
    rotation, and restoration. The `create_root_key`, `import_root_key`,
    `create_standard_key`, and `import_standard_key` attributes will only affect
    keys at creation time. All other
    {{site.data.keyword.keymanagementserviceshort}} actions (wrap, unwrap, etc.)
    are unaffected and can be invoked on the key as usual.

### Enabling and updating a keyCreateImportAccess policy for your {{site.data.keyword.keymanagementserviceshort}} instance with the console
{: #enable-keyCreateImportAccess-policy-console}

If you prefer to manage keyCreateImportAccess policy settings using
{{site.data.keyword.keymanagementserviceshort}}'s graphical interface,
you can use the IBM Cloud console.

If "enforce_token" is enabled, all import key actions will not be available in
the UI. The "enforce_token" option makes
[Secure Import](/docs/key-protect?topic=key-protect-create-import-tokens)
a requirement for all key imports. Secure Import support is only available
in the {{site.data.keyword.keymanagementserviceshort}} CLI or API.
{: note}

After creating a {{site.data.keyword.keymanagementserviceshort}} instance,
complete the following steps to enable a keyCreateImportAccess policy:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
    provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. Click the **Instance policies** link on the left side of the page.

    - Find the `Create and import access` panel (at the top of the page).

    - Enable or disable any keyCreateImportAccess settings you desire. Note that
        any create or import key actions that have been disabled will no longer be
        available via the "Add Key" modal.

    - Click `Save` or `Cancel` (whichever is appropriate).

### Enabling and updating a keyCreateImportAccess policy for your {{site.data.keyword.keymanagementserviceshort}} instance with the API
{: #enable-keyCreateImportAccess-policy-api}

As a security admin, you can enable or update a keyCreateImportAccess policy for
a {{site.data.keyword.keymanagementserviceshort}} instance by making a `PUT`
call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess
```
{: codeblock}

If you are updating your {{site.data.keyword.keymanagementserviceshort}}
instance's keyCreateImportAccess policy, keep in mind that if an attribute is
omitted from the request, the field will be set to the default value and the
existing value for the omitted field will be overwritten with the default value.
{: important}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable keyCreateImportAccess policies, you must be assigned a _Manager_
    access policy for your {{site.data.keyword.keymanagementserviceshort}}
    instance. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Enable or update a keyCreateImportAccess policy for your
    {{site.data.keyword.keymanagementserviceshort}} instance by running the
    following `curl` command.

    ```sh
    $ curl -X PUT \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.policy+json" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.policy+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                        "policy_type": "keyCreateImportAccess",
                        "policy_data": {
                            "enabled": true,
                            "attributes": {
                                "create_root_key": <true/false>,
                                "create_standard_key": <true/false>,
                                "import_root_key": <true/false>,
                                "import_standard_key": <true/false>,
                                "enforce_token": <true/false>
                            }
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
|create_root_key|**Required**. Set to true to allow root keys to be created in your {{site.data.keyword.keymanagementserviceshort}} instance. Set to false to prevent root keys from being created in your instance<br><br>Note: If omitted, POST /instance/policies will set this attribute to the default value (true).|
|create_standard_key|**Required**. Set to true to allow standard keys to be created in your {{site.data.keyword.keymanagementserviceshort}} instance. Set to false to prevent standard keys from being created in your instance.<br><br>Note: If omitted, POST /instance/policies will set this attribute to the default value (true).|
|import_root_key|**Required**. Set to true to allow root keys to be imported into your {{site.data.keyword.keymanagementserviceshort}} instance. Set to false to prevent root keys from being imported into your instance <br><br>Note: If omitted, POST /instance/policies will set this attribute to the default value (true).|
|import_standard_key|**Required**. Set to true to allow standard keys to be imported into your {{site.data.keyword.keymanagementserviceshort}} instance. Set to false to prevent standard keys from being imported into your instance.<br><br>Note: If omitted, POST /instance/policies will set this attribute to the default value (true).|
|enforce_token|**Required**. Set to `true` to prevent authorized users from importing key material into the your {{site.data.keyword.keymanagementserviceshort}} instance without using an import token. Set to `false` to allow authorized users to import key material into your instance without using an import token.<br><br>If enabled, this attribute will take precedence over the import_root_key and import_standard_key attributes.<br><br>Note: If omitted, POST /instance/policies will set this attribute to the default value (false).|
{: caption="Table 1. Describes the variables that are needed to enable a keyCreateImportAccess policy." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which
indicates that your {{site.data.keyword.keymanagementserviceshort}}
instance now has an enabled keyCreateImportAccess policy.
Your {{site.data.keyword.keymanagementserviceshort}} instance will now only
allow the creation or importation of keys from the methods specified in your
request.

### Optional: Verify key create import access policy enablement
{: #key-create-import-access-verify}

You can verify that a key create import access policy has been enabled by issuing a list policies request:

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess" \
    -H "accept: application/vnd.ibm.kms.policy+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Where the `<instance_ID>` is the name of your instance and your `<IAM_token>` is your IAM token.
### Disabling a keyCreateImportAccess policy for your {{site.data.keyword.keymanagementserviceshort}} instance
{: #disable-key-create-import-policy}

As a manager of a {{site.data.keyword.keymanagementserviceshort}} instance,
disable an existing keyCreateImportAccess policy for your
{{site.data.keyword.keymanagementserviceshort}} instance by making a `PUT` call
to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess
```
{: codeblock}

Do not provide any attributes when making a request to disable your
keyCreateImportAccess policy.
{: note}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To disable keyCreateImportAccess policies, you must be assigned a _Manager_
    access policy for your {{site.data.keyword.keymanagementserviceshort}}
    instance. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Disable an existing keyCreateImportAccess policy for your
    {{site.data.keyword.keymanagementserviceshort}} instance by running the
    following `curl` command.

    ```sh
    $ curl -X PUT \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.policy+json" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.policy+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                        "policy_type": "keyCreateImportAccess",
                        "policy_data": {
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
{: caption="Table 2. Describes the variables that are needed to disable a keyCreateImportAccess policy." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which
indicates that the keyCreateImportAccess policy was updated for your service
instance.

### Optional: Verify key create import access policy enablement
{: #key-create-import-access-disable-api-verify}

You can verify that a key create import access policy has been disabled by issuing a list policies request:

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess" \
    -H "accept: application/vnd.ibm.kms.policy+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Where the `<instance_ID>` is the name of your instance and your `<IAM_token>` is your IAM token.


