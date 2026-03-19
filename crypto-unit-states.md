---

copyright:
  years: 2026
lastupdated: "2026-03-19"

keywords: crypto unit, crypto unit states, reserved, claimed, initialized, kms-authorized, kms-initialized, maintenance

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Crypto unit states
{: #crypto-unit-states}

When you provision a {{site.data.keyword.keymanagementserviceshort}} instance, the crypto units that are associated with your instance go through a series of states before they are ready to use. Understanding these states helps you track the initialization progress and troubleshoot any issues.
{: shortdesc}

## Checking crypto unit state
{: #check-crypto-unit-state}

To check the state of your crypto units, run the [`kp crypto-units`](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-crypto-units) command.

## Crypto unit state definitions
{: #crypto-unit-state-definitions}

The following table describes the different states that a crypto unit can be in and the recommended next steps for each state.

| State | Description | Recommended next step |
|-------|-------------|----------------------|
| `reserved` | The crypto unit is uninitialized. This is the initial state when you first provision an instance. The crypto unit must be fully initialized to enable {{site.data.keyword.keymanagementserviceshort}} operations. | Follow the [initialization documentation](/docs/key-protect?topic=key-protect-st-init-cli) in its entirety. |
| `claimed` | The crypto unit has been [claimed](/docs/key-protect?topic=key-protect-st-init-cli#st-init-cli-generate-admin). The crypto unit is not yet fully initialized to support {{site.data.keyword.keymanagementserviceshort}} operations. | [Upload the master key](/docs/key-protect?topic=key-protect-st-init-cli#st-init-cli-crypto-units-master-key) and permit the `kmsCryptoUser` identity. |
| `initialized` | The master key has been [uploaded](/docs/key-protect?topic=key-protect-st-init-cli#st-init-cli-crypto-units-master-key) to the crypto unit. The crypto unit is not yet fully initialized to support {{site.data.keyword.keymanagementserviceshort}} operations. | Permit the `kmsCryptoUser` identity to complete initialization. |
| `kms-authorized` | The `kmsCryptoUser` identity has been permitted to the crypto unit. The crypto unit is not yet fully initialized to support {{site.data.keyword.keymanagementserviceshort}} operations. | [Upload the master key](/docs/key-protect?topic=key-protect-st-init-cli#st-init-cli-crypto-units-master-key) to complete initialization. |
| `kms-initialized` | The crypto unit is fully initialized and ready for use with {{site.data.keyword.keymanagementserviceshort}} operations. | No action required. Note that it might take a few minutes after reaching this state for operations to become available. |
| `maintenance` | The {{site.data.keyword.keymanagementserviceshort}} service team is performing maintenance, upgrades, or security patches on the hardware backing the crypto unit. A crypto unit in maintenance state is not used for operations. | No action required. Wait for maintenance to complete. During this period, instances under high request load might experience minor performance degradation. |
{: caption="Table 1. Crypto unit states" caption-side="bottom"}

## Understanding the kmsCryptoUser identity
{: #kms-crypto-user}

The `kmsCryptoUser` identity is used by your {{site.data.keyword.keymanagementserviceshort}} instance to perform operations. Each instance has a unique `kmsCryptoUser` identity.

## Maintenance state considerations
{: #maintenance-considerations}

When a crypto unit enters the maintenance state, keep the following considerations in mind:

* No more than one crypto unit of an instance is placed in maintenance state at any given time.
* If your instance has only one crypto unit in `kms-initialized` state and it moves into maintenance state, {{site.data.keyword.keymanagementserviceshort}} operations are not available.
* Maintenance on crypto unit hardware proceeds regardless of whether a crypto unit is the only `kms-initialized` crypto unit in the instance.
* You cannot control or prevent crypto unit hardware maintenance.

## Best practices
{: #crypto-unit-best-practices}

Follow these best practices to ensure optimal performance and availability:

* You must have at least one crypto unit in `kms-initialized` state for {{site.data.keyword.keymanagementserviceshort}} operations to be available.
* Initialize all crypto units in your instance to `kms-initialized` state for maximum performance and resiliency.
* Provision at least two crypto units for your instance to maintain availability during maintenance windows. The minimum number of crypto units you can provision is two.
* If you expect your instance to experience heavy request loads, consider provisioning a three crypto unit instance. Remember that the number of crypto units assigned to an instance cannot be changed after provisioning.

## Master key requirements
{: #master-key-requirements}

Master keys must be of the same material for proper operation. Initializing crypto units with mismatched master keys causes the service to return `HTTP 500 Internal Server Error` or `HTTP 503 no healthy upstream` errors.
