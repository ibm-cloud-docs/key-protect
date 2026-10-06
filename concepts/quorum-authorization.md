---

copyright:
  years: 2026

lastupdated: "2026-10-06"

keywords: quorum authorization, signature threshold, revocation threshold, multi-admin approval, crypto unit quorum, dedicated key protect quorum

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Quorum authorization for Dedicated {{site.data.keyword.keymanagementserviceshort}}
{: #quorum-authorization}

Quorum authorization is a security control available on Dedicated {{site.data.keyword.keymanagementserviceshort}} instances that requires multiple administrators to jointly approve sensitive crypto unit operations. By requiring more than one admin signature, no single admin can unilaterally alter the configuration of a crypto unit.
{: shortdesc}

## How quorum authorization works
{: #quorum-how-it-works}

Every Dedicated {{site.data.keyword.keymanagementserviceshort}} crypto unit maintains two configurable thresholds:

| Threshold | Controls | Default |
|-----------|----------|---------|
| **Signature threshold** | Number of admin signatures required to authorize sensitive operations, such as adding or removing a user or importing master key material. | `1` (single admin) |
| **Revocation threshold** | Number of admin signatures required to revoke (remove) an admin user. | `1` (single admin) |
{: caption="Table 1. Quorum threshold types" caption-side="bottom"}

When a threshold is set to a value greater than `1`, the crypto unit enforces quorum authorization. An operation that requires the signature threshold is rejected unless the required number of distinct admin credentials are presented together in the same command invocation. For example, with a signature threshold of `2`, the `--auth` JSON array must include credentials for at least two admins.

The threshold values must not exceed the total number of admin users currently configured on the crypto unit. Both thresholds can be set to any integer between `1` and `5`.

## Which operations are quorum-enforced
{: #quorum-enforced-operations}

When the signature threshold is greater than `1`, the following operations require multiple admin signatures:

- Adding an admin user: `ibmcloud kp crypto-unit user add --type admin`
- Removing an admin user: `ibmcloud kp crypto-unit user remove`
- Importing master key material: `ibmcloud kp crypto-unit master-key import`

When the revocation threshold is greater than `1`, removing an admin user also requires the additional revocation signatures.

Setting thresholds (`ibmcloud kp crypto-unit threshold set`) requires the current admin credentials to authenticate; it is not subject to the threshold it is configuring.

## Configuring quorum authorization
{: #quorum-configure}

You configure quorum by setting the signature and revocation thresholds on a crypto unit. Thresholds can be set during initialization or at any later point, but the crypto unit must be in the `claimed` state to change them. See [Setting a signature and revocation threshold](/docs/key-protect?topic=key-protect-provision-ded-instance#getting-started-set-threshold) for the full setup procedure.

To check the current threshold values at any time, run:

```sh
ibmcloud kp crypto-unit threshold get
```
{: pre}

If no thresholds have been configured, the command returns `not configured` and both thresholds default to `1`.

## Monitoring quorum activity
{: #quorum-monitoring}

{{site.data.keyword.keymanagementserviceshort}} emits IBM Cloud Activity Tracker events when quorum-related operations are performed on crypto units. Use these events to audit who configured threshold values and when.

| Activity Tracker event | Description |
|------------------------|-------------|
| `kms.crypto-unit-threshold-config.generate` | Threshold configuration was generated (set) for a crypto unit |
{: caption="Table 2. Quorum audit events" caption-side="bottom"}

For a complete list of Activity Tracker events, see [Activity Tracker events](/docs/key-protect?topic=key-protect-at-events#quorum-actions).

## Key considerations
{: #quorum-considerations}

- Thresholds apply uniformly across all crypto units in an instance. All crypto units must be configured identically for consistent behavior.
- The threshold value can never exceed the number of admin users on the crypto unit. If you plan to set a threshold of `2`, add at least two admin users first.
- The crypto unit **restarts** when threshold configuration is applied. Plan for a brief period of unavailability on the affected crypto unit.
- To change a threshold after initialization, the crypto unit must be returned to the `claimed` state, which means re-initializing. Plan your threshold configuration before completing initialization.
- Threshold configuration requires the credentials of an existing admin for authentication.

## Best practices
{: #quorum-best-practices}

- **Use quorum for high-value instances**: Set a signature threshold of at least `2` whenever the crypto units protect production keys or sensitive workloads.
- **Distribute key material**: Ensure each admin holds their own credentials securely and independently so that quorum cannot be circumvented by a single person.
- **Align thresholds to your admin count**: Do not set a threshold higher than your available admins minus one, so that you always retain the ability to reach quorum in the event a credential is unavailable.
- **Verify thresholds on all crypto units**: After setting thresholds, run `ibmcloud kp crypto-unit threshold get` to confirm that the same values are reported for every crypto unit in the instance.
