---

copyright:
  years: 2017, 2025
lastupdated: "2025-05-28"

keywords: Key Protect private endpoints, VPE, service endpoints

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
{:term: .term}

# Using virtual private endpoints (VPEs)
{: #private-endpoints}

Create and manage {{site.data.keyword.keymanagementserviceshort}} resources on {{site.data.keyword.cloud_notm}}'s virtual private endpoints (VPEs) by targeting a private service endpoint.
{: shortdesc}

As of 11 January 2024, it is possible to access VPEs using the {{site.data.keyword.keymanagementserviceshort}} control plane UI, allowing users to create and manage keys for instances using a private endpoint. Similarly, keys created using the CLI or the SDK or related method can now be seen and updated using the UI.
{: tip}

To get started, enable
[virtual routing and forwarding (VRF) and service endpoints](/docs/account?topic=account-vrf-service-endpoint){: external}
for your infrastructure account. After you enable VRF for your account, you can
connect to {{site.data.keyword.keymanagementserviceshort}} by using a private IP
that is accessible only through the {{site.data.keyword.cloud_notm}} private
network. To learn more about private connections on
{{site.data.keyword.cloud_notm}}, see
[Service endpoints for private connections](/docs/account?topic=account-service-endpoints-overview){: external}.

## Before you begin
{: #private-network-prereqs}

Before you target a VPE for
{{site.data.keyword.keymanagementserviceshort}}:

1. Ensure that your {{site.data.keyword.cloud_notm}} infrastructure account is
    enabled for
    [virtual routing and forwarding (VRF)](/docs/account?topic=account-vrf-service-endpoint&interface=ui){: external}.

    When you enable VRF, a separate routing table is created for your account,
    and connections to and from your account's resources are routed separately
    on the {{site.data.keyword.cloud_notm}} network. To learn more about VRF
    technology, see
    [Virtual routing and forwarding on {{site.data.keyword.cloud_notm}}](/docs/dl?topic=dl-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud){: external}.

    Enabling VRF permanently alters networking for your account. Be sure that
    you understand the impact to your account and resources. After you enable
    VRF, it cannot be disabled.
    {: important}

2. Ensure that your {{site.data.keyword.cloud_notm}} infrastructure account is
    enabled for
    [VPEs](/docs/account?topic=account-vrf-service-endpoint#service-endpoint){: external}.

    After you enable VRF and VPE for your account, all existing
    and future {{site.data.keyword.keymanagementserviceshort}} resources and
    instances become available from both the public endpoints and VPEs.
    {: note}

VPE settings, specifically the Internet Protocol (IP) address, may need to be manually updated during [Disaster recovery and business continuity actions](/docs/key-protect?topic=key-protect-shared-responsibilities#disaster-recovery). 
{: important}

## Step 1. Configure the {{site.data.keyword.cloud_notm}} VPE on your virtual server
{: #configure-private-network}

Prepare your VSI or test machine by configuring your routing table for the
{{site.data.keyword.cloud_notm}} VPE.

1. To route traffic to the {{site.data.keyword.cloud_notm}} VPE,
    run the following command on your VSI:

    ```sh
    route add -net 166.9.0.0/16 gw <gateway> dev <gateway_interface>
    ```
    {: pre}

    Replace `<gateway>` (for example, `10.x.x.x`) and `<gateway_interface>`
    (for example, `eth10`) with the appropriate values.

2. Optional: Verify that the route was added successfully by displaying your new
    routing table.

    ```sh
    route -n
    ```
    {: pre}

## Step 2. Target the {{site.data.keyword.keymanagementserviceshort}} VPE
{: #target-private-endpoint}

After you configure your VSI to accept {{site.data.keyword.cloud_notm}} traffic over a VPE, you can target the VPE for
{{site.data.keyword.keymanagementserviceshort}} by using the
{{site.data.keyword.keymanagementserviceshort}} API or
{{site.data.keyword.keymanagementserviceshort}} CLI plug-in.

1. In a terminal window, log in to {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login
    ```
    {: pre}

    If the login fails, run the `ibmcloud login --sso` command to try again. The
    `--sso` parameter is required when you log in with a federated ID. If this
    option is used, go to the link listed in the CLI output to generate a
    one-time passcode.
    {: note}

2. Optional: Ensure that your account is enabled for VRF and service endpoints.

    ```sh
    ibmcloud account show
    ```
    {: pre}

    The following CLI output shows the account details of a VRF and service
    endpoint-enabled account.

    ```plaintext
    Retrieving account John Doe's Account of john.doe@email.com...
    OK

    Account ID:                   d154dfbd0bc2edefthyufffc9b5ca318
    Currently Targeted Account:   true
    Linked Softlayer Account:     1008967
    VRF Enabled:                  true
    Service Endpoint Enabled:     true
    ```
    {: screen}

    See
    [Enabling VRF and service endpoints](/docs/account?topic=account-vrf-service-endpoint){: external}
    to learn how to set up your account for connecting to a VPE.
    {: tip}

3. Set an environment variable to target a
    {{site.data.keyword.keymanagementserviceshort}} VPE.

    ```sh
    export KP_PRIVATE_ADDR=https://private.<region>.kms.cloud.ibm.com
    ```
    {: pre}

    Replace `<region>` with the region abbreviation that represents the
    geographic area where your {{site.data.keyword.keymanagementserviceshort}}
    instance resides. For the complete list of endpoints, see
    [Regions and endpoints](/docs/key-protect?topic=key-protect-regions#connectivity-options).

## Step 3. Create a {{site.data.keyword.keymanagementserviceshort}} resource on the VPE
{: #create-key-private-network}

Test your VPE connection by using the
[{{site.data.keyword.keymanagementserviceshort}} CLI plug-in](/docs/key-protect?topic=key-protect-set-up-cli).

1. Create a [root key](/docs/key-protect?topic=key-protect-create-root-keys) by
    targeting the VPE.

    ```sh
    ibmcloud kp key create <key_name> -i <instance_ID>
    ```
    {: pre}

    Replace `<key_name>` with a human-readable alias for easy identification of
    your key. Replace `<instance_ID>` with the {{site.data.keyword.cloud_notm}}
    instance ID that identifies your
    {{site.data.keyword.keymanagementserviceshort}} instance.

2. Optional: Verify that the key was created successfully by listing the keys
    that are available in your {{site.data.keyword.keymanagementserviceshort}}
    instance.

    ```sh
    ibmcloud kp keys -i <instance_ID>
    ```
    {: pre}

    Replace `<instance_ID>` with the {{site.data.keyword.cloud_notm}} instance
    ID that identifies your {{site.data.keyword.keymanagementserviceshort}}
    instance.

## Next steps
{: #private-network-next-steps}

You're now set to interact with {{site.data.keyword.keymanagementserviceshort}}
through a VPE.

- To find out more about managing keys with
    {{site.data.keyword.keymanagementserviceshort}},
    [check out the {{site.data.keyword.keymanagementserviceshort}} CLI reference doc](/docs/key-protect?topic=key-protect-key-protect-cli-reference).
