---

copyright:
  years: 2026

lastupdated: "2026-03-20"

keywords: HPCS migration, Key Protect Dedicated migration, CRK migration, customer root key migration, migration tool, HPCS to Key Protect

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Migrating from HPCS to Key Protect Dedicated with the Key Migration Tool
{: #migrate-tool}

The Key Migration Tool (CRKM) helps HPCS users migrate Customer Root Keys (CRKs) to {{site.data.keyword.keymanagementserviceshort}} Dedicated. This document describes how to use CRKM to migrate your keys and associated {{site.data.keyword.cloud_notm}} resources. CRKM applies only to CRKs that are used by used by {{site.data.keyword.cloud_notm}} services and IBM software that participate in the automated migration workflow. See [Migrating from Hyper Protect Crypto Services (HPCS) to {{site.data.keyword.keymanagementserviceshort}} Dedicated](/docs/key-protect?topic=key-protect-migrate-st) for detailed information.
{: shortdesc}

Further mentions of "key" without additional context refer to a customer root key (CRK).

## Downloading the tool
{: #migrate-tool-download}

1. Create an IBM Support ticket for Key Protect to request access to the HPCS to Key Protect migration tooling.

2. Download the tool binary provided in the support ticket.

3. Verify the SHA-256 checksum of the downloaded binary matches the value provided in the support ticket. Compare the values directly; they must match exactly.

Run the appropriate command for your operating system to get the SHA-256 checksum and compare with the value provided in the support ticket:

### macOS

```sh
shasum -a 256 <crkm-binary>
```

Example:

```sh
shasum -a 256 crkm-darwin-arm64-1.0.0
```

### Linux

```sh
sha256sum <crkm-binary>
```

Example:

```sh
sha256sum crkm-linux-amd64-1.0.0
```

### Windows (Command Prompt)

```cmd
certutil -hashfile <crkm-binary> SHA256
```

Example:

```cmd
certutil -hashfile crkm-windows-amd64-1.0.0.exe SHA256
```

### Windows (PowerShell)

```powershell
Get-FileHash <crkm-binary> -Algorithm SHA256
```

Example:

```powershell
Get-FileHash crkm-windows-amd64-1.0.0.exe -Algorithm SHA256
```
{: note}


Make the binary executable (macOS/Linux):

```sh
chmod +x <crkm-binary>
```
{: pre}


## Understanding the migration tool output
{: #migrate-tool-output}

The `status`, `create`, and `sync` operations from the CLI tool produce the following two files:

* A `.csv` output file is created after the operation completes, with `execution` in the file name, the operation name, and the date-time of command run. The file contains a copy of the input data, and in addition for each row:
    * The number of registrations found on the HPCS key
    * The number of registrations found on the Key Protect Dedicated key
    * The status of the operation on the HPCS key
* A `.log` output file is created after the operation completes, with `summary` in the file name, the operation name, and the date-time of command run. The file provides an overview of the status of keys with migration intents and their number of registrations.

In addition:
* Using the `status` operation is a good way to verify your input file is correctly formatted as no action is taken.
* The same input file can be used repeatedly. It is not necessary to change the CSV to remove keys that have been fully migrated, as the status indicates why the operation was not performed.

## Before you begin
{: #migrate-tool-prereqs}

Complete all migration prerequisites before using the migration tool.

### Setting up IAM authorization policies
{: #migrate-tool-iam}

Configure IAM authorization policies from services to Key Protect Dedicated as documented. Specifically, the "Enable authorizations to be delegated by source and dependent services" checkbox must be selected when creating a policy to Hybrid HPCS if the documentation requires it. See documentation examples for:
* ICD PostgreSQL: [Granting service authorization](/docs/databases-for-postgresql?topic=databases-for-postgresql-key-protect&interface=ui#granting-service-auth)
* IKS: [Encryption](/docs/containers?topic=containers-encryption)

### Creating target keys
{: #migrate-tool-create-keys}

Create keys in Key Protect Dedicated that will be the target of the migration.

## Setting up the input file
{: #migrate-tool-input}

The CLI takes as input a `.csv` file without a header row, where:

* Each row has the CRN of the `sourceCRK` in the first column. This is the source HPCS key.
* Each row has the CRN of the `targetCRK` in the second column. This is the target Key Protect Dedicated key.
* Each row represents the intent to have any cloud resource that is encrypted by `sourceCRK` to be, after the migration is completed, encrypted by the `targetCRK`.

### Example input file
{: #migrate-tool-input-example}

(The following table shows the structure. Do not include headers in your CSV file.)

| Source HPCS Key CRN | Target Key Protect Dedicated Key CRN |
| --- | --- |
| `crn:v1:bluemix:public:hs-crypto:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab` | `crn:v1:bluemix:public:kms:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab` |
| `crn:v1:bluemix:public:hs-crypto:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab` | `crn:v1:bluemix:public:kms:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab` |
| `crn:v1:bluemix:public:hs-crypto:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab` | `crn:v1:bluemix:public:kms:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab` |
{: caption="Table 1. Example input file format" caption-side="bottom"}

There is no header row in the actual CSV file.
{: note}


## Configuring the migration tool
{: #migrate-tool-setup}

Run the following commands in your terminal:

```sh
export HPCS_API_ENDPOINT=<HPCS endpoint>
export KP_ST_API_ENDPOINT=<Key Protect Dedicated endpoint>
export IBMCLOUD_API_KEY=<IBM Cloud API key>
export IBMCLOUD_API_KEY_KP_ST=<IBM Cloud API key that can access Key Protect Dedicated instance>
```
{: pre}

Example for `HPCS_API_ENDPOINT` and `KP_ST_API_ENDPOINT`:

```sh
export HPCS_API_ENDPOINT=https://fadedbee-0000-0000-0000-1234567890ab.api.us-south.hs-crypto.appdomain.cloud
export KP_ST_API_ENDPOINT=https://fadedbee-0000-0000-0000-1234567890ab.api.us-south.kms.appdomain.cloud
```
{: pre}

The `IBMCLOUD_API_KEY` must be associated with an IAM identity (for example, an IBM Cloud user or service ID) that has the `Manager` role for both HPCS and Key Protect Dedicated.

The `IBMCLOUD_API_KEY_KP_ST` variable is required only if `IBMCLOUD_API_KEY` does not have access to the target Key Protect Dedicated instance. 

This typically occurs when:
- The HPCS instance and Key Protect Dedicated instance are in different IBM Cloud accounts
- Separate IAM identities are used for source and target environments

### Optional environment variables
{: #migrate-tool-optional-vars}

The following optional environment variables can be configured:

`IAM_API_ENDPOINT`
:   Takes a custom IAM endpoint if the default `https://iam.cloud.ibm.com/identity/token` should not be used.

`DEBUG_MODE`
:   Takes `true` or `false`. When set to `true`, the tool produces detailed logs.

## Validating the input file
{: #migrate-tool-validate}

Use the `status` operation to check the status of the migration and verify your input file is correctly formatted. No migration action is taken with this command.

```sh
./<crkm-binary> status <.csv input file>
```
{: pre}

You may see "FAILED - No Migration Intent found for the key" in the status columns for all key inputs in the output `.csv` file and in the console log. This is normal as no migration intent has been created yet.

## Checking IAM authorization policies
{: #migrate-tool-authz-check}

Use the `authz-check` command to verify that IAM authorization policies are in place so that cloud services currently using HPCS source keys can use the corresponding Key Protect Dedicated target keys after migration.

```sh
./<crkm-binary> authz-check <.csv input file>
```
{: pre}

The command works in two phases:

### Phase 1: Fetch policies
{: #migrate-tool-authz-phase1}

The tool extracts the distinct account IDs from the TargetCRK column in the CSV, fetches all IAM authorization policies for each account, and writes them to JSON files (`policies-{serviceName}-{accountID}.json`) for reference.

### Phase 2: Match registrations
{: #migrate-tool-authz-phase2}

For each row in the CSV, the tool fetches all registrations on the source HPCS key (the cloud resources encrypted by that key) and checks whether an IAM authorization policy exists that would allow each registered service to use the target Key Protect Dedicated key.

For each registration, the tool reports either:

`MATCH`
:   A valid authorization policy was found. The policy JSON is printed.

`NO MATCH`
:   No authorization policy was found. The tool prints a message explaining what is missing and a colored JSON template of a policy that would satisfy the match. In the template, required fields are shown in green and optional fields in blue.

A summary is printed at the end with the total number of checked, matched, and unmatched registrations.

### Authorization check flags
{: #migrate-tool-authz-flags}

`--match-all-authz-policies`
:   By default, the tool stops at the first matching policy per registration. Use this flag to log all matching policies.

### Matching criteria
{: #migrate-tool-authz-criteria}

An authorization policy matches a registration when all of the following are true:

1. The policy's **subjects** (who is authorized) contain a `serviceName` and `accountId` matching the registered cloud service.
2. The policy's **resources** (what is being accessed) contain a `serviceName` and `accountId` matching the target Key Protect Dedicated key's service.
3. The policy's **roles** grant appropriate access (`Reader`, `ReaderPlus`, `Writer`, or `Manager`).
4. If the registered service requires it (for example, `databases-for-*`, `messages-for-rabbitmq`, `containers-kubernetes`), the policy also includes the `AuthorizationDelegator` role.

If the policy specifies optional attributes such as `serviceInstance`, `keyRing`, or `resource`, those must also match the target key.

## Creating migration intents
{: #migrate-tool-create}

After creating a `.csv` file with the pairing of HPCS keys to migrate to their respective Key Protect Dedicated keys, use the `create` operation to create migration intents on the HPCS keys.

```sh
./<crkm-binary> create <.csv input file>
```
{: pre}

This operation:
* Triggers sync events to services that have registrations (resources associated with the keys being migrated) so they know to start migrating the HPCS key.
* Allows you to monitor event acknowledgement and migration in the activity tracking logs. See [Migration not completed in time](#migrate-tool-troubleshoot-time) for troubleshooting using activity tracking.

If a migration intent already exists on a key and you want to update it with an updated `.csv` input file, run the CLI with the `--replace-mi` flag.

See [Troubleshooting](#migrate-tool-troubleshoot) if there are issues observed in the creation of migration intents.

## Syncing keys
{: #migrate-tool-sync}

After five minutes from the completion of the previous step, use the `sync` operation:

```sh
./<crkm-binary> sync <.csv input file>
```
{: pre}

This operation triggers sync events to services that have registrations (resources associated with the keys being migrated). This step ensures that even for services that use HPCS keys indirectly (for example, ICD uses COS to store backups, COS uses Key Protect) the migration can take place.

Wait at least five minutes before checking on the status of the migration. The migration is performed by HPCS adopting services, which have up to four hours to respond to HPCS.
For Event Streams, after the creation of a migration intent, the migration might take up to one business day.

## Checking migration status
{: #migrate-tool-status}

Use the `status` operation to check on the status of the migration:

```sh
./<crkm-binary> status <.csv input file>
```
{: pre}

If the number of registrations of an HPCS key was not zero at the start of the migration and it drops to zero at some point, that indicates that migration has been completed.

See [Troubleshooting](#migrate-tool-troubleshoot) if there are issues observed in the status column or the number of HPCS registrations (`KpRegistrationsCount`) is still non-zero.

## Troubleshooting
{: #migrate-tool-troubleshoot}

Use the following information to troubleshoot common issues with the migration tool.

### Migration not completed in time
{: #migrate-tool-troubleshoot-time}

Sometimes services may encounter problems migrating keys. 

Use the `sync` operation with the `.csv` file as input to notify services with resources associated with the key to try the migration again:

```sh
./<crkm-binary> sync <.csv input file>
```
{: pre}

If the number of registrations of an HPCS key was not zero at the start of the migration and it does not drops to zero at some point, that might indicate services are encountering problems migrating keys or there are state registrations.
Create an IBM Support ticket for Key Protect, mention HPCS to Key Protect migration.


### Invalid resource CRN format
{: #migrate-tool-troubleshoot-crn}

Error message:

```text
invalid key crn for key : crn must be specified to 10 segments
```
{: screen}

The CSV status column output also reads: "FAILED - crn must be specified to 10 segments"

Review the input `.csv` file and ensure the CRN for the HPCS keys and Key Protect Dedicated keys are formatted correctly.

### Key not found
{: #migrate-tool-troubleshoot-notfound}

Error message:

```text
2023/03/29 09:23:39 Sleeping for 100ms before processing row 5
2023/03/29 09:23:39 Retrieving Key...
2023/03/29 09:23:39 Get Key failed for keyID d4c4cbee-20ed-4deb-b59d-4674f70713b5 with the following error: kp.Error: correlation_id='643a49af-a3cc-4eba-b23b-368bbc33115d', msg='Not Found: Key could not be retrieved: Please see `reasons` for more details (KEY_NOT_FOUND_ERR)', reasons='[KEY_NOT_FOUND_ERR: key does not exist - FOR_MORE_INFO_REFER: https://cloud.ibm.com/apidocs/key-protect]'
2023/03/29 09:23:39 -----------------------------------------------
```
{: screen}

The CSV status column output also reads:

```text
FAILED - kp.Error: correlation_id='643a49af-a3cc-4eba-b23b-368bbc33115d', msg='Not Found: Key could not be retrieved: Please see `reasons` for more details (KEY_NOT_FOUND_ERR)', reasons='[KEY_NOT_FOUND_ERR: key does not exist - FOR_MORE_INFO_REFER: https://cloud.ibm.com/apidocs/key-protect]'
```
{: screen}

This means the CRN for the key does not exist. Review the input `.csv` file and ensure the HPCS keys to migrate exist and the Key Protect Dedicated keys to migrate to exist.

### Instance does not exist or user is unauthorized
{: #migrate-tool-troubleshoot-unauthorized}

Error message:

```text
2023/03/29 09:26:44 Retrieving Key...
2023/03/29 09:26:44 Get Key failed for keyID d4c4cbee-20ed-4deb-b59d-4674f70713b5 with the following error: kp.Error: correlation_id='4629f1af-1a9b-44ea-9139-3c5c39b9790e', msg='Unauthorized: Either the user does not have access to the specified resource, the resource does not exist, or the region is incorrectly set'
2023/03/29 09:26:44 -----------------------------------------------
```
{: screen}

The CSV status column output also reads:

```text
FAILED - kp.Error: correlation_id='4629f1af-1a9b-44ea-9139-3c5c39b9790e', msg='Unauthorized: Either the user does not have access to the specified resource, the resource does not exist, or the region is incorrectly set'
```
{: screen}

This means the user does not have access to the key, or the instance in the CRN of the key does not exist.

### Create command: No registrations found
{: #migrate-tool-troubleshoot-create-noreg}

Error message:

```text
2023/03/22 10:39:36 Retrieving Key...
2023/03/22 10:39:36 Key retrieved successfully
2023/03/22 10:39:36 Retrieving Registrations...
2023/03/22 10:39:36 Registrations retrieved successfully
2023/03/22 10:39:36 Retrieving Migration Intent...
2023/03/22 10:39:36 Migration Intent retrieved successfully
2023/03/22 10:39:36 No registrations found for the key
2023/03/22 10:39:36 -----------------------------------------------
```
{: screen}

The CSV status column output also reads: "FAILED - No registrations were found for the key"

This means the HPCS key has no resources it is protecting or no resources it is associated with. Therefore, there is no reason to create a migration intent on the HPCS key and no reason to perform migration.

### Create command: Migration intent already exists
{: #migrate-tool-troubleshoot-create-exists}

Error message:

```text
2023/03/22 10:39:35 Retrieving Key...
2023/03/22 10:39:35 Key retrieved successfully
2023/03/22 10:39:35 Retrieving Registrations...
2023/03/22 10:39:35 Registrations retrieved successfully
2023/03/22 10:39:35 Retrieving Migration Intent...
2023/03/22 10:39:35 Migration Intent retrieved successfully
2023/03/22 10:39:35 Migration Intent already exists
2023/03/22 10:39:35 -----------------------------------------------
```
{: screen}

The CSV status column output also reads: "FAILED - Migration Intent already exists for the key"

This means the HPCS key already has an existing migration intent. If you want to update the migration intent, first update the input `.csv` with the new CRN of the Key Protect Dedicated key to migrate to, then add the `--replace-mi` flag to the create command and run the command again.

### Status command: No migration intent found
{: #migrate-tool-troubleshoot-status-nomi}

Error message:

```text
2023/03/22 13:15:56 Retrieving Key...
2023/03/22 13:15:56 Key retrieved successfully
2023/03/22 13:15:56 Retrieving Registrations...
2023/03/22 13:15:56 Registrations retrieved successfully
2023/03/22 13:15:56 Retrieving Migration Intent...
2023/03/22 13:15:56 No Migration Intent found for keyID a2cd689e-9a49-4cd7-ba0d-07d25a0b3a25
2023/03/22 13:15:56 -----------------------------------------------
```
{: screen}

The CSV status column output also reads: "FAILED - No Migration Intent found for the key"

This means a migration intent does not exist for that HPCS key. Retry [Creating migration intents](#migrate-tool-create), ensuring the key resource CRN is included in the input file used, and check on the status of the operation.

### Sync command: No registrations found
{: #migrate-tool-troubleshoot-sync-noreg}

Error message:

```text
2023/03/22 13:13:45 Retrieving Key...
2023/03/22 13:13:45 Key retrieved successfully
2023/03/22 13:13:45 Retrieving Registrations...
2023/03/22 13:13:45 Registrations retrieved successfully
2023/03/22 13:13:45 Retrieving Migration Intent...
2023/03/22 13:13:45 Migration Intent retrieved successfully
2023/03/22 13:13:45 No Registrations found for keyID d4c3cbee-20ed-4deb-b59d-4674f70713b5
2023/03/22 13:13:45 -----------------------------------------------
```
{: screen}

The CSV status column output also reads: "FAILED - No registrations were found for the key"

This means the HPCS key has no resources it is protecting or no resources it is associated with. Therefore, there is no reason to perform a syncing of resources. This also means the migration has been completed for the key.

### Sync command: No migration intent found
{: #migrate-tool-troubleshoot-sync-nomi}

Error message:

```text
2023/03/22 13:15:13 Retrieving Key...
2023/03/22 13:15:13 Key retrieved successfully
2023/03/22 13:15:13 Retrieving Registrations...
2023/03/22 13:15:13 Registrations retrieved successfully
2023/03/22 13:15:13 Retrieving Migration Intent...
2023/03/22 13:15:13 Migration Intent retrieved successfully
2023/03/22 13:15:13 No Migration Intent found for keyID a2cd689e-9a49-4cd7-ba0d-07d25a0b3a25
2023/03/22 13:15:13 -----------------------------------------------
```
{: screen}

The CSV status column output also reads: "FAILED - No Migration Intent found for the key"

This means a migration intent does not exist for that key. Retry [Creating migration intents](#migrate-tool-create), ensuring the key resource CRN is included in the input file used, and check on the status of the operation.

### Sync command: Request too early
{: #migrate-tool-troubleshoot-sync-early}

Error message:

```text
2023/03/29 09:17:06 Sleeping for 100ms before processing row 2
2023/03/29 09:17:06 Retrieving Key...
2023/03/29 09:17:06 Key retrieved successfully
2023/03/29 09:17:06 Retrieving Registrations...
2023/03/29 09:17:06 Registrations retrieved successfully
2023/03/29 09:17:06 Retrieving Migration Intent...
2023/03/29 09:17:06 Migration Intent retrieved successfully
2023/03/29 09:17:06 Post Sync Resources for key...
2023/03/29 09:17:06 Post Sync Call Failed: kp.Error: correlation_id='8f088c29-2f61-4dc4-b00f-c99f7edd00fe', msg='Conflict: Could not initiate sync requests to associated resources: Please see `reasons` for more details (REQ_TOO_EARLY_ERR)', reasons='[REQ_TOO_EARLY_ERR: The key was updated recently: Please wait and try again: Sync cannot be performed until at least 2023-03-29T14:17:19Z - FOR_MORE_INFO_REFER: https://cloud.ibm.com/apidocs/key-protect]'
2023/03/29 09:17:06 -----------------------------------------------
```
{: screen}

The CSV status column output also reads:

```text
FAILED - kp.Error: correlation_id='0cefcea8-7e29-4c4a-9721-00244eb9752c', msg='Conflict: Could not initiate sync requests to associated resources: Please see `reasons` for more details (REQ_TOO_EARLY_ERR)', reasons='[REQ_TOO_EARLY_ERR: The key was updated recently: Please wait and try again: Sync cannot be performed until at least 2023-03-29T14:17:19Z - FOR_MORE_INFO_REFER: https://cloud.ibm.com/apidocs/key-protect]'
```
{: screen}

This means the `sync` operation is being performed too quickly. Wait at least five minutes before trying again.
