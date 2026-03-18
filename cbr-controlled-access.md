---

copyright:
  years: 2025
lastupdated: "2025-05-28"

keywords: context-based restrictions, access allowlist, network security

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

# Access control with context-based restrictions
{: #access-control-with-cbr}

After you set up your {{site.data.keyword.keymanagementservicelong}} service instance, you can manage access by using the {{site.data.keyword.keymanagementserviceshort}} support for [Context-based restrictions (CBR)](https://cloud.ibm.com/context-based-restrictions).
{: shortdesc}

## Managing CBR settings
{: #manage-cbr-settings}

[CBR](/docs/account?topic=account-context-restrictions-whatis) allows you to manage user and service access to network resources, including Virtual Private Cloud (VPC) references and Internet Protocol (IP) addresses linking your {{site.data.keyword.keymanagementserviceshort}} resources.

For more information about the services integrated with CBR, check out [Services integrated with context-based restrictions](/docs/account?topic=account-context-restrictions-whatis#cbr-access-reqs-target-service).
{: note}

## Overview
{: #cbr-overview}

There are two parts in the instructions to restrict access, [Creating Zones](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create), and [Creating Rules](/docs/account?topic=account-context-restrictions-create&interface=ui), each with multiple steps. First, create a zone with the appropriate details for network or resource definitions like VPC settings. Then, attach that zone to the resource to restrict access. There are two possible paths for achieving this goal: either using a RESTful [API](/apidocs/context-based-restrictions), or with [Context-based restrictions](https://cloud.ibm.com/context-based-restrictions).  Note that after creating or updating a zone or a rule it may take a few minutes for the change to take effect.

CBR rules do not apply to provisioning or deprovision processes.
{: note}

## About Network Zones
{: #cbr-network-zones}

By creating network zones, you can create a list of allowed locations where an access request originates. A set of one or more network locations can be specified by IP addresses such as individual addresses, ranges or subnets, and VPC IDs. After you create a network zone, you can add it to a rule.

### Create Network Zones using the CBR API
{: #cbr-create-zones-api}

The API supports defining [network zones](/apidocs/context-based-restrictions#network-zones) by calling on both public (https://cbr.cloud.ibm.com), and private (https://private.cbr.cloud.ibm.com), endpoints.

Using the path: "/v1/zones" with the GET method will list the zones. Using POST, you can create a new zone with the appropriate information using the following request body format example as a guide:

```json
{
  "name": "an example of a zone",
  "description": "this is an example of a zone",
  "account_id": "12ab34cd56ef78ab90cd12ef34ab56cd",
  "addresses": [
    {
      "type": "ipAddress",
      "value": "169.23.56.234"
    },
    {
      "type": "ipRange",
      "value": "169.23.22.0-169.23.22.255"
    },
    {
      "type": "subnet",
      "value": "192.0.2.0/24"
    },
    {
      "type": "vpc",
      "value": "crn:v1:bluemix:public:is:us-south:a/12ab34cd56ef78ab90cd12ef34ab56cd::vpc:r134-d98a1702-b39a-449a-86d4-ef8dbacf281e"
    }
  ],
  "excluded": [
    {
      "type": "ipAddress",
      "value": "169.23.22.127"
    }
  ]
}
```
{: codeblock}

You can determine which services are available by checking for [reference targets](/apidocs/context-based-restrictions#list-available-serviceref-targets).
{: note}

After creating zones, you can also [update](/apidocs/context-based-restrictions#replace-zone) and [delete](/docs/account?topic=account-context-restrictions-update&interface=ui#context-restrictions-remove-rules) them.

### Create Network Zones using the CBR UI
{: #cbr-create-zone-ui}

With the prerequisites and requirements in place, you can follow the steps to [create zones in the UI](/docs/account?topic=account-context-restrictions-create&interface=ui).

Instead of creating a zone by using UI inputs, you can use the **JSON code form** to directly enter JSON to create a zone by clicking **Enter as JSON code**.
{: note}

After creating zones, you can also [update](/docs/account?topic=account-context-restrictions-update) and [delete](/docs/account?topic=account-context-restrictions-update&interface=ui#context-restrictions-remove-rules) them.

## About Network Rules
{: #cbr-network-rules}

After you have created your zones, you can attach the zones to your networked resources by creating rules.

You can choose from the available [types of endpoints](/docs/account?topic=account-context-restrictions-whatis#context-restrictions-endpint-type) specific to your network topology when you add resources to a rule.
{: note}

### Create Network Rules using the CBR API
{: #cbr-create-rules-api}

The API supports defining [network rules](/apidocs/context-based-restrictions#rules), and you will need the information from creating the network zone for the next steps. 

Using the path: "/v1/rules" with the same endpoint as above, the GET method lists current rules. Sending a POST to the same path with the following example format guiding your own payload, you can create new rules:

```json
{
  "description": "this is an example of a rule",
  "resources": [
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "12ab34cd56ef78ab90cd12ef34ab56cd"
        },
        {
          "name": "serviceName",
          "value": "kms"
        }
      ]
    }
  ],
  "contexts": [
    {
      "attributes": [
        {
          "name": "networkZoneId",
          "value": "65810ac762004f22ac19f8f8edf70a34"
        }
      ]
    }
  ]
}
```
{: codeblock}

After creating rules, you can also [update](/apidocs/context-based-restrictions#replace-rule) and [delete](/apidocs/context-based-restrictions#delete-rule) them.

### Create Network Rules using the CBR UI
{: #cbr-create-rules-ui}

[Follow the steps to add resources and contexts](/docs/account?topic=account-context-restrictions-create&interface=ui) to your network rule(s), but keep in mind some limitations.

When you create context-based restriction for the IAM Access Groups service, users who don't satisfy the rule will not be able to view any groups in the account, including the public access group.
{: note}

Unlike IAM policies, context-based restrictions don't assign access. Context-based restrictions check that an access request comes from an allowed context that you configure. Also, the rules may not take effect immediately due to synchronization and resource availability. 
{: important}

After creating rules, you can also [update](/docs/account?topic=account-context-restrictions-update&interface=ui) and [delete](/docs/account?topic=account-context-restrictions-update&interface=ui#context-restrictions-remove-rules) them.

## Next steps
{: #cbr-next-steps}

Users who attempt to access your resources outside of the defined zones will receive HTTP error `401` when the appropriate rules have been established.
{: note}

Follow the creation or modification of any zones or rules with adequate testing to ensure access and availability.
