---

copyright:
  years: 2026

lastupdated: "2026-09-15"

keywords: key usage reporter, KUR, encryption report, key scan, activity tracking, audit logs

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Using the Key Usage Reporter (KUR) tool
{: #kur}

The Key Usage Reporter (KUR) CLI scans an {{site.data.keyword.Bluemix_notm}} account and produces a comprehensive report of which cloud resources are encrypted by which KMS keys. The tool supports both {{site.data.keyword.keymanagementserviceshort}} (`kms`) and {{site.data.keyword.hscrypto}} (`hs-crypto`). The tool is also capable of processing activity tracking audit log files, producing CSV summaries that help identify KMS utilization.
{: shortdesc}

KUR is provided as-is and on a best-effort basis. The tool might not detect all possible usages of keys, and the results should not be treated as authoritative. Some services, configurations, or edge cases might not be covered.
{: important}


## Downloading the tool
{: #kur-download}

1. Create an [IBM Support ticket](https://www.ibm.com/mysupport/s/?language=en_US){: external} for Key Protect to request access to the HPCS to Key Protect migration tooling.

2. Download the tool binary provided in the support ticket.

3. Verify the SHA-256 checksum of the downloaded binary matches the value that is provided in the support ticket. Compare the values directly; they must match exactly.

   Run the appropriate command for your operating system to get the SHA-256 checksum and compare with the value that is provided in the support ticket:

   **macOS:**
   ```sh
   shasum -a 256 <kur-binary>
   ```
   {: pre}

   Example:
   ```sh
   shasum -a 256 kur-darwin-arm64-1.0.0
   ```
   {: screen}

   **Linux:**
   ```sh
   sha256sum <kur-binary>
   ```
   {: pre}

   Example:
   ```sh
   sha256sum kur-linux-amd64-1.0.0
   ```
   {: screen}

   **Windows (Command Prompt):**
   ```cmd
   certutil -hashfile <kur-binary> SHA256
   ```
   {: pre}

   Example:
   ```cmd
   certutil -hashfile kur-windows-amd64-1.0.0.exe SHA256
   ```
   {: screen}

   **Windows (PowerShell):**
   ```powershell
   Get-FileHash <kur-binary> -Algorithm SHA256
   ```
   {: pre}

   Example:
   ```powershell
   Get-FileHash kur-windows-amd64-1.0.0.exe -Algorithm SHA256
   ```
   {: screen}


4. Make the binary executable (macOS/Linux):

   ```sh
   chmod +x <kur-binary>
   ```
   {: pre}


## Prerequisites
{: #kur-prereqs}

Before running the tool, ensure that the following requirements are met:

* **{{site.data.keyword.Bluemix_notm}} CLI** (`ibmcloud`) is installed. For more information, see the instructions in [Getting started with the {{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cli-getting-started).
* The following **{{site.data.keyword.Bluemix_notm}} CLI plugin** is installed and up-to-date:
   * `vpc-infrastructure`

   Install it with:
   ```sh
   ibmcloud plugin install vpc-infrastructure
   ```
   {: pre}

* You are **logged in** to the {{site.data.keyword.Bluemix_notm}} CLI and targeting the account that you want to scan:
   ```sh
   ibmcloud login
   ```
   {: pre}

   For unattended runs, you can instead export an IAM API key and let the tool log in for you. See [Environment variables](#kur-env-vars).
   ```sh
   export IBMCLOUD_API_KEY=<api-key>
   ```
   {: pre}

* Your **IAM token is valid** and has at least 3 minutes of remaining validity. If in doubt, refresh it:
   ```sh
   ibmcloud login
   ```
   {: pre}

* The identity that runs the tool requires account-wide **read-only** access. Assign the **Viewer** platform access role and the _Reader_ service access role across the account to the identity (user or API key) that you authenticate with.

  This read-only, auditor-style access is the same level used to audit an account. It covers everything that the tool inspects, including:
  - {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} instances and keys
  - Cloud services that can be encrypted by those keys (for example, Cloud Object Storage, Databases for PostgreSQL and the other {{site.data.keyword.Bluemix_notm}} Databases, Secrets Manager, Event Streams, App ID, Event Notifications, App Configuration, Schematics, Kubernetes clusters, and VPC block volumes, file shares and custom images)

  Some services are inspected through their own API rather than through the platform's resource list (Cloud Object Storage buckets, Event Notifications and App Configuration integrations, Kubernetes clusters, Schematics workspaces). Without the Reader role on such a service, its resources are not attributed to a key. Attributing Schematics workspaces to a specific key also requires access to the account's Schematics KMS settings; without it, workspaces are reported at the KMS instance level under [Unknowns](#kur-unknowns).

  KUR performs read operations only and does not create, modify, or delete any resources.


The access requirements in this section apply to the account scan. The `process-at` subcommand works entirely on a local activity tracking file and requires no {{site.data.keyword.Bluemix_notm}} access.
{: note}

## Running the tool
{: #kur-running}

The following examples show how to run the Key Usage Reporter tool with different options and configurations.

### Basic usage: scan for HPCS keys (default)
{: #kur-basic-usage}

Use the following command to scan the currently targeted {{site.data.keyword.Bluemix_notm}} account for all `hs-crypto` instances, their keys, and any cloud resources encrypted by those keys.

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

You can filter the scan to include only {{site.data.keyword.keymanagementserviceshort}} Standard (multi-tenant) instances.

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
| `--output`             | Auto-named | Output file path. Defaults to `encryption-key-usage-report-<service>[-<service-type>]-<account-name>.json`. The `<service-type>` segment is included only when `--service-type` is specified (for example, `encryption-key-usage-report-kms-dedicated-my-account.json`). |
{: caption="CLI flags for the Key Usage Reporter tool" caption-side="bottom"}

Flags can use single dash (`-flag`) or double dash (`--flag`).
{: note}

## Environment variables
{: #kur-env-vars}

| Variable | Default | Description |
|----------|---------|-------------|
| `IBMCLOUD_API_KEY` | (None) | {{site.data.keyword.Bluemix_notm}} IAM API key. When set, the tool runs `ibmcloud login` with it before doing anything else, replacing any existing CLI session. The key is read by the CLI from the environment and never appears on a command line. |
| `IBMCLOUD_REGION` | `us-south` | Region targeted by that login. Only used together with `IBMCLOUD_API_KEY`. The VPC scan visits every region regardless of this value. |
{: caption="Environment variables" caption-side="bottom"}

```sh
IBMCLOUD_API_KEY=<api-key> ./<kur-binary> --service kms
```
{: pre}

The {{site.data.keyword.Bluemix_notm}} CLI and the `vpc-infrastructure` plugin are still required when you use the API key.
{: note}

## How usage is detected
{: #kur-how-detected}

The tool lists every service instance in the account, the Kubernetes and OpenShift clusters, and, in every VPC region, the block volumes, file shares, custom images, snapshots, and backup policies. It then looks at each resource for a reference to a Key Protect or HPCS key:

* For most services, the platform's resource record carries the key CRN the resource was provisioned with. {{site.data.keyword.Bluemix_notm}} Databases, Secrets Manager, Event Streams, App ID, Cloudant, and the VPC resources are attributed this way.
* Some services do not expose the key in their record, so the tool asks the service itself: Cloud Object Storage (bucket encryption settings), Event Notifications and App Configuration (their KMS integrations), Kubernetes and OpenShift (cluster details), and Schematics (workspace encryption together with the account's Schematics KMS settings).

In the report, each resource is listed once per key under a label of the form `<service> (<display name>)`, for example `cloud-object-storage (Cloud Object Storage)`. VPC resources are labelled by resource type: `volume (VPC Block Storage Volume)`, `share (VPC File Share)`, `image (VPC Image)`.

## Output files
{: #kur-output-files}

The tool produces two output files:

JSON report
:   The main output file (for example, `encryption-key-usage-report-kms-kp-stage.json`), containing the full hierarchical report of KMS instances, keys, and resource usages.

Log file
:   A companion log file with the same base name and a `-log.txt` suffix (for example, `encryption-key-usage-report-kms-kp-stage-log.txt`), containing all log messages from the run.

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

Metadata includes execution context such as the tool version, target KMS service, {{site.data.keyword.Bluemix_notm}} API endpoint, account name, account ID, and the user who ran the scan.

### KMS instances
{: #kur-kms-instances}

One entry per KMS or HPCS instance found in the account. Instances with detected key usage are listed first, then instances with no detected usage. Each instance contains:

Instance metadata
:   Name, CRN, state, allowed network, public and private endpoints, type (for Key Protect: `multi-tenant` or `dedicated`).

`found_by_kms_instance_listing`
:   `true` if the instance was found by listing KMS instances in the account.

`found_by_resource_scan`
:   `true` if the resource scan found at least one resource encrypted by this instance, either through one of its keys or through a reference to the instance alone (see [Unknowns](#kur-unknowns)).

`instance_stats`
:   Key counts by state:
   * `active_crk_count`, `suspended_crk_count`, `deactivated_crk_count`, `destroyed_crk_count`
   * `active_standard_key_count`, `destroyed_standard_key_count`.

`keys[]`
:   Full key inventory from the Key Protect API. Each key includes:
   * `type`: `crk` (Customer Root Key) or `standard_key`.
   * `state_name`: `pre-activation`, `active`, `suspended`, `deactivated`, or `destroyed`.
   * `name`: key name.
   * `id`: key UUID.
   * `has_migration_intent`: whether the key has a migration intent set.
   * `migration_intent_target_crk`: target CRK CRN (only present when `has_migration_intent` is `true`).
   * `found_by_kms_key_listing` or `found_by_resource_scan`: how the key was discovered.
   * `associations[]`: cloud resources registered against the key (from the Key Protect registrations API). Each entry shows the `resource_crn` and whether it has `prevent_key_deletion` enabled. Omitted when a key has no registrations.
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

The subcommand takes a TSV file that is exported from the {{site.data.keyword.logs_full_notm}} Logs activity tracking event routing archive query, extracts the JSON events from the `text` column, and filters for KMS and HPCS-related events (`kms.*` and `hs-crypto.*` actions). It then produces four output files:

`<base>_events.json`
:   All events extracted as a formatted JSON array.

`<base>_events.csv`
:   Flat CSV with one row per event, containing: serviceName, region, accountId, instanceId, keyId, action, outcome, reasonType, reasonCode, initiatorId, initiatorName, authId, requestInstanceId, eventTime, correlationId, agent.

`<base>_events_summary.csv`
:   Grouped summary with event counts, grouped by service, region, account, instance, key, action, outcome, reason, and initiator.

`<base>_events_summary_by_action.csv`
:   Grouped summary with event counts, grouped by service, region, account, instance, key, action, and initiator (without outcome or reason breakdown).

Where `<base>` is derived from the input file name (stripping `_logs.tsv` or `.tsv`).

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

## Troubleshooting
{: #kur-troubleshooting}

The following information helps you to resolve common issues when running the Key Usage Reporter tool.

### {{site.data.keyword.Bluemix_notm}} CLI not installed
{: #kur-ts-cli-not-installed}

```text
{{site.data.keyword.Bluemix_notm}} CLI is not installed. Please install it first.
Visit: https://cloud.ibm.com/docs/cli?topic=cli-getting-started
```
{: screen}

Install the {{site.data.keyword.Bluemix_notm}} CLI by following the [Getting started with the {{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cli-getting-started) documentation.

### Missing CLI plugins
{: #kur-ts-missing-plugins}

If required CLI plugins are not installed, you see an error message that lists the missing plugins.

```text
missing required {{site.data.keyword.Bluemix_notm}} CLI plugins: [vpc-infrastructure]
```
{: screen}

Install the missing plugin:

```sh
ibmcloud plugin install vpc-infrastructure
```
{: pre}

### Outdated CLI plugins
{: #kur-ts-outdated-plugins}

If your CLI plugins are outdated, you see a warning message that lists which plugins need to be updated.

```text
the following {{site.data.keyword.Bluemix_notm}} CLI plugins are outdated: [vpc-infrastructure]
```
{: screen}

Update the plugin:

```sh
ibmcloud plugin update vpc-infrastructure
```
{: pre}

### Not logged in
{: #kur-ts-not-logged-in}

If you are not logged in to {{site.data.keyword.Bluemix_notm}}, the tool displays an error message.

```text
not logged in to {{site.data.keyword.Bluemix_notm}}. Please login first
```
{: screen}

Log in to {{site.data.keyword.Bluemix_notm}}:

```sh
ibmcloud login
```
{: pre}

### API key login failed
{: #kur-ts-api-key-login}

```text
login with IBMCLOUD_API_KEY failed: command failed: ibmcloud login -r us-south -q: exit status 1
```
{: screen}

`IBMCLOUD_API_KEY` is set but the {{site.data.keyword.Bluemix_notm}} CLI could not log in with it. The CLI's own message follows on the next lines and usually names the cause: a key that was deleted or mistyped, a locked user, or a region that does not exist. Fix the key or unset the variable to fall back to your existing CLI session.

### Token expired or about to expire
{: #kur-ts-token-expired}

The tool requires a valid IAM token with at least 3 minutes of remaining validity.

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

### Private-only instances
{: #kur-ts-private-only}

If a KMS instance allows only private network access and you are not connected to the {{site.data.keyword.Bluemix_notm}} private network, the tool cannot fetch stats or keys for that instance. Use `--skip-private-calls` to skip these instances rather than having the tool fail on them:

```sh
./<kur-binary> --service kms --skip-private-calls
```
{: pre}

### 100+ KMS instances
{: #kur-ts-100-instances}

The {{site.data.keyword.Bluemix_notm}} resource listing API has a limit on the number of instances that can be returned.

```text
[WARNING] 100 or more KMS instances, only the first 100 instances will be processed.
```
{: screen}

The {{site.data.keyword.Bluemix_notm}} resource listing API returns a maximum of 100 instances. If the account has more than 100 KMS instances, only the first 100 are included in the report. This behavior is a known limitation.

### Invalid service-type with hs-crypto
{: #kur-ts-invalid-service-type}

The `--service-type` flag is valid only when scanning Key Protect instances.

```text
error: --service-type can only be used with --service kms
```
{: screen}

The `--service-type` flag (to filter by `dedicated` or `multi-tenant`) applies only to Key Protect (`--service kms`). It is not applicable to HPCS instances.
