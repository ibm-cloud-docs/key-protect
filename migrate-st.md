---

copyright:
  years: 2026
lastupdated: "2026-04-02"

keywords: Key Protect migration, Hyper Protect Crypto services migration, HPCS migration, migration

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Migrating from Hyper Protect Crypto Services (HPCS) to {{site.data.keyword.keymanagementserviceshort}} Dedicated
{: #migrate-st}

If you use Hyper Protect Crypto Services (HPCS) and need to migrate to {{site.data.keyword.keymanagementserviceshort}} Dedicated, follow this migration guide. It covers:
{: shortdesc}

**Assessment Phase:**
- [Identifying HPCS usage](#migration-identify-hpcs-usage)
- [Searching for usage](#migration-search-for-usage)
- [Classifying usage](#migration-classify-usage)

**Migration by Feature:**
- [Customer root keys (CRKs)](#migration-crk-migration)
- [Standard keys](#migration-standard-key-migration)
- [KMIP for VMWare](#kmip-migration)
- [PKCS #11 (GREP11)](#migration-pkcs11-grep11)
- [Unified Key Orchestrator (UKO)](#migration-uko)
- [Terraform](#migration-terraform)
- [CLI provisioning](#migration-cli)
- [Secure import](#migration-secure-import)

**Completion:**
- [Post migration validation](#migration-post-migration)

## Identifying HPCS usage
{: #migration-identify-hpcs-usage}

Check all {{site.data.keyword.cloud_notm}} accounts for [HPCS](/docs/hs-crypto?topic=hs-crypto-get-started&interface=ui) instances.

For each {{site.data.keyword.cloud_notm}} account, run the following [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started) [command](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_service_instances):

```sh
ibmcloud resource service-instances --all-resource-groups --long --service-name hs-crypto --limit 100
```
{: pre}

Before you run the command, confirm that the {{site.data.keyword.cloud_notm}} CLI is targeting the intended account:

```sh
ibmcloud target
```
{: pre}

Verify that the account that is shown in the output matches the account that you want to check. The {{site.data.keyword.cloud_notm}} CLI operates on a single active account at a time. Running the command against the wrong account might result in missing HPCS instances.
{: tip}

Ensure that you:

- Log in with a user that is an account administrator with account-wide Viewer (platform) and Reader (service) access across all services.
- Explicitly target each account that you want to check by using the following command:
  
    ```sh
    ibmcloud target -c <account_id>
    ```
    {: pre}

The command searches for HPCS instances across all resource groups in the targeted account, but only instances that you are authorized to see are returned. An empty result might indicate insufficient permissions or the absence of HPCS instances.

Listing HPCS instances requires service-level access because {{site.data.keyword.cloud_notm}} IAM enforces both platform and service authorization, and HPCS restricts instance discovery to authorized users.

You can also verify HPCS usage by reviewing {{site.data.keyword.cloud_notm}} billing reports. The presence of HPCS charges indicates that an HPCS instance exists in the account. To do so, log in to IBM Cloud with a user that is an account administrator and has sufficient permissions to view billing and usage data, open [https://cloud.ibm.com/billing/usage](https://cloud.ibm.com/billing/usage) and check for usage type `Hyper Protect Crypto Services`.

You can also verify HPCS usage by reviewing the IBM Cloud Resource list. To do so log in to IBM Cloud with a user that is an account administrator with account-wide Viewer (platform) and Reader (service) access across all services, open [https://cloud.ibm.com/resources](https://cloud.ibm.com/resources) and check for resource instances of product `Hyper Protect Crypto Services`.

For more information about IAM roles and how to assign access, check out [{{site.data.keyword.cloud_notm}} IAM roles](/docs/account?topic=account-userroles).

If no HPCS instances exist, no migration is required.

## Searching for usage
{: #migration-search-for-usage}

If you have HPCS instances, you need to determine how you are using those resources. The following table describes various methods for identifying HPCS usage:

| Method | Description | Considerations |
|--------|-------------|----------------|
| [Activity tracking events](/docs/hs-crypto?topic=hs-crypto-at-events) | Provides factual indication of HPCS usage through logged events | Search for events by using the largest time window possible. Lack of events does not necessarily mean no usage. Usage can occur during rare events (for example, restart of an {{site.data.keyword.cloud_notm}} service instance) or between long intervals that might exceed the event retention period. |
| [Associations](/docs/hs-crypto?topic=hs-crypto-view-protected-resources&interface=ui) | Shows HPCS usage by {{site.data.keyword.cloud_notm}} resources | Lack of associations does not necessarily mean no usage due to the nature of distributed computing systems in which resources are not always in sync. Conversely, the presence of associations does not necessarily mean active usage. Associations can be stale. Some {{site.data.keyword.cloud_notm}} resources do not create or use associations. List associations by using the [`kp registrations` command](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-registrations). |
| [Sync associated resources](/docs/hs-crypto?topic=hs-crypto-sync-associated-resources&interface=ui) | Improves synchronization of associations | Use the [`kp key sync` command](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-key-sync) to explicitly sync associated resources and get more accurate association data. |
| [Key Usage Reporter (KUR)](/docs/key-protect?topic=key-protect-kur) | CLI tool provided by IBM that scans {{site.data.keyword.cloud_notm}} accounts and generates a report of resources that reference HPCS keys, which are grouped by KMS instance and key. Also, capable of processing activity tracking audit log files. | Discovery and reporting tool only. Does not perform migration actions. The tool might not detect all possible usages of keys. |
{: caption="Table 1. Methods for identifying HPCS usage" caption-side="bottom"}

Two separate tools are referenced in this document:

- **Key Migration Tool (CRKM)** – used to create migration intents and trigger synchronization. This tool is required for automated CRK migration, see [Key Migration Tool (CRKM)](/docs/key-protect?topic=key-protect-migrate-tool).
- **Key Usage Reporter (KUR)** – a discovery and reporting tool that is used to identify services that reference HPCS keys. KUR does not perform migration actions. See [Key Usage Reporter (KUR)](/docs/key-protect?topic=key-protect-kur).

Before you proceed with migration activities, ensure that you have the latest version of the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in installed. This update ensures compatibility with all migration features and commands.

To check your current plug-in version:

```sh
ibmcloud plugin show key-protect
```
{: pre}

To update the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in to the latest version:

```sh
ibmcloud plugin update key-protect
```
{: pre}

If the plug-in is not installed, you can install it by running:

```sh
ibmcloud plugin install key-protect
```
{: pre}

For both HPCS and {{site.data.keyword.keymanagementserviceshort}} Dedicated, the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in must read the target instance endpoint from the environment variable `KP_TARGET_ADDR`. The `KP_TARGET_ADDR` variable works for both private and public endpoints.

This example command targets an example HPCS instance:

```sh
export KP_TARGET_ADDR=https://fadedbee-0000-0000-0000-1234567890ab.api.us-south.hs-crypto.appdomain.cloud
```
{: pre}

This example command targets an example {{site.data.keyword.keymanagementserviceshort}} Dedicated instance:

```sh
export KP_TARGET_ADDR=https://fadedbee-0000-0000-0000-1234567890ab.api.us-south.kms.appdomain.cloud
```
{: pre}

You can find the instance endpoint for both HPCS and {{site.data.keyword.keymanagementserviceshort}} Dedicated in the {{site.data.keyword.cloud_notm}} UI console for the specific instance.

When you follow this document and use the {{site.data.keyword.cloud_notm}} CLI to connect to HPCS, ensure that your login user has an IAM policy at the HPCS instance level. An IAM policy at the key ring level or the key level might not list all associations and other resources.

## Custom apps versus {{site.data.keyword.cloud_notm}} services and software HPCS usage
{: #migration-custom-apps-vs-ibm-cloud}

HPCS usage comes from two main sources: custom apps and {{site.data.keyword.cloud_notm}} services or software.

### Custom apps
{: #migration-custom-apps}

HPCS usage by custom apps occurs when custom code or ISV applications make direct use of HPCS.

Searching for HPCS usage by custom apps is a task that you need to perform with the help of [HPCS activity tracking events](/docs/hs-crypto?topic=hs-crypto-at-events), code search, and other methods.

Search for usage of the following:

- References to host names that contain `hs-crypto`
- [HPCS REST API](/apidocs/hs-crypto)
- [GREP11](/docs/hs-crypto?topic=hs-crypto-uko-grep11-intro)
- [HPCS CLI plug-ins](/docs/hs-crypto?topic=hs-crypto-cli-change-log&interface=ui)

Also, search for usage of HPCS through the HPCS PKCS11 library:
- [HPCS PKCS #11](https://github.com/IBM-Cloud/hpcs-pkcs11)

Also, search for usage through client SDKs:

- [Keyprotect-go-client](https://github.com/IBM/keyprotect-go-client)
- [Keyprotect-python-client](https://github.com/IBM/keyprotect-python-client)
- [Keyprotect-java-client](https://github.com/IBM/keyprotect-java-client)
- [Keyprotect-nodejs-client](https://github.com/IBM/keyprotect-nodejs-client)
- [redstone](https://github.com/IBM/redstone)
- [hpcs-grep11](https://github.com/IBM-Cloud/hpcs-grep11)

Search IAM identities with access to HPCS, mostly [service IDs](/docs/account?topic=account-serviceids&interface=ui) and [Trusted Profiles](/docs/vpc?topic=vpc-imd-trusted-profile-metadata&interface=ui), and less common user identities. Any identity with roles that are scoped to the HPCS service, instance, key ring, or key is a strong indicator of potential custom application usage.

### {{site.data.keyword.cloud_notm}} services and software
{: #migration-ibm-cloud-services}

To identify {{site.data.keyword.cloud_notm}} services and software that are using HPCS, follow this recommended approach:

1. **Start with the Key Usage Reporter (KUR)** - The [Key Usage Reporter (KUR)](/docs/key-protect?topic=key-protect-kur) tool is the recommended starting point. It scans your {{site.data.keyword.cloud_notm}} accounts and generates a comprehensive report of resources that reference HPCS keys, which are grouped by service and key.

2. **Cross-reference with Activity Tracking** - Review [HPCS activity tracking events](/docs/hs-crypto?topic=hs-crypto-at-events) over the largest available time window to identify services that performed cryptographic operations. The [Key Usage Reporter (KUR)](/docs/key-protect?topic=key-protect-kur) tool can process activity tracking audit log files, producing CSV summaries that help identify HPCS utilization.

## Classifying usage
{: #migration-classify-usage}

Each type of HPCS usage relevant to the migration falls into one of the following categories:

| Usage type | Description |
|-----------|-------------|
| [Customer root keys (CRKs)](/docs/hs-crypto?topic=hs-crypto-envelope-encryption#key-types) | Encryption of data encryption keys |
| [Standard keys](/docs/hs-crypto?topic=hs-crypto-envelope-encryption#key-types) | Secrets |
| [KMIP for VMWare](/docs/vmwaresolutions?topic=vmwaresolutions-kmip_standalone_considerations) | Used by VMware KMIP clients |
| [Enterprise PKCS#11 keys](/docs/hs-crypto?topic=hs-crypto-pkcs11-intro) | Used through PKCS #11 or [GREP11](/docs/hs-crypto?topic=hs-crypto-uko-grep11-intro) interfaces |
| [UKO-managed keys](/docs/hs-crypto?topic=hs-crypto-introduce-uko) | Managed by Unified Key Orchestrator |
| [Terraform](/docs/hs-crypto?topic=hs-crypto-terraform-setup-for-hpcs) | Provisioning HPCS instances by using infrastructure as code |
| [Instance provisioning by using the IBM Cloud CLI](/docs/hs-crypto?topic=hs-crypto-hpcs-cli-plugin) | Instance provisioning |
| [Secure import of root key material](/docs/hs-crypto?topic=hs-crypto-importing-keys) | Optionally used as part of key import |
{: caption="Table 2. HPCS usage types" caption-side="bottom"}

## Migrating your root keys (CRKs)
{: #migration-crk-migration}

### Checking for the existence of CRKs
{: #migration-check-crk-existence}

Use the following Bash script to count the total number of CRKs in all [states](/docs/hs-crypto?topic=hs-crypto-key-states) for an HPCS instance.

Make sure that you are logged in to {{site.data.keyword.cloud_notm}} through the {{site.data.keyword.cloud_notm}} CLI.
{: important}


```sh
# count the total number of CRKs in all states 
HPCS_ADDR=https://fadedbee-0000-0000-0000-1234567890ab.api.us-south.hs-crypto.appdomain.cloud
HPCS_INSTANCE_ID=fadedbee-0000-0000-0000-1234567890ab
AUTH_HEADER="${AUTH_TOKEN:-$(jq -r .IAMToken ~/.bluemix/config.json)}"
header="$(curl -i -k -s --head \
  "${HPCS_ADDR}/api/v2/keys?state=0,1,2,3,5&extractable=false" \
  -H "authorization: ${AUTH_HEADER}" \
  -H "bluemix-instance: ${HPCS_INSTANCE_ID}" \
  -H "prefer: return=representation" \
| grep '^Key-Total:' \
| tr -d '\r')"
if [ -n "$header" ]; then
  total="${header#Key-Total: }"
  echo "Total number of CRKs in all states: $total"
else
  echo "Error: Key-Total header not found. Try logging into IBM Cloud again. Check endpoint, auth token, or permissions." >&2
fi
```
{: pre}

Replace `HPCS_ADDR` and `HPCS_INSTANCE_ID` with valid values for each HPCS instance.
You can find the instance endpoint for HPCS and the instance ID in the {{site.data.keyword.cloud_notm}} UI console for the specific instance.

The output looks similar to the following example:

```sh
Total number of CRKs in all states: 11
```
{: screen}

If zero CRKs exist across all HPCS instances, CRK migration is not required.
{: tip}

Check the count of CRKs in the Active (1) and Deactivated (Expired) (3) states by using the following script. Only CRKs in the Active (1) or Deactivated (Expired) (3) states can be used for cryptographic operations such as [wrap](/docs/hs-crypto?topic=hs-crypto-wrap-keys), [unwrap](/docs/hs-crypto?topic=hs-crypto-unwrap-keys), and [rewrap](/docs/hs-crypto?topic=hs-crypto-rewrap-keys). A CRK in Deactivated (3) supports unwrap and rewrap but not wrap.

```sh
# count the total number of CRKs in Active (1) or Deactivated (Expired) (3) states
HPCS_ADDR=https://fadedbee-0000-0000-0000-1234567890ab.api.us-south.hs-crypto.appdomain.cloud
HPCS_INSTANCE_ID=fadedbee-0000-0000-0000-1234567890ab
AUTH_HEADER="${AUTH_TOKEN:-$(jq -r .IAMToken ~/.bluemix/config.json)}"
header="$(curl -i -k -s --head \
  "${HPCS_ADDR}/api/v2/keys?state=1,3&extractable=false" \
  -H "authorization: ${AUTH_HEADER}" \
  -H "bluemix-instance: ${HPCS_INSTANCE_ID}" \
  -H "prefer: return=representation" \
| grep '^Key-Total:' \
| tr -d '\r')"
if [ -n "$header" ]; then
  total="${header#Key-Total: }"
  echo "Total number of CRKs in Active (1) or Deactivated (Expired) (3) states: $total"
else
  echo "Error: Key-Total header not found. Try logging into IBM Cloud again. Check endpoint, auth token, or permissions." >&2
fi
```
{: pre}

If zero CRKs exist in the Active (1) or Deactivated (Expired) (3) states across all HPCS instances, no CRKs are available for cryptographic operations. However, HPCS might still be in use. Resources or applications might still be configured to reference CRKs in other states. Any attempt to perform cryptographic operations with such CRKs fails.

Interpreting CRK counts:

| Condition | Interpretation | Action |
|----------|---------------|--------|
| Total CRKs = 0 (all states) | No CRKs exist in any HPCS instance | CRK migration is not required |
| Total CRKs > 0, but Active (1) + Deactivated (3) = 0 | No CRKs are currently usable for cryptographic operations | Migration might still be required. Verify whether any resources or applications reference CRKs in other states |
| Active (1) or Deactivated (3) CRKs exist | CRKs are available for cryptographic operations (fully or partially) | CRK migration is required |

You can obtain the full CRN of HPCS CRKs by using the {{site.data.keyword.cloud_notm}} CLI [`kp keys` command](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-keys).

The following example lists CRKs in all states:

```sh
export KP_TARGET_ADDR=https://fadedbee-0000-0000-0000-1234567890ab.api.us-south.hs-crypto.appdomain.cloud
ibmcloud kp keys --instance-id fadedbee-0000-0000-0000-1234567890ab --crn --key-type root-key --key-states active,suspended,deactivated,destroyed --number-of-keys 5000
```
{: pre}

Replace `KP_TARGET_ADDR` with valid values for each HPCS instance.
You can find the instance endpoint for HPCS in the {{site.data.keyword.cloud_notm}} UI console for the specific instance.

- The command lists CRKs in all states, including Suspended (Disabled) and Destroyed (soft deleted) states.
- Active cryptographic use of CRKs in Suspended (Disabled) and Destroyed (soft deleted) states is not allowed but CRKs in those states can still be referenced by {{site.data.keyword.cloud_notm}} resources or custom code.
- It is possible to move CRKs from Suspended (Disabled) and Destroyed (soft deleted) states to Active state.
- The command can list up to 5000 CRKs at a time. Pagination might be needed to list all CRKs.

### Customer root keys (CRKs) migration in custom applications
{: #migration-crk-migration-custom-apps}

Custom applications can migrate CRKs to {{site.data.keyword.keymanagementserviceshort}} Dedicated by rewrapping the data encryption key (DEK) that is protected by HPCS.

The migration process involves the following steps:

- Unwrap the [data encryption key](/docs/hs-crypto?topic=hs-crypto-envelope-encryption#envelope-encryption-overview) (DEK) from HPCS.
- Wrap the DEK with {{site.data.keyword.keymanagementserviceshort}} Dedicated.
- Generate a new wrapped data encryption key (WDEK).
- Use the new WDEK for subsequent cryptographic operations.

In all cases, custom applications must:

- Use a different endpoint. A {{site.data.keyword.keymanagementserviceshort}} Dedicated instance has an endpoint that is specific to that instance.
- Use a different key ID.
- Use an IAM identity, most likely a service ID, that allows access to {{site.data.keyword.keymanagementserviceshort}} Dedicated at the appropriate level. New IAM policies that target {{site.data.keyword.keymanagementserviceshort}} Dedicated might be required.

For more information about the {{site.data.keyword.keymanagementserviceshort}} API, see the [{{site.data.keyword.keymanagementserviceshort}} API reference](/apidocs/key-protect).

### Customer root keys (CRKs) migration in {{site.data.keyword.cloud_notm}} services and software
{: #migration-crk-migration-ibm-cloud-services}

Some {{site.data.keyword.cloud_notm}} services and IBM software that integrate with HPCS can participate in an automated CRK migration workflow to {{site.data.keyword.keymanagementserviceshort}} Dedicated. This workflow is based on migration intents and key lifecycle synchronization events and minimizes disruption while preserving cryptographic continuity.

#### Alternative migration path: re-creating service instances
{: #migration-alternative-path}

An alternative to the migration intent workflow is to create a new instance of the {{site.data.keyword.cloud_notm}} service and configure it with a CRK from {{site.data.keyword.keymanagementserviceshort}} Dedicated from the start. You then copy the data and metadata from the existing service instance to the new one. After the new instance is verified, the original instance that uses the HPCS CRK can be decommissioned.

This approach might cause interruption of service during the transition period while data is being copied and references are updated to point to the new instance. The tradeoff is that this approach requires provisioning new infrastructure, copying data, and updating any references (for example, endpoints, bindings, or application configuration) that point to the original service instance. Evaluate the operational cost of recreating the service instance against the simplicity of starting fresh with a {{site.data.keyword.keymanagementserviceshort}} Dedicated CRK.

The following sections describe the migration intent model, prerequisites, migration flow, and how to monitor progress.

#### Migration overview
{: #migration-overview}

In {{site.data.keyword.cloud_notm}} services, Customer Root Keys (CRKs) are typically used to encrypt service-managed Data Encryption Keys (DEKs). During migration, the DEKs are rewrapped so that they become encrypted by a {{site.data.keyword.keymanagementserviceshort}} Dedicated CRK instead of an HPCS CRK without requiring you to reencrypt data.

At a high level, the migration works as follows:

1. You declare an intent to migrate an HPCS CRK to a specific {{site.data.keyword.keymanagementserviceshort}} Dedicated CRK.
2. {{site.data.keyword.cloud_notm}} services that are associated with that HPCS CRK detect the intent.
3. Each service rewraps its DEKs and updates its key associations.
4. Associations with the HPCS CRK are removed after migration is complete.

#### Prerequisites
{: #migration-prerequisites}

Before you start CRK migration for {{site.data.keyword.cloud_notm}} services and software, ensure that the following requirements are met:

- Service support
  - Only IBM Cloud services and software that explicitly support HPCS-to-{{site.data.keyword.keymanagementserviceshort}} Dedicated CRK Migration Intent can participate.
  - Currently, the IBM services and software that support Migration Intent are:
    - [App Config](/docs/app-configuration?topic=app-configuration-getting-started)
    - [Block Storage for VPC](/docs/vpc?topic=vpc-block-storage-about)
    - [Cloud Object Storage (COS)](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage)
    - [Database Services (ICD)](https://www.ibm.com/products/cloud-databases)
    - [Event Notifications](/docs/event-notifications?topic=event-notifications-en-about)
    - [Event Streams](/docs/EventStreams?topic=EventStreams-about)
    - [Secrets Manager](/docs/secrets-manager?topic=secrets-manager-getting-started)
      
- Support for the following IBM services and software is not currently available:
    - [Kubernetes (IKS)](/docs/containers)
    - [Red Hat OpenShift (ROKS)](/docs/openshift)
    - [Schematics](/docs/schematics?topic=schematics-learn-about-schematics)
    - [VPC File Storage](/docs/vpc?topic=vpc-file-storage-vpc-about)
    - [VPC Images](/docs/vpc?topic=vpc-planning-custom-images)
    - [VPC VSI](/docs/vpc?topic=vpc-about-advanced-virtual-servers)

    You do not need to wait for all services to support migration intents before you begin the migration. Use the [Key Usage Reporter (KUR)](/docs/key-protect?topic=key-protect-kur) tool and [activity tracking events](/docs/hs-crypto?topic=hs-crypto-at-events) to determine which services are using your HPCS CRKs. If your HPCS keys are used only by services that support migration intents, you can complete the migration now.

    A single HPCS CRK can be used by both supported and unsupported services at the same time. In this case, create the migration intent now. The services that support migration intents detect the intent and complete their migration. The migration intent remains attached to the CRK. When more services add migration intent support, you need to run the sync command from the [Key Migration Tool (CRKM)](/docs/key-protect?topic=key-protect-migrate-tool) on the same CRKs. You do not need to create new migration intents.

    This means that you can start the migration process today and return later to complete it for the remaining services as support becomes available.
    {: tip}

- Target CRKs
:   {{site.data.keyword.keymanagementserviceshort}} Dedicated CRKs must exist. Target CRKs can be generated or imported, with or without customer-supplied key material, by using the API, CLI, or the UI.

- IAM authorization
:   Service-to-service IAM authorization policies must allow {{site.data.keyword.cloud_notm}} services to access the {{site.data.keyword.keymanagementserviceshort}} Dedicated instance, key ring, or individual key. Refer to the documentation of each service for information about how to establish those service-to-service IAM authorization policies. The Service-to-service IAM authorization policies must be defined in the same account as the target {{site.data.keyword.keymanagementserviceshort}} Dedicated instance. That account might be different than the account of the service instance. For use cases such as IBM Cloud Databases, Messages for RabbitMQ, Kubernetes, and OpenShift services, ensure that delegated authorization is enabled when you create the IAM policy. Most cases of failed migrations occur because this step is not performed or is performed incorrectly.

#### Migration intents
{: #migration-intents}

A migration intent is an optional subresource that is attached to an HPCS CRK. It specifies the target {{site.data.keyword.keymanagementserviceshort}} Dedicated CRK by CRN.

To initiate migration:

1. Create a migration intent on the source HPCS CRK.
2. The intent references the target {{site.data.keyword.keymanagementserviceshort}} Dedicated CRK.
3. Migration intents are created by using [the Key Migration Tool](/docs/key-protect?topic=key-protect-migrate-tool). 

After a migration intent is created, the HPCS service emits synchronization events (one per existing Association) that inform {{site.data.keyword.cloud_notm}} services that a migration is requested.

For some services (for example, IBM Cloud Databases, Messages for RabbitMQ, Kubernetes, and OpenShift services), more synchronization events must be explicitly triggered a few minutes after intent creation. You can trigger these events by using the Key Migration Tool sync command.

#### Migration logic used by {{site.data.keyword.cloud_notm}} services
{: #migration-logic}

When an {{site.data.keyword.cloud_notm}} service processes a migration intent for an HPCS CRK, it performs the following steps:

1. **Unwrap**: The service unwraps the existing Wrapped DEK (WDEK) by calling HPCS to retrieve the plaintext DEK.

2. **Wrap**: The DEK is wrapped by using the target {{site.data.keyword.keymanagementserviceshort}} Dedicated CRK, producing a new WDEK.

3. **Replace**: The service replaces the HPCS-wrapped DEK with the {{site.data.keyword.keymanagementserviceshort}} Dedicated-wrapped DEK.

4. **Association**: A new association is created in {{site.data.keyword.keymanagementserviceshort}} Dedicated, linking the target CRK to the service resource.

5. **Inform**: The service notifies HPCS that migration for that resource is complete, which causes HPCS to automatically remove the original association.

This process is performed independently by each service resource that is associated with the HPCS CRK.

#### Monitoring migration progress
{: #monitoring-migration}

You can monitor migration progress by using several mechanisms:

Associations
:   The number of Associations that are associated with the HPCS CRK decreases, ideally to zero if no state associations exist. The number of associations that are associated with the {{site.data.keyword.keymanagementserviceshort}} Dedicated CRK increases.

Key Migration Tool (CRKM)
:   Reports Association counts for both source and target CRKs. Supports bulk status inspection and retry operations.

Manual synchronization
:   Synchronization events can be retriggered at any time through the REST API or Key Migration Tool to retry incomplete migrations.

For [Event Streams](/docs/EventStreams?topic=EventStreams-about), after the creation of a migration intent, the migration might take up to one business day.
For other services, the migration is expected to finish in less than four hours.

#### Identifying HPCS CRK usage with the Key Usage Reporter (KUR)
{: #migration-key-usage-reporter}

To assist with identifying {{site.data.keyword.cloud_notm}} services that are using HPCS Customer Root Keys (CRKs), IBM provides the [Key Usage Reporter (KUR) tool](/docs/key-protect?topic=key-protect-kur).

KUR is a command-line tool that scans {{site.data.keyword.cloud_notm}} accounts and generates a report of resources that reference HPCS keys. It helps identify services and resources that are using HPCS keys and might require migration.

The report groups resources by service and includes the CRNs of both the encrypted resources and the associated keys. You can use this information to:
- Identify candidate services for key migration.
- Cross-check Associations and activity tracking data.
- Support migration planning and validation.

KUR is also capable of processing activity tracking audit log files, producing CSV summaries that help identify HPCS utilization patterns.

#### Important considerations and limitations
{: #migration-considerations}

- The migration tooling is provided on a best-effort basis and might not detect every possible usage pattern.
- Not all {{site.data.keyword.cloud_notm}} services currently support migration intent.
- Some services or specific parts of services (for example, IKS and ROKS persistent volume claims) require specific procedures and are not fully covered by migration intents. See the next sections for more information.
- You are responsible for validating that all HPCS usage stopped before you decommission HPCS.

#### Example migration scenario
{: #migration-scenario-example}

The following end-to-end example illustrates how to migrate an HPCS CRK that is used by a Cloud Object Storage instance.

**Starting point:**

- An HPCS CRK (`HPCS_key_1`) protects a DEK used by a Cloud Object Storage instance (`COS_1`).
- The objective is for `COS_1` to use a {{site.data.keyword.keymanagementserviceshort}} Dedicated CRK (`KP_D_key_1`) instead, without moving any data.

**Step 1: Identify HPCS CRK usage**

Use the [Key Usage Reporter (KUR)](/docs/key-protect?topic=key-protect-kur) tool to scan your accounts and identify which services and resources are using `HPCS_key_1`. Cross-reference the KUR report with [activity tracking events](/docs/hs-crypto?topic=hs-crypto-at-events) to confirm usage.

**Step 2: Create the target CRK in {{site.data.keyword.keymanagementserviceshort}} Dedicated**

Create `KP_D_key_1` in your {{site.data.keyword.keymanagementserviceshort}} Dedicated instance. The target CRK can be generated or imported, with or without customer-supplied key material, through the API, CLI, or the UI.

**Step 3: Set up IAM authorization policies**

Create service-to-service IAM authorization policies that allow Cloud Object Storage to access the {{site.data.keyword.keymanagementserviceshort}} Dedicated instance, key ring, or individual key where `KP_D_key_1` resides. The IAM policies must be defined in the same account as the target {{site.data.keyword.keymanagementserviceshort}} Dedicated instance. For services such as IBM Cloud Databases, Messages for RabbitMQ, Kubernetes, and OpenShift, ensure that delegated authorization is enabled when you create the IAM policy.

Most cases of failed migrations occur because IAM authorization policies are not configured or are configured incorrectly.
{: important}

Before you proceed, use the CRKM tool `authz-check` command to verify that the required IAM authorization policies are in place. The `authz-check` command inspects the association on each source HPCS CRK and checks whether a matching IAM authorization policy exists that would allow each registered service to access the target {{site.data.keyword.keymanagementserviceshort}} Dedicated CRK. For each association, the tool reports whether a matching policy was found or whether a policy is missing, along with a template of the policy that needs to be created. Running this check before you create migration intents helps you identify and fix authorization gaps that would otherwise cause migration failures. For more information, see [Key Migration Tool (CRKM)](/docs/key-protect?topic=key-protect-migrate-tool).

**Step 4: Create the migration intent**

Use the [Key Migration Tool (CRKM)](/docs/key-protect?topic=key-protect-migrate-tool) to create a migration intent on `HPCS_key_1` that references the target CRK `KP_D_key_1`. The CRKM tool accepts a CSV file that contains pairs of source HPCS CRK CRNs and target {{site.data.keyword.keymanagementserviceshort}} Dedicated CRK CRNs, which makes it possible to create migration intents in bulk.

After the migration intent is created, HPCS emits synchronization events that notify associated services about the migration request.

**Step 5: Run sync**

For some services (for example, IBM Cloud Databases, Messages for RabbitMQ, Kubernetes, and OpenShift), more synchronization events must be explicitly triggered a few minutes after intent creation. Use the CRKM tool sync command to trigger these events.

You can run the sync command at any time to retry incomplete migrations.

**Step 6: Monitor migration progress**

Use the CRKM tool Status command to check the migration progress. The tool reports the association counts for both the source HPCS CRK and the target {{site.data.keyword.keymanagementserviceshort}} Dedicated CRK. As services complete migration:

- The number of associations on `HPCS_key_1` decreases.
- The number of associations on `KP_D_key_1` increases.

For Event Streams, migration might take up to one business day. For other services, migration is expected to finish in less than four hours.

**About the Key Migration Tool (CRKM)**

The [Key Migration Tool (CRKM)](/docs/key-protect?topic=key-protect-migrate-tool) is a CLI tool that supports the following operations:

- **Status**: Reports migration progress by showing association counts for source and target CRKs across all CRK pairs.
- **Authz-check**: Verifies that the required IAM authorization policies are in place for each registered service before migration. Reports match and missing policies with actionable templates.
- **Create**: Creates migration intents in bulk from a CSV file of source and target CRK CRN pairs.
- **Sync**: Triggers synchronization events to prompt services to process the migration intent. Can be run multiple times to retry incomplete migrations.

The CRKM tool is required for automated CRK migration and works with the KUR tool, which handles discovery and reporting.

## Standard Key Migration
{: #migration-standard-key-migration}

[Standard keys](/docs/hs-crypto?topic=hs-crypto-envelope-encryption#key-types) in HPCS store secret material such as API keys, passwords, or encryption keys that are used directly by applications. Unlike CRKs, standard keys do not use the migration intent workflow. Migration of standard keys requires you to retrieve the key material from HPCS and reprovision it in a supported service.

### Checking for existence of Standard Keys
{: #migration-check-standard-key-existence}

Use the following bash script to count the total number of Standard Keys in all valid standard key [states](/docs/hs-crypto?topic=hs-crypto-key-states), in each HPCS instance.

Make sure that you are logged in to {{site.data.keyword.cloud_notm}} through the {{site.data.keyword.cloud_notm}} CLI.
{: important}

```sh
# count the total number of Standard keys in all states 
HPCS_ADDR=https://fadedbee-0000-0000-0000-1234567890ab.api.us-south.hs-crypto.appdomain.cloud
HPCS_INSTANCE_ID=fadedbee-0000-0000-0000-1234567890ab
AUTH_HEADER="${AUTH_TOKEN:-$(jq -r .IAMToken ~/.bluemix/config.json)}"
header="$(curl -i -k -s --head \
  "${HPCS_ADDR}/api/v2/keys?state=1,5&extractable=true" \
  -H "authorization: ${AUTH_HEADER}" \
  -H "bluemix-instance: ${HPCS_INSTANCE_ID}" \
  -H "prefer: return=representation" \
| grep '^Key-Total:' \
| tr -d '\r')"
if [ -n "$header" ]; then
  total="${header#Key-Total: }"
  echo "Total number of Standard keys in all states: $total"
else
  echo "Error: Key-Total header not found. Try logging into IBM Cloud again. Check endpoint, auth token, or permissions." >&2
fi
```
{: pre}

Replace `HPCS_ADDR` and `HPCS_INSTANCE_ID` with valid values for each HPCS instance.
You can find the instance endpoint for HPCS and the instance ID in the {{site.data.keyword.cloud_notm}} UI console for the specific instance.


The output looks similar to the following example:

```sh
Total number of Standard keys in all states: 4
```
{: screen}

If the output is an empty line, log in to {{site.data.keyword.cloud_notm}} through the {{site.data.keyword.cloud_notm}} CLI again.
{: tip}


If zero Standard Keys exist in all the HPCS instances, Standard Key migration is not required.
{: tip}

Check the count of standard keys in the Destroyed (5) state by using the following script.

```sh
# count the total number of Standard keys in Destroyed (5) state.
HPCS_ADDR=https://fadedbee-0000-0000-0000-1234567890ab.api.us-south.hs-crypto.appdomain.cloud
HPCS_INSTANCE_ID=fadedbee-0000-0000-0000-1234567890ab
AUTH_HEADER="${AUTH_TOKEN:-$(jq -r .IAMToken ~/.bluemix/config.json)}"
header="$(curl -i -k -s --head \
  "${HPCS_ADDR}/api/v2/keys?state=5&extractable=true" \
  -H "authorization: ${AUTH_HEADER}" \
  -H "bluemix-instance: ${HPCS_INSTANCE_ID}" \
  -H "prefer: return=representation" \
| grep '^Key-Total:' \
| tr -d '\r')"
if [ -n "$header" ]; then
  total="${header#Key-Total: }"
  echo "Total number of Standard keys in Destroyed (5) state: $total"
else
  echo "Error: Key-Total header not found. Try logging into IBM Cloud again. Check endpoint, auth token, or permissions." >&2
fi
```
{: pre}

If all standard keys are in the Destroyed (5) (soft deleted) state, usage is not guaranteed to be absent. An {{site.data.keyword.cloud_notm}} resource or custom application might still reference the key. In this case, operations are expected to fail the next time key material retrieval is attempted.

Standard keys can exist only in Active (1) or Destroyed (5) states. Other key states apply only to CRKs. 

You can obtain the full CRN of HPCS standard keys by using the {{site.data.keyword.cloud_notm}} CLI [`kp keys` command](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-keys).

The following example lists standard keys in possible states:

```sh
export KP_TARGET_ADDR=https://fadedbee-0000-0000-0000-1234567890ab.api.us-south.hs-crypto.appdomain.cloud
ibmcloud kp keys --instance-id fadedbee-0000-0000-0000-1234567890ab --crn --key-type standard-key --key-states active,destroyed --number-of-keys 5000
```
{: pre}

Replace `KP_TARGET_ADDR` with valid values for each HPCS instance.
You can find the instance endpoint for HPCS in the {{site.data.keyword.cloud_notm}} UI console for the specific instance.

- The command can list up to 5000 standard keys at a time. Pagination might be needed to list all standard keys.

####  Checking HPCS Usage on AIX
{: #migration-aix}

If you have AIX systems (on IBM Cloud or on-premises) and Standard Keys are present in HPCS, run the following commands on the AIX host to verify whether those keys are in use. The `TYPE` field in the output of the `hdcryptmgr` commands indicates the HPCS authentication method.

```
keysvrmgr show -t hpcs
hdcryptmgr showlv <lvname> -v
hdcryptmgr showpv <pvname> -v
```
{: pre}

If no logical volumes or physical volumes report `TYPE=hpcs`, the AIX system is not actively using HPCS Standard Keys.

Starting with AIX® 7.3 Technology Level (TL) 4 Service Pack (SP) 1, the {{site.data.keyword.keymanagementserviceshort}} Dedicated Server authentication method replaces the deprecated Hyper Protect Crypto Services (HPCS) authentication method. The -t option of the hdcryptmgr command is updated to include the kms as the valid value in place of hpcs value. The value of the hpcs option is accepted even for Key Protect. To migrate from HPCS to Key Protect Server, see Migrating from HPCS to Key Protect Server section in the Encrypted logical volumes topic.

####  Checking HPCS Standard key usage by Direct Link
{: #migration-direct-link}

Refer to the Direct Link documentation at [Migrating Direct Link MACsec CAKs and MD5 keys from HPCS to Secrets Manager](/docs/dl?topic=dl-hpcs-migration&interface=ui)

#### Migration of standard keys
{: #migration-standard-key-process}

If standard keys exist, besides the Direct Link and AIX usages previously mentioned, you must migrate them by following these steps:

1. **Retrieve the key material**: Use the [HPCS API](/apidocs/hs-crypto#getkey) to retrieve the plaintext key material of each standard key.

2. **Reprovision the key material**: Store the retrieved key material in a supported service. You can do this by:
   - Importing the key material into a new {{site.data.keyword.keymanagementserviceshort}} Dedicated standard key.
   - Storing the secret in {{site.data.keyword.cloud_notm}} Secrets Manager, which is the recommended solution for general secret material.

3. **Update application references**: Update any custom applications, service configurations, or IAM policies that reference the HPCS standard key. Applications must be updated with the new service endpoint, key ID, and any required IAM policies that grant access to the new key in {{site.data.keyword.keymanagementserviceshort}} Dedicated or Secrets Manager.

4. **Validate**: Confirm that all applications and services are functioning correctly with the new key before you decommission the HPCS standard key.

   
## KMIP for VMWare Migration
{: #kmip-migration}

VMware KMIP support for HPCS ends on 31 December 2026, after which the KMIP for VMware service no longer works. Detailed instructions on migrating to {{site.data.keyword.keymanagementserviceshort}} Dedicated are published [here](/docs/vmwaresolutions?topic=vmwaresolutions-eos-kmip).

## PKCS #11 (GREP11)
{: #migration-pkcs11-grep11}

[Enterprise PKCS #11 keys](/docs/hs-crypto?topic=hs-crypto-pkcs11-intro) that are used through PKCS #11 or [GREP11](/docs/hs-crypto?topic=hs-crypto-uko-grep11-intro) interfaces are not supported by {{site.data.keyword.keymanagementserviceshort}} Dedicated.

To determine whether this feature is being used, check the HPCS activity tracking logs for entries where the action field is `hs-crypto.ep11.use` or starts with `hs-crypto.keystore`. The presence of these entries indicates that PKCS #11 (GREP11) is being used.

Refer to the [GREP11/PKCS#11 Migration Guide](/docs/hs-crypto?topic=hs-crypto-migrate-hpcs-to-CCRT).

## Unified Key Orchestrator (UKO)
{: #migration-uko}

[UKO-managed keys](/docs/hs-crypto?topic=hs-crypto-introduce-uko) are not supported by {{site.data.keyword.keymanagementserviceshort}} Dedicated.

Refer to the [UKO Migration Guide](/docs/hs-crypto?topic=hs-crypto-migration-guide).

## Terraform
{: #migration-terraform}

To use Terraform with {{site.data.keyword.keymanagementserviceshort}} Dedicated, the environment variable `IBMCLOUD_KP_API_ENDPOINT` must be set to the public or private API endpoint of the specific {{site.data.keyword.keymanagementserviceshort}} Dedicated instance.

Provisioning a new {{site.data.keyword.keymanagementserviceshort}} Dedicated instance is available through the IBM Cloud Console UI and the IBM Cloud CLI.
Creating new {{site.data.keyword.keymanagementserviceshort}} Dedicated instances with Terraform is not supported.

For more information, see [Setting up Terraform for Key Protect](/docs/key-protect?topic=key-protect-terraform-setup#install-terraform)

## Instance provisioning by using the IBM Cloud CLI
{: #migration-cli}

The process for provisioning {{site.data.keyword.hscrypto}} instances differs from the process for provisioning {{site.data.keyword.keymanagementserviceshort}} Dedicated instances by using the IBM Cloud CLI.

See [instructions](/docs/key-protect?topic=key-protect-st-init-cli) for provisioning {{site.data.keyword.keymanagementserviceshort}} Dedicated instances by using the IBM Cloud CLI.

## Secure import of root key material
{: #migration-secure-import}

[Secure import of root key material](/docs/hs-crypto?topic=hs-crypto-importing-keys#using-import-tokens) is not supported by {{site.data.keyword.keymanagementserviceshort}} Dedicated.

To determine whether this feature is being used, check the HPCS activity tracking logs for entries where the action field is `hs-crypto.import-token.create` or `hs-crypto.import-token.read`. The presence of these entries indicates that secure import of root key material is being used.

{{site.data.keyword.keymanagementserviceshort}} Dedicated supports regular import of root key material, where the key material is encrypted in transit by using HTTPS.

## Post migration
{: #migration-post-migration}

After you complete the migration to {{site.data.keyword.keymanagementserviceshort}} Dedicated, you must validate that HPCS instances are no longer in active use and take controlled steps to reduce risk before the HPCS end-of-service date.

### Validate that HPCS is no longer in use
{: #migration-validate-no-usage}

After migration, inspect HPCS activity tracking events to confirm that no operations are performed against HPCS instances.

Review events over the largest available retention window.

If activity tracking events indicate continued usage:

1. Identify the service or workload that is responsible for the usage.
2. Verify whether the resource supports CRK migration by using migration intent.
3. Complete or retry migration for that usage before you proceed.

Lack of activity tracking events does not conclusively prove the absence of usage. Some services and custom apps use keys infrequently or only during lifecycle events such as restart, restore, or failover.
{: tip}

### Gradually disable migrated HPCS CRKs
{: #migration-disable-keys}

After you are confident that specific HPCS CRKs are no longer required, you can disable those CRKs.

Disabling CRKs is recommended before deletion because:
- Any remaining cryptographic operations fail immediately with a clear error.
- Disabled keys can be reenabled quickly if unexpected dependencies are discovered.
- A safe rollback mechanism during validation is provided.

A recommended milestone is to ensure that all HPCS CRKs that were successfully migrated are in the Disabled state.

CRKs in the Disabled state can be re-enabled at any time and do not permanently block remediation.

### Final milestones and decommissioning considerations
{: #migration-decommissioning}

Deleting HPCS CRKs and Standard Keys is technically possible. However, approach deletion with caution:

- Deleted keys can be recovered only for a limited period after deletion.
- After the recovery window expires, the deletion is permanent.
- Recovery becomes increasingly difficult as time passes and workloads evolve.

For these reasons, you are not required to delete HPCS keys as part of migration.

A conservative and recommended approach is:
1. Leave HPCS instances and CRKs disabled.
2. Do not reenable or modify them after validation.

This approach minimizes risk while ensuring that cryptographic migration completes successfully.

### Your responsibilities
{: #migration-customer-responsibility}

You are responsible for:
- Verifying that all HPCS usage stopped
- Validating application and service behavior after migration

Proceed with decommissioning activities only when you are confident that HPCS is no longer required by any workload.
