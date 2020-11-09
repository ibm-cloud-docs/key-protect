---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-1"

keywords: service instance, create service instance, KMS service instance, Key Protect service instance

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
{:term: .term}

# Provisioning the {{site.data.keyword.keymanagementserviceshort}} service
{: #provision}

You can create an instance of {{site.data.keyword.keymanagementservicefull}} by
using the {{site.data.keyword.cloud_notm}} console or the
{{site.data.keyword.cloud_notm}} CLI.
{: shortdesc}

## Overview
{: #provision-overview}

In {{site.data.keyword.keymanagementserviceshort}}, an `instance` (sometimes
referred to as a `service instance`) is a namespace.

A namespace organizes keys into logical groups and provides isolation and
protection between namespaces.

- For example, you may choose to have two
  {{site.data.keyword.keymanagementserviceshort}} instances - one for the
  finance department and one for manufacturing. Two instances provides isolation
  so keys in one business unit are not accessible from other business units.

The term `instance` is sometimes used to describe compute resources, such as
[virtual servers](/docs/virtual-servers?topic=virtual-servers-getting-started-tutorial){: external}
or
[virtual server instances for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers){: external}.
An instance, in {{site.data.keyword.keymanagementserviceshort}} should **not**
be equated with compute resources. A
{{site.data.keyword.keymanagementserviceshort}} instance is a namespace.

## Provisioning {{site.data.keyword.keymanagementserviceshort}} from the {{site.data.keyword.cloud_notm}} console
{: #provision-gui}

To provision an instance of {{site.data.keyword.keymanagementserviceshort}} from
the {{site.data.keyword.cloud_notm}} console, complete the following steps.

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://{DomainName}/){: external}.

2. Click **Catalog** to view the list of services that are available on
   {{site.data.keyword.cloud_notm}}.

3. From the All Categories navigation pane, click the **Security and Identity**
   category.

4. From the list of services, click the
   **{{site.data.keyword.keymanagementserviceshort}}** tile.

5. Select a service plan, and click **Create** to provision an instance of
   {{site.data.keyword.keymanagementserviceshort}} in the account, region, and
   resource group where you are logged in.

## Provisioning {{site.data.keyword.keymanagementserviceshort}} from the {{site.data.keyword.cloud_notm}} CLI
{: #provision-cli}

You can also provision an instance of
{{site.data.keyword.keymanagementserviceshort}} by using the
{{site.data.keyword.cloud_notm}} CLI.

### Login using the CLI
{: #provision-login-cli}

Login to {{site.data.keyword.cloud_notm}} through the
[{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.
This is the account used for this instance of
{{site.data.keyword.keymanagementserviceshort}}.

## Login
{: #provision-login}

Login with the `login` command. If this fails you should try the `--sso` option
to login with a federated ID (see the next section).

This example fails because the email address belongs to a federated ID.

```sh
ibmcloud login
```
{: pre}

### Example
{: #provision-login-example}

```
$ ibmcloud login
API endpoint: https://cloud.ibm.com
Region: us-south

Email> <email address>

Password>
Authenticating...
You are using a federated user ID, please use one time passcode ( ibmcloud login --sso ), or use API key ( ibmcloud --apikey key or @key_file ) to authenticate.

API endpoint:   https://cloud.ibm.com
Region:         us-south
Not logged in.
FAILED
```
{: screen}

## Login with a federated ID
{: #provision-login-federated}

Logging in with the `--sso` option opens a browser window (requires a login) and
creates a one-time passcode. The passcode is used to complete the login process.

```sh
ibmcloud login --sso
```
{: pre}

### Example
{: #provision-login-federated-example}

```
$ ibmcloud login --sso
API endpoint: https://cloud.ibm.com
Region: us-south

Get One Time Code from https://identity-2.us-south.iam.cloud.ibm.com/identity/passcode to proceed.
Open the URL in the default browser? [Y/n] > Y
One Time Code > <paste the one-time passcode from your browser>
Authenticating...
OK

Select an account:
1. Account name (ea988d3289c24739a0977651b46fb145)
Enter a number> 1
Targeted account Account name (ea988d3289c24739a0977651b46fb145)

API endpoint:      https://cloud.ibm.com
Region:            us-south
User:              <email address>
Account:           Account name (ea988d3289c24739a0977651b46fb145)
Resource group:    No resource group targeted, use 'ibmcloud target -g RESOURCE_GROUP'
CF API endpoint:
Org:
Space:
```
{: screen}

## Select a region and resource group
{: #provision-select-region}

Select the region and resource group where you would like to create a
{{site.data.keyword.keymanagementserviceshort}} instance.

The `<resource_group_name>` can be `Default` (case-sensitive).

```sh
ibmcloud target -r <region_name> -g <resource_group_name>
```
{: pre}

### Example
{: #provision-select-region-example}

```
$ ibmcloud target -r us-south -g Default
Targeted resource group Default

Switched to region us-south

API endpoint:      https://cloud.ibm.com
Region:            us-south
User:              <email address>
Account:           Account name (ea988d3289c24739a0977651b46fb145)
Resource group:    Default
CF API endpoint:
Org:
Space:
```
{: screen}

## Provision a public instance
{: #provision-public}

Provision a public {{site.data.keyword.keymanagementserviceshort}} instance. The
next section has an example of provisioning a `private` instance.

- Public endpoints are **outside** the {{site.data.keyword.cloud_notm}}.

- Private endpoints are **inside** the {{site.data.keyword.cloud_notm}}.

A public instance accepts API requests from both `public and private` endpoints.
Public network access is the default setting and is used if a policy is not set.

A private instance accepts API requests from only `private` endpoints.

See
[Managing network access policies](/docs/key-protect?topic=key-protect-managing-network-access-policies)
for learn more about public and private access.

**Note** the GUID is the instance ID. In this example, the instance ID is
`ea557753-a15b-4570-a9a3-1efefbd2d382`.

```sh
ibmcloud resource service-instance-create <instance_name> kms tiered-pricing <region>
```
{: pre}

### Example
{: #provision-public-example}

```
# create a public service instance
$ ibmcloud resource service-instance-create <instance_name> kms tiered-pricing us-south
Creating service instance <instance_name> in resource group Default of account <account name> as <email address>...
OK
Service instance <instance_name> was created.

Name:             <instance_name>
ID:               crn:v1:bluemix:public:kms:us-south:a/ea988d3289c24739a0977651b46fb145:ea557753-a15b-4570-a9a3-1efefbd2d382::
GUID:             ea557753-a15b-4570-a9a3-1efefbd2d382
Location:         us-south
State:            active
Type:             service_instance
Sub Type:         kms
Allow Cleanup:    false
Locked:           false
Created at:       2020-05-31T15:04:50Z
Updated at:       2020-05-31T15:04:50Z
Last Operation:
                  Status    create succeeded
                  Message   Completed create instance operation

# list the service instances
$ ibmcloud resource service-instances
Retrieving instances with type service_instance in resource group Default in all locations under account <account name> as <email address>...
OK
Name                 Location   State    Type
<instance_name>      us-south   active   service_instance

# delete the public service instance
$ ibmcloud resource service-instance-delete <instance_name>
Deleting service instance <instance_name> in resource group Default under account <account name> as <email address>...
Really delete the service instance <instance_name> with ID crn:v1:bluemix:public:kms:us-south:a/ea988d3289c24739a0977651b46fb145:ea557753-a15b-4570-a9a3-1efefbd2d382::? [y/N] > y
OK
Service instance <instance_name> with ID crn:v1:bluemix:public:kms:us-south:a/ea988d3289c24739a0977651b46fb145:ea557753-a15b-4570-a9a3-1efefbd2d382:: is deleted successfully
```
{: screen}

## Provision a private instance
{: #provision-private}

Provision a private {{site.data.keyword.keymanagementserviceshort}} instance.

Remember that a private instance accepts API requests from only private
endpoints, which are inside the {{site.data.keyword.cloud_notm}}. You cannot use
the CLI with a private instance if you are outside the
{{site.data.keyword.cloud_notm}}.

```sh
ibmcloud resource service-instance-create <instance_name> kms tiered-pricing <region> -p '{"allowed_network": "private-only"}'
```
{: pre}

### Example
{: #provision-private-example}

After the private instance is created, a request is made to create a key. The
request fails because the user does not have access to the `private` instance
from outside the {{site.data.keyword.cloud_notm}}.

The `-p` option specifies a JSON file or JSON string of parameters used to
create the service instance.

```
# create a private service instance
$ ibmcloud resource service-instance-create <service-name> kms tiered-pricing us-south -p '{"allowed_network": "private-only"}'
Creating service instance <service-name> in resource group Default of account <account name> as <email address>...
OK
Service instance <service-name> was created.

Name:             <service-name>
ID:               crn:v1:bluemix:public:kms:us-south:a/ea988d3289c24739a0977651b46fb145:a152eee4-262e-4a39-ae13-a71b9882dcb6::
GUID:             a152eee4-262e-4a39-ae13-a71b9882dcb6
Location:         us-south
State:            active
Type:             service_instance
Sub Type:         kms
Allow Cleanup:    false
Locked:           false
Created at:       2020-05-31T15:10:23Z
Updated at:       2020-05-31T15:10:23Z
Last Operation:
                  Status    create succeeded
                  Message   Completed create instance operation

# list the service instances
$ ibmcloud resource service-instances
Retrieving instances with type service_instance in resource group Default in all locations under account <account name> as <email address>...
OK
Name                  Location   State    Type
<service-name>        us-south   active   service_instance

# list the private service instance
$ ibmcloud resource service-instance <service-name>
Retrieving service instance <service-name> in resource group Default under account <account name> as <email address>...
OK

Name:                  <service-name>
ID:                    crn:v1:bluemix:public:kms:us-south:a/ea988d3289c24739a0977651b46fb145:a152eee4-262e-4a39-ae13-a71b9882dcb6::
GUID:                  a152eee4-262e-4a39-ae13-a71b9882dcb6
Location:              us-south
Service Name:          kms
Service Plan Name:     tiered-pricing
Resource Group Name:   Default
State:                 active
Type:                  service_instance
Sub Type:              kms
Created at:            2020-05-31T15:10:23Z
Created by:            <email address>
Updated at:            2020-05-31T15:10:23Z
Last Operation:
                       Status    create succeeded
                       Message   Completed create instance operation

# list keys in the private service instance
$ ibmcloud kp keys -i a152eee4-262e-4a39-ae13-a71b9882dcb6
Retrieving keys...

FAILED

kp.Error: correlation_id='59578794-2b5b-48ca-9074-bbe23760d94a', msg='Unauthorized: The user does not have access to the specified resource'

# delete the private service instance
$ ibmcloud resource service-instance-delete <service-name>
Deleting service instance <service-name> in resource group Default under account <account name> as <email address>...
Really delete the service instance <service-name> with ID crn:v1:bluemix:public:kms:us-south:a/ea988d3289c24739a0977651b46fb145:a152eee4-262e-4a39-ae13-a71b9882dcb6::? [y/N] > y
OK
Service instance <service-name> with ID crn:v1:bluemix:public:kms:us-south:a/ea988d3289c24739a0977651b46fb145:a152eee4-262e-4a39-ae13-a71b9882dcb6:: is deleted successfully
```
{: screen}

## Deleting an instance with existing keys
{: #provision-delete}

You cannot delete a service instance that contains keys.

### Example
{: #provision-delete-example}

```
% ibmcloud resource service-instance-delete <instance_name>
Deleting service instance <instance_name> in resource group Default under account <account name> as <email address>...
Really delete the service instance <instance_name> with ID crn:v1:bluemix:public:kms:us-south:a/ea988d3289c24739a0977651b46fb145:a152eee4-262e-4a39-ae13-a71b9882dcb6::? [y/N] > y
FAILED
Error Code: RC-ServiceBrokerErrorResponse
Message: [409, Conflict] Conflict: Instance contains 2 active keys. Remove all keys before de-provisioning
```
{: screen}

## What's next
{: #provision-service-next-steps}

To find out more about programmatically managing your keys,
[check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc](/apidocs/key-protect){: external}.
