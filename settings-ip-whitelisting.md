---

copyright:
  years: 2020
lastupdated: "2020-07-28"

keywords: instance settings, service settings, ip whitelisting, ip whitelist

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

# Managing ip whitelisting
{: #manage-ip-whitelisting}

After you set up your {{site.data.keyword.keymanagementservicelong}} service
instance, you can manage ip whitelist policies by using the service API.
{: shortdesc}

## Managing ip whitelisting settings
{: #manage-ip-whitelisting-instance-policy}

Ip whitelisting for {{site.data.keyword.keymanagementserviceshort}} service
instances is an extra policy that you can use to restrict access to your {{site.data.keyword.keymanagementserviceshort}} service instance through public and private endpoints. When you enable this 
policy at the instance level, {{site.data.keyword.keymanagementserviceshort}} only permits access to the resources in your {{site.data.keyword.keymanagementserviceshort}} instance from trusted ip 
addresses.

Setting and retrieving a ip white policy is only supported through the
application programming interface (API). IP Whitelist policy support will be
added to the user interface (UI), command line interface (CLI), and software
development kit (SDK) in the future. To find out more about
accessing the {{site.data.keyword.keymanagementserviceshort}} APIs, check out
[Setting up the API](/docs/key-protect?topic=key-protect-set-up-api).

{{site.data.keyword.keymanagementserviceshort}} currently supports the IP whitelist feature on an account basis. If you need to enable an ip whitelist policy for your service instance, [open](/docs/get-support?topic=get-support-getting-customer-support) a support ticket to request access to this feature.
{: important}

Before you enable ip whitelisting for your service instance, keep in mind the
following considerations:

- **When you enable ip whitelisting for your service instance, the resources in your instance will not be displayed in the UI .**
After enabling an ip whitelist policy, you will only be able to view and access the keys and associated resources in your instance through the API. 
- **If both a network policy and an ip whitelist policy are enabled at the same time, only traffic aligning with both policies will be allowed.**
When assigning an ip whitelist policy to a service instance that has an existing allowed network policy, the ip addresses that are listed on the policy must access 
the instance from the network specified in the allowed network policy.

### Enabling ip whitelisting for your service instance
{: #enable-ip-whitelisting-instance-policy}

As a security admin, enable an ip whitelist policy for a {{site.data.keyword.keymanagementserviceshort}} service instance by making a `PUT` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=ipWhitelist
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable and disable ip whitelist policies, you must be assigned a
    _Manager_ access policy for your service instance. To learn how IAM roles
    map to {{site.data.keyword.keymanagementserviceshort}} service actions,
    check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Enable an ip whitelist policy for your service instance by running the
following cURL command.

    ```cURL
    curl -X PUT \
      'https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=ipWhitelist' \
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
            "policy_type": "ipWhitelist",
            "policy_data": {
              "enabled": <true|false>,
              "attributes": {
                "allowed_ip": [<ip_address_list>]
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
            {{site.data.keyword.keymanagementserviceshort}} service instance
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
          <strong>Required.</strong> Set to <code>true</code> to enable an
          ip whitelist policy. Set to <code>false</code> to disable the ip
          whitelist policy.
        </td>
      </tr>

      <tr>
        <td>
          <varname>ip_address_list</varname>
        </td>
        <td>
          <strong>Required.</strong> A list of ip subnets that are allowed to send traffic to your
          {{site.data.keyword.keymanagementserviceshort}} service instance. There must be at least one value entered. Each subnet must be specified with CIDR notation. 
          Currently, only IPv4 notation is accepted. Acceptable list format is `["X.X.X.X/N", "X.X.X.X/N"]`. The maximum amount of subnets that can added to an ip 
          whitelist policy is 10,000.

          This attribute should not be provided when disabling a policy.
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to enable ip
        whitelisting at the instance level.
      </caption>
    </table>

    A successful request returns an HTTP `204 No Content` response, which
    indicates that your service instance is now enabled for ip whitelisting.
    Your service instance will now only accept traffic from the ip addresses specified in your request. 

3. Optional: Verify that the ip whitelist policy was created by browsing
the policies that are available for your
{{site.data.keyword.keymanagementserviceshort}} service instance.

    ```cURL
    curl -X GET \
      'https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=ipWhitelist' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'accept: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

### Disabling ip whitelisting for your service instance
{: #disable-ip-whitelist-instance-policy}

As an instance manager, disable an existing ip whitelist policy for a
{{site.data.keyword.keymanagementserviceshort}} service instance by making a
`PUT` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=ipWhitelist
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To disable ip whitelist policies, you must be assigned a
    _Manager_ access policy for your service instance. To learn how IAM roles
    map to {{site.data.keyword.keymanagementserviceshort}} service actions,
    check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Disable an existing ip whitelist policy for your service instance by
running the following cURL command.

    ```cURL
    curl -X PUT \
      'https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=ipWhitelist' \
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
            "ipWhitelist": {
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
            {{site.data.keyword.keymanagementserviceshort}} service instance
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
        Table 2. Describes the variables that are needed to disable ip
        whitelisting at the instance level.
      </caption>
    </table>

    A successful request returns an HTTP `204 No Content` response, which
    indicates that the ip whitelist policy was updated for your service
    instance. 

3. Optional: Verify that the ip whitelist policy was updated by browsing
the policies that are available for your
{{site.data.keyword.keymanagementserviceshort}} service instance.

    ```cURL
    curl -X GET \
      'https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=ipWhitelist' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'accept: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

## Accessing an instance via public endpoint
{: #access-ip-whitelist-public-endpoint}

When you create an ip whitelist policy, you can access your instance via public endpoint as long as the ip address is on the list of approved ip addresses associated 
with the policy. If you send a request to your instance through an unauthorized ip address, you will receive a `HTTP 401` error stating that you are unauthorized to 
make a request to the service instance.

## Accessing an instance via private endpoint
{: #access-ip-whitelist-private-endpoint}

When you create an ip whitelist policy, {{site.data.keyword.keymanagementserviceshort}} assigns a private 
endpoint port to your policy. Once you retrieve the port, the port value must be appended to the private service 
hostname in your request to your instance via a Key Protect private service endpoint. 

The private endpoint port should only be used when accessing your instance via a private service endpoint.
{: note}

### Retrieving the private port for the your ip whitelist policy
{: #retrieve-ip-whitelist-port}

You can retrieve the private endpoint port associated with your service instance's active whitelist policy 
by making a `GET` call to the following endpoint. Note that calls to this api bypasses ip whitelist enforcement.

```
https://<region>.kms.cloud.ibm.com/api/v2/ip_whitelist_port
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To retrieve the private endpoint port associated with your ip whitelist policy, you must be assigned a
    _Manager_ access policy for your service instance. To learn how IAM roles
    map to {{site.data.keyword.keymanagementserviceshort}} service actions,
    check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Retrieve the private endpoint port assigned to ip whitelist policy by running the
following cURL command.

    ```cURL
    curl -X PUT \
      'https://<region>.kms.cloud.ibm.com/api/v2/ip_whitelist_port' \
      -H 'accept: application/vnd.ibm.kms.policy+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.ip_whitelist_port+json' \
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
            {{site.data.keyword.keymanagementserviceshort}} service instance
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
        Table 3. Describes the variables that are needed to retrieve the private endpoint port associated 
        with your service instance.
      </caption>
    </table>

    A successful `GET api/v2/ip_whitelist_port` response returns the private endpoint port that was assigned 
    to your instance upon creation of its associated ip whitelist policy in the `private_endpoint_port` field. If the instance doesn't have an 
    enabled ip whitelist policy, no information will be returned.

### Sending traffic to your service instance through a private endpoint port
{: #send-private-ip-whitelist-traffic}

When you retrieve the private endpoint port associated with your ip whitelist policy, you should append it to the host name of the private service endpoint. For 
example, if the private endpoint port associated with your ip whitelist policy is 8888, the endpoint that you will make a request through to list the keys in your 
instance is `https://private.us-south.kms.cloud.ibm.com:8888/api/v2/keys`. 

You can use the following example request to retrieve a list of keys for your service instance via a private endpoint.

```cURL
curl -X GET \
  'https://private.<region>.kms.cloud.ibm.com:<private_endpoint_port>/api/v2/keys' \
  -H 'accept: application/vnd.ibm.collection+json' \
  -H 'authorization: Bearer <IAM_token>' \
  -H 'bluemix-instance: <instance_ID>'
```
{: codeblock}

Replace the variables in your request according to the
following table.

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
        {{site.data.keyword.keymanagementserviceshort}} service instance
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
      <varname>private_endpoint_port</varname>
    </td>
    <td>
      <p>
        <strong>Required.</strong> The port associated with an instance with an ip whitelist policy.
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
    Table 4. Describes the variables needed to make a list keys request through a private endpoint on an instance with an ip whitelist policy.
  </caption>
</table>

## Using ip whitelist policy on an instance that is integrated with other {{site.data.keyword.Bluemix_notm}} services
{: #ip-whitelist-s2s}

{{site.data.keyword.keymanagementserviceshort}} currently has limited ip whitelist policy support for integrated services. If you would like to create an ip whitelist policy for a {{site.data.keyword.keymanagementserviceshort}} service instance that is integrated with another service, keep in mind the
following considerations before creating an ip whitelist policy:

- {{site.data.keyword.keymanagementserviceshort}} does not currently have ip whitelist policy support for instances integrated with {site.data.keyword.containerlong_notm}}.
- If your service instance has an ip whitelist policy, you will not be able to integrate {{site.data.keyword.keymanagementserviceshort}} with another service via the UI. You will need to integrate your service via the API.
- The {{site.data.keyword.databases-for}} UI will not display any {{site.data.keyword.keymanagementserviceshort}} related information when your {{site.data.keyword.keymanagementserviceshort}} service instance has an enabled ip whitelist policy.
- Service to service calls will bypass ip whitelist enforcement if the service instance has an enabled ip whitelist policy.