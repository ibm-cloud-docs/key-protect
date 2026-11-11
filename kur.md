---

copyright:
  years: 2026

lastupdated: "2026-11-11"

keywords: key usage reporter, KUR, encryption report, key scan, activity tracking, audit logs

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Using the Key Usage Reporter (KUR) tool
{: #kur}

The Key Usage Reporter (KUR) CLI scans an IBM Cloud account and produces a comprehensive report of which cloud resources are encrypted by which KMS keys. The tool supports both Key Protect (`kms`) and Hyper Protect Crypto Services (`hs-crypto`). The tool is also capable of processing activity tracking audit log files, producing CSV summaries that help identify KMS utilization, and of checking key registrations for stale entries whose resource no longer exists.
{: shortdesc}

KUR is provided as-is and on a best-effort basis. The tool might not detect all possible usages of keys, and the results should not be treated as authoritative. Some services, configurations, or edge cases might not be covered.
{: important}


## Downloading the tool
{: #kur-download}

1. Create an [IBM Support ticket](https://www.ibm.com/mysupport/s/?language=en_US){: external} for Key Protect to request access to the HPCS to Key Protect migration tooling.

2. Download the `hpcs-2-kp-kur-tool-<version>.zip` file that is attached to the support ticket. It contains one binary for each supported platform: `kur-darwin-arm64-<version>` (macOS on Apple silicon), `kur-linux-amd64-<version>` (Linux on x86-64), and `kur-windows-amd64-<version>.exe` (Windows on x86-64).

3. Verify the SHA-256 checksum of the zip file before you extract it, and of the binary that you use after you extract it. The support ticket lists the expected value for each file. Compare the values directly; they must match exactly.

   Run the appropriate command for your operating system to get the SHA-256 checksum and compare with the value that is provided in the support ticket. The commands work for the zip file and for the binaries.

   **macOS:**
   ```sh
   shasum -a 256 <kur-binary>
   ```
   {: pre}

   Example:
   ```sh
   shasum -a 256 kur-darwin-arm64-1.3.0
   ```
   {: screen}

   **Linux:**
   ```sh
   sha256sum <kur-binary>
   ```
   {: pre}

   Example:
   ```sh
   sha256sum kur-linux-amd64-1.3.0
   ```
   {: screen}

   **Windows (Command Prompt):**
   ```cmd
   certutil -hashfile <kur-binary> SHA256
   ```
   {: pre}

   Example:
   ```cmd
   certutil -hashfile kur-windows-amd64-1.3.0.exe SHA256
   ```
   {: screen}

   **Windows (PowerShell):**
   ```powershell
   Get-FileHash <kur-binary> -Algorithm SHA256
   ```
   {: pre}

   Example:
   ```powershell
   Get-FileHash kur-windows-amd64-1.3.0.exe -Algorithm SHA256
   ```
   {: screen}


4. Make the binary executable (macOS/Linux):

   ```sh
   chmod +x <kur-binary>
   ```
   {: pre}

5. Check the version of the tool:

   ```sh
   ./<kur-binary> version
   ```
   {: pre}

   The output shows the version and the build, for example `kur 1.3.0 (commit 1a2b3c4)`. The `--version` and `-version` flags print the same.


## Prerequisites
{: #kur-prereqs}

Before running the tool, ensure that the following requirements are met:

* **IBM Cloud CLI** (`ibmcloud`) is installed. For more information, see the instructions in [Getting started with the IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cli-getting-started&format=markdown).
* The following **IBM Cloud CLI plugin** is installed and up-to-date:
   * `vpc-infrastructure`

   Install it with:
   ```sh
   ibmcloud plugin install vpc-infrastructure
   ```
   {: pre}

* You are **logged in** to the IBM Cloud CLI and targeting the account that you want to scan:
   ```sh
   ibmcloud login
   ```
   {: pre}

   For unattended runs, you can instead export an IAM API key and let the tool log in for you. See [Environment variables](#kur-env-vars).
   ```sh
   export IBMCLOUD_API_KEY=<api-key>
   ```
   {: pre}

   On Windows, use `$env:IBMCLOUD_API_KEY = "<api-key>"` in PowerShell or `set IBMCLOUD_API_KEY=<api-key>` in Command Prompt.

   The tool scans one account per run. To scan more than one account, see [Scanning multiple accounts](#kur-multiple-accounts).

* Your **IAM token is valid** and has at least 3 minutes of remaining validity. If in doubt, refresh it:
   ```sh
   ibmcloud login
   ```
   {: pre}

* The identity that runs the tool requires account-wide **read-only** access. Assign the **Viewer** platform access role and the _Reader_ service access role across the account to the identity (user or API key) that you authenticate with.

  This read-only, auditor-style access is the same level used to audit an account. It covers everything that the tool inspects, including:
  - Key Protect and Hyper Protect Crypto Services instances and keys
  - Cloud services that can be encrypted by those keys (for example, Cloud Object Storage, Databases for PostgreSQL and the other IBM Cloud Databases, Secrets Manager, Event Streams, App ID, Event Notifications, App Configuration, Schematics, Kubernetes clusters, and VPC block volumes, file shares and custom images)

  Some services are inspected through their own API rather than through the platform's resource list (Cloud Object Storage buckets, Event Notifications and App Configuration integrations, Kubernetes clusters, Schematics workspaces). Without the Reader role on such a service, its resources are not attributed to a key. Attributing Schematics workspaces to a specific key also requires access to the account's Schematics KMS settings; without it, workspaces are reported at the KMS instance level under [Unknowns](#kur-unknowns).

  KUR performs read operations only and does not create, modify, or delete any resources.


The access requirements in this section apply to the account scan. The `process-at` subcommand works entirely on a local activity tracking file and requires no IBM Cloud access. The `check-registrations` subcommand needs read access only to the resources that it checks, and does not need the `vpc-infrastructure` plugin; see [Checking key registrations for stale entries](#kur-check-registrations).
{: note}

## Scanning multiple accounts
{: #kur-multiple-accounts}

Each run of the tool scans a single IBM Cloud account: the account that the IBM Cloud CLI is targeting. Run the tool in every account where HPCS keys might be used, not only in the accounts that contain HPCS instances.
{: important}

An HPCS key in one account can encrypt resources in another account. In each run, the tool lists the HPCS instances and keys of the targeted account, but it scans for encrypted resources only in the targeted account. For example, if a Cloud Object Storage bucket in account B is encrypted with an HPCS key from account A, the resource scan finds that bucket only when you run the tool in account B. In the report for account B, the HPCS instance of that key is listed with `found_by_kms_instance_listing` set to `false`, because the instance belongs to account A.

To find the accounts that contain HPCS instances, see [Identifying HPCS usage](/docs/key-protect?topic=key-protect-migrate-st#migration-identify-hpcs-usage). Also scan every account whose resources might use keys from those HPCS instances.

Complete the following steps for each account:

1. Target the account that you want to scan:
   ```sh
   ibmcloud target -c <account_id>
   ```
   {: pre}

2. Confirm that the CLI is targeting the intended account. The IBM Cloud CLI operates on a single active account at a time.
   ```sh
   ibmcloud target
   ```
   {: pre}

3. Run the tool as described in [Running the tool](#kur-running).

The identity that runs the tool needs the access that is described in [Prerequisites](#kur-prereqs) in every account that you scan.

By default, the report file name includes the account name, so reports from different accounts do not overwrite each other. If you use the `--output` flag, specify a different file name for each account.

If `IBMCLOUD_API_KEY` is set, the tool logs in with that API key at the start of every run, which replaces the account that you selected with `ibmcloud target -c`. The tool then scans the account that is associated with the API key. Use an API key from each account that you scan:

```sh
IBMCLOUD_API_KEY=<api-key-for-the-account> ./<kur-binary>
```
{: pre}

## Running the tool
{: #kur-running}

The following examples show how to run the Key Usage Reporter tool with different options and configurations.

The examples use macOS and Linux syntax. On Windows, run the binary as `.\<kur-binary>` in PowerShell or `<kur-binary>` in Command Prompt. The flags are the same.
{: note}

### Basic usage: scan for HPCS keys (default)
{: #kur-basic-usage}

Use the following command to scan the currently targeted IBM Cloud account for all `hs-crypto` instances, their keys, and any cloud resources encrypted by those keys.

```sh
./<kur-binary>
```
{: pre}

### Scan for Key Protect keys
{: #kur-scan-kp}

To scan for Key Protect instances instead of HPCS, use the `--service kms` flag.

```sh
./<kur-binary> --service kms
```
{: pre}

### Scan for Key Protect Dedicated instances only
{: #kur-scan-kp-dedicated}

You can filter the scan to include only Key Protect Dedicated instances.

```sh
./<kur-binary> --service kms --service-type dedicated
```
{: pre}

### Scan for Key Protect multi-tenant instances only
{: #kur-scan-kp-multitenant}

You can filter the scan to include only Key Protect Standard (multi-tenant) instances.

```sh
./<kur-binary> --service kms --service-type multi-tenant
```
{: pre}

### Enable debug logging
{: #kur-debug}

Enable detailed debug output to troubleshoot issues or understand the tool's behavior.

```sh
./<kur-binary> --service kms --debug
```
{: pre}

### Specify a custom output file path
{: #kur-custom-output}

By default, the tool generates an output file with an auto-generated name, but you can specify a custom path.

```sh
./<kur-binary> --service kms --output my-report.json
```
{: pre}

## CLI flags
{: #kur-cli-flags}

The following table lists all available command-line flags for the Key Usage Reporter tool.

| Flag                   | Default | Description |
|------------------------|---------|-------------|
| `--service`            | `hs-crypto` | KMS service to scan: `hs-crypto` or `kms` |
| `--service-type`       | (None) | Filter KMS instances by type: `dedicated` or `multi-tenant`. Only valid with `-service kms`. |
| `--skip-private-calls` | `false` | Skip REST calls to private endpoints. Instances without a public endpoint are skipped. |
| `--debug`              | `false` | Enable debug mode: show detailed log messages on stderr |
| `--output`             | Auto-named | Output file path. Defaults to `encryption-key-usage-report-<service>[-<service-type>]-<account-name>.json`. The `<service-type>` segment is included only when `--service-type` is specified (for example, `encryption-key-usage-report-kms-dedicated-my-account.json`). In `<account-name>`, the account name is in lowercase, and characters other than letters, digits, and hyphens are replaced by hyphens. |
| `--version`            | `false` | Print the version and exit |
{: caption="CLI flags for the Key Usage Reporter tool" caption-side="bottom"}

Flags can use single dash (`-flag`) or double dash (`--flag`).
{: note}

## Environment variables
{: #kur-env-vars}

| Variable | Default | Description |
|----------|---------|-------------|
| `IBMCLOUD_API_KEY` | (None) | IBM Cloud IAM API key. When set, the tool runs `ibmcloud login` with it at the start of the run, replacing any existing CLI session. The key is read by the CLI from the environment and never appears on a command line. |
| `IBMCLOUD_REGION` | `us-south` | Region targeted by that login. Only used together with `IBMCLOUD_API_KEY`. The VPC scan visits every region regardless of this value. |
| `IBMCLOUD_API_KEY_<account ID>` | (None) | For `check-registrations` only. An API key of the account in the variable name, for example `IBMCLOUD_API_KEY_c73effe6a5be4c1e8a9a3b91e43b7165`, used to check the resources of that account. The tool logs in with it in a private, temporary CLI configuration, so your own CLI session is not changed. See [Resources in several accounts](#kur-cr-accounts). |
{: caption="Environment variables" caption-side="bottom"}

```sh
IBMCLOUD_API_KEY=<api-key> ./<kur-binary> --service kms
```
{: pre}

On Windows PowerShell:

```powershell
$env:IBMCLOUD_API_KEY = "<api-key>"
.\<kur-binary> --service kms
```
{: pre}

The IBM Cloud CLI and the `vpc-infrastructure` plugin are still required when you use the API key.
{: note}

## How usage is detected
{: #kur-how-detected}

The tool lists every service instance in the account, the Kubernetes and OpenShift clusters, and, in every VPC region, the block volumes, file shares, custom images, snapshots, and backup policies. It then looks at each resource for a reference to a Key Protect or HPCS key:

* For most services, the platform's resource record carries the key CRN the resource was provisioned with. IBM Cloud Databases, Secrets Manager, Event Streams, App ID, Cloudant, and the VPC resources are attributed this way.
* Some services do not expose the key in their record, so the tool asks the service itself: Cloud Object Storage (bucket encryption settings and backup vault keys), Event Notifications and App Configuration (their KMS integrations), Kubernetes and OpenShift (cluster details), and Schematics (workspace encryption together with the account's Schematics KMS settings).

In the report, each resource is listed once per key under a label of the form `<service> (<display name>)`, for example `cloud-object-storage (Cloud Object Storage)`. VPC resources are labelled by resource type: `volume (VPC Block Storage Volume)`, `share (VPC File Share)`, `image (VPC Image)`, `snapshot (VPC Snapshot)`, `backup-policy (VPC Backup Policy)`.

To list the VPC resources, the tool targets each VPC region in turn with `ibmcloud target -r` and switches the CLI back to its original region at the end, also when you press Ctrl+C. Do not use the IBM Cloud CLI in another terminal while the tool runs, because both use the same session.

## Output files
{: #kur-output-files}

The tool produces two output files:

JSON report
:   The main output file (for example, `encryption-key-usage-report-kms-kp-stage.json`), containing the full hierarchical report of KMS instances, keys, and resource usages.

Log file
:   A companion log file with the same base name and a `-log.txt` suffix (for example, `encryption-key-usage-report-kms-kp-stage-log.txt`), containing all log messages from the run.

Only your user can read the output files.

## Understanding the output
{: #kur-understanding-output}

The JSON report has the following top-level structure:

```json
{
  "metadata": { ... },
  "result": {
    "kms_instances": [ ... ],
    "crns": [ ... ],
    "unknowns": [ ... ]
  }
}
```
{: codeblock}

### Metadata
{: #kur-metadata}

Metadata includes execution context such as the tool version, target KMS service, IBM Cloud API endpoint, account name, account ID, and the user who ran the scan.

### KMS instances
{: #kur-kms-instances}

One entry per KMS or HPCS instance found in the account, or referenced by a resource in the account (for example, an HPCS instance in another account). Instances with detected key usage are listed first, then instances with no detected usage. Each instance contains:

Instance metadata
:   Name, CRN, state, allowed network, public and private endpoints, type (for Key Protect: `multi-tenant` or `dedicated`).

`found_by_kms_instance_listing`
:   `true` if the instance was found by listing KMS instances in the account. `false` when the instance is known only because a resource in the account references it, for example when the instance belongs to another account.

`found_by_resource_scan`
:   `true` if the resource scan found at least one resource encrypted by this instance, either through one of its keys or through a reference to the instance alone (see [Unknowns](#kur-unknowns)).

`instance_stats`
:   Key counts by state:
   * `active_crk_count`, `suspended_crk_count`, `deactivated_crk_count`, `destroyed_crk_count`
   * `active_standard_key_count`, `destroyed_standard_key_count`.

`keys[]`
:   Full key inventory from the Key Protect API, with keys in every state, including destroyed keys. Each key includes:
   * `type`: `crk` (Customer Root Key) or `standard_key`.
   * `state_name`: `pre-activation`, `active`, `suspended`, `deactivated`, or `destroyed`.
   * `name`: key name.
   * `id`: key UUID.
   * `has_migration_intent`: whether the key has a migration intent set.
   * `migration_intent_target_crk`: target CRK CRN (only present when `has_migration_intent` is `true`).
   * `found_by_kms_key_listing` or `found_by_resource_scan`: how the key was discovered.
   * `associations[]`: cloud resources registered against the key (from the Key Protect registrations API). Each entry shows the `resource_crn` and whether it has `prevent_key_deletion` enabled. Omitted when a key has no registrations. Some associations can be stale, which means that they point to resources that were deleted; see [Checking key registrations for stale entries](#kur-check-registrations).
   * `service_usage`: map of service label to the encrypted resources detected by the account-wide resource scan, each resource listed once per key (see [How usage is detected](#kur-how-detected) for the labels). Only present for keys found by the resource scan.

### CRNs
{: #kur-crns}

Resources that reference encryption identifiers that match a CRN pattern but not a KMS or HPCS key CRN are captured here to ensure that nothing is silently dropped.

### Unknowns
{: #kur-unknowns}

Resources for which no Key Protect or HPCS key CRN could be identified are listed here. This grouping includes resources whose encryption identifier could not be parsed as a CRN, resources that referenced only a KMS instance CRN rather than a specific key, and resources with an unrecognized string. These resources are grouped under a single entry with an ID of `unknown`, so that no resources are silently dropped.

Schematics workspaces and agents are a special case. The Schematics API exposes only the KMS instance that a workspace is encrypted with, while the key is configured once per account and geography in the Schematics KMS settings. The tool reads those settings and attributes the workspace to the configured key when exactly one configured key belongs to the instance the workspace reports. Otherwise, for example when the settings cannot be read or when two keys of that instance are configured during a key rotation, the workspace is listed here with its `kms_instance_crn`, and the log states why.

### Example instance entry
{: #kur-example-instance}

The following example shows the structure of a KMS instance entry in the JSON report.

```json
{
  "name": "my-kp-instance",
  "type": "multi-tenant",
  "crn": "crn:v1:bluemix:public:kms:us-south:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab::",
  "state": "active",
  "allowed_network": "public-and-private",
  "public_endpoint": "https://us-south.kms.cloud.ibm.com",
  "private_endpoint": "https://private.us-south.kms.cloud.ibm.com",
  "found_by_kms_instance_listing": true,
  "found_by_resource_scan": true,
  "instance_stats": {
    "active_crk_count": 5,
    "suspended_crk_count": 0,
    "deactivated_crk_count": 1,
    "destroyed_crk_count": 2,
    "active_standard_key_count": 3,
    "destroyed_standard_key_count": 1
  },
  "keys": [
    {
      "type": "crk",
      "state_name": "active",
      "name": "my-root-key",
      "id": "abc12345-6789-0abc-def0-1234567890ab",
      "has_migration_intent": false,
      "found_by_kms_key_listing": true,
      "found_by_resource_scan": true,
      "associations": [
        {
          "resource_crn": "crn:v1:bluemix:public:cloud-object-storage:global:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:bucket:my-encrypted-bucket",
          "prevent_key_deletion": true
        }
      ],
      "service_usage": {
        "cloud-object-storage (Cloud Object Storage)": [
          {
            "encrypted_resource": "crn:v1:bluemix:public:cloud-object-storage:global:a/00000000000000000000000000000000:deadbeef-0000-0000-0000-1234567890ab:bucket:my-encrypted-bucket"
          }
        ]
      }
    },
    {
      "type": "standard_key",
      "state_name": "active",
      "name": "my-standard-key",
      "id": "def45678-9012-3456-7890-abcdef012345",
      "has_migration_intent": false,
      "found_by_kms_key_listing": true,
      "found_by_resource_scan": false
    }
  ]
}
```
{: codeblock}

## Processing activity tracking logs
{: #kur-process-at}

In addition to generating the main report, the tool includes a subcommand to process activity tracking audit logs.

### Usage
{: #kur-at-usage}

Use the `process-at` subcommand to process activity tracking log files.

```sh
./<kur-binary> process-at <input.tsv>
```
{: pre}

### What it does
{: #kur-at-function}

The subcommand takes a TSV file that is exported from the IBM Cloud Logs activity tracking event routing archive query, extracts the JSON events from the `text` column, and filters for KMS and HPCS-related events (`kms.*` and `hs-crypto.*` actions). It then produces four output files, next to the input file:

`<base>_events.json`
:   All events extracted as a formatted JSON array.

`<base>_events.csv`
:   Flat CSV with one row per event, containing: serviceName, region, accountId, instanceId, keyId, action, outcome, reasonType, reasonCode, initiatorId, initiatorName, authId, requestInstanceId, eventTime, correlationId, agent.

`<base>_events_summary.csv`
:   Grouped summary with event counts, grouped by service, region, account, instance, key, action, outcome, reason, and initiator.

`<base>_events_summary_by_action.csv`
:   Grouped summary with event counts, grouped by service, region, account, instance, key, action, and initiator (without outcome or reason breakdown).

Where `<base>` is derived from the input file name (stripping `_logs.tsv` or `.tsv`, in any letter case).

The input file must be UTF-8 encoded. A leading byte order mark and Windows (CRLF) line endings are accepted, so a file that is saved again as UTF-8 by Excel or Notepad still works. In the CSV files, a text value that starts with `=`, `+`, `-`, or `@` gets a leading `'`, so that a spreadsheet does not run it as a formula.

### Example
{: #kur-at-example}

The following example shows how to process an activity tracking log file and the output files that are generated.

```sh
./<kur-binary> process-at hpcs-at-data-1-day_logs.tsv
```
{: pre}

The command produces the following output files:

* `hpcs-at-data-1-day_events.json`
* `hpcs-at-data-1-day_events.csv`
* `hpcs-at-data-1-day_events_summary.csv`
* `hpcs-at-data-1-day_events_summary_by_action.csv`

This feature is useful for analyzing KMS key activity patterns, identifying which services and users are performing key operations, and investigating migration-related events like `ack-migrate`.

## Checking key registrations for stale entries
{: #kur-check-registrations}

A Key Protect or HPCS key records which resources use it as registrations, one per resource CRN. The HPCS migration documentation calls them associations. The service that encrypts a resource creates the registration and is expected to delete it when the resource is deleted. When that does not happen, the key keeps registrations for resources that no longer exist. The `check-registrations` subcommand takes the registrations of a key and asks the service that owns each resource whether the resource still exists.

The subcommand is read-only: it does not delete or change registrations or resources.
{: note}

### Usage
{: #kur-cr-usage}

1. Save the registrations of the key in a file. The simplest way is the `reg-download` command of the Key Migration Tool (CRKM), which writes one complete file per key, for example `hpcs-<key ID>-registrations.json`. See [Downloading associations](/docs/key-protect?topic=key-protect-migrate-tool#migrate-tool-reg-download).

2. Log in to IBM Cloud, preferably to the account that owns most of the **resources** (not necessarily the account of the KMS instance), and run:

    ```sh
    ./<kur-binary> check-registrations registrations.json
    ```
    {: pre}

The identity needs read access to the resources: Viewer on the service instances (Resource Controller), Reader on Schematics workspaces and actions, Cloud Object Storage buckets, VPC infrastructure and Kubernetes Service clusters. An answer of HTTP 403 is never treated as proof that a resource is gone. The `vpc-infrastructure` plugin is not needed: VPC resources are checked through the VPC REST API.

| Flag                   | Default | Description |
|------------------------|---------|-------------|
| `--output`             | `<input name>-check.json` | JSON report path. The default is next to the input file. The CSV and the log take the same name, with `.csv` and `-log.txt` in place of `.json`. |
| `--workers`            | `10` | Number of resources checked concurrently, from 1 to 20. |
| `--skip-private-calls` | `false` | Skip REST calls to private endpoints. |
| `--no-switch-accounts` | `false` | Check only the logged-in account, without `ibmcloud target -c` or `IBMCLOUD_API_KEY_<account ID>` logins. Registrations of resources in other accounts are reported as `unknown`. |
| `--debug`              | `false` | Show detailed log messages on stderr. |
{: caption="Flags of the check-registrations subcommand" caption-side="bottom"}

Flags can be placed before or after the file name.

### Resources in several accounts
{: #kur-cr-accounts}

A resource can be checked only with a token of the account that owns it, the `a/<account ID>` part of its CRN. The subcommand checks the logged-in account first and then every other account in the file, the account with the most registrations first. It reaches each other account in one of two ways:

- **With an API key of the account.** When the variable `IBMCLOUD_API_KEY_<account ID>` is set, the tool logs in with that key in a private, temporary CLI configuration and removes the configuration afterwards. Your own CLI session is not changed. An API key logs in only to the account in which it was created. This works with either kind of main login.
- **By switching your CLI session.** Otherwise, the tool runs `ibmcloud target -c <account ID>`, as you would yourself. This works when you logged in with `ibmcloud login` as a user who is a member of that account. At the end, also after an error or when the run is interrupted (Ctrl+C, `SIGTERM`, or closing the terminal), the tool switches the CLI back to the account, region, and resource group that were targeted before. If that fails, the log shows the `ibmcloud target` command to run.

A login with `IBMCLOUD_API_KEY` cannot switch accounts. With it, only the accounts that have an `IBMCLOUD_API_KEY_<account ID>` variable are checked.

```sh
export IBMCLOUD_API_KEY=<API key of the main account>
export IBMCLOUD_API_KEY_c73effe6a5be4c1e8a9a3b91e43b7165=<API key of account c73effe6a5be4c1e8a9a3b91e43b7165>
./<kur-binary> check-registrations registrations.json
```
{: pre}

Do not use the IBM Cloud CLI in another terminal while the subcommand runs, because both use the same session. Before it checks an account, the tool confirms that the token of the CLI session belongs to that account. If the CLI was switched to another account in the meantime, the registrations of that account are reported as `unknown`.

The log has one line per account that says how the account was reached. When an account cannot be reached, for example because you are not a member and no API key is set for it, its registrations are reported as `unknown` with the reason, and `summary.not_checked_accounts` counts them.

Each registration gets one of the following statuses:

| Status | Meaning |
|--------|---------|
| `stale` | The resource is gone: the service answered HTTP 404 or 410, the Resource Controller reports the instance as `removed`, or the parent instance is gone. A resource that exists but is encrypted with a different key, or with no customer-managed key, is stale for this key as well. |
| `pending_reclamation` | The parent instance is deleted but can still be restored. The registration is expected to stay until the reclamation ends. |
| `unknown` | The check could not decide: resource in an account that could not be checked, access denied, error after retries, invalid CRN, a CRN without a service instance, or a resource type that is not checked. The `reason` says which. |
| `parent_in_use` | Only the parent instance could be checked, and it exists. |
| `in_use` | The resource exists. `key_verified` is `true` when the service also reported the key, and it matches. |
{: caption="Statuses of check-registrations" caption-side="bottom"}

Confirm a `stale` result with its `reason` field, which records the answer that led to it. For what each status means for a migration from HPCS, see [Stale associations](/docs/key-protect?topic=key-protect-migrate-st#migration-stale-associations).

The subcommand exits with status `0` when no registration is stale and `2` when at least one registration is stale; the report is written in both cases. It exits with `1` on an error, and also when it could not switch the CLI back to the account that was targeted before; the report is still written in that case. 

### Output files
{: #kur-cr-output}

With the default `--output`, the files are named after the input file:

`<input name>-check.json`
:   The report. `metadata` describes the run: the account and user, the input file and its registration counts, and `accounts`, with one entry per account that says how it was reached (`login`) or why it was not checked (`error`). `summary` counts the registrations by status and by service, and those of accounts that were not checked (`not_checked_accounts`). `registrations` holds one entry per registration, sorted by status (`stale` first) and then by CRN, with the fields that are shown in the following example, and `resource_key_crn`, the key that the service reported, when it reported one.

`<input name>-check.csv`
:   The `registrations` entries with the same columns, for filtering in a spreadsheet. A text value that starts with `=`, `+`, `-`, or `@` gets a leading `'`, so that a spreadsheet does not run it as a formula.

`<input name>-check-log.txt`
:   The log of the run, including the summary.

Only your user can read the output files.

```json
{
  "resource_crn": "crn:v1:bluemix:public:schematics:us-south:a/<account>:<guid>:workspace:us-east.workspace.my-workspace.5498fb66",
  "key_id": "7ffee345-b7b8-454a-a438-88bcba3cea27",
  "key_name": "root-key-at-rest",
  "service": "schematics",
  "resource_type": "workspace",
  "status": "stale",
  "reason": "Schematics (us-east): workspace not found (HTTP 404)",
  "key_verified": false,
  "prevent_key_deletion": false,
  "creation_date": "2024-11-06T20:54:10Z",
  "last_updated": "2024-11-06T20:54:10Z"
}
```
{: codeblock}

## Troubleshooting
{: #kur-troubleshooting}

The following information helps you to resolve common issues when running the Key Usage Reporter tool.

### IBM Cloud CLI not installed
{: #kur-ts-cli-not-installed}

```text
Error: failed to retrieve account information: command failed: ibmcloud target --output json: exec: "ibmcloud": executable file not found in $PATH
```
{: screen}

When `IBMCLOUD_API_KEY` is set, the message is:

```text
Error: {{site.data.keyword.Bluemix_notm}} CLI is not installed. Please install it first.
Visit: https://cloud.ibm.com/docs/cli?topic=cli-getting-started
```
{: screen}

Install the IBM Cloud CLI by following the [Getting started with the IBM Cloud CLI](/docs/cli?topic=cli-getting-started) documentation.

### Missing CLI plugins
{: #kur-ts-missing-plugins}

If required CLI plugins are not installed, the tool stops with an error message that lists the missing plugins.

```text
Error: prerequisite check failed: missing required {{site.data.keyword.Bluemix_notm}} CLI plugins: [vpc-infrastructure]
Install them using: ibmcloud plugin install <plugin-name>
```
{: screen}

Install the missing plugin:

```sh
ibmcloud plugin install vpc-infrastructure
```
{: pre}

### Outdated CLI plugins
{: #kur-ts-outdated-plugins}

If your CLI plugins are outdated, the tool stops with an error message that lists which plugins need to be updated.

```text
Error: prerequisite check failed: the following {{site.data.keyword.Bluemix_notm}} CLI plugins are outdated: [vpc-infrastructure]
Update them using: ibmcloud plugin update <plugin-name>
```
{: screen}

Update the plugin:

```sh
ibmcloud plugin update vpc-infrastructure
```
{: pre}

### Not logged in
{: #kur-ts-not-logged-in}

If you are not logged in to IBM Cloud, the tool displays an error message.

```text
Error: prerequisite check failed: not logged in to {{site.data.keyword.Bluemix_notm}}. Please login first
```
{: screen}

Log in to IBM Cloud:

```sh
ibmcloud login
```
{: pre}

### API key login failed
{: #kur-ts-api-key-login}

```text
Error: login with IBMCLOUD_API_KEY failed: command failed: ibmcloud login -r us-south -q: exit status 1
```
{: screen}

`IBMCLOUD_API_KEY` is set but the IBM Cloud CLI could not log in with it. The CLI's own message follows on the next lines and usually names the cause: a key that was deleted or mistyped, a locked user, or a region that does not exist. Fix the key or unset the variable to fall back to your existing CLI session.

### Token expired or about to expire
{: #kur-ts-token-expired}

The tool requires a valid IAM token with at least 3 minutes of remaining validity.

```text
Error: prerequisite check failed: token is expired or will expire in less than 180 seconds
```
{: screen}

If your IAM token has less than 3 minutes of remaining validity, the tool rejects it. Refresh your session:

```sh
ibmcloud login
```
{: pre}

### Resource no longer exists
{: #kur-ts-stale-resource}

```text
[WARNING] failed to process resource crn:...:schematics:...:agent:<name>: ... HTTP 404
```
{: screen}

The platform's resource list still contains a record for a resource that the service no longer recognizes, for example a deleted Schematics agent. The tool reports it and continues; the rest of the report is not affected. Deleting the stale record from the account removes the warning.

### Clusters missing from the report
{: #kur-ts-cluster-listing}

```text
[WARNING] Failed to list the <provider> clusters, their keys are not in the report: <error>
```
{: screen}

The tool could not list the Kubernetes or OpenShift clusters of one provider, for example because the identity lacks the Reader role on Kubernetes Service. It continues with the rest of the scan, but the keys of those clusters are not in the report. Fix the cause that the message names and run the tool again.

### Private-only instances
{: #kur-ts-private-only}

If a KMS instance allows only private network access and you are not connected to the IBM Cloud private network, the tool cannot fetch stats or keys for that instance. It logs a warning such as `Failed to fetch keys for instance <CRN>: ...` and continues, so the report has no keys for that instance. Use `--skip-private-calls` to skip these instances without trying to reach them:

```sh
./<kur-binary> --service kms --skip-private-calls
```
{: pre}

### 100+ KMS instances
{: #kur-ts-100-instances}

The IBM Cloud resource listing API has a limit on the number of instances that can be returned.

```text
[WARNING] 100 or more KMS instances, only the first 100 instances will be processed.
```
{: screen}

The IBM Cloud resource listing API returns a maximum of 100 instances. If the account has more than 100 KMS instances, only the first 100 are included in the report. This behavior is a known limitation.

### Invalid service-type with hs-crypto
{: #kur-ts-invalid-service-type}

The `--service-type` flag is valid only when scanning Key Protect instances.

```text
error: --service-type can only be used with --service kms
```
{: screen}

The `--service-type` flag (to filter by `dedicated` or `multi-tenant`) applies only to Key Protect (`--service kms`). It is not applicable to HPCS instances.

### Invalid output file name
{: #kur-ts-invalid-output-name}

The tool rejects the file name that is passed to `--output`.

```text
Error: invalid output file name "a:b.json"
```
{: screen}

On Windows, the output file name cannot contain a colon or be a reserved device name, such as `NUL`, `CON`, or `COM1`. Choose a different name.

### Activity tracking file is UTF-16 encoded
{: #kur-ts-at-utf16}

The `process-at` subcommand rejects the input file.

```text
Error: stage 1 (TSV to JSON): input file is UTF-16 encoded; save it as UTF-8 (tab-separated) and retry
```
{: screen}

The file was saved as UTF-16, for example as "Unicode Text" from Excel or through a PowerShell 5 `>` redirect. Save the file again as UTF-8 (in Notepad, select **Save As** and set **Encoding** to **UTF-8**; in PowerShell, use `Out-File -Encoding utf8`) and run the command again.

### Registrations file holds only one page
{: #kur-ts-cr-truncated}

The `check-registrations` subcommand warns that the file is incomplete.

```text
[WARNING] The file holds 200 of 1670 registrations (metadata.totalCount): it is one page of the Key Protect API. Download every page (?totalCount=true&limit=5000, then offset=5000, 10000, ...) and merge their resources, or use crkm reg-download, to check them all.
[WARNING] The file holds exactly 200 registrations, the default page size of the Key Protect API, and no metadata.totalCount: it may be the first page only. Download every page (...)
```
{: screen}

The registrations API returns one page at a time. The first message is certain: the file says how many registrations exist. The second message is a guess for a file that was downloaded without `totalCount=true`: a file that holds exactly one page size, 200 or 5000 registrations, is usually a first page, but it can also belong to a key with exactly that many registrations. Download the registrations again with the `reg-download` command of the Key Migration Tool, whose files are always complete, or with `totalCount=true` and every page (see [Usage](#kur-cr-usage)), and run the subcommand again. The registrations in the file are still checked.

### Registrations in another account
{: #kur-ts-cr-other-account}

```text
[WARNING] Account c73effe6a5be4c1e8a9a3b91e43b7165: 3 registration(s) not checked: switching the IBM Cloud CLI to the account failed (...); log in to IBM Cloud as a member of the account, or set IBMCLOUD_API_KEY_c73effe6a5be4c1e8a9a3b91e43b7165 to an API key of the account, and rerun
[WARNING] 3 registration(s) belong to account c73effe6a5be4c1e8a9a3b91e43b7165 and were not checked (the reason is in the report and above)
```
{: screen}

A resource can be checked only with a token of the account that owns it, and the tool could not get one (see [Resources in several accounts](#kur-cr-accounts)). The reason is also in the `reason` of each registration and in `metadata.accounts[].error`. Log in to IBM Cloud as a user who is a member of that account, or set `IBMCLOUD_API_KEY_<account ID>` to an API key that was created in that account, and run `check-registrations` on the same file again. With `IBMCLOUD_API_KEY` as the main login, only the API key works. If you used `--no-switch-accounts`, run the subcommand without it.

If the reason is `the OAuth token of the CLI session is for account <account ID>, not <account ID>; was the CLI target changed in another terminal?`, the CLI session was switched to another account while the subcommand ran. Do not use the IBM Cloud CLI in another terminal during the run, and run the subcommand again.

### CLI not switched back to your account
{: #kur-ts-cr-restore}

```text
[WARNING] Failed to switch the IBM Cloud CLI back to account df7e97f1aa8e4c3097ecc594d5af11a8: <error>. Run this command to do it: ibmcloud target -c df7e97f1aa8e4c3097ecc594d5af11a8 -r us-south -g Default -q
[WARNING] The IBM Cloud CLI may still target another account. Run this command to switch it back: ibmcloud target -c df7e97f1aa8e4c3097ecc594d5af11a8 -r us-south -g Default -q
```
{: screen}

The tool switched your CLI session to another account to check its resources and could not switch it back. The report is written, and the subcommand exits with status `1`. Run the command from the message, or `ibmcloud target -c <account ID>` for the account that you want.

The account scan switches only the region of the CLI, to list VPC resources. If it cannot switch the region back, it logs `Failed to switch the IBM Cloud CLI back to its region: <error>. Run this command to do it: ibmcloud target -r <region> -q`. Run that command.
