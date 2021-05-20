---

copyright:
  years: 2019, 2021
lastupdated: "2021-04-01"

keywords: crypto erasure, enable BYOK, onboard to Key Protect, Key Protect onboarding, internal, key registration, KYOK, BYOK

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# BYOK cheatsheet
{: #byok-cheatsheet}

A consolidated list to help your service properly enable Bring Your Own Key
(BYOK).
{: shortdesc}

## Core concepts
{: #byok-concepts}

Although you don't necessarily need to understand the details of each topic,
they are there for your awareness. The important thing to know is that if your
service addresses each concept, you will have enabled BYOK appropriately.

- You must have a basic understanding of IAM concepts, such as
  [granting service to service access](/docs/get-coding?topic=get-coding-servicetoservice){: external}.

- You must have a basic understanding of the
  [envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption) process.

- You must have a basic understanding of how the platform uses
  [Global Search and Tagging (GhoST)](/docs/get-coding?topic=get-coding-ghost_overview){: external}.

- You must have a basic understanding of
  [key registration](/docs/key-protect?topic=key-protect-register-protected-resources).

## Required actions
{: #byok-required-actions}

If you complete all the following steps, your service will meet all the BYOK
requirements. When you plan to adopt BYOK, follow the steps listed in order.
You can also use these steps to estimate and group the effort required to become
BYOK ready.

| Actions                                                                                                                                        | Steps |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ----- |
| [Onboard your service to {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-onboard-service)                 | <ol><li>[Submit a request to onboard your service](/docs/key-protect?topic=key-protect-onboard-service#submit-request)</li><li>[Create a CRN token](/docs/key-protect?topic=key-protect-onboard-service#submit-request)</li><li>[Discover KMS instances](/docs/key-protect?topic=key-protect-onboard-service#discover-kms-instances)</li></ol> |
| [Set up the {{site.data.keyword.keymanagementserviceshort}} APIs](/docs/key-protect?topic=key-protect-configure-api)                           | <ol><li>[Create a test instance](/docs/key-protect?topic=key-protect-configure-api#configure-provision-service)</li><li>[Create a root key](/docs/key-protect?topic=key-protect-configure-api#create-root)</li><li>[Wrap a data encryption key](/docs/key-protect?topic=key-protect-configure-api#wrap-key)</li><li>[Unwrap the data encryption key](/docs/key-protect?topic=key-protect-configure-api#unwrap-key)</li></ol> |
| [Map cloud resources to a root key](/docs/key-protect?topic=key-protect-register-protected-resources)                                          | <ol><li>[Register cloud resources](/docs/key-protect?topic=key-protect-register-protected-resources#create-registration)</li><li>[De-register cloud resources](/docs/key-protect?topic=key-protect-register-protected-resources#delete-registration)</li></ol> |
| [Onboard your service to Hyperwarp](https://github.ibm.com/kms/BYOK_Adopter_services/blob/master/How_to_subscribe_to_hyperwarp.md){: external} | <ol><li>[Submit a request to BSS to onboard your service](https://github.ibm.com/kms/BYOK_Adopter_services/blob/master/How_to_subscribe_to_hyperwarp.md#details){: external}</li><li>[Determine if your service will need a regional or global subscription](https://github.ibm.com/kms/BYOK_Adopter_services/blob/master/How_to_subscribe_to_hyperwarp.md#regional-vs-global-subscription){: external}</li><li>[Submit a Hyperwarp Integration request with {{site.data.keyword.keymanagementserviceshort}}](https://github.ibm.com/kms/customer-issues/blob/master/.github/ISSUE_TEMPLATE/hyperwarp-integration-onboard-request.md){: external}</li></ol> |

## Features
{: #byok-features}

The following features are available for BYOK enabled services. For up-to-date
information, reach out to us in the `#keyprotect-byok-adopters-channel` Slack
channel.

Use the
[adopter's guide](https://github.ibm.com/kms/BYOK_Adopter_services/blob/master/Key%20Protect%20Adopters_v1.docx){: external}
for more information on BYOK requirements, features, and use cases.
{: note}

### Plan for cryptoerasure
{: #byok-plan-for-cryptoerasure}

[Plan for cryptoerasure](/docs/key-protect?topic=key-protect-key-erasure)

BYOK Crypto Erasure is an effort to bridge the gap and ensure that when a
customer deletes the key, it raises service events that invalidates the data
encryption keys (DEKs) without deleting the data source itself.

1. [Delete a root key](/docs/key-protect?topic=key-protect-delete-keys)

2. [Check for a Hyperwarp deletion event from {{site.data.keyword.keymanagementserviceshort}}](https://github.ibm.com/kms/BYOK_Adopter_services/blob/master/How_to_subscribe_to_hyperwarp.md#event-structure){: external}

3. [Process Hyperwarp deletion event](https://github.ibm.com/kms/Adopter_services/blob/master/src/github.ibm.com/skms/key-protect/event_processor.go){: external}

4. [Notify {{site.data.keyword.keymanagementserviceshort}} that the event was processed](/apidocs/key-protect#acknowledge-key-events){: external}

5. [Verify end to end deletion flow in {{site.data.keyword.at_short}} logs](/docs/observability?topic=observability-pattern1#pattern1_step4){: external}

### Plan for key rotation
{: #byok-plan-for-key-rotation}

[Plan for key rotation](/docs/key-protect?topic=key-protect-dek-rewrap)

When a root key is rotated, your service must rewrap all DEKs associated with
the rotated key when notified. Your service needs to persist the new wrapped
data encryption key (wDEK).

1. [Rotate a root key](/docs/key-protect?topic=key-protect-rotate-keys)

2. [Check for a Hyperwarp rotation event from {{site.data.keyword.keymanagementserviceshort}}](https://github.ibm.com/kms/BYOK_Adopter_services/blob/master/How_to_subscribe_to_hyperwarp.md#event-structure){: external}

3. [Process Hyperwarp rotation event](https://github.ibm.com/kms/Adopter_services/blob/master/src/github.ibm.com/skms/key-protect/event_processor.go){: external}

4. [Notify {{site.data.keyword.keymanagementserviceshort}} that the event was processed](/apidocs/key-protect#acknowledge-key-events){: external}

5. [Verify end to end rotation flow in {{site.data.keyword.at_short}} logs](/docs/observability?topic=observability-pattern1#pattern1_step4){: external}

### Enable / Disable a root key
{: #byok-enable-disable-a-root-key}

[Enable / Disable a root key](/docs/key-protect?topic=key-protect-disable-keys)

As an admin, you might need to temporarily disable a root key if you suspect a
possible security exposure, compromise, or breach with your data.

When you disable a root key, you suspend its encrypt and decrypt operations.
After confirming that a security risk is no longer active, you can restore
access to your data by enabling the disabled root key.

1. [Disable/Enable a root key](/apidocs/key-protect#invoke-an-action-on-a-key){: external}
[Check for a Hyperwarp event from {{site.data.keyword.keymanagementserviceshort}}](https://github.ibm.com/kms/BYOK_Adopter_services/blob/master/How_to_subscribe_to_hyperwarp.md#event-structure){: external}

2. [Process Hyperwarp enable/disable event](https://github.ibm.com/kms/Adopter_services/blob/master/src/github.ibm.com/skms/key-protect/event_processor.go){: external}

3. [Notify {{site.data.keyword.keymanagementserviceshort}} that the event was processed](/apidocs/key-protect#acknowledge-key-events){: external}

4. [Verify end to end enable/disable flow in {{site.data.keyword.at_short}} logs](/docs/observability?topic=observability-pattern1#pattern1_step4){: external}

### Restore a root key
{: #byok-restore-a-root-key}

[Restore a root key](/docs/key-protect?topic=key-protect-disable-keys)

As an admin, you might need to temporarily disable a root key if you suspect a
possible security exposure, compromise, or breach with your data.

When you disable a root key, you suspend its encrypt and decrypt operations.
After confirming that a security risk is no longer active, you can restore
access to your data by enabling the disabled root key.

1. [Restore a root key](/apidocs/key-protect#invoke-an-action-on-a-key){: external}

2. [Check for a Hyperwarp restoration event from {{site.data.keyword.keymanagementserviceshort}}](https://github.ibm.com/kms/BYOK_Adopter_services/blob/master/How_to_subscribe_to_hyperwarp.md#event-structure){: external}

3. [Process Hyperwarp restoration event](https://github.ibm.com/kms/Adopter_services/blob/master/src/github.ibm.com/skms/key-protect/event_processor.go){: external}

4. [Notify {{site.data.keyword.keymanagementserviceshort}} that the event was processed](/apidocs/key-protect#acknowledge-key-events){: external}

5. [Verify end to end restoration flow in {{site.data.keyword.at_short}} logs](/docs/observability?topic=observability-pattern1#pattern1_step4){: external}

### Encryption key location
{: #byok-encryption-key-location}

Your service should allow keys from any region to protect resources in the
region where service is deployed.

Your service should not enforce that the key should be allocated in the same
region where the resource is located.

## Policies
{: #byok-policies}

In the event that your service cannot contact
{{site.data.keyword.keymanagementserviceshort}} for any reason to unwrap a data
encryption key and you have the key cached, you can continue using the DEK up to
4 hours after it was last refreshed (unwrapped via the KMS). If after 4 hours,
you still cannot contact {{site.data.keyword.keymanagementserviceshort}}, your
service MUST throw away the cached DEK and refuse to decrypt customer data.
