---

copyright:
  years: 2017, 2026

lastupdated: "2026-08-11"

keywords: getting started, key management, encryption keys, create keys, manage keys, Dedicated Key Protect, single-tenant, KYOK, API, Terraform, dedicated, single-tenant-initialize

subcollection: key-protect

content-type: howto

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting initialization issues for Dedicated {{site.data.keyword.keymanagementserviceshort}}
{: #troubleshooting-init}

Review solutions to common errors you might encounter while initializing a Dedicated {{site.data.keyword.keymanagementservicelong_notm}} instance.
{: shortdesc}

## Why do I get an `Unable to obtain plug-in's metadata` error?
{: #unable-get-metadata-error}
{: troubleshoot}

**`Unable to obtain plug-in's metadata` error during KP CLI plugin install or upgrade**

If you receive the following error when you install the {{site.data.keyword.keymanagementservicelong_notm}} CLI plugin:

```screen
Installing binary...
FAILED
Unable to obtain plug-in's metadata. Error: exit status 1
```
{: screen}

### Linux environment
{: #linux-error}

Install or update the `libstdc++` system library with GLIBCXX version 3.4.26 or later from your distribution's package manager. Use the following example installation commands:
- Ubuntu/Debian: `apt-get update && apt-get install libstdc++6`
- RHEL/Fedora/CentOS: `yum install libstdc++`
- Alpine: `apk add --no-cache gcompat libstdc++`

If this does not resolve the error, contact {{site.data.keyword.keymanagementserviceshort}} support.

### Windows or macOS environment
{: #windows-macos-error}

Contact {{site.data.keyword.keymanagementserviceshort}} support.

## Why do I get a `command failed with error code: e00bad05` error?
{: #command-failed-with-error-code-e00bad05-error}
{: troubleshoot}

**`command failed with error code: e00bad05` error**

If an `ibmcloud kp crypto-unit` command returns the following error:

```screen
FAILED
command failed with error code: e00bad05
```
{: screen}

This error might indicate that your system is not compatible with the `ibmcloud kp crypto-unit` feature. The recommended system requirements are:
- Windows: AMD64 (Windows 10 or later)
- Linux: AMD64 (Debian, Ubuntu, Red Hat)
- macOS: ARM64 (Apple Silicon)

Systems outside this list might still be compatible with the `ibmcloud kp crypto-unit` feature. If you want to confirm compatibility with your specific system, or if the `e00bad05` error persists despite meeting the recommended system requirements, contact {{site.data.keyword.keymanagementserviceshort}} support.

## Why do I get an HTTP 503 `no healthy upstream` error?
{: #no-healthy-upstream-error}
{: troubleshoot}

**HTTP 503 `no healthy upstream` error**

If calls to {{site.data.keyword.keymanagementserviceshort}} [operations](/apidocs/key-protect) return HTTP 503 with the message `no healthy upstream: no crypto units are in kms-initialized state at this time`, the following causes are possible:

- You have not yet completed the Dedicated initialization steps.
- You completed the Dedicated initialization steps, but need to wait a few minutes for {{site.data.keyword.keymanagementserviceshort}} to recognize the newly `kms-initialized` crypto units.
- You have only one crypto unit in `kms-initialized` state, and that crypto unit is down for maintenance.
- You uploaded mismatched master key material to one or more crypto units.

## Why do I get a `context deadline exceeded` error?
{: #context-deadline-exceeded-error}
{: troubleshoot}

If CLI commands return the error `context deadline exceeded (Client.Timeout exceeded while awaiting headers)`, you set `KP_TARGET_ADDR` to a private endpoint from a system that does not meet private endpoint requirements.

To resolve this error:

- Use the public endpoint from the [Getting the endpoint and instance ID](/docs/key-protect?topic=key-protect-st-init-cli#getting-started-get-endpoint) step.
- If you intend to use the private endpoint, see [Private endpoints](/docs/key-protect?topic=key-protect-regions#connectivity-options-private) for information about making calls to the private endpoint.

## Why do crypto unit commands fail to apply to all crypto units?
{: #crypto-unit-partial-failure}
{: troubleshoot}

If the `crypto-unit claim`, `crypto-unit master-key import`, or `crypto-unit user add --type kmsCryptoUser` commands fail to apply to all crypto units, you might see output similar to the following example:

```screen
Executing operation Generate Master Key against CryptoUnit with ID fadedbee-0000-0000-0000-1234567890ab
OK
Executing operation Generate Master Key against CryptoUnit with ID addedace-0000-0000-0000-1234567890ab
FAILED
```
{: screen}

To resolve this issue:
1. By default, the `claim`, `master-key import`, and `user add` commands attempt to apply to all crypto units. If these commands are only partially successful (applied to only a subset of the crypto units in the instance), retry the command only against the crypto units that returned a failure. Each of these commands can be configured to target specific crypto units. To determine how to target specific crypto units, append `-h` to any `crypto-unit` command to view the help text, or see the [CLI reference](/docs/key-protect?topic=key-protect-key-protect-cli-reference).

2. Run the `kp crypto-units` command in the [CLI reference](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-crypto-units) to confirm that all crypto units are in the same state.
   - If crypto unit states are mismatched, see [Crypto unit states](/docs/key-protect?topic=key-protect-crypto-unit-states).
   - If any crypto unit is in `maintenance` state, retry the `kp crypto-unit` commands at a later time.
