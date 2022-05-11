---

copyright:
  years: 2020, 2022
lastupdated: "2022-05-11"

keywords: instance settings, service settings, allowed ip, ip allowlist

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
{:deprecated: .deprecated}

# Managing an allowed IP policy
{: #manage-allowed-ip}

After you set up your {{site.data.keyword.keymanagementservicelong}} service
instance, you can manage allowed IP policies by using the
{{site.data.keyword.keymanagementserviceshort}} service API.
{: shortdesc}

For the best results now available, use [context-based restrictions (CBR)](/docs/account?topic=account-context-restrictions-whatis) instead of an allowed IP policy to define and enforce access restrictions for {{site.data.keyword.cloud_notm}} resources based on the network location of access requests.
{: deprecated}

## Managing allowed IP settings
{: #manage-allowed-ip-instance-policy}

Allowed IP for {{site.data.keyword.keymanagementserviceshort}}
instances is a type of network policy that you can use to restrict access to
your {{site.data.keyword.keymanagementserviceshort}} instance through public and
private endpoints. When you enable this policy at the instance level,
{{site.data.keyword.keymanagementserviceshort}} only permits access to the
resources in your {{site.data.keyword.keymanagementserviceshort}} instance from
IP addresses that you have listed on your allowed IP policy.

Setting and retrieving an allowed IP policy is only supported through the
application programming interface (API). Allowed IP policy support will be
added to the user interface (UI), command line interface (CLI), and software
development kit (SDK) in the future. To find out more about
accessing the {{site.data.keyword.keymanagementserviceshort}} APIs, check out
[Setting up the API](/docs/key-protect?topic=key-protect-set-up-api).

{{site.data.keyword.keymanagementserviceshort}} currently supports the allowed
IP policy feature on an account basis. If you need to enable an allowed IP
policy for your {{site.data.keyword.keymanagementserviceshort}} instance,
[get support](/docs/get-support?topic=get-support-using-avatar){: external}
or
[create a case](/unifiedsupport/cases/form){: external}
to request access to this feature.
{: important}

Before you enable and allowed IP policy for your
{{site.data.keyword.keymanagementserviceshort}} instance, keep in mind the
following considerations:

- **If you are using a private endpoint, your private environment should be set up prior to enabling an allowed IP policy for your {{site.data.keyword.keymanagementserviceshort}} instance.**
    Before you create an instance policy for your
    {{site.data.keyword.keymanagementserviceshort}} instance, ensure that you have
    [configured](/docs/key-protect?topic=key-protect-private-endpoints#configure-private-network)
    the private network on your server so that you will be able to assign the
    correct IP addresses to the policy.

- **Allowed IP policies only support traffic from IPv4 addresses through private endpoint.**
    Keep in mind that while both IPv4 and IPv6 subnets can be added to an allowed
    IP policy, an active allowed IP policy can only allow IPv4 private network
    gateway addresses to access
    {{site.data.keyword.keymanagementserviceshort}} instances via a private
    endpoint.

- **You will need to need to use a private port to access your {{site.data.keyword.keymanagementserviceshort}} instance via private network.**
    Once you create an allowed IP policy, your
    {{site.data.keyword.keymanagementserviceshort}} instance will be assigned a
    private endpoint port. You will need to
    [retrieve](#retrieve-allowed-ip-port)
    the port and
    [specify](#send-private-allowed-ip-traffic)
    the port number during each request via a private endpoint.

- **There is limited allowed IP policy support for {{site.data.keyword.keymanagementserviceshort}} instances that are integrated with other {{site.data.keyword.Bluemix_notm}} services.**
    Service to service calls will bypass allowed IP policy enforcement. To find
    out more information
    about how allowed IP policies affect
    {{site.data.keyword.keymanagementserviceshort}} instances that are integrated
    with other {{site.data.keyword.Bluemix_notm}} services, see
    [Using an allowed IP policy on an instance that is integrated with other {{site.data.keyword.Bluemix_notm}} services](#allowed-ip-s2s).

- **When you enable an allowed IP policy for your {{site.data.keyword.keymanagementserviceshort}} instance, the resources in your instance will not be displayed in the UI.**
    After enabling an allowed IP policy, you will only be able to view and access
    the keys and associated resources in your
    {{site.data.keyword.keymanagementserviceshort}} instance through the
    {{site.data.keyword.keymanagementserviceshort}} API. Before making a request,
    make sure that you are assigned the correct access policy for your
    {{site.data.keyword.keymanagementserviceshort}} instance and use the correct
    endpoints and port (for private endpoints).

- **If both a network policy and an allowed IP policy are enabled at the same time, only traffic aligning with both policies will be allowed.**
    When assigning an allowed IP policy to a
    {{site.data.keyword.keymanagementserviceshort}} instance that has an existing
    allowed network policy, the IP addresses that are listed on the policy must
    access the instance from the network specified in the allowed network policy.

- **`ipWhitelist` has been deprecated and replaced with `allowedIP`.**
    Due to the deprecation of the previous `ipWhitelist` term, if you don't
    specify a query param while making a `GET` instance policies request, you will
    receive a duplicate allowed IP record when you retrieving your list of
    instance policies.

### Enabling and updating an allowed IP policy for your {{site.data.keyword.keymanagementserviceshort}} instance
{: #enable-allowed-ip-instance-policy}

As a security admin, you can enable or update an allowed IP policy for a
{{site.data.keyword.keymanagementserviceshort}} instance by making a `PUT` call
to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=allowedIP
```
{: codeblock}

If you are updating your {{site.data.keyword.keymanagementserviceshort}}
instance's allowed IP policy, you will need to provide all of the IP addresses
that are already allowed under the policy, as the new request will override the
existing policy.
{: important}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable and disable allowed IP policies, you must be assigned a _Manager_
    access policy for your {{site.data.keyword.keymanagementserviceshort}}
    instance. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Enable or update an allowed IP policy for your
    {{site.data.keyword.keymanagementserviceshort}} instance by running the
    following `curl` command.

    ```sh
    $ curl -X PUT \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=allowedIP" \
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
                        "policy_type": "allowedIP",
                        "policy_data": {
                            "enabled": <true|false>,
                            "attributes": {
                                "allowed_ip": [<ip_address_list>]
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
|enabled|**Required**. Set to true to enable an allowed IP policy. Set to false to disable the allowed IP policy.|
|ip_address_list|**Required**. A list of IPv4 or IPv6 subnets that are allowed to send traffic to your {{site.data.keyword.keymanagementserviceshort}} instance. There must be at least one value entered. Each subnet must be specified with CIDR notation. Currently, only IPv4 notation is accepted. Acceptable list format is ["X.X.X.X/N", "X.X.X.X.X.X.X.X/N"]. The  maximum amount of subnets that can added to an allowed IP policy is 1,000. This attribute should not be provided when disabling a policy.|
{: caption="Table 1. Describes the variables that are needed to enable an allowed IP policy at the instance level." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which
indicates that your {{site.data.keyword.keymanagementserviceshort}}
instance now has an enabled allowed IP policy.
Your {{site.data.keyword.keymanagementserviceshort}} instance will now only
accept traffic from the IP addresses specified in your request.

### Optional: Verify allowed ip policy enablement
{: #allowed-ip-key-api-verify}

You can verify that an allowed ip policy has been enabled by issuing a list policies request:

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=allowedIP" \
    -H "accept: application/vnd.ibm.kms.policy+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

`ipWhitelist` has been deprecated and replaced with `allowedIP`. You will
see duplicate records when you retrieve your allowed IP policy.
{: note}

Where the `<instance_ID>` is the name of your instance and your `<IAM_token>` is your IAM token.    

### Disabling an allowed IP policy for your {{site.data.keyword.keymanagementserviceshort}} instance
{: #disable-allowed-ip-instance-policy}

As a manager of a {{site.data.keyword.keymanagementserviceshort}} instance,
disable an existing allowed IP policy for your
{{site.data.keyword.keymanagementserviceshort}} instance by making a `PUT` call
to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=allowedIP
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To disable allowed IP policies, you must be assigned a _Manager_ access
    policy for your {{site.data.keyword.keymanagementserviceshort}} instance. To
    learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}}
    service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Disable an existing allowed IP policy for your
    {{site.data.keyword.keymanagementserviceshort}} instance by running the
    following `curl` command.

    ```sh
    $ curl -X PUT \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=allowedIP" \
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
                        "policy_type": "allowedIP",
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
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl  request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 2. Describes the variables that are needed to disable an allowed IP policy at the instance level." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which
indicates that the allowed IP policy was updated for your service
instance.

### Optional: Verify allowed ip policy disablement
{: #allowed-ip-key-disable-api-verify}

You can verify that an allowed ip policy has been disabled by issuing a list policies request:

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=allowedIP" \
    -H "accept: application/vnd.ibm.kms.policy+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

## Accessing an instance via public endpoint
{: #access-allowed-ip-public-endpoint}

When you create an allowed IP policy, you can access your
{{site.data.keyword.keymanagementserviceshort}} instance via public endpoint as
long as the requesting IP address is on the list of approved IPv4 or IPv6
addresses associated with the policy. If you send a request to your instance
through an unauthorized IP address, you will receive a `HTTP 401` error stating
that you are unauthorized to make a request to the
{{site.data.keyword.keymanagementserviceshort}} instance.

Only IPv4 addresses will be accepted via private endpoints. If your private
network gateway has both an IPv4 and an IPv6 address, it is recommended that you
include the `--ipv4` flag in all of your `curl` requests to ensure that the
requests are resolved to IPv4.

The following example shows how to utilize the `--ipv4` flag in a `GET` keys
request to a public endpoint.

```sh
$ curl --ipv4 -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys" \
    -H "accept: application/vnd.ibm.collection+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

When using the `--ipv4` flag, the flag should come before the `-X` flag to avoid
running into issues with the `curl` request.
{: important}

## Accessing an instance via private endpoint
{: #access-allowed-ip-private-endpoint}

When you create an allowed IP policy,
{{site.data.keyword.keymanagementserviceshort}} assigns a private endpoint port
to your policy. Once you retrieve the port, the port value must be appended to
the private service hostname in your request to your
{{site.data.keyword.keymanagementserviceshort}} instance via a
{{site.data.keyword.keymanagementserviceshort}} private service endpoint.

The private endpoint port should only be used when accessing your
{{site.data.keyword.keymanagementserviceshort}} instance via a private service
endpoint.
{: note}

Only IPv4 addresses will be accepted via private endpoints. If your private
network gateway has both an IPv4 and an IPv6 address, it is recommended that you
include the `--ipv4` flag in all of your `curl` requests to ensure that the
requests are resolved to IPv4.

`ipWhitelist` has been deprecated and replaced with `allowedIP`. If there isn't
a query parameter specified in your `GET` instance policies request, you will
see duplicate records when you retrieve your allowed IP policy.
{: note}

The following example shows how to utilize the `--ipv4` flag in a `GET` instance
policies request via a private endpoint.

```sh
$ curl -k -L --ipv4 -X GET \
    "https://private.<region>.kms.cloud.ibm.com:<private_enpoint_port>/api/v2/instance/policies" \
    -H "accept: application/vnd.ibm.kms.policy+json" \
    -H "authorization: Bearer <token>" \
    -H "bluemix-instance: <instance_ID>" \
    -H "correlation-id: <correlation_ID>"
```
{: codeblock}

When using the `--ipv4` flag, the flag should come before the `-X` flag to avoid
running into issues with the `curl` request.
{: important}

### Retrieving the private port for an allowed IP policy enabled {{site.data.keyword.keymanagementserviceshort}} instance
{: #retrieve-allowed-ip-port}

You can retrieve the private endpoint port associated with your
{{site.data.keyword.keymanagementserviceshort}} instance's active allowed IP
policy by making a `GET` call to the following endpoint. **Note** that calls to
this API bypass allowed IP policy enforcement.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/instance/allowed_ip_port
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To retrieve the private endpoint port associated with your allowed IP
    policy, you must be assigned a _Manager_ access policy for your
    {{site.data.keyword.keymanagementserviceshort}} instance. To learn how IAM
    roles map to {{site.data.keyword.keymanagementserviceshort}} service
    actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Retrieve the private endpoint port assigned to your allowed IP policy by
    running the following `curl` command.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/allowed_ip_port" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.allowed_ip_port+json"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 3. Describes the variables that are needed to retrieve the private endpoint port associated with your {{site.data.keyword.keymanagementserviceshort}} instance." caption-side="top"}

A successful `GET api/v2/instance/allowed_ip_port` response returns the
private endpoint port that was assigned to your
{{site.data.keyword.keymanagementserviceshort}} instance upon creation of
its associated allowed IP policy in the `private_endpoint_port` field. If
the instance doesn't have an enabled allowed IP policy, no information will
be returned.

### Sending traffic to your {{site.data.keyword.keymanagementserviceshort}} instance through a private endpoint port
{: #send-private-allowed-ip-traffic}

When you retrieve the private endpoint port associated with your allowed IP
policy, you should append it to the host name of the private service endpoint.
For example, if the private endpoint port associated with your allowed IP policy
is 8888, the endpoint that you will make a request through to list the keys in
your {{site.data.keyword.keymanagementserviceshort}} instance is
`https://private.us-south.kms.cloud.ibm.com:8888/api/v2/keys`.

When making a request through a private network, the allowed IP policy will only
consider the private network's gateway address, and not the IP address of the
requester. Therefore, it is important that you add the IPv4 address associated
with the private network gateway to your
{{site.data.keyword.keymanagementserviceshort}} instance's allowed IP policy.
{: note}

You can use the following example request to retrieve a list of keys for your
{{site.data.keyword.keymanagementserviceshort}} instance via a private endpoint.

```sh
$ curl --ipv4 -X GET \
    "https://private.<region>.kms.cloud.ibm.com:<private_endpoint_port>/api/v2/keys" \
    -H "accept: application/vnd.ibm.kms.key+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Replace the variables in your request according to the following table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|private_endpoint_port|**Required**. The port associated with an instance with an allowed IP policy.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 4. Describes the variables needed to make a list keys request through a private endpoint on an instance with an allowed IP policy." caption-side="top"}

## Using an allowed IP policy on an instance that is integrated with other {{site.data.keyword.Bluemix_notm}} services
{: #allowed-ip-s2s}

{{site.data.keyword.keymanagementserviceshort}} currently has limited allowed
IP policy support for integrated services. If you would like to create an
allowed IP policy for a {{site.data.keyword.keymanagementserviceshort}} instance
that is integrated with another service, keep in mind the following
considerations before creating an allowed IP policy:

- {{site.data.keyword.keymanagementserviceshort}} does not currently have
    allowed IP policy support for instances integrated with
    {{site.data.keyword.containerlong_notm}}.

- If your {{site.data.keyword.keymanagementserviceshort}} instance has an
    enabled allowed IP policy, you will not be able to integrate
    {{site.data.keyword.keymanagementserviceshort}} with another service via the
    UI. You will need to integrate your service via the
    {{site.data.keyword.keymanagementserviceshort}} API.

- Service to service calls will bypass allowed IP policy enforcement.

- If your integrated {{site.data.keyword.keymanagementserviceshort}} instance
    has an active allowed IP policy, you will not be able to view the resources in
    your {{site.data.keyword.keymanagementserviceshort}} instance via the UI.


