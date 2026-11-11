---

copyright:
  years: 2024, 2026

lastupdated: "2026-11-11"

keywords: HPCS migration, Key Protect Dedicated migration, CRK migration, customer root key migration, migration tool, HPCS to Key Protect

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Migrating from HPCS to Key Protect Dedicated with the Key Migration Tool
{: #migrate-tool}

The Key Migration Tool (CRKM) helps HPCS users migrate Customer Root Keys (CRKs) to {{site.data.keyword.keymanagementserviceshort}} Dedicated. This document describes how to use CRKM to migrate your keys and associated {{site.data.keyword.cloud_notm}} resources. CRKM applies only to CRKs that are used by {{site.data.keyword.cloud_notm}} services and IBM software that participate in the automated migration workflow. See [Migrating from Hyper Protect Crypto Services (HPCS) to {{site.data.keyword.keymanagementserviceshort}} Dedicated](/docs/key-protect?topic=key-protect-migrate-st) for detailed information.
{: shortdesc}

In this topic, "key" refers to a customer root key (CRK) unless otherwise specified.
{: note}

## Downloading the tool
{: #migrate-tool-download}

1. Create an IBM Support ticket for {{site.data.keyword.keymanagementserviceshort}} to request access to the HPCS to {{site.data.keyword.keymanagementserviceshort}} migration tools.

2. Download the `hpcs-2-kp-crkm-tool-<version>.zip` file that is attached to the support ticket. It contains one binary for each supported platform:
    - `crkm-darwin-arm64-<version>` for macOS on Apple silicon
    - `crkm-linux-amd64-<version>` for Linux on x86-64
    - `crkm-windows-amd64-<version>.exe` for Windows on x86-64

3. Verify the SHA-256 checksum of the zip file before you extract it, and of the binary that you use after you extract it. The support ticket lists the expected value for each file. Compare the values directly; they must match exactly.

Run the appropriate command for your operating system to get the SHA-256 checksum and compare it with the value that is provided in the support ticket. The commands work for the zip file and for the binaries.

### macOS
{: #migrate-tool-download-macos}

```sh
shasum -a 256 <crkm-binary>
```
{: pre}

Example:

```sh
shasum -a 256 crkm-darwin-arm64-1.3.0
```
{: pre}

### Linux
{: #migrate-tool-download-linux}

```sh
sha256sum <crkm-binary>
```
{: pre}

Example:

```sh
sha256sum crkm-linux-amd64-1.3.0
```
{: pre}

### Windows (Command Prompt)
{: #migrate-tool-download-windows-cmd}

```cmd
certutil -hashfile <crkm-binary> SHA256
```
{: pre}

Example:

```cmd
certutil -hashfile crkm-windows-amd64-1.3.0.exe SHA256
```
{: pre}

### Windows (PowerShell)
{: #migrate-tool-download-windows-ps}

```powershell
Get-FileHash <crkm-binary> -Algorithm SHA256
```
{: pre}

Example:

```powershell
Get-FileHash crkm-windows-amd64-1.3.0.exe -Algorithm SHA256
```
{: pre}


Make the binary executable (macOS/Linux):

```sh
chmod +x <crkm-binary>
```
{: pre}

### Checking the version
{: #migrate-tool-version}

To see which version of the tool you have, run:

```sh
./<crkm-binary> version
```
{: pre}

The output shows the version and the build, for example `crkm 1.3.0 (commit 1a2b3c4)`. The `--version` and `-version` flags print the same.

## Commands
{: #migrate-tool-commands}

The tool has the following commands, listed in the order in which a migration uses them. Run a command with `--help` to see its usage, for example `./<crkm-binary> create --help`.

| Command | What it does | Input |
| --- | --- | --- |
| `status` | Reports the association counts, the key states, and the migration intent status of each key pair. Read-only; also validates the input file. | `.csv` file |
| `authz-check` | Checks that IAM authorization policies allow the services that use each HPCS key to use the target key. Read-only. | `.csv` file |
| `create` | Creates a migration intent on each HPCS key. | `.csv` file |
| `sync` | Triggers sync events to the services that use each HPCS key. | `.csv` file |
| `get` | Shows the migration intent of each HPCS key. Read-only. | `.csv` file or HPCS key CRN |
| `delete` | Deletes the migration intent of each HPCS key. | `.csv` file or HPCS key CRN |
| `reg-download` | Downloads all associations of each source and target key, to check them for stale associations. Read-only. | `.csv` file |
| `version` | Prints the version of the tool. | None |
{: caption="Table 1. Key Migration Tool commands" caption-side="bottom"}

## Understanding the migration tool output
{: #migrate-tool-output}

- The `status`, `create`, and `sync` operations from the CLI tool produce the following two files in the current directory:

    `.csv` output file
    :   Named `<input file name>_execution_<operation>_<date-time>.csv`, with the date and time in UTC (for example, `keys_execution_status_20261104T153000Z.csv`). The file has one row for each row of the input file, with the following columns:
        - `SourceCRK` and `TargetCRK`: the key pair from the input file
        - `HpcsRegistrationsCount`: the number of associations of the HPCS key
        - `KpStRegistrationsCount`: the number of associations of the {{site.data.keyword.keymanagementserviceshort}} Dedicated key
        - `Status`: `SUCCESS`, `SKIPPED - <reason>` (`sync` only: the key has no associations left to sync), or `FAILED - <reason>`
        - `SourceKeyState` and `TargetKeyState`: the state of the HPCS key and of the {{site.data.keyword.keymanagementserviceshort}} Dedicated key (for example, `Active (1)`)

    `.log` output file
    :   Named `<input file name>_summary_<operation>_<date-time>.log`. The file provides an overview of the status of keys with migration intents and their number of associations.

- The `get`, `delete`, and `reg-download` operations produce only the `execution` `.csv` file. For `get` and `delete`, `TargetCRK` shows the target key of the existing migration intent, and the association counts are `0`. The `authz-check` operation writes policy files instead (see [Phase 1: Fetch policies](#migrate-tool-authz-phase1)).

- Only your user can read the output files.

- A row that fails is recorded as `FAILED - <reason>`, and the tool continues with the next row. When the operation ends, its exit status shows whether every row succeeded, so that scripts can check the result:

    `0`
    :   Every row succeeded or was skipped. For `authz-check`, every association has a matching policy.

    `1`
    :   The operation did not run, and no row was processed: a required environment variable or argument is missing, or the input file, an API key, or an option is not valid.

    `2`
    :   The operation ran, but at least one row failed. For `authz-check`, at least one association is `NO MATCH`, or a row or association was skipped.

- The `status` operation is useful for verifying that your input file is correctly formatted, because this command takes no action. You can use the same input file repeatedly without removing keys that are fully migrated, as the status indicates why the operation was not performed.

If a key already has a migration intent whose target key differs from the second column of the input file, the `status`, `create`, `sync`, `get`, and `delete` operations print a warning:

```text
WARNING: target CRK mismatch for source CRK <HPCS key CRN>: the existing Migration Intent targets <target key CRN> but the CSV file specifies <target key CRN>.
```
{: screen}

To point the migration intent to the target key in the file, run the `create` operation with the `--replace-mi` flag.

For a migration to succeed, the source HPCS key must be in the `Active` or `Deactivated` state (so that it can unwrap data) and the target {{site.data.keyword.keymanagementserviceshort}} Dedicated key must be in the `Active` state (so that it can wrap and unwrap data). If a key is in any other state, the tool prints a warning to the console because the migration is likely to fail.
{: note}

## Before you begin
{: #migrate-tool-prereqs}

Complete all migration prerequisites to use the migration tool.

### Setting up IAM authorization policies
{: #migrate-tool-iam}

Configure IAM authorization policies from services to {{site.data.keyword.keymanagementserviceshort}} Dedicated as documented. When you create a policy to Hybrid HPCS, select the **Enable authorizations to be delegated by source and dependent services** checkbox, if the documentation requires it.

For examples, see:
- ICD PostgreSQL: [Granting service authorization](/docs/databases-for-postgresql?topic=databases-for-postgresql-key-protect&interface=ui#granting-service-auth)
- IKS: [Encryption](/docs/containers?topic=containers-encryption)

### Creating target keys
{: #migrate-tool-create-keys}

Create keys in {{site.data.keyword.keymanagementserviceshort}} Dedicated that are the target of the migration.

## Setting up the input file
{: #migrate-tool-input}

The CLI takes as input a `.csv` file without a header row. Each row in the file contains:

- **First column**: The CRN of the source HPCS key (`sourceCRK`)
- **Second column**: The CRN of the target {{site.data.keyword.keymanagementserviceshort}} Dedicated key (`targetCRK`)

Each row pairs an HPCS key with the {{site.data.keyword.keymanagementserviceshort}} Dedicated key that replaces it. The cloud resources that use the `sourceCRK` rewrap their data encryption keys with the `targetCRK`. The data itself is not re-encrypted.

### Example input file
{: #migrate-tool-input-example}

(The following table shows the structure. Do not include headers in your CSV file.)

| Source HPCS Key CRN | Target {{site.data.keyword.keymanagementserviceshort}} Dedicated Key CRN |
| --- | --- |
| `crn:v1:bluemix:public:hs-crypto:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab` | `crn:v1:bluemix:public:kms:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab` |
| `crn:v1:bluemix:public:hs-crypto:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab` | `crn:v1:bluemix:public:kms:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab` |
| `crn:v1:bluemix:public:hs-crypto:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab` | `crn:v1:bluemix:public:kms:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab` |
{: caption="Table 2. Example input file format" caption-side="bottom"}

Do not include a header row in the actual CSV file.
{: note}

The tool accepts files that are separated by commas, or by semicolons as some spreadsheet programs save them, and encoded in UTF-8 (with or without a byte order mark) or in UTF-16 with a byte order mark. Empty lines and spaces around a CRN are ignored. Columns after the second are ignored, but every row must have the same number of columns. If a row has fewer than two columns, or a value that does not start with `crn:` (for example, a header row), the tool stops before it processes any row.

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

The tool also accepts the earlier name `KP_ST_ENDPOINT` for `KP_ST_API_ENDPOINT`. If both are set, `KP_ST_API_ENDPOINT` is used.

For the `status`, `create`, `sync`, `get`, `delete`, and `reg-download` commands, ensure that the API keys are associated with the IAM identities that have the following service access roles:

IBMCLOUD_API_KEY (HPCS)
:   Requires the `Manager` service access role. Service access roles are additive, so `Manager` includes `Reader` and `Writer`, covering all six commands. The `Manager` role is required because the `delete` command and the `create` command with the `--replace-mi` option remove a migration intent from the source key. The `status`, `get`, and `reg-download` commands need only the `Reader` role.

IBMCLOUD_API_KEY_KP_ST ({{site.data.keyword.keymanagementserviceshort}} Dedicated)
:   Requires the `Reader` service access role. The `status`, `create`, `sync`, and `reg-download` commands only read the target key, so the `Reader` role is sufficient. The `get` and `delete` commands do not access {{site.data.keyword.keymanagementserviceshort}} Dedicated.

The `authz-check` command requires different service access roles than the other commands. For more information, see [Checking IAM authorization policies](#migrate-tool-authz-check).

The `IBMCLOUD_API_KEY_KP_ST` variable is required only if `IBMCLOUD_API_KEY` does not have access to the target {{site.data.keyword.keymanagementserviceshort}} Dedicated instance. It occurs when:
- The HPCS instance and {{site.data.keyword.keymanagementserviceshort}} Dedicated instance are in different {{site.data.keyword.cloud_notm}} accounts
- Separate IAM identities are used for source and target environments

### Optional environment variables
{: #migrate-tool-optional-vars}

The following optional environment variables can be configured:

`IAM_API_ENDPOINT`
:   Takes the full URL of a custom IAM token endpoint, if the default `https://iam.cloud.ibm.com/oidc/token` is not used.

`IAM_POLICY_API_ENDPOINT`
:   Used by `authz-check` only. Takes the base URL of the IAM Policy Management API. The default is the scheme and host of `IAM_API_ENDPOINT`, or `https://iam.cloud.ibm.com`.

`RESOURCE_CONTROLLER_API_ENDPOINT`
:   Used by `authz-check` only. Takes the base URL of the Resource Controller API. The default is `https://resource-controller.cloud.ibm.com`.

`DEBUG_MODE`
:   Takes `true` or `false`. When set to `true`, the tool logs the endpoints that it uses at startup, and for each migration intent, association, and sync request: the method, path with query string, response status, and correlation ID. For requests that fail, the response body is also logged. Authorization headers and API keys are never logged.

## Validating the input file
{: #migrate-tool-validate}

Use the `status` operation to check the status of the migration and verify that your input file is correctly formatted. No migration action is taken with this command.

```sh
./<crkm-binary> status <.csv input file>
```
{: pre}

You might see `FAILED - No Migration Intent found for the key` in the status columns for all key inputs in the output `.csv` file and in the console log. This is expected when no migration intent is created yet.

## Checking IAM authorization policies
{: #migrate-tool-authz-check}

Use the `authz-check` command to verify that IAM authorization policies are in place so that cloud services that currently use HPCS source keys can use the corresponding {{site.data.keyword.keymanagementserviceshort}} Dedicated target keys after migration.

```sh
./<crkm-binary> authz-check <.csv input file>
```
{: pre}

### Required access
{: #migrate-tool-authz-access}

The `authz-check` command only reads information. It does not create, update, or delete any keys or policies, so `Writer` or `Manager` access is not required.

The command requires the following access:

**HPCS instance**
:   `Reader` service access role to read the source key's associations.

**{{site.data.keyword.keymanagementserviceshort}} Dedicated instance**
:   `Reader` service access role to read the target key's metadata.

**IAM authorization policies**
:   Account-level permission to view the IAM authorization policies of every account that is referenced in the TargetCRK column. This is not a {{site.data.keyword.keymanagementserviceshort}} or HPCS service access role. It is account-level access to {{site.data.keyword.cloud_notm}} Identity and Access Management.

**Resource groups**
:   Needed only when a policy is scoped to a resource group. The identity associated with `IBMCLOUD_API_KEY` needs the `Viewer` platform role on the service instances of the associated resources, and the identity used for {{site.data.keyword.keymanagementserviceshort}} Dedicated needs it on the target {{site.data.keyword.keymanagementserviceshort}} Dedicated instance.

The identity used for the IAM policy lookup is the one associated with `IBMCLOUD_API_KEY_KP_ST`. If `IBMCLOUD_API_KEY_KP_ST` is not set, the identity associated with `IBMCLOUD_API_KEY` is used.

If the identity cannot view the policies of a target account, the tool cannot verify the associations of the keys in that account. It reports each of them as `NO MATCH (could not verify - …)`, even if a valid policy exists. See [Authorization check: No policies found](#migrate-tool-troubleshoot-authz-nopolicies).
{: important}

The command works in two phases:

### Phase 1: Fetch policies
{: #migrate-tool-authz-phase1}

The tool extracts the distinct account IDs from the TargetCRK column in the CSV, fetches the IAM authorization policies of each account whose resource is {{site.data.keyword.keymanagementserviceshort}} or HPCS, and writes them to JSON files (`policies-{serviceName}-{accountID}.json`) in the current directory for reference.

### Phase 2: Match associations
{: #migrate-tool-authz-phase2}

For each row in the CSV, the tool fetches all associations on the source HPCS key (the cloud resources encrypted by that key). The tool checks whether an IAM authorization policy exists that allows each associated service to use the target {{site.data.keyword.keymanagementserviceshort}} Dedicated key.

When a candidate policy is scoped to a resource group, the tool uses the {{site.data.keyword.cloud_notm}} resource controller API to look up the resource group of the relevant instance — the target {{site.data.keyword.keymanagementserviceshort}} Dedicated instance when the policy's resources are scoped by resourceGroupId, or the source instance when the policy's subjects are scoped by resourceGroupId. This allows the tool to match policies that are scoped to a resource group, such as a single policy that authorizes all services in a resource group to use {{site.data.keyword.keymanagementserviceshort}}, regardless of service type.

For each association, the tool reports either:

`MATCH`
:   A valid authorization policy was found. The policy JSON is printed.

`NO MATCH`
:   No authorization policy was found. The tool prints a message that explains what is missing and a JSON template of a policy that satisfies the match. When the output is a terminal, required fields are shown in green and optional fields in blue. `NO MATCH (could not verify - …; see warnings above)` means that the tool could not check the association, so a valid policy might exist. The reason is either `the authorization policies of account <account ID> could not be read`, when the identity cannot view the policies of the target account, or `resource-group lookup failed`, when the tool could not read a resource group that a candidate policy depends on.

A summary is printed at the end with the total number of checked, matched, and unmatched associations, how many of the unmatched associations could not be verified, and how many rows or associations were skipped because the tool could not read them (for example, a target key that does not exist).

### Authorization check flags
{: #migrate-tool-authz-flags}

`--match-all-authz-policies`
:   By default, the tool stops at the first matching policy per association. Use this flag to log all matching policies.

`--debug`
:   Logs the candidate policies of each association and why each one does or does not match. Setting `DEBUG_MODE=true` has the same effect.

### Matching criteria
{: #migrate-tool-authz-criteria}

An authorization policy matches an association when the following conditions are true:

1. The policy's **subjects** (who is authorized) contain an `accountId` that matches the associated cloud service, and are scoped by a `serviceName` that matches the service, a `resourceGroupId` that matches the resource group of the association's source instance, or both. A subject that is scoped by `accountId` alone does not match. If the subjects also specify a `serviceInstance`, it must match the instance of the associated resource.
2. The policy's **resources** (what is being accessed) contain a `serviceName` and `accountId` that match the target {{site.data.keyword.keymanagementserviceshort}} Dedicated key's service.
3. The policy's **roles** grant appropriate access (`Reader`, `ReaderPlus`, `Writer`, or `Manager`).
4. If the associated service requires it (for example, `databases-for-*`, `messages-for-rabbitmq`, `containers-kubernetes`), the policy also includes the `AuthorizationDelegator` role.

If the policy specifies optional attributes such as `resourceGroupId`, `serviceInstance`, `keyRing`, or `resource`, those attributes must also match the target key or its {{site.data.keyword.keymanagementserviceshort}} Dedicated instance.

## Creating migration intents
{: #migrate-tool-create}

After you create a `.csv` file with the pairing of HPCS keys to migrate to their respective {{site.data.keyword.keymanagementserviceshort}} Dedicated keys, use the `create` operation to create migration intents on the HPCS keys.

```sh
./<crkm-binary> create <.csv input file>
```
{: pre}

This operation performs the following actions:
- Triggers sync events to services that have associations (cloud resources that use the keys being migrated) so they can start migrating the HPCS key
- You must monitor event acknowledgment and migration in the activity tracking logs (see [Migration that is not completed in time](#migrate-tool-troubleshoot-time) for troubleshooting)

If an HPCS key has no associations, the tool logs `No registrations found for the key` and creates the migration intent anyway. No resource needs to be migrated for that key.

If a migration intent exists on a key and you want to update it with an updated `.csv` input file, run the CLI tool with the `--replace-mi` flag.

The tool pauses for 100 milliseconds before each row. If requests are rate limited, increase the pause with the `--sleep` flag, for example `--sleep 1s`. The `sync` operation takes the same flag.

For any issues that are observed in the creation of migration intents, see [troubleshooting](#migrate-tool-troubleshoot) section.

## Syncing keys
{: #migrate-tool-sync}

After five minutes from the completion of the previous step, use the `sync` operation:

```sh
./<crkm-binary> sync <.csv input file>
```
{: pre}

This operation triggers sync events to services that have associations (cloud resources that use the keys being migrated). This step ensures that the migration can occur even for services that use HPCS keys indirectly. For example, ICD uses {{site.data.keyword.cos_full_notm}} to store backups, and {{site.data.keyword.cos_full_notm}} uses {{site.data.keyword.keymanagementserviceshort}}.

Keys that have no associations or no migration intent are skipped, and the status column says why.

Wait at least five minutes and check the migration status. The HPCS-adopting services do the migration, which has up to four hours to respond to HPCS. For Event Streams, the migration might take up to one business day after you create a migration intent.


## Checking migration status
{: #migrate-tool-status}

Use the `status` operation to check on the status of the migration:

```sh
./<crkm-binary> status <.csv input file>
```
{: pre}

If the number of associations of an HPCS key (`HpcsRegistrationsCount`) was not zero at the start of the migration and drops to zero, the migration is complete. The count might stop above zero because of stale associations, which point to resources that were deleted. Stale associations are not migrated, and they do not need to be. To find out whether the remaining associations are stale, see [Downloading associations](#migrate-tool-reg-download).

If you observe issues in the status column or `HpcsRegistrationsCount` is still nonzero, see [troubleshooting](#migrate-tool-troubleshoot).

## Retrieving migration intents
{: #migrate-tool-get}

Use the `get` operation to retrieve the migration intent of one or more HPCS keys. For each key, the operation checks whether a migration intent exists. If one exists, the migration intent properties are returned (ID, source CRK, target CRK, creator, and creation date). If no migration intent exists, the row is recorded as `FAILED - No Migration Intent found for the key`. The `get` operation is read-only and takes no action on the keys.

The `status` operation reports the overall migration progress for each key. The `get` operation shows the details of the migration intent itself. Use the `get` operation to confirm which target key a migration intent points to before you run `sync`, or to check a single key without preparing a `.csv` file.
{: note}

Like `delete`, the `get` operation accepts either a `.csv` input file or a single HPCS key CRN.

To retrieve the migration intents for every HPCS key in a `.csv` file, use the following command:

```sh
./<crkm-binary> get <.csv input file>
```
{: pre}

To retrieve the migration intent for a single HPCS key by passing its full CRN, use the following command:

```sh
./<crkm-binary> get <HPCS key CRN>
```
{: pre}

Example:

```sh
./<crkm-binary> get crn:v1:bluemix:public:hs-crypto:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab
```
{: pre}

When you pass a `.csv` input file, the tool warns if the target key of an existing migration intent differs from the target key in the file (see [Understanding the migration tool output](#migrate-tool-output)). No warning is displayed when you pass a single CRN, because there is no target key to compare against.

The `get` operation acts only on the source HPCS key. Therefore, only the `HPCS_API_ENDPOINT` and `IBMCLOUD_API_KEY` environment variables are required. The {{site.data.keyword.keymanagementserviceshort}} Dedicated variables are not required. On the HPCS instance, the `Reader` service access role is sufficient.

## Deleting migration intents
{: #migrate-tool-delete}

Use the `delete` operation to remove the migration intent from one or more HPCS keys. For example, use this operation to undo a migration intent that was created with the wrong target key before migration begins. For each key, the operation checks whether a migration intent exists. If one exists, the migration intent properties are returned (ID, source CRK, target CRK, creator, and creation date) and the migration intent is deleted. If no migration intent exists, the row is recorded as `FAILED - No Migration Intent found for the key`.

Like the `get` operation, `delete` accepts either a `.csv` input file or a single HPCS key CRN. Other operations require a `.csv` file.

To delete the migration intents for every HPCS key in a `.csv` file, use the following command:

```sh
./<crkm-binary> delete <.csv input file>
```
{: pre}

To delete the migration intent for a single HPCS key by passing its full CRN, use the following command:

```sh
./<crkm-binary> delete <HPCS key CRN>
```
{: pre}

Example:

```sh
./<crkm-binary> delete crn:v1:bluemix:public:hs-crypto:eu-fr2:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:key:deadbeef-0000-0000-0000-1234567890ab
```
{: pre}

The `delete` operation acts only on the source HPCS key. Therefore, only the `HPCS_API_ENDPOINT` and `IBMCLOUD_API_KEY` environment variables are required. The {{site.data.keyword.keymanagementserviceshort}} Dedicated variables are not required.

The `delete` operation removes the migration intent immediately without prompting for confirmation. When you pass a `.csv` file, the migration intent of every key in the file is deleted.
{: important}

## Downloading associations
{: #migrate-tool-reg-download}

Use the `reg-download` operation to save all associations of the source and target keys to JSON files. The [Key Usage Reporter (KUR)](/docs/key-protect?topic=key-protect-kur) reads these files to find stale associations, which point to resources that no longer exist. The operation is read-only and takes no action on the keys.

```sh
./<crkm-binary> reg-download <.csv input file> --dir <folder>
```
{: pre}

For each key that has at least one association, the tool writes one file to the `--dir` folder (default: the current directory, created if it does not exist):

- `hpcs-<key ID>-registrations.json` for an HPCS key
- `kp-st-<key ID>-registrations.json` for a {{site.data.keyword.keymanagementserviceshort}} Dedicated key

Each file holds every association of the key, in the format of the {{site.data.keyword.keymanagementserviceshort}} registrations API: the tool reads them 5000 at a time until it has all of them. A key without associations gets no file, an existing file of the same key is replaced, and a key that appears in several rows is downloaded once. Only your user can read the files. The `execution` `.csv` file records the association counts and the status of each row.

The operation needs the same environment variables as `status`, and the `Reader` service access role on HPCS and on {{site.data.keyword.keymanagementserviceshort}} Dedicated.

If the associations of a key change during the download, the tool logs `Warning: the API reported <count> registrations for key <key ID> but <count> were downloaded; they changed during the download, run the command again for a consistent file`. Run the command again.

### Checking for stale associations
{: #migrate-tool-reg-download-check}

Pass the file of an HPCS key to the `check-registrations` command of KUR:

```sh
./<kur-binary> check-registrations <folder>/hpcs-<key ID>-registrations.json
```
{: pre}

KUR asks the service that owns each resource whether the resource still exists and reports each association as `stale`, `pending_reclamation`, `unknown`, `parent_in_use`, or `in_use`. For how to run the check, see [Checking key registrations for stale entries](/docs/key-protect?topic=key-protect-kur#kur-check-registrations). For what each status means for the migration, see [Stale associations](/docs/key-protect?topic=key-protect-migrate-st#migration-stale-associations).

A stale association still points to a resource that was deleted. It is not migrated, because no resource exists to act on the migration intent, and it does not need to be. An HPCS key can have many associations when migration starts, and many of them might be stale. Stale associations stay on the HPCS CRK and do not affect the migration of resources that still exist.

## Troubleshooting
{: #migrate-tool-troubleshoot}

Use the following information to troubleshoot common issues with the migration tool.

### Migration not completed in time
{: #migrate-tool-troubleshoot-time}

Sometimes services might encounter problems, while migrating keys.

Use the `sync` operation with the `.csv` file as input to notify services with associations to the key to try the migration again:

```sh
./<crkm-binary> sync <.csv input file>
```
{: pre}

If the number of associations of an HPCS key still does not drop to zero, check the remaining associations:

1. Download the associations with `reg-download` and check the file of the HPCS key with KUR, as described in [Downloading associations](#migrate-tool-reg-download).
2. Associations reported as `stale` point to resources that were deleted. They are not migrated, and they do not need to be.
3. For associations reported as `in_use` or `parent_in_use`, run `authz-check` to confirm that the IAM authorization policies are in place, and then run `sync` again. For associations reported as `unknown`, fix the cause that the `reason` field names and check again.
4. If an association is still reported as `in_use` after the expected migration time (up to four hours, or up to one business day for Event Streams), create an IBM Support ticket for {{site.data.keyword.keymanagementserviceshort}} and mention HPCS to {{site.data.keyword.keymanagementserviceshort}} migration.


### Invalid resource CRN format
{: #migrate-tool-troubleshoot-crn}

Error message:

```text
invalid key crn given : crn must be specified to 10 segments
```
{: screen}

The CSV status column output also reads: `FAILED - invalid key crn given : crn must be specified to 10 segments`

Review the input `.csv` file and ensure the CRN for the HPCS keys and {{site.data.keyword.keymanagementserviceshort}} Dedicated keys are formatted correctly. A key CRN has 10 segments that are separated by colons and ends with `:key:<key ID>`.

A value that does not start with `crn:` stops the tool before it processes any row, for example:

```text
Reading CSV file failed: CSV row 1: SourceCRK "sourceCRK" is not a valid CRN (must start with "crn:")
```
{: screen}

The same message appears for a header row. Remove the header row, or correct the value, and run the command again.

### Key not found
{: #migrate-tool-troubleshoot-notfound}

Error message:

```text
2023/03/29 09:23:39 Retrieving HPCS key...
2023/03/29 09:23:39 get key failed for keyID d4c4cbee-20ed-4deb-b59d-4674f70713b5 with the following error: kp.Error: correlation_id='643a49af-a3cc-4eba-b23b-368bbc33115d', msg='Not Found: Key could not be retrieved: Please see `reasons` for more details (KEY_NOT_FOUND_ERR)', reasons='[KEY_NOT_FOUND_ERR: key does not exist - FOR_MORE_INFO_REFER: https://cloud.ibm.com/apidocs/key-protect]'
```
{: screen}

The CSV status column output also reads:

```text
FAILED - get key failed for keyID d4c4cbee-20ed-4deb-b59d-4674f70713b5 with the following error: kp.Error: correlation_id='643a49af-a3cc-4eba-b23b-368bbc33115d', msg='Not Found: Key could not be retrieved: Please see `reasons` for more details (KEY_NOT_FOUND_ERR)', reasons='[KEY_NOT_FOUND_ERR: key does not exist - FOR_MORE_INFO_REFER: https://cloud.ibm.com/apidocs/key-protect]'
```
{: screen}

The CRN for the key does not exist. Review the input `.csv` file and ensure that both the HPCS keys to migrate and the {{site.data.keyword.keymanagementserviceshort}} Dedicated target keys exist. For a {{site.data.keyword.keymanagementserviceshort}} Dedicated key, the message follows `Retrieving KP-ST key...`.

### Instance does not exist or the user is unauthorized
{: #migrate-tool-troubleshoot-unauthorized}

Error message:

```text
2023/03/29 09:26:44 Retrieving HPCS key...
2023/03/29 09:26:44 get key failed for keyID d4c4cbee-20ed-4deb-b59d-4674f70713b5 with the following error: kp.Error: correlation_id='4629f1af-1a9b-44ea-9139-3c5c39b9790e', msg='Unauthorized: Either the user does not have access to the specified resource, the resource does not exist, or the region is incorrectly set'
```
{: screen}

The CSV status column output also reads:

```text
FAILED - get key failed for keyID d4c4cbee-20ed-4deb-b59d-4674f70713b5 with the following error: kp.Error: correlation_id='4629f1af-1a9b-44ea-9139-3c5c39b9790e', msg='Unauthorized: Either the user does not have access to the specified resource, the resource does not exist, or the region is incorrectly set'
```
{: screen}

The user does not have access to the key, or the instance in the CRN of the key does not exist.

### Create command: No associations found
{: #migrate-tool-troubleshoot-create-noreg}

Log message:

```text
2023/03/22 10:39:36 No registrations found for the key
```
{: screen}

The HPCS key has no cloud resources that are associated with it. The tool creates the migration intent anyway, and the CSV status column reads `SUCCESS`. No resource needs to be migrated for this key.

### Create command: Migration intent exists
{: #migrate-tool-troubleshoot-create-exists}

Error message:

```text
2023/03/22 10:39:35 Retrieving Migration Intent...
2023/03/22 10:39:35 Migration Intent found for keyID d4c4cbee-20ed-4deb-b59d-4674f70713b5
2023/03/22 10:39:35 Migration Intent already exists
```
{: screen}

The CSV status column output also reads: `FAILED - Migration Intent already exists for the key`

The HPCS key already has an existing migration intent. To update the migration intent, complete the following steps:

1. Update the input `.csv` file with the new CRN of the {{site.data.keyword.keymanagementserviceshort}} Dedicated key to migrate to.
2. Add the `--replace-mi` flag to the create command.
3. Run the command again.

If the migration intent already targets the key in the file, `--replace-mi` leaves it unchanged, and the status reads `FAILED - Migration Intent already exists with identical TargetCRK for the key`. If the tool deleted the old migration intent but could not create the new one, the status reads `FAILED - the existing Migration Intent was deleted but creating the new one failed: <error>; rerun create`. Run the `create` operation again.

### Status command: No migration intent found
{: #migrate-tool-troubleshoot-status-nomi}

Error message:

```text
2023/03/22 13:15:56 Retrieving Migration Intent...
2023/03/22 13:15:56 No Migration Intent found for keyID a2cd689e-9a49-4cd7-ba0d-07d25a0b3a25
```
{: screen}

The CSV status column output also reads: `FAILED - No Migration Intent found for the key`

A migration intent does not exist for that HPCS key. Retry [Creating migration intents](#migrate-tool-create), to ensure that the key resource CRN is included in the input file, and check the status of the operation.

### Sync command: No associations found
{: #migrate-tool-troubleshoot-sync-noreg}

Log message:

```text
2023/03/22 13:13:45 Skipping sync for keyID d4c3cbee-20ed-4deb-b59d-4674f70713b5: the source key has zero registrations, so there is nothing to sync (any associations have already migrated off this key).
```
{: screen}

The CSV status column reads `SKIPPED - No registrations were found for the key`. A skipped row is not a failure.

The HPCS key has no cloud resources that are associated with it. Therefore, you do not need to perform a syncing of resources. This also indicates that the migration is complete for the key.

### Sync command: No migration intent found
{: #migrate-tool-troubleshoot-sync-nomi}

Error message:

```text
2023/03/22 13:15:13 Skipping sync for keyID a2cd689e-9a49-4cd7-ba0d-07d25a0b3a25: no migration intent exists on the source key, so there is nothing to sync. Run `create` first if you intended to migrate this key.
```
{: screen}

The CSV status column output also reads: `FAILED - No Migration Intent found for the key`

A migration intent does not exist for that key. Retry [Creating migration intents](#migrate-tool-create), ensuring the key resource CRN is included in the input file, and check the status of the operation.

### Sync command: Request too early
{: #migrate-tool-troubleshoot-sync-early}

Error message:

```text
2023/03/29 09:17:06 Post Sync Resources for key...
2023/03/29 09:17:06 Correlation ID (sync resources): 8f088c29-2f61-4dc4-b00f-c99f7edd00fe
2023/03/29 09:17:06 Post Sync Call Failed: kp.Error: correlation_id='8f088c29-2f61-4dc4-b00f-c99f7edd00fe', msg='Conflict: Could not initiate sync requests to associated resources: Please see `reasons` for more details (REQ_TOO_EARLY_ERR)', reasons='[REQ_TOO_EARLY_ERR: The key was updated recently: Please wait and try again: Sync cannot be performed until at least 2023-03-29T14:17:19Z - FOR_MORE_INFO_REFER: https://cloud.ibm.com/apidocs/key-protect]'
```
{: screen}

The CSV status column output also reads:

```text
FAILED - kp.Error: correlation_id='8f088c29-2f61-4dc4-b00f-c99f7edd00fe', msg='Conflict: Could not initiate sync requests to associated resources: Please see `reasons` for more details (REQ_TOO_EARLY_ERR)', reasons='[REQ_TOO_EARLY_ERR: The key was updated recently: Please wait and try again: Sync cannot be performed until at least 2023-03-29T14:17:19Z - FOR_MORE_INFO_REFER: https://cloud.ibm.com/apidocs/key-protect]'
```
{: screen}

The `sync` operation is being performed too quickly. Wait at least five minutes before trying again.

### Authorization check: No policies found
{: #migrate-tool-troubleshoot-authz-nopolicies}

Log messages when the authorization policies of the target account cannot be read:

```text
2026/11/04 10:12:01 Failed to fetch policies for account 00000000000000000000000000000000: <error>
2026/11/04 10:12:03   Warning: the authorization policies of account 00000000000000000000000000000000 could not be read (see above); every association of this key is reported as NO MATCH
```
{: screen}

Log message when the account has no authorization policies for the {{site.data.keyword.keymanagementserviceshort}} service:

```text
2026/11/04 10:12:03   No policies found for service kms in account 00000000000000000000000000000000
```
{: screen}

In both cases, the tool has no policy to match, so every association of the key is reported as `NO MATCH` and counted as not matched in the summary.

- If the policies cannot be read, the associations show `NO MATCH (could not verify - …)`. Give the identity access to view the authorization policies of that account (see [Required access](#migrate-tool-authz-access)) and run `authz-check` again.
- If the account has no policies, the associations show `NO MATCH: no authorization policy for resource`. Create the policies that the associated services need (see [Setting up IAM authorization policies](#migrate-tool-iam)) and run `authz-check` again.
