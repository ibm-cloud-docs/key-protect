---

copyright:
  years: 2020
lastupdated: "2020-08-04"

keywords: instance settings, service settings, allowed ip, ip allowlist

subcollection: key-protect

---
<!-- DRAFT ONLY, Do NOT push to publish -->

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

# Managing an allowed ip policy
{: #manage-ip-allowlist}

After you set up your {{site.data.keyword.keymanagementservicelong}} service
instance, you can manage allowed ip policies by using the {{site.data.keyword.keymanagementservicelong}} service API.
{: shortdesc}

## Managing allowed ip settings
{: #manage-allowed-ip-instance-policy}

Allowed ip for {{site.data.keyword.keymanagementserviceshort}}
instances is a type of network policy that you can use to restrict access to your {{site.data.keyword.keymanagementserviceshort}} instance through public and private 
endpoints. When you enable this policy at the instance level, {{site.data.keyword.keymanagementserviceshort}} only permits access to the resources in your {{site.data.keyword.keymanagementserviceshort}}
instance from ip addresses that you have listed on your allowed ip policy.

Setting and retrieving an allowed ip policy is only supported through the
application programming interface (API). Allowed ip policy support will be
added to the user interface (UI), command line interface (CLI), and software
development kit (SDK) in the future. To find out more about
accessing the {{site.data.keyword.keymanagementserviceshort}} APIs, check out
[Setting up the API](/docs/key-protect?topic=key-protect-set-up-api).

{{site.data.keyword.keymanagementserviceshort}} currently supports the allowed ip policy feature on an account basis. If you need to enable an allowed ip policy for your {{site.data.keyword.keymanagementserviceshort}} instance, [open](/docs/get-support?topic=get-support-getting-customer-support) a support ticket to request access to this feature.
{: important}

Before you enable and allowed ip policy for your {{site.data.keyword.keymanagementserviceshort}} instance, keep in mind the
following considerations:

- **Allowed ip policies only support ipv4 notation.** 
{{site.data.keyword.keymanagementserviceshort}} currently only supports ipv4 notation at this time. If your service has both an ipv4 and an ipv6 address, you will 
need to resolve all requests to the ipv4 address.
- **If you are using a private endpoint, your private environment should be set up prior to enabling an allowed ip policy for your {{site.data.keyword.keymanagementserviceshort}} instance.**
Before you create an instance policy for your {{site.data.keyword.keymanagementserviceshort}} instance, ensure that you have [configured](/docs/key-protect?topic=key-protect-private-endpoints#configure-private-network) the private network on 
your server so that you will be able to assign the correct ip addresses to the policy.
- **You will need to need to use a private port to access your {{site.data.keyword.keymanagementserviceshort}} instance via private network.**
Once you create an allowed ip policy, your instance will be assigned a private endpoint port. You will need to [retrieve](#retrieve-ip-allowlist-port) the port 
and [specify](#send-private-ip-allowlist-traffic) the port number during each request via a private endpoint.
- **There is limited allowed ip policy support for {{site.data.keyword.keymanagementserviceshort}} instances that are integrated with other {{site.data.keyword.Bluemix_notm}} services.**
Service to service calls will bypass allowed ip policy enforcement. To find out more information 
about how allowed ip policies affect {{site.data.keyword.keymanagementserviceshort}} instances that are integrated with other {{site.data.keyword.Bluemix_notm}} services, see [Using an allowed ip policy on an instance that is integrated with other {{site.data.keyword.Bluemix_notm}} services](#ip-allowlist-s2s).
- **When you enable an allowed ip policy for your {{site.data.keyword.keymanagementserviceshort}} instance, the resources in your instance will not be displayed in the UI.**
After enabling an allowed ip policy, you will only be able to view and access the keys and associated resources in your instance through the {{site.data.keyword.keymanagementserviceshort}} API. 
Before making a request, make sure that you are assigned the correct access policy for your {{site.data.keyword.keymanagementserviceshort}} instance and use the correct endpoints and port (for private 
endpoints). 
- **If both a network policy and an allowed ip policy are enabled at the same time, only traffic aligning with both policies will be allowed.**
When assigning an allowed ip policy to a {{site.data.keyword.keymanagementserviceshort}} instance that has an existing allowed network policy, the ip addresses that are listed on the policy must access 
the instance from the network specified in the allowed network policy.

### Enabling and updating an allowed ip policy for your {{site.data.keyword.keymanagementserviceshort}} instance
{: #enable-allowed-ip-instance-policy}

As a security admin, you can enable or update an allowed ip policy for a {{site.data.keyword.keymanagementserviceshort}} instance by making a `PUT` call 
to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=ipWhitelist
```
{: codeblock}


If you are updating your {{site.data.keyword.keymanagementserviceshort}} instance's allowed ip policy, you will need to provide all of the ip addresses that are already allowed under the policy, as the 
new request will override the existing policy.
{: important}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable and disable allowed ip policies, you must be assigned a
    _Manager_ access policy for your {{site.data.keyword.keymanagementserviceshort}} instance. To learn how IAM roles
    map to {{site.data.keyword.keymanagementserviceshort}} service actions,
    check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Enable or update an allowed ip policy for your {{site.data.keyword.keymanagementserviceshort}} instance by running the
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
          <strong>Required.</strong> Set to <code>true</code> to enable an
          allowed ip policy. Set to <code>false</code> to disable the allowed ip policy.
        </td>
      </tr>

      <tr>
        <td>
          <varname>ip_address_list</varname>
        </td>
        <td>
          <strong>Required.</strong> A list of ip subnets that are allowed to send traffic to your
          {{site.data.keyword.keymanagementserviceshort}} instance. There must be at least one value entered. Each subnet must be specified with CIDR 
          notation. 
          Currently, only IPv4 notation is accepted. Acceptable list format is `["X.X.X.X/N", "X.X.X.X/N"]`. The maximum amount of subnets that can added to an allowed ip policy is 1,000.

          This attribute should not be provided when disabling a policy.
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to enable an allowed ip
        policy at the instance level.
      </caption>
    </table>

    A successful request returns an HTTP `204 No Content` response, which
    indicates that your {{site.data.keyword.keymanagementserviceshort}} instance now has an enabled allowed ip policy.
    Your {{site.data.keyword.keymanagementserviceshort}} instance will now only accept traffic from the ip addresses specified in your request. 

3. Optional: Verify that the allowed ip policy was created by browsing
the policies that are available for your
{{site.data.keyword.keymanagementserviceshort}} instance.

    ```cURL
    curl --ipv4 -X GET \
      'https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=ipWhitelist' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'accept: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

### Disabling an allowed ip policy for your {{site.data.keyword.keymanagementserviceshort}} instance
{: #disable-allowed-ip-instance-policy}

As a manager of a {{site.data.keyword.keymanagementserviceshort} instance, disable an existing allowed ip policy for your
{{site.data.keyword.keymanagementserviceshort}} instance by making a
`PUT` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=ipWhitelist
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To disable allowed ip policies, you must be assigned a
    _Manager_ access policy for your {{site.data.keyword.keymanagementserviceshort}} instance. To learn how IAM roles
    map to {{site.data.keyword.keymanagementserviceshort}} service actions,
    check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Disable an existing allowed ip policy for your {{site.data.keyword.keymanagementserviceshort}} instance by
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
        Table 2. Describes the variables that are needed to disable an allowed
        ip policy at the instance level.
      </caption>
    </table>

    A successful request returns an HTTP `204 No Content` response, which
    indicates that the allowed ip policy was updated for your service
    instance. 

3. Optional: Verify that the allowed ip policy was updated by browsing
the policies that are available for your
{{site.data.keyword.keymanagementserviceshort}} instance.

    ```cURL
    curl --ipv4 -X GET \
      'https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=ipWhitelist' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'accept: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

## Accessing an instance via public endpoint
{: #access-allowed-ip-public-endpoint}

When you create an allowed ip policy, you can access your instance via public endpoint as long as the requesting ip address is on the list of approved ip addresses 
associated with the policy. If you send a request to your instance through an unauthorized ip address, you will receive a `HTTP 401` error stating that you are unauthorized 
to make a request to the {{site.data.keyword.keymanagementserviceshort}} instance.

Currently, only ipv4 notation is accepted. If you have both an ipv4 and an ipv6 address, it is recommended that you include the `--ipv4` flag in all of your cURL 
requests to ensure that the requests are resolved to ipv4.

The following example shows how to utilize the `--ipv4` flag in a `GET` keys request to a public endpoint.

```cURL
curl --ipv4 -X GET \
  'https://<region>.kms.cloud.ibm.com/api/v2/keys' \
  -H 'accept: application/vnd.ibm.collection+json' \
  -H 'authorization: Bearer <IAM_token>' \
  -H 'bluemix-instance: <instance_ID>' \
  -H 'correlation-id: <correlation_ID>'
```
{: codeblock}

When using the `--ipv4` flag, the flag should come before the `-X` flag to avoid running into issues with the cURL request.
{: important}

## Accessing an instance via private endpoint
{: #access-allowed-ip-private-endpoint}

When you create an allowed ip policy, {{site.data.keyword.keymanagementserviceshort}} assigns a private 
endpoint port to your policy. Once you retrieve the port, the port value must be appended to the private service 
hostname in your request to your instance via a Key Protect private service endpoint. 

The private endpoint port should only be used when accessing your instance via a private service endpoint.
{: note}

Currently, only ipv4 notation is accepted. If you have both an ipv4 and an ipv6 address, it is recommended that you include the `--ipv4` flag in all of your cURL 
requests to ensure that the requests are resolved to ipv4.

The following example shows how to utilize the `--ipv4` flag in a `GET` policies request via a private endpoint.

```cURL
curl -k -L --ipv4 -X GET \
  'https://private.<region>.kms.test.cloud.ibm.com:<private_enpoint_port>/api/v2/instance/policies' \
  -H 'Authorization: Bearer <token>' \
  -H 'bluemix-instance: <instance_ID>' \
  -H 'correlation-id: <correlation_ID>'
```
{: codeblock}

When using the `--ipv4` flag, the flag should come before the `-X` flag to avoid running into issues with the cURL request.
{: important}

### Retrieving the private port for an allowed ip policy enabled {{site.data.keyword.keymanagementserviceshort}} instance
{: #retrieve-allowed-ip-port}

You can retrieve the private endpoint port associated with your {{site.data.keyword.keymanagementserviceshort}} instance's active allowed ip policy 
by making a `GET` call to the following endpoint. **Note** that calls to this api bypass allowed ip policy enforcement.

```
https://<region>.kms.cloud.ibm.com/api/v2/ip_whitelist_port
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To retrieve the private endpoint port associated with your allowed ip policy, you must be assigned a
    _Manager_ access policy for your {{site.data.keyword.keymanagementserviceshort}} instance. To learn how IAM roles
    map to {{site.data.keyword.keymanagementserviceshort}} service actions,
    check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Retrieve the private endpoint port assigned to your allowed ip policy by running the
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
        Table 3. Describes the variables that are needed to retrieve the private endpoint port associated 
        with your {{site.data.keyword.keymanagementserviceshort}} instance.
      </caption>
    </table>

    A successful `GET api/v2/ip_whitelist_port` response returns the private endpoint port that was assigned 
    to your instance upon creation of its associated allowed ip policy in the `private_endpoint_port` field. If the instance doesn't have an 
    enabled allowed ip policy, no information will be returned.

### Sending traffic to your {{site.data.keyword.keymanagementserviceshort}} instance through a private endpoint port
{: #send-private-allowed-ip-traffic}

When you retrieve the private endpoint port associated with your allowed ip policy, you should append it to the host name of the private service endpoint. For 
example, if the private endpoint port associated with your allowed ip policy is 8888, the endpoint that you will make a request through to list the keys in your 
instance is `https://private.us-south.kms.cloud.ibm.com:8888/api/v2/keys`. 

When making a request through a private network, the allowed ip policy will only consider the private network's gateway address, and not the ip address of the 
requester. Therefore, it is important that you add the ipv4 address associated with the private network gateway to your {{site.data.keyword.keymanagementserviceshort}} instance's allowed ip policy.
{: note}

You can use the following example request to retrieve a list of keys for your {{site.data.keyword.keymanagementserviceshort}} instance via a private endpoint.

```cURL
curl --ipv4 -X GET \
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
      <varname>private_endpoint_port</varname>
    </td>
    <td>
      <p>
        <strong>Required.</strong> The port associated with an instance with an allowed ip policy.
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
    Table 4. Describes the variables needed to make a list keys request through a private endpoint on an instance with an allowed ip policy.
  </caption>
</table>

## Using an allowed ip policy on an instance that is integrated with other {{site.data.keyword.Bluemix_notm}} services
{: #allowed-ip-s2s}

{{site.data.keyword.keymanagementserviceshort}} currently has limited allowed ip policy support for integrated services. If you would like to create an allowed ip policy for a {{site.data.keyword.keymanagementserviceshort}} instance that is integrated with another service, keep in mind the
following considerations before creating an allowed ip policy:

- {{site.data.keyword.keymanagementserviceshort}} does not currently have allowed ip policy support for instances integrated with {{site.data.keyword.containerlong_notm}}.
- If your {{site.data.keyword.keymanagementserviceshort}} instance has an enabled allowed ip policy, you will not be able to integrate {{site.data.keyword.keymanagementserviceshort}} with another 
service via the UI. You will need to integrate your service via the {{site.data.keyword.keymanagementserviceshort}} API.
- Service to service calls will bypass allowed ip policy enforcement.
- If your integrated {{site.data.keyword.keymanagementserviceshort}} instance has an active allowed ip policy, you will not be able to view the resources in your instance via the UI.