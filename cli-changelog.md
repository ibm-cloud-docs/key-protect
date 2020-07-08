---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-19"

keywords: Key Protect CLI plug-in, CLI changelog

subcollection: key-protect

---

{:external: target="_blank" .external}
{:important: .important}
{:shortdesc: .shortdesc}
{:tip: .tip}

# CLI changelog
{: #cli-changelog}

Learn about updates to the {{site.data.keyword.keymanagementserviceshort}} CLI
plug-in.
{: shortdesc}

To update your {{site.data.keyword.keymanagementserviceshort}} CLI plug-in, see
[updating the CLI plug-in](/docs/key-protect?topic=key-protect-set-up-cli#update-cli).

When you log in to the
[{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external},
you're notified when updates are available.

Be sure to keep your CLI up-to-date so that you can use the commands and flags
that are available for the {{site.data.keyword.keymanagementserviceshort}} CLI
plug-in.
{: tip}

## CLI version 0.5.0
{: #cli-changelog-050}

<dl>
  <dt>
    <varname>Release date</varname>
  </dt>
  <dd>
    19 June 2020
  </dd>

  <dt>
    <varname>Documentation</varname>
  </dt>
  <dd>
    [CLI reference, version 0.5.0](/docs/key-protect?topic=key-protect-cli-reference)
  </dd>
</dl>

### Add these commands
{: #cli-changelog-050-add}

#### kp instance
{: #cli-changelog-050-add-kp-instance}

* kp instance [policies](/docs/key-protect?topic=key-protect-cli-reference#kp-instance-policies) - list policies associated with an instance

* kp instance [policy-update allowed-network](/docs/key-protect?topic=key-protect-cli-reference#kp-instance-policy-update-allowed) - update the instance policy and set the "allowed network" to public-and-private or private-only

* kp instance [policy-update dual-auth-delete](/docs/key-protect?topic=key-protect-cli-reference#kp-instance-policy-update-dual) - update the instance policy and enable or disable the "dual authorization delete" policy

#### kp key
{: #cli-changelog-050-add-kp-key}

* kp key [cancel-delete](/docs/key-protect?topic=key-protect-cli-reference#kp-key-cancel-delete) - cancel a previously scheduled request to delete a key

* kp key [disable](/docs/key-protect?topic=key-protect-cli-reference#kp-key-disable) - disable a root key and temporarily revoke access to the key's associated data in the cloud

* kp key [enable](/docs/key-protect?topic=key-protect-cli-reference#kp-key-enable) - enable a root key that was previously disabled; this action restores the key's encrypt and decrypt operations

* kp key [restore](/docs/key-protect?topic=key-protect-cli-reference#kp-key-restore) - restore a previously deleted root key, which restores access to its associated data in the cloud

* kp key [policy-update dual-auth-delete](/docs/key-protect?topic=key-protect-cli-reference#kp-key-policy-update-dual) - update the "dual authorization delete" policy associated with a key

* kp key [policy-update rotation](/docs/key-protect?topic=key-protect-cli-reference#kp-key-policy-update-rotation) - update the "rotation" policy associated with a key

* kp key [schedule-delete](/docs/key-protect?topic=key-protect-cli-reference#kp-key-schedule-delete) - authorize a key, with a "dual-auth-delete" policy, to be deleted

#### kp registrations
{: #cli-changelog-050-add-kp-registrations}

* kp [registrations](/docs/key-protect?topic=key-protect-cli-reference#kp-registrations) - list registrations, which are associations between keys and other cloud resources such as Cloud Object Storage (COS) buckets or Cloud Databases deployments

### Update these commands
{: #cli-changelog-050-update}

#### kp import-token
{: #cli-changelog-050-update-kp-import-token}

* kp import-token [key-encrypt](/docs/key-protect?topic=key-protect-cli-reference#kp-import-token-key-encrypt) - add the `-a` (--hash) option; encrypt keys using the SHA-1 algorithm for Hyper Protect Crypto Services (HPCS)

* kp import-token [nonce-encrypt](/docs/key-protect?topic=key-protect-cli-reference#kp-import-token-nonce-encrypt) - add the `-c` (--cbc) option; encrypt the nonce using the AES-CBC encryption algorithm; only supported for HPCS

#### kp key
{: #cli-changelog-050-update-kp-key}

* kp key [delete](/docs/key-protect?topic=key-protect-cli-reference#kp-key-delete) - add the `-f` (--force) option to delete a key, with force, which is used to delete a key that has existing "registrations"

#### kp keys
{: #cli-changelog-050-update-kp-keys}

* kp [keys](/docs/key-protect?topic=key-protect-cli-reference#kp-keys) - add the `-n` (--number-of-keys) and `-s` (--starting-offset) options to retrieve a subset of keys

## CLI version 0.4.0
{: #cli-changelog-040}

<dl>
  <dt>
    <varname>Release date</varname>
  </dt>
  <dd>
    1 May 2020
  </dd>

  <dt>
    <varname>Documentation</varname>
  </dt>
  <dd>
    [CLI reference, version 0.4.0](/docs/key-protect?topic=key-protect-cli-reference-040)
  </dd>
</dl>

Major changes were made to the `ibmcloud kp` command structure.

### Add these commands
{: #cli-changelog-040-add}

* `kp import-token create` - create an import token

* `kp import-token key-encrypt` - encrypt the key that you want to import to the service

* `kp import-token nonce-encrypt` - encrypt the nonce that is generated by {{site.data.keyword.keymanagementserviceshort}}

* `kp import-token show` - retrieve an import token

* `kp key` - perform operations on keys

* `kp keys` - list the keys that are available in your service instance

* `kp region-set` - target a different
{{site.data.keyword.keymanagementserviceshort}} regional endpoint

### Deprecate these commands
{: #cli-changelog-040-deprecate}

* `kp create` - use `kp import-token create` or `kp key create`

* `kp delete` - use `kp key delete`

* `kp get` - use `kp key show`

* `kp import-token encrypt-key` - use `kp import-token key-encrypt`

* `kp import-token encrypt-nonce` - use `kp import-token nonce-encrypt`

* `kp import-token get` - use `kp import-token show`

* `kp list` - use `kp keys`

* `kp policy` - use `kp key policies` or `kp key policy-update`

* `kp rotate` - use `kp key rotate`

* `kp unwrap` - use `kp key unwrap`

* `kp wrap` - use `kp key wrap`

### Deprecated commands
{: #cli-changelog-deprecated}

All deprecated commands work in versions 0.4.0 and 0.5.0. That is, version
0.5.0 is backwards compatible with versions 0.3.9 and 0.4.0. For example, you
can issue the `kp create` command even though it's deprecated in version 0.5.0.

The intent is to remove support for deprecated commands in the next CLI version,
which is anticipated the end of September, 2020.
{: important}

## CLI version 0.3.9
{: #cli-changelog-039}

<dl>
  <dt>
    <varname>Release date</varname>
  </dt>
  <dd>
    11 November 2019
  </dd>

  <dt>
    <varname>Documentation</varname>
  </dt>
  <dd>
    [CLI reference, version 0.3.9](/docs/key-protect?topic=key-protect-cli-reference-039)
  </dd>
</dl>

### Updates
{: #cli-changelog-039-update}

* Update usage information for all `ibmcloud kp` commands and sub-commands
* Fix bug in the JSON output format option (`--output json`) for all commands
that support JSON output
