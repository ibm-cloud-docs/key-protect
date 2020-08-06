---

copyright:
  years: 2020
lastupdated: "2020-04-22"

keywords: instance settings, service settings, network access policies

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

# Managing network access policies
{: #managing-network-access-policies}

After you set up your {{site.data.keyword.keymanagementservicelong}} service
instance, you manage network access policies by using the service API.
{: shortdesc}

## Managing network access policy settings
{: #managing-network-access-policy-settings}

A network access policy for {{site.data.keyword.keymanagementserviceshort}}
instances is an extra policy that customers can use to block a
{{site.data.keyword.keymanagementserviceshort}} instance from getting API
requests from public or private networks.

The network access policy applies to newly provisioned and existing instances.
For existing instances the network access policy is enforced after it is set.

The network access policy capability is available only through the
{{site.data.keyword.keymanagementserviceshort}} API. To find out more about
accessing the {{site.data.keyword.keymanagementserviceshort}} APIs, check out
[Setting up the API](/docs/key-protect?topic=key-protect-set-up-api).
{: preview}

### Understanding network access policies
{: #understanding-network-access-policies}

Two options control network access to
{{site.data.keyword.keymanagementserviceshort}} instances:

* Public and private network access - this is the default
* Private network access only

#### Public and private network access
{: #public-and-private}

The {{site.data.keyword.keymanagementserviceshort}} instance accepts API
requests from both `public and private` endpoints.

Public and private network access is the default setting and is used if a policy
is not set.

For example, multiple teams are testing a solution that uses
{{site.data.keyword.keymanagementserviceshort}} instances. Development and test
teams issue API requests from both outside (public endpoints) and inside
(private endpoints) the IBM Cloud. You allow public and private API requests to
ensure each team has access to {{site.data.keyword.keymanagementserviceshort}}
instances during this phase of the project.

#### Private network access only
{: #private-only}

The {{site.data.keyword.keymanagementserviceshort}} instance accepts API
requests from only `private` endpoints.

For example, development and testing is complete and the solution that uses
{{site.data.keyword.keymanagementserviceshort}} instances is in production. You
want to limit API requests to private networks for security reasons. All
{{site.data.keyword.keymanagementserviceshort}} API requests must originate from
within the IBM Cloud.

In the `Regions and endpoints` section there is a section that explains how to
[enable private endpoints](/docs/key-protect?topic=key-protect-regions#connectivity-options).

After the network access policy is set to `private-only` you cannot make any
{{site.data.keyword.keymanagementserviceshort}} API calls from the public
network, including the API to change the policy. Make sure the private
environment is set up before setting the network access policy to
`private-only`. See
[using private endpoints](/docs/key-protect?topic=key-protect-private-endpoints).
{: note}

#### Deprovision a {{site.data.keyword.keymanagementserviceshort}} instance
{: #deprovision-a-service-instance}

The network access policy is not enforced when a request for a
{{site.data.keyword.keymanagementserviceshort}} instance is
deprovisioned.

### Enabling network access to your {{site.data.keyword.keymanagementserviceshort}} instance with the console
{: #enabling-network-access-to-your-service-instance-ui}

If you prefer to enable a dual authorization policy on your instance by using a graphical interface, you can use the IBM Cloud console.

After the network access policy is set to `private-only` the UI cannot be used
for any {{site.data.keyword.keymanagementserviceshort}} actions.
{: important}

Keys in a `private-only` instance will not be shown in the UI and any
{{site.data.keyword.keymanagementserviceshort}} actions in the UI will return an
unauthorized error (HTTP status code 401).

After creating a {{site.data.keyword.keymanagementserviceshort}} instance, complete the following steps to create a network access policy:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.
2. Go to **Menu** &gt; **Resource List** to view a list of your resources.
3. From your {{site.data.keyword.cloud_notm}} resource list, select your
provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.
4. On the **Manage instance policies** page, click the **Add policy** button.
5. Choose **Network** and select the network that you would like traffic to come through.
6. Click the **Add policy** button.
7. Confirm the policy was created in the updated **Policies** table.

### Enabling network access to your {{site.data.keyword.keymanagementserviceshort}} instance with the API
{: #enabling-network-access-to-your-service-instance-api}

As an admin, enable a network access policy for a
{{site.data.keyword.keymanagementserviceshort}} instance by making a
`PUT` call to the following endpoint. See these API references to
[set](/apidocs/key-protect#set-instance-policies){: external}
and
[list](/apidocs/key-protect#list-instance-policies){: external}
instance policies.

```
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=allowedNetwork
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To change a network access policy, you must be assigned a _Manager_ access
    policy for your {{site.data.keyword.keymanagementserviceshort}} instance. To learn how IAM (identity and access
    management) roles map to {{site.data.keyword.keymanagementserviceshort}}
    service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Enable a network access policy for your {{site.data.keyword.keymanagementserviceshort}} instance by running the
following cURL command.

    ```cURL
    curl -X PUT \
      'https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=allowedNetwork' \
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
            "policy_type": "allowedNetwork",
            "policy_data": {
              "enabled": <enabled>,
              "attributes": {
                "allowed_network": "<access_type>"
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
          network access policy. Set to <code>false</code> to remove the network
          access policy, that is, the policy is not enforced.
        </td>
      </tr>

      <tr>
        <td>
          <varname>access_type</varname>
        </td>
        <td>
          <strong>Required.</strong> The network access policy to apply to your
          {{site.data.keyword.keymanagementserviceshort}} instance.
          Acceptable values are <code>public-and-private</code> or
          <code>private-only</code>.
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to set a network access
        policy at the instance level.
      </caption>
    </table>

    A successful request returns an HTTP `204 No Content` response, which
    indicates that your {{site.data.keyword.keymanagementserviceshort}} instance now enforces a network access policy.
    API requests to the service are restricted to the policy you set.

    This policy applies to {{site.data.keyword.keymanagementserviceshort}}
    instances only. The network access policy does not apply to specific keys.

3. Optional: Verify that the network access policy was created by browsing the
policies that are available for your
{{site.data.keyword.keymanagementserviceshort}} instance.

    ```cURL
    curl -X GET \
      'https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=allowedNetwork' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'accept: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

### Disabling network access to your {{site.data.keyword.keymanagementserviceshort}} instance with the console
{: #disabling-network-access-to-your-service-instance-ui}

If you prefer to disable a network access policy on your instance by using a graphical interface, you can use the IBM Cloud console.

After creating a network access policy, complete the following steps to disable a network access policy:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.
2. Go to **Menu** &gt; **Resource List** to view a list of your resources.
3. From your {{site.data.keyword.cloud_notm}} resource list, select your
provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.
4. On the **Manage instance policies** page, use the **Policies** table to browse the policies in
your {{site.data.keyword.keymanagementserviceshort}} instance.
5. Click the ⋯ icon to open a list of options for the policy that you want to
disable.
6. From the options menu, click **Disable policy** and confirm the policy was disabled in
the updated **Policies** table.

## What's next
{: #managing-network-access-policies-next-steps}

These are API references to set and list instance policies.

* [Set instance policies](/apidocs/key-protect#set-instance-policies){: external}
* [List instance policies](/apidocs/key-protect#list-instance-policies){: external}
