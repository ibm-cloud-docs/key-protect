---

copyright:
  years: 2020
lastupdated: "2020-09-10"

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

# Managing a keyCreateImportAccess policy
{: #manage-keyCreateImportAccess}

After you set up your {{site.data.keyword.keymanagementservicelong}} service
instance, you can manage keyCreateImportAccess policies by using the
{{site.data.keyword.keymanagementservicelong}} service API.
{: shortdesc}

## Managing keyCreateImportAccess settings
{: #manage-keyCreateImportAccess-instance-policy}

A keyCreateImportAccess policy for {{site.data.keyword.keymanagementserviceshort}}
instances is an access policy that you can use to restrict how keys are created and imported into
your {{site.data.keyword.keymanagementserviceshort}} instance. When you enable this policy,
{{site.data.keyword.keymanagementserviceshort}} only permits the creation or importation of keys in your 
{{site.data.keyword.keymanagementserviceshort}} instance that follow
the key creation and importation settings listed on the keyCreateImportAccess policy.

Setting and retrieving a keyCreateImportAccess policy is supported through the
application programming interface (API). KeyCreateImportAccess policy support will be
added to the user interface (UI), command line interface (CLI), and software
development kit (SDK) in the future. To find out more about
accessing the {{site.data.keyword.keymanagementserviceshort}} APIs, check out
[Setting up the API](/docs/key-protect?topic=key-protect-set-up-api).

Before you enable a keyCreateImportAccess policy for your
{{site.data.keyword.keymanagementserviceshort}} instance, keep in mind the
following considerations:

- **KeyCreateImportAccess policies do not affect keys that existed prior to policy creation.**
  KeyCreateImportAccess policies only affect Key Protect requests that are sent after the policy is set. You will still have access to all keys that existed in your Key Protect instance prior to policy creation.

- **KeyCreateImportAccess policies can affect your keys across various key actions.**
  The `enforce_token` attribute will affect imported keys during creation, rotation, and restoration. The `create_root_key`, `import_root_key`, 
  `create_standard_key`, and `import_standard_key` attributes will only affect keys at creation time. All other {{site.data.keyword.keymanagementserviceshort}} 
  actions (wrap, unwrap, etc.) are unaffected and can be invoked on the key as usual.
  
### Enabling and updating a keyCreateImportAccess policy for your {{site.data.keyword.keymanagementserviceshort}} instance
{: #enable-keyCreateImportAccess-policy}

As a security admin, you can enable or update a keyCreateImportAccess policy for a
{{site.data.keyword.keymanagementserviceshort}} instance by making a `PUT` call
to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess
```
{: codeblock}

If you are updating your {{site.data.keyword.keymanagementserviceshort}}
instance's keyCreateImportAccess policy, keep in mind that if an attribute is omitted from the request, the field will be set to the default value 
and the existing value for the omitted field will be overwritten.
{: important}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable keyCreateImportAccess policies, you must be assigned a _Manager_
    access policy for your {{site.data.keyword.keymanagementserviceshort}}
    instance. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Enable or update a keyCreateImportAccess policy for your
   {{site.data.keyword.keymanagementserviceshort}} instance by running the
   following cURL command.

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

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>

      <tr>
        <td>
          <varname>region</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The region abbreviation, such as
            <code>us-south</code> or <code>eu-gb</code>, that represents the
            geographic area where your
            {{site.data.keyword.keymanagementserviceshort}} instance
            resides.
          </p>
          <p>
            For more information, see
            [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>IAM_token</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> Your {{site.data.keyword.cloud_notm}}
            access token. Include the full contents of the <code>IAM</code>
            token, including the Bearer value, in the cURL request.
          </p>
          <p>
            For more information, see
            [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>instance_ID</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The unique identifier that is assigned to
            your {{site.data.keyword.keymanagementserviceshort}} service
            instance.
          </p>
          <p>
            For more information, see
            [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>create_root_key</varname>
        </td>
        <td>
          <strong>Required.</strong> Set to <code>true</code> to allow root keys to be created
          in your {{site.data.keyword.keymanagementserviceshort}} instance. Set to <code>false</code> to 
          prevent root keys from being created in your instance.

          Note: If omitted, `POST /instance/policies` will set this attribute to the default value (`true`).
        </td>
      </tr>

      <tr>
        <td>
          <varname>create_standard_key</varname>
        </td>
        <td>
          <strong>Required.</strong> Set to <code>true</code> to allow standard keys to be created
          in your {{site.data.keyword.keymanagementserviceshort}} instance. Set to <code>false</code> to 
          prevent standard keys from being created in your instance.

          Note: If omitted, `POST /instance/policies` will set this attribute to the default value (`true`).
        </td>
      </tr>

      <tr>
        <td>
          <varname>import_root_key</varname>
        </td>
        <td>
          <strong>Required.</strong> Set to <code>true</code> to allow root keys to be imported
          into your {{site.data.keyword.keymanagementserviceshort}} instance. Set to <code>false</code> to 
          prevent root keys from being imported into your instance.

          Note: If omitted, `POST /instance/policies` will set this attribute to the default value (`true`).
        </td>
      </tr>

      <tr>
        <td>
          <varname>import_standard_key</varname>
        </td>
        <td>
          <strong>Required.</strong> Set to <code>true</code> to allow standard keys to be imported
          into your {{site.data.keyword.keymanagementserviceshort}} instance. Set to <code>false</code> to 
          prevent standard keys from being imported into your instance.

          Note: If omitted, `POST /instance/policies` will set this attribute to the default value (`true`).
        </td>
      </tr>

      <tr>
        <td>
          <varname>enforce_token</varname>
        </td>
        <td>
          <strong>Required.</strong> Set to <code>true</code> to prevent authorized users from importing key material 
          into the your {{site.data.keyword.keymanagementserviceshort}} instance without using an import token. Set to <code>false</code> to 
          allow authorized users to import key material into your instance without using an import token.

          If enabled, this attribute will take precedence over the `import_root_key` and `import_standard_key` attributes.

          Note: If omitted, `POST /instance/policies` will set this attribute to the default value (`false`).
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to enable a keyCreateImportAccess
        policy.
      </caption>
    </table>

    A successful request returns an HTTP `204 No Content` response, which
    indicates that your {{site.data.keyword.keymanagementserviceshort}}
    instance now has an enabled keyCreateImportAccess policy.
    Your {{site.data.keyword.keymanagementserviceshort}} instance will now only
    allow the creation or importation of keys from the methods specified in your request.

3. Optional: Verify that the keyCreateImportAccess policy was created/updated by retrieving 
   the policy details for your {{site.data.keyword.keymanagementserviceshort}} instance.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

### Disabling a keyCreateImportAccess policy for your {{site.data.keyword.keymanagementserviceshort}} instance
{: #disable-key-create-import-policy}

As a manager of a {{site.data.keyword.keymanagementserviceshort}} instance,
disable an existing keyCreateImportAccess policy for your
{{site.data.keyword.keymanagementserviceshort}} instance by making a `PUT` call
to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess
```
{: codeblock}

Do not provide any attributes when making a request to disable your keyCreateImportAccess policy.
{:note}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To disable keyCreateImportAccess policies, you must be assigned a _Manager_ access
    policy for your {{site.data.keyword.keymanagementserviceshort}} instance. To
    learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}}
    service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Disable an existing keyCreateImportAccess policy for your
   {{site.data.keyword.keymanagementserviceshort}} instance by running the
   following cURL command.

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

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>

      <tr>
        <td>
          <varname>region</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The region abbreviation, such as
            <code>us-south</code> or <code>eu-gb</code>, that represents the
            geographic area where your
            {{site.data.keyword.keymanagementserviceshort}} instance
            resides.
          </p>
          <p>
            For more information, see
            [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>IAM_token</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> Your {{site.data.keyword.cloud_notm}}
            access token. Include the full contents of the <code>IAM</code>
            token, including the Bearer value, in the cURL request.
          </p>
          <p>
            For more information, see
            [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>instance_ID</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The unique identifier that is assigned to
            your {{site.data.keyword.keymanagementserviceshort}} service
            instance.
          </p>
          <p>
            For more information, see
            [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 2. Describes the variables that are needed to disable a keyCreateImportAccess
        policy.
      </caption>
    </table>

    A successful request returns an HTTP `204 No Content` response, which
    indicates that the keyCreateImportAccess policy was updated for your service
    instance.

3. Optional: Verify that the keyCreateImportAccess policy was disabled by retrieving 
   the policy details for your {{site.data.keyword.keymanagementserviceshort}} instance.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}
    