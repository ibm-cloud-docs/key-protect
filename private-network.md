---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-31"

keywords: Key Protect private endpoints, Key Protect private network, VRF, service endpoints

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

# Using private endpoints
{: #private-endpoints}  

Create and manage {{site.data.keyword.keymanagementserviceshort}} resources on {{site.data.keyword.cloud_notm}}'s private network by targeting a private service endpoint.
{: shortdesc} 

To get started, enable [virtual routing and forwarding (VRF) and service endpoints](docs/account?topic=account-vrf-service-endpoint){: external} for your infrastructure account. After you enable VRF for your account, you can connect to {{site.data.keyword.keymanagementserviceshort}} by using a private IP that is accessible only through the {{site.data.keyword.cloud_notm}} private network. To learn more about private connections on {{site.data.keyword.cloud_notm}}, see [Service endpoints for private connections](/docs/resources?topic=resources-service-endpoints){:external}.

To connect to {{site.data.keyword.keymanagementserviceshort}} by using a private network connection, you must use the {{site.data.keyword.keymanagementserviceshort}} API or the [{{site.data.keyword.keymanagementserviceshort}} CLI plug-in](/docs/services/key-protect?topic=key-protect-cli-reference). This capability is not available from the {{site.data.keyword.keymanagementserviceshort}} GUI.
{: note}

## Before you begin
{: #private-network-prereqs}

Before you target a private endpoint for {{site.data.keyword.keymanagementserviceshort}}: 

1. Ensure that your {{site.data.keyword.cloud_notm}} infrastructure account is enabled for [virtual routing and forwarding (VRF)](/docs/account?topic=account-vrf-service-endpoint#vrf){: external}.

    When you enable VRF, a separate routing table is created for your account, and connections to and from your account's resources are routed separately on the {{site.data.keyword.cloud_notm}} network. To learn more about VRF technology, see [Virtual routing and forwarding on {{site.data.keyword.cloud_notm}}](/docs/resources?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud){: external}.

    Enabling VRF permanently alters networking for your account. Be sure that you understand the impact to your account and resources. After you enable VRF, it cannot be disabled.
    {: important}
2. Ensure that your {{site.data.keyword.cloud_notm}} infrastructure account is enabled for [service endpoints](/docs/account?topic=account-vrf-service-endpoint#service-endpoint){: external}.

    After you enable VRF and service endpoints for your account, all existing and future {{site.data.keyword.keymanagementserviceshort}} resources and service instances become available from both the public and private endpoints.
    {: note}
    
## Step 1. Configure the {{site.data.keyword.cloud_notm}} private network on your virtual server
{: #configure-private-network}

Prepare your VSI or test machine by configuring your routing table for the {{site.data.keyword.cloud_notm}} private network.

1. To route traffic to the {{site.data.keyword.cloud_notm}} private network, run the following command on your VSI:

    ```sh
    route add -net 166.9.0.0/16 gw <gateway> dev <gateway_interface>
    ```
    {: pre}

    Replace `<gateway>` (for example, `10.x.x.x`) and `<gateway_interface>` (for example, `eth10`) with the appropriate values. 

2. Optional: Verify that the route was added successfully by displaying your new routing table.

    ```sh
    route -n
    ```
    {: pre}

## Step 2. Target the {{site.data.keyword.keymanagementserviceshort}} private endpoint 
{: #target-private-endpoint}

After you configure your VSI to accept {{site.data.keyword.cloud_notm}} private network traffic, you can target the private endpoint for {{site.data.keyword.keymanagementserviceshort}} by using the {{site.data.keyword.keymanagementserviceshort}} API or {{site.data.keyword.keymanagementserviceshort}} CLI plug-in.

1. In a terminal window, log in to {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    If the login fails, run the `ibmcloud login --sso` command to try again. The `--sso` parameter is required when you log in with a federated ID. If this option is used, go to the link listed in the CLI output to generate a one-time passcode.
    {: note}

2. Optional: Ensure that your account is enabled for VRF and service endpoints.

    ```sh
    ibmcloud account show
    ```
    {: pre}

    The following CLI output shows the account details of a VRF and service endpoint-enabled account.

    ```
    Retrieving account John Doe's Account of john.doe@email.com...
    OK

    Account ID:                   d154dfbd0bc2edefthyufffc9b5ca318   
    Currently Targeted Account:   true   
    Linked Softlayer Account:     1008967 
    VRF Enabled:                  true  
    Service Endpoint Enabled:     true
    ```
    {: screen}

    See [Enabling VRF and service endpoints](/docs/account?topic=account-vrf-service-endpoint){: external} to learn how to set up your account for connecting to a private network.
    {: tip}

3. Set an environment variable to target a {{site.data.keyword.keymanagementserviceshort}} private endpoint.

    ```sh
    export KP_PRIVATE_ADDR=https://private.<region>.kms.cloud.ibm.com
    ```
    {: pre}

    Replace `<region>` with the region abbreviation that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For the complete list of endpoints, see [Regions and endpoints](/docs/services/key-protect?topic=key-protect-regions#connectivity-options).

## Step 3. Create a {{site.data.keyword.keymanagementserviceshort}} resource on the private network
{: #create-key-private-network}

Test your private network connection by using the [{{site.data.keyword.keymanagementserviceshort}} CLI plug-in](/docs/services/key-protect?topic=key-protect-set-up-cli).

1. Create a [root key](/docs/services/key-protect?topic=key-protect-create-root-keys) by targeting the private endpoint.

    ```sh
    ibmcloud kp create <key_name> -i <instance_ID>
    ```
    {: pre}

    Replace `<key_name>` with a human-readable alias for easy identification of your key. Replace `<instance_ID>` with the {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.

2. Optional: Verify that the key was created successfully by listing the keys that are available in your {{site.data.keyword.keymanagementserviceshort}} service instance.

    ```sh
    ibmcloud kp list -i <instance_ID>
    ```
    {: pre}

    Replace `<instance_ID>` with the {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.

## Next steps
{: #private-network-next-steps}

You're now set to interact with {{site.data.keyword.keymanagementserviceshort}} through a private endpoint.

- To find out more about managing keys with {{site.data.keyword.keymanagementserviceshort}}, [check out the {{site.data.keyword.keymanagementserviceshort}} CLI reference doc](/docs/services/key-protect?topic=key-protect-cli-reference).


