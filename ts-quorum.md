---

copyright:
  years: 2026

lastupdated: "2026-10-06"

keywords: quorum troubleshooting, threshold not configured, quorum error, signature threshold, revocation threshold, multi-admin, crypto unit quorum

subcollection: key-protect

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting quorum authorization issues
{: #troubleshooting-quorum}

Review solutions to common errors you might encounter when working with quorum authorization on a Dedicated {{site.data.keyword.keymanagementserviceshort}} instance.
{: shortdesc}

For background on how quorum authorization works, see [Quorum authorization](/docs/key-protect?topic=key-protect-quorum-authorization).

## Why do I get a `threshold not configured` error?
{: #ts-quorum-threshold-not-configured}
{: troubleshoot}

**`Cannot perform action, threshold not configured` error**

When you run a sensitive `crypto-unit` command such as `user add`, `user remove`, or `master-key import`, you receive the following error:

```screen
FAILED
Cannot perform action, threshold not configured. Run `ibmcloud kp crypto-unit threshold set` before retrying. Note: the crypto unit will restart when the threshold configuration is applied.
```
{: screen}

### Cause
{: #ts-quorum-threshold-not-configured-cause}

The crypto unit requires a threshold to be explicitly configured before quorum-enforced operations can proceed. This applies even when only one admin is present, because the threshold value of `1` must be explicitly set.

### Resolution
{: #ts-quorum-threshold-not-configured-resolution}

1. Check the current threshold status:

   ```sh
   ibmcloud kp crypto-unit threshold get
   ```
   {: pre}

   If the output shows `not configured`, proceed to the next step.

2. Set the signature and revocation thresholds. The value must be between `1` and `5` and cannot exceed the number of admin users on the crypto unit.

   For [macOS]{: tag-macos}:

   ```sh
   ibmcloud kp crypto-unit threshold set --signature-threshold <value> --revocation-threshold <value> --auth '[{"<admin_username>": "<admin_key_file>#<password>"}]'
   ```
   {: pre}

   For [Windows]{: tag-windows} PowerShell:

   ```powershell
   ibmcloud kp crypto-unit threshold set --signature-threshold <value> --revocation-threshold <value> --auth '[{"""<admin_username>""": """<admin_key_file>#<password>"""}]'
   ```
   {: codeblock}

   For [Windows]{: tag-windows} CMD:

   ```sh
   ibmcloud kp crypto-unit threshold set --signature-threshold <value> --revocation-threshold <value> --auth "[{\"<admin_username>\": \"<admin_key_file>#<password>\"}]"
   ```
   {: codeblock}

   The crypto unit restarts when the threshold configuration is applied. Wait for the restart to complete before retrying.
   {: important}

3. Confirm the threshold was applied:

   ```sh
   ibmcloud kp crypto-unit threshold get
   ```
   {: pre}

4. Retry the original operation.

## Why does my quorum-enforced command fail even though I provided multiple admin credentials?
{: #ts-quorum-insufficient-signatures}
{: troubleshoot}

**Quorum command rejected despite providing multiple `--auth` entries**

You set a signature threshold greater than `1` and provided multiple admin credentials in the `--auth` JSON array, but the command still fails.

### Cause
{: #ts-quorum-insufficient-signatures-cause}

Possible causes include:

- The number of credentials in the `--auth` JSON array is less than the configured signature threshold.
- One or more of the credential entries contains a typo or the wrong passphrase, causing authentication to fail for that entry.
- The credentials provided belong to the same admin user (duplicates do not count as separate signatures).
- The total number of admin users on the crypto unit is less than the threshold value. This should be prevented at configuration time, but can occur if admin users were removed after the threshold was set.

### Resolution
{: #ts-quorum-insufficient-signatures-resolution}

1. Check the current threshold and admin count:

   ```sh
   ibmcloud kp crypto-unit threshold get
   ibmcloud kp crypto-unit users
   ```
   {: pre}

2. Ensure that the `--auth` JSON array contains one entry per required admin, each with a distinct username and a valid credential path and passphrase. For example, to satisfy a threshold of `2`:

   ```sh
   ibmcloud kp crypto-unit user add --type admin --name <new_user> --credential <new_user_key_file> \
     --auth '[{"ADMIN1": "<admin1_key_file>#<password1>", "ADMIN2": "<admin2_key_file>#<password2>"}]'
   ```
   {: pre}

3. If you cannot assemble the required number of distinct admin credentials (for example, a credential was lost), contact [{{site.data.keyword.keymanagementserviceshort}} support](/docs/key-protect?topic=key-protect-getting-help).

## Why does the `threshold set` command fail with an error about admin count?
{: #ts-quorum-threshold-exceeds-admin-count}
{: troubleshoot}

**Threshold value rejected because it exceeds the number of admin users**

When you run `ibmcloud kp crypto-unit threshold set`, you receive an error indicating the threshold value is too high.

### Cause
{: #ts-quorum-threshold-exceeds-admin-count-cause}

Both `--signature-threshold` and `--revocation-threshold` must be less than or equal to the total number of admin users currently configured on the crypto unit. If you try to set a threshold of `3` but only `2` admins exist, the command fails.

### Resolution
{: #ts-quorum-threshold-exceeds-admin-count-resolution}

1. Check the current admin users:

   ```sh
   ibmcloud kp crypto-unit users
   ```
   {: pre}

2. Either reduce the requested threshold to match the number of existing admins, or add additional admin users first, then retry the threshold command.

   To add an admin user:

   ```sh
   ibmcloud kp crypto-unit user add --type admin --name <new_admin> --credential <new_admin_key_file> --auth '[{"ADMIN": "<admin_key_file>#<password>"}]'
   ```
   {: pre}

## Why are the threshold values different across my crypto units?
{: #ts-quorum-mismatched-thresholds}
{: troubleshoot}

**`threshold get` returns different values for different crypto units**

When you run `ibmcloud kp crypto-unit threshold get`, the command returns different threshold values for the crypto units in your instance.

### Cause
{: #ts-quorum-mismatched-thresholds-cause}

The `threshold set` command failed to apply to all crypto units. By default, `threshold set` targets all crypto units in the instance, but if one crypto unit was unavailable or in `maintenance` state at the time the command ran, it may not have been updated.

### Resolution
{: #ts-quorum-mismatched-thresholds-resolution}

1. Identify which crypto units have the incorrect threshold:

   ```sh
   ibmcloud kp crypto-unit threshold get
   ```
   {: pre}

2. Re-run `threshold set` targeting only the crypto unit with the incorrect value. Use either `--auth` with `--id`, or `--cu` with the crypto unit ID and credentials bundled:

   ```sh
   ibmcloud kp crypto-unit threshold set --signature-threshold <value> --revocation-threshold <value> \
     --auth '[{"<admin_username>": "<admin_key_file>#<password>"}]' --id <crypto_unit_id>
   ```
   {: pre}

   Or using `--cu`:

   ```sh
   ibmcloud kp crypto-unit threshold set --signature-threshold <value> --revocation-threshold <value> \
     --cu '[{"CryptoUnitId": "<crypto_unit_id>", "Auth": [{"<admin_username>": "<admin_key_file>#<password>"}]}]'
   ```
   {: pre}

   Replace `<crypto_unit_id>` with the ID reported by `ibmcloud kp crypto-units`.

3. Verify all crypto units now report the same threshold:

   ```sh
   ibmcloud kp crypto-unit threshold get
   ```
   {: pre}

   All crypto units must have identical threshold values for consistent behavior.

## Why does the crypto unit restart after I set a threshold?
{: #ts-quorum-restart-after-set}
{: troubleshoot}

**Crypto unit restarts unexpectedly after `threshold set`**

After running `ibmcloud kp crypto-unit threshold set`, you notice the crypto unit restarted.

### Cause
{: #ts-quorum-restart-after-set-cause}

This is expected behavior. The crypto unit always restarts when threshold configuration is applied. This is a hardware-level enforcement mechanism and is not an error.

### Resolution
{: #ts-quorum-restart-after-set-resolution}

No action is required. Wait for the crypto unit to return to `kms-initialized` state before retrying operations. To check the state of your crypto units, run:

```sh
ibmcloud kp crypto-units
```
{: pre}

It might take a few minutes for the crypto unit to return to the `kms-initialized` state after restarting. For more information about crypto unit states, see [Crypto unit states](/docs/key-protect?topic=key-protect-crypto-unit-states).
