---

copyright:
  years: 2017, 2022
lastupdated: "2022-02-11"

keywords: CLI plug-in, CLI changelog, Key Protect CLI

subcollection: key-protect

---

{:external: target="_blank" .external}
{:important: .important}
{:shortdesc: .shortdesc}
{:note: .note}
{:tip: .tip}

# CLI changelog
{: #cli-changelog}

Learn about updates to the {{site.data.keyword.keymanagementserviceshort}}
Command Line Interface (CLI) plug-in.
{: shortdesc}

To update your {{site.data.keyword.keymanagementserviceshort}} CLI plug-in, see
[updating the CLI plug-in](/docs/key-protect?topic=key-protect-set-up-cli#update-cli).

When you log in to the
[{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external},
you're notified when updates are available.

Be sure to keep your CLI installation up-to-date so that you can use the latest commands and flags that are available for the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in.
{: tip}
<staging -- cli-test>
## CLI version 0.6.10
{: #cli-changelog-0610}

Release date: 2022-02-08

### Changes
{: #cli-changelog-0610-changes}

Changes included in this [version](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference):

- The environment variable **`KP_PRIVATE_ADDR`** is no longer required when authenticating to `private.cloud.ibm.com`. More information is available about using the CLI with [private endpoints](/docs/cli?topic=cli-cli-private-endpoints).
- The environment variable **`IBMCLOUD_TRACE`** gives you more visibility into your diagnostics. Learn more at the [CLI documentation](/docs/cli?topic=cli-ibmcloud_env_var#IBMCLOUD_TRACE).
- Updated help text and messages.</staging -- cli-test>

## CLI version 0.6.9
{: #cli-changelog-069}

Release date: 2021-12-14

### Changes
{: #cli-changelog-069-changes}

Changes included in this [version](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference):

- Added support for [S&atilde;o-Paulo region](/docs/key-protect?topic=key-protect-regions).

## CLI version 0.6.8
{: #cli-changelog-068}

Release date: 2021-11-05

### Changes
{: #cli-changelog-068-changes}

Changes included in this [version](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference):

- Added support for [targeting private endpoints](/docs/key-protect?topic=key-protect-private-endpoints#target-private-endpoint).

## CLI version 0.6.7
{: #cli-changelog-067}

The release binary is not currently available. When released, please use v0.6.8.

## CLI version 0.6.6
{: #cli-changelog-066}

Release date: 2021-10-11

### Changes
{: #cli-changelog-066-changes}

Changes included in this [version](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference):

- Added feature for [key synchronizing](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-sync) as a new subcommand under **`key`**.

- Updated the "Internal server error" response message to return the actual error message.

## CLI version 0.6.5
{: #cli-changelog-065}

Release date: 2021-08-30

### Changes
{: #cli-changelog-065-changes}

Changes included in this [version](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference):

- The JSON response for the retrieval of multiple objects like listing keys, policies, and registrations is set as an empty array: **`[]`**  when there are no objects returned by the service.

- The JSON response for the retrieval of a single object, like a key policy, is set as an empty object: **`{}`** when there is no policy returned by the service.

## CLI version 0.6.4
{: #cli-changelog-064}

Release date: 2021-07-23

### Changes
{: #cli-changelog-064-changes}

Changes included in this [version](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference):

- Error messages have been formatted to make them easier for users to understand.

- Additional validation for sha1 encryption ensures that requests are only made to the correct endpoint.

## CLI version 0.6.3
{: #cli-changelog-063}

Release date: 2021-06-25

### Changes
{: #cli-changelog-063-changes}

There are minor changes in this latest [release](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference).

Support for {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.hscrypto}} specific algorithms in [Key Create](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-create) and [Key Rotate](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-rotate).

## CLI version 0.6.2
{: #cli-changelog-062}

Release date: 2021-05-20

### Changes
{: #cli-changelog-062-changes}

There are minor changes in this [release](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference).

- [Key purge](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference) command supports purging a deleted key resulting in the permanent deletion of information related to that key. After being purged, the key is not available to be restored: `ibmcloud kp key purge <keyID>`.
- Support for the new `ca-tor` regional [endpoint](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference); all Key Protect operations supported via CLI can be performed for this region: `ibmcloud kp region-set ca-tor` (sets the target endpoint to key protect **`ca-tor`** endpoint).


## CLI version 0.6.1
{: #cli-changelog-061}

Release date: 2021-04-30

### Changes
{: #cli-changelog-061-changes}

There are minor changes in this [release](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference).

- Key Update command has been added, supporting transferring your key from its current key ring to a new key ring.

## CLI version 0.6.0
{: #cli-changelog-060}

Release date: 2021-02-25

### Changes
{: #cli-changelog-060-changes}

There are major changes in this [release](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference).

- Key Ring support is added to the CLI supporting customization and best practices.
- Key Alias feature is added to the CLI adding first-in-class management support. 
- Added support for Osaka (jp-osa) endpoint.
- Instance policy command supports the new sub commands.
- Minor changes include JSON output support in sub commands and the deprecation of some commands, features and flags.

## CLI version 0.5.2
{: #cli-changelog-052}

Release date: 2020-09-21

### Changes
{: #cli-changelog-052-changes}

- Commands that specify JSON outout (`--output json`) now return an empty JSON
    structure if there is no output.

## CLI version 0.5.1 (deprecated)
{: #cli-changelog-051}

Release date: 2020-07-21

### Changes
{: #cli-changelog-051-changes}

- Changed the `command successful` message from `SUCCESS` TO `OK`.

- Remove additional line breaks in the response.

- For Windows, `OK` and `FAILED` messages are the same color as the `ibmcloud`
    command colors.

## CLI version 0.5.0 (deprecated)
{: #cli-changelog-050}

Release date: 2020-06-19

Documentation:
[CLI reference, version 0.5.0](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference)

### Add these commands
{: #cli-changelog-050-add}

#### kp instance
{: #cli-changelog-050-add-kp-instance}

- kp instance
    [policies](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-instance-policies)
    : list policies associated with an instance

- kp instance
    [policy-update allowed-network](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-instance-policy-update-allowed)
    : update the instance policy and set the "allowed network" to
    public-and-private or private-only

- kp instance
    [policy-update dual-auth-delete](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-instance-policy-update-dual)
    : update the instance policy and enable or disable the
    "dual authorization delete" policy

#### kp key
{: #cli-changelog-050-add-kp-key}

- kp key
    [cancel-delete](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-cancel-delete)
    : cancel a previously scheduled request to delete a key

- kp key
    [disable](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-disable)
    : disable a root key and temporarily revoke access to the key's associated
    data in the cloud

- kp key
    [enable](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-enable)
    : enable a root key that was previously disabled; this action restores the
    key's encrypt and decrypt operations

- kp key
    [restore](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-restore)
    : restore a previously deleted root key, which restores access to its
    associated data in the cloud

- kp key
    [policy-update dual-auth-delete](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-policy-update-dual)
    : update the "dual authorization delete" policy associated with a key

- kp key
    [policy-update rotation](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-policy-update-rotation)
    : update the "rotation" policy associated with a key

- kp key
    [schedule-delete](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-schedule-delete)
    : authorize a key, with a "dual-auth-delete" policy, to be deleted

#### kp registrations
{: #cli-changelog-050-add-kp-registrations}

- kp
    [registrations](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-registrations)
    : list registrations, which are associations between keys and other cloud
    resources such as Cloud Object Storage (COS) buckets or Cloud Databases
    deployments

### Update these commands
{: #cli-changelog-050-update}

#### kp import-token
{: #cli-changelog-050-update-kp-import-token}

- kp import-token
    [key-encrypt](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-import-token-key-encrypt)
    : add the `-a` (--hash) option; encrypt keys using the SHA-1 algorithm for
    Hyper Protect Crypto Services (HPCS)

- kp import-token
    [nonce-encrypt](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-import-token-nonce-encrypt)
    : add the `-c` (--cbc) option; encrypt the nonce using the AES-CBC encryption
    algorithm; only supported for HPCS

#### kp key
{: #cli-changelog-050-update-kp-key}

- kp key
    [delete](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-delete)
    : add the `-f` (--force) option to delete a key, with force, which is used to
    delete a key that has existing "registrations"

#### kp keys
{: #cli-changelog-050-update-kp-keys}

- kp
    [keys](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-keys)
    : add the `-n` (--number-of-keys) and `-s` (--starting-offset) options to
    retrieve a subset of keys

## CLI version 0.4.0 (deprecated)
{: #cli-changelog-040}

Release date: 2020-05-01

Documentation:
[CLI reference, version 0.4.0](/docs/key-protect?topic=key-protect-cli-reference-040)

Major changes were made to the `ibmcloud kp` command structure.

### Add these commands
{: #cli-changelog-040-add}

- `kp import-token create` : create an import token

- `kp import-token key-encrypt` : encrypt the key that you want to import to the
    service

- `kp import-token nonce-encrypt` : encrypt the nonce that is generated by
    {{site.data.keyword.keymanagementserviceshort}}

- `kp import-token show` : retrieve an import token

- `kp key` : perform operations on keys

- `kp keys` : list the keys that are available in your
    {{site.data.keyword.keymanagementserviceshort}} instance

- `kp region-set` : target a different
{{site.data.keyword.keymanagementserviceshort}} regional endpoint

### Deprecate these commands
{: #cli-changelog-040-deprecate}

- `kp create` : use `kp import-token create` or `kp key create`

- `kp delete` : use `kp key delete`

- `kp get` : use `kp key show`

- `kp import-token encrypt-key` : use `kp import-token key-encrypt`

- `kp import-token encrypt-nonce` : use `kp import-token nonce-encrypt`

- `kp import-token get` : use `kp import-token show`

- `kp list` : use `kp keys`

- `kp policy` : use `kp key policies` or `kp key policy-update`

- `kp rotate` : use `kp key rotate`

- `kp unwrap` : use `kp key unwrap`

- `kp wrap` : use `kp key wrap`

### Deprecated commands
{: #cli-changelog-deprecated}

All deprecated commands work in versions 0.4.0 and 0.5.0. That is, version
0.5.0 is backwards compatible with versions 0.3.9 and 0.4.0. For example, you
can issue the `kp create` command even though it's deprecated in version 0.5.0.

The intent is to remove support for deprecated commands in the next CLI version,
which is anticipated the end of September, 2020.
{: important}

## CLI version 0.3.9 (deprecated)
{: #cli-changelog-039}

Release date: 2019-11-11

Documentation:
[CLI reference, version 0.3.9](/docs/key-protect?topic=key-protect-cli-reference-039)

### Updates
{: #cli-changelog-039-update}

- Update usage information for all `ibmcloud kp` commands and sub-commands

- Fix bug in the JSON output format option (`--output json`) for all commands
    that support JSON output


