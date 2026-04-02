---

copyright:
  years: 2026

lastupdated: "2026-04-02"

keywords: key usage reporter, KUR, encryption report, key scan, activity tracking, audit logs

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Using the Key Usage Reporter (KUR) tool
{: #kur}

The Key Usage Reporter (KUR) CLI scans an IBM Cloud account and produces a comprehensive report of which cloud resources are encrypted by which KMS keys. The tool supports both {{site.data.keyword.keymanagementserviceshort}} (`kms`) and {{site.data.keyword.hscrypto}} (`hs-crypto`). The tool is also capable of processing activity tracking audit log files, producing CSV summaries that help identify KMS utilization.
{: shortdesc}

KUR is provided as-is and on a best-effort basis. The tool does not detect all possible usages of keys, and the results should not be treated as authoritative. Some services, configurations, or edge cases might not be covered.
{: important}


## Downloading the tool
{: #kur-download}

1. Create an [IBM Support ticket](https://www.ibm.com/mysupport/s/?language=en_US) for Key Protect to request access to the HPCS to Key Protect migration tooling.

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

* **IBM Cloud CLI** (`ibmcloud`) is installed by following the instructions in [Getting started with the IBM Cloud CLI](/docs/cli?topic=cli-getting-started).
* The following **IBM Cloud CLI plugins** are installed and up-to-date:
   * `container-service`
   * `vpc-infrastructure`
   * `event-notifications`

   Install any missing plugin with:
   ```sh
   ibmcloud plugin install <plugin-name>
   ```
   {: pre}

* You are **logged in** to the IBM Cloud CLI and targeting the account that you want to scan:
   ```sh
   ibmcloud login
   ```
   {: pre}

* Your **IAM token is valid** and has at least 3 minutes of remaining validity. If in doubt, refresh it:
   ```sh
   ibmcloud login
   ```
   {: pre}

* Your user or API key must have **read access** to the KMS instances, keys, and cloud resources in the account.


## Running the tool
{: #kur-running}

The following examples show how to run the Key Usage Reporter tool with different options and configurations.

### Basic usage: scan for HPCS keys (default)
{: #kur-basic-usage}

Use the following command to scan the currently targeted IBM Cloud account for all `hs-crypto` instances, their keys, and any cloud resources encrypted by those keys.

```sh
./<kur-binary> 
```
{: pre}

### Scan for Key Protect keys
{: #kur-scan-kp}

To scan for Key Protect instances instead of HPCS, use the `-service kms` flag.

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

You can filter the scan to include only Key Protect Standard (mult-tenant) instances.

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
| `--output`             | Auto-named | Output file path. Defaults to `encryption-key-usage-report-<service>-<account-name>.json` |
{: caption="Table 1. CLI flags for the Key Usage Reporter tool" caption-side="bottom"}

Flags can use single dash (`-flag`) or double dash (`--flag`).
{: note}

## Output files
{: #kur-output-files}

The tool produces two output files:

JSON report
:   The main output file (e.g., `encryption-key-usage-report-kms-kp-stage.json`), containing the full hierarchical report of KMS instances, keys, and resource usages.

Log file
:   A companion log file with the same base name and a `-log.txt` suffix (e.g., `encryption-key-usage-report-kms-kp-stage-log.txt`), containing all log messages from the run.

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

One entry per KMS or HPCS instance found in the account. Instances with detected key usage are listed first, then instances with no detected usage. Each instance contains:

Instance metadata
:   Name, CRN, state, allowed network, public and private endpoints, type (for Key Protect: `multi-tenant` or `dedicated`)

`found_by_kms_instance_listing`
:   `true` if the instance was found by listing KMS instances in the account.

`found_by_resource_scan`
:   `true` if the instance with keys detected during the resource scan.

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
   * `service_usage`: map of service name to encrypted resources detected by the account-wide resource scan. Only present for keys found by the resource scan.

### CRNs
{: #kur-crns}

Resources that reference encryption identifiers that match a CRN pattern but not a KMS or HPCS key CRN are captured here to ensure that nothing is silently dropped.

### Unknowns
{: #kur-unknowns}

Resources that reference encryption identifiers that could not be parsed as CRNs at all are listed here.

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

Takes a TSV file that is exported from the IBM Cloud Logs activity tracking event routing archive query, extracts the JSON events from the `text` column, and filters for KMS and HPCS-related events (`kms.*` and `hs-crypto.*` actions). It then produces four output files:

`<base>_events.json`
:   All events extracted as a formatted JSON array.

`<base>_events.csv`
:   Flat CSV with one row per event, containing: serviceName, region, accountId, instanceId, keyId, action, outcome, reasonType, reasonCode, initiatorId, initiatorName, authId, requestInstanceId, eventTime, correlationId, agent.

`<base>_events_summary.csv`
:   Grouped summary with event counts, which are grouped by service, region, account, instance, key, action, outcome, reason, and initiator.

`<base>_events_summary_by_action.csv`
:   Grouped summary with event counts, which are grouped by service, region, account, instance, key, action, and initiator (without outcome or reason breakdown).

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

### IBM Cloud CLI not installed
{: #kur-ts-cli-not-installed}

```text
IBM Cloud CLI is not installed. Please install it first.
Visit: https://cloud.ibm.com/docs/cli?topic=cli-getting-started
```
{: screen}

Install the IBM Cloud CLI by following the [Getting started with the IBM Cloud CLI](/docs/cli?topic=cli-getting-started) documentation.

### Missing CLI plugins
{: #kur-ts-missing-plugins}

If required CLI plugins are not installed, you see an error message that lists the missing plugins.

```text
missing required IBM Cloud CLI plugins: [container-service vpc-infrastructure]
```
{: screen}

Install the missing plugins:

```sh
ibmcloud plugin install container-service
ibmcloud plugin install vpc-infrastructure
ibmcloud plugin install event-notifications
```
{: pre}

### Outdated CLI plugins
{: #kur-ts-outdated-plugins}

If your CLI plugins are outdated, you see a warning message that lists which plugins need to be updated.

```text
the following IBM Cloud CLI plugins are outdated: [container-service]
```
{: screen}

Update the plugin:

```sh
ibmcloud plugin update container-service
```
{: pre}

### Not logged in
{: #kur-ts-not-logged-in}

If you're not logged in to IBM Cloud, the tool displays an error message.

```text
not logged in to IBM Cloud. Please login first
```
{: screen}

Log in to IBM Cloud:

```sh
ibmcloud login
```
{: pre}

### Token expired or about to expire
{: #kur-ts-token-expired}

The tool requires a valid IAM token with at least 3 minutes of remaining validity.

If your IAM token has less than 3 minutes of remaining validity, the tool rejects it. Refresh your session:

```sh
ibmcloud login
```
{: pre}

### Private-only instances
{: #kur-ts-private-only}

Some KMS instances might be configured to allow only private network access. If a KMS instance allows only private network access and you are not connected to the IBM Cloud Private network, the tool cannot fetch stats or keys for that instance. Use `--skip-private-calls` to skip these instances rather than having the tool fail on them:

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
