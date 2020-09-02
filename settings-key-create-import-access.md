---

copyright:
  years: 2020
lastupdated: "2020-09-01"

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

# Managing a key creation/import policy
{: #manage-key-creation-import}

After you set up your {{site.data.keyword.keymanagementservicelong}} service
instance, you can manage key creation/import policies by using the
{{site.data.keyword.keymanagementservicelong}} service API.
{: shortdesc}

## Managing key creation/import settings
{: #manage-key-creation-instance-policy}

Key creation/import for {{site.data.keyword.keymanagementserviceshort}}
instances is an access policy that you can use to restrict how keys are created and imported in
your {{site.data.keyword.keymanagementserviceshort}} instance. When you enable this policy,
{{site.data.keyword.keymanagementserviceshort}} only permits the creation or import of keys in your 
{{site.data.keyword.keymanagementserviceshort}} instance that follow
the key creation and import settings listed on the key creation/import policy.

Setting and retrieving a key creation/import policy is supported through the
application programming interface (API). Key creation/import policy support will be
added to the user interface (UI), command line interface (CLI), and software
development kit (SDK) in the future. To find out more about
accessing the {{site.data.keyword.keymanagementserviceshort}} APIs, check out
[Setting up the API](/docs/key-protect?topic=key-protect-set-up-api).

Before you enable a key creation/import policy for your
{{site.data.keyword.keymanagementserviceshort}} instance, keep in mind the
following considerations:

- **Key creation/import policies do not affect keys that existed prior to policy creation.**
  The policy only applies to keys that are created or imported after the policy is created. You will still have
  access to the keys that existed in your {{site.data.keyword.keymanagementserviceshort}} instance prior to policy creation.

- **Once a key creation/import policy is set, you will not be able to create a key in your instance via the UI.**
  Key import/creation policies are not supported in the UI at this time. Once a key/import creation policy is enabled for your instance,
  you will need to use the {{site.data.keyword.keymanagementserviceshort}} api to [create or import] (/apidocs/key-protect#createkey) 
  keys into your instance. 

- **Key creation/import policies only apply to your keys during the time of key creation/import.**
  The policy is only enforced when keys are being created in your {{site.data.keyword.keymanagementserviceshort}} instance. 
  Once the key is created, authorized users can invoke all of the {{site.data.keyword.keymanagementserviceshort}} actions (rotate,wrap, unwrap, etc.)
  on the key as usual.
  
### Enabling and updating a key creation/import policy for your {{site.data.keyword.keymanagementserviceshort}} instance
{: #enable-key-creation-import-policy}

As a security admin, you can enable or update a key creation/import policy for a
{{site.data.keyword.keymanagementserviceshort}} instance by making a `PUT` call
to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess
```
{: codeblock}

If you are updating your {{site.data.keyword.keymanagementserviceshort}}
instance's key creation/import policy, you will need to provide all of the previous attributes
that were allowed under the policy, as the new request will override the
existing policy.
{: important}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable and disable key creation/import policies, you must be assigned a _Manager_
    access policy for your {{site.data.keyword.keymanagementserviceshort}}
    instance. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Enable or update a key creation/import policy for your
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
                            "enabled": <true|false>,
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
          <varname>enabled</varname>
        </td>
        <td>
          <strong>Required.</strong> Set to <code>true</code> to enable a
          key creation/import policy. Set to <code>false</code> to disable your key creation/import
          policy.
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
        </td>
      </tr>

      <tr>
        <td>
          <varname>enforce_token</varname>
        </td>
        <td>
          <strong>Required.</strong> Set to <code>true</code> to prevent authorized users from importing key material 
          into the your {{site.data.keyword.keymanagementserviceshort}} instance without using an import token. Set to <code>false</code> to 
          allow authorized users to import key material into the your instance without using an import token..
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to enable a key creation/import
        policy.
      </caption>
    </table>

    A successful request returns an HTTP `204 No Content` response, which
    indicates that your {{site.data.keyword.keymanagementserviceshort}}
    instance now has an enabled key creation/import policy.
    Your {{site.data.keyword.keymanagementserviceshort}} instance will now only
    allow the creation or importing of keys from the methods specified in your request.

3. Optional: Verify that the key creation/import policy was created by browsing the
   policies that are available for your
   {{site.data.keyword.keymanagementserviceshort}} instance.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

### Disabling a key creation/import policy for your {{site.data.keyword.keymanagementserviceshort}} instance
{: #disable-key-create-import-policy}

As a manager of a {{site.data.keyword.keymanagementserviceshort}} instance,
disable an existing key creation/import policy for your
{{site.data.keyword.keymanagementserviceshort}} instance by making a `PUT` call
to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To disable key creation/import policies, you must be assigned a _Manager_ access
    policy for your {{site.data.keyword.keymanagementserviceshort}} instance. To
    learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}}
    service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Disable an existing key creation/import policy for your
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
        Table 2. Describes the variables that are needed to disable a key creation/import
        policy.
      </caption>
    </table>

    A successful request returns an HTTP `204 No Content` response, which
    indicates that the key creation/import policy was updated for your service
    instance.

3. Optional: Verify that the key creation/import policy was updated by browsing
   the policies that are available for your
   {{site.data.keyword.keymanagementserviceshort}} instance.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=keyCreateImportAccess" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}