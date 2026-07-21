---

copyright:
  years: 2017, 2026

lastupdated: "2026-07-21"

keywords: getting started, key management, encryption keys, create keys, manage keys, Dedicated Key Protect, single-tenant, KYOK, API, Terraform, dedicated, single-tenant-initialize

subcollection: key-protect

content-type: howto

---

{{site.data.keyword.attribute-definition-list}}

# Creating a Dedicated {{site.data.keyword.keymanagementserviceshort}} instance
{: #provision-ded-instance}

Learn how to provision and initialize a Dedicated {{site.data.keyword.keymanagementserviceshort}} instance, create and manage encryption keys using the console, CLI, or Terraform, and integrate with {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

Three different versions of each CLI command are presented in this topic where applicable: for Mac/Linux, Windows PowerShell, or Windows command prompt (CMD). Make sure you are using the command that corresponds to your system.
{: important}

For more information about the key concepts for Dedicated {{site.data.keyword.keymanagementserviceshort}}, see [About Standard and Dedicated {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-about).

## Before you begin
{: #getting-started-prereqs}

To use Dedicated {{site.data.keyword.keymanagementserviceshort}}, make sure that you meet the following prerequisites:

1. You have an [{{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-accounts){: external}.

2. You have the appropriate IAM permissions to provision and manage {{site.data.keyword.keymanagementserviceshort}} instances. For more information, see [Managing user access](/docs/key-protect?topic=key-protect-manage-access).

3. Install the latest version of the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external} and the {{site.data.keyword.keymanagementserviceshort}} CLI plugin. You must use the latest version of the CLI to complete the initialization, even if you deploy your instance using the console.

4. (Optional) For Terraform: Install [Terraform and configure the {{site.data.keyword.cloud_notm}} Provider](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started){: external}

The Terraform resource is supported on Linux (amd64) and Windows (amd64). macOS (ARM) is unofficially supported. You must use the latest version of the {{site.data.keyword.cloud_notm}} CLI and Terraform to complete the initialization, even if you deploy your instance using the console. If you receive the `Unable to obtain plug-in's metadata` error while installing the latest version of the KP CLI plugin, see [troubleshooting steps](/docs/key-protect?topic=key-protect-troubleshooting-init#unable-get-metadata-error).
{: note}

## Provision a Dedicated instance
{: #getting-started-provision}

You can create a Dedicated instance of {{site.data.keyword.keymanagementserviceshort}} using the {{site.data.keyword.cloud_notm}} console, CLI, or Terraform.

### Provisioning from the console
{: #getting-started-provision-console}
{: ui}

1. [Log in to your {{site.data.keyword.cloud_notm}} account](/login/){: external}.

2. Click **Catalog** to view the list of services available on {{site.data.keyword.cloud_notm}}.

3. Search for "Key Protect" and click the **Key Protect** tile.

4. Select the **Dedicated Plan**.

5. Configure your instance:
   - **Service name**: Enter a unique name for your instance
   - **Resource group**: Select the resource group where you want to create the instance
   - **Location**: Choose the region where you want to deploy your instance
   - **Crypto units**: Select 2 or 3 crypto units (cannot be changed later)
    
    If you do not specify a number of crypto units, your instance is provisioned with two. You can also specify three crypto units using the drop-down list. The value cannot be changed later.
    {: note}

6. Click **Create** to provision your instance.

The provisioning process can take several minutes. After provisioning, you must initialize your instance using CLI before you can create keys. For more information, see [Initializing your Dedicated instance](/docs/key-protect?topic=key-protect-provision-ded-instance&interface=cli#getting-started-initialize).
{: important}

### Provisioning from the CLI
{: #getting-started-provision-cli}
{: cli}

Before crypto units can be created and your instance initialized, your instance must be created. First, set a resource group to target by issuing:

```sh
ibmcloud target -c <resource_group>
```
{: pre}

If you do not know your resource group, you can find out which ones you have by issuing:

```sh
ibmcloud resource groups
```
{: pre}

After you have your resource group set, provision a Dedicated instance using the {{site.data.keyword.cloud_notm}} CLI.

For a Dedicated instance with 2 crypto units (default):

```sh
ibmcloud resource service-instance-create <instance_name> kms dedicated <region>
```
{: pre}

For a Dedicated instance with 3 crypto units:

```sh
ibmcloud resource service-instance-create <instance_name> kms dedicated <region> -p '{"crypto_units": 3}'
```
{: pre}

Specifying a number of crypto units other than `2` or `3` returns an error. You cannot change the number of crypto units later.
{: important}

Provisioning a Dedicated instance can take several minutes. You can check on the status of your instance by issuing:

```sh
ibmcloud resource service-instance <instance_name>
```
{: pre}

Where:

* `<instance_name>` is the name you gave your instance in the previous step.

The instance can have one of two states: **active** or **in progress**. An **active** instance has not yet been initialized; initialization requires completing the remaining steps in this topic. Until you initialize your instance, it cannot be used because your identities have not yet been configured with your crypto units to create the master key.

You might have to wait for a few minutes after provisioning before your crypto units are available. For more information, see [Crypto unit states](/docs/key-protect?topic=key-protect-crypto-unit-states)
{: note}

### Provisioning with Terraform
{: #getting-started-provision-terraform}
{: terraform}

```hcl
data "ibm_resource_group" "resource_group" {
   name = "<your_resource_group_name>"
  }
  resource "ibm_resource_instance" "key_protect_instance" {
  name              = "<your_instance_name>"
  resource_group_id = data.ibm_resource_group.resource_group.id
  service           = "kms"
  plan              = "dedicated"
  location          = "us-south"
  tags              = ["<tag1>"]
  parameters = {
    crypto_units = "3"
  }
}
```
{: codeblock}

## Getting the endpoint and instance ID
{: #getting-started-get-endpoint}
{: cli}

After your instance is active, get the endpoint and GUID by issuing:

```sh
ibmcloud resource service-instance <instance_name> -o json
```
{: pre}

Where:

* `<instance_name>` is the name you gave your instance in the previous step.

The endpoint is the `public` parameter value in the `endpoints` stanza of the json output from the previous command. It takes the format `https://<instance-id>.api.<region>.kms.appdomain.cloud`. The GUID is the `GUID` parameter value in that output. It takes the format of `UUID`.

You can get the endpoint by issuing: `ibmcloud resource service-instance <instance-id> --output json | jq -r '.[].extensions.endpoints'`.
{: tip}

Save the full endpoint as an environment variable by issuing two commands on one of three supported operating systems.

For [macOS]{: tag-macos}:

```sh
export KP_TARGET_ADDR=<st_instance_endpoint>
```
{: pre}

And:

```sh
export KP_INSTANCE_ID=<guid>
```
{: pre}

For [Windows]{: tag-windows} PowerShell:

```powershell
$Env:KP_INSTANCE_ID = <guid>
```
{: codeblock}

And:

```powershell
$Env:KP_TARGET_ADDR = <st_instance_endpoint>
```
{: codeblock}

For [Windows]{: tag-windows} CMD:

```sh
set KP_INSTANCE_ID=<guid>
```
{: codeblock}

And:

```sh
set KP_TARGET_ADDR=<st_instance_endpoint>
```
{: codeblock}

Where:

* `<st_instance_endpoint>` is the full endpoint of your instance, taking the format of `https://<instance-id>.api.<region>.kms.appdomain.cloud`.
* `<guid>` is the instance ID from the previous output.

You are now ready to generate admin credentials.

You must complete initialization using the CLI before you can use the console or Terraform to manage keys.
{: important}

## Initializing your Dedicated instance
{: #getting-started-initialize}
{: cli}

Before you can create keys, you must initialize your Dedicated instance. This process establishes your ownership and control over the crypto units.

The initialization process involves:

1. **Generating administrator credentials**: Create RSA signature authentication keys that identify you as an administrator. For more information, see [Generating admin credentials](#getting-started-generate-admin).
2. **Claiming your crypto units**: Use your credentials to claim ownership of the crypto units
3. **Creating and loading the master key**: Generate and load the master key that encrypts all other keys in your instance

You must complete initialization using the CLI before you can use the console, API, or Terraform to manage keys.
{: important}

### Generating admin credentials
{: #getting-started-generate-admin}

A crypto unit is managed by an admin or admins, which means you either need to have identities available or create them. If you have properly formatted admin identities (a symmetric 256-bit AES key using RSA-2048), you can skip down to [Creating and loading the master key](#getting-started-master-key).

If you need to create an admin credential, issue:

For [macOS]{: tag-macos}:

```sh
ibmcloud kp crypto-unit sig-key generate --file <admin_key_file> --passphrase <pwd> --algo RSA-2048
```
{: pre}

For [Windows]{: tag-windows} PowerShell:

```powershell
ibmcloud kp crypto-unit sig-key generate --file <admin_key_file> --passphrase <pwd> --algo RSA-2048
```
{: codeblock}

For [Windows]{: tag-windows} CMD:

```sh
ibmcloud kp crypto-unit sig-key generate --file <admin_key_file> --passphrase <pwd> --algo RSA-2048
```
{: codeblock}

Where:

* `<admin_key_file>` is the location on your machine where the identity is created (for example, `admin-keyfile.key`).
* `<pwd>` is an optional password or passphrase that is used to encrypt the file at rest. Specify "-" to be prompted to enter a passphrase.

Save a copy of this keyfile and remember the passphrase. It is required for all authenticated commands when interacting with the crypto units. If any `ibmcloud kp crypto-unit` command returns error code `e00bad05`, see [Troubleshooting](/docs/key-protect?topic=key-protect-troubleshooting-init#command-failed-with-error-code-e00bad05-error) section.
{: note}

### Claiming your crypto units
{: #getting-started-claim-crypto-units}

Crypto units that are assigned to a user start in a [cleared state](/docs/key-protect?topic=key-protect-crypto-unit-states). All crypto units in a service instance must be configured the same way. If one availability zone in the region where your instance is located cannot be accessed, the operational crypto units can be used interchangeably for load balancing or for high availability.

The master key in all crypto units in a single service instance must be set the same. The same set of administrators must be added to all crypto units, and all crypto units must initialize at the same time.

For more information about the states a crypto unit can be in, see [Crypto unit states](/docs/key-protect?topic=key-protect-crypto-unit-states).
{: tip}

To display the service instances and crypto units in the target resource group under the current user account, use the following command:

```sh
ibmcloud kp crypto-units
```
{: pre}

The following output is an example that is displayed. The ID column in the output table identifies the crypto units that are targeted by later administrative commands that are issued by the KP CLI plug-in.

```bash
*******************************************************
Id                                     InstanceID                             State
6e0aead3-9d44-4c92-a4c4-f7a1ab415420   c28a8939-3980-4697-a80c-50b1f8bbf160   reserved
3bb363fc-b1f9-4237-b37b-2c9e07784e3c   c28a8939-3980-4697-a80c-50b1f8bbf160   reserved
*******************************************************
```
{: screen}

The public part of the RSA key pair is placed in a certificate that is installed in the target crypto unit to define a crypto unit administrator. To upload it as the default admin of your crypto units, issue the claim command:

For [macOS]{: tag-macos}:

```sh
ibmcloud kp crypto-unit claim --credential <admin_key_file>
```
{: pre}

For [Windows]{: tag-windows} PowerShell:

```powershell
ibmcloud kp crypto-unit claim --credential <admin_key_file>
```
{: codeblock}

For [Windows]{: tag-windows} CMD:

```sh
ibmcloud kp crypto-unit claim --credential <admin_key_file>
```
{: codeblock}

Where:

* `<admin_key_file>` is the file where the identity was stored.

All `crypto-unit` commands apply to all of the crypto units. They are effectively clones of each other.
{: tip}

### Creating and loading the master key
{: #getting-started-master-key}

{{site.data.keyword.keymanagementserviceshort}} does not store or back up your master key. Maintain a secure backup of your master key credentials in a safe location.
{: tip}

After you create your instance and admin identity, you can use them to create your master key. The master key, also known as the HSM master key, is used to encrypt the service instance for key storage. It is a symmetric 256-bit AES key. With the master key, you take ownership of the cloud HSM and own the root of trust that encrypts the entire hierarchy of encryption keys, including root keys and standard keys in the key management keystore. One service instance can have only one master key. Deleting the master key of the service instance effectively crypto-shreds all data that was encrypted with the keys that are managed in the service.

Dedicated {{site.data.keyword.keymanagementserviceshort}} uses key splitting, in which a cryptographic key is split into multiple pieces to enhance security. At least 2 keyshares must be created, though more can be used depending on the use case.
{: important}

To generate the master key locally, issue the command on one of the three supported operating systems.

For [macOS]{: tag-macos}:

```sh
ibmcloud kp crypto-unit master-key generate --keyshare-files '["<keyshare_file_1>#<password1>", "<keyshare_file_2>#<password2>"]' --keyshare-minimum 2 --algo AES-256 --key-name <key_name> --auth '[{"ADMIN": "<admin_key_file>#<password3>"}]'
```
{: pre}

For [Windows]{: tag-windows} PowerShell:

```powershell
ibmcloud kp crypto-unit master-key generate --keyshare-files '["""<keyshare_file_1>#<password1>""","""<keyshare_file_2>#<password2>"""]' --keyshare-minimum 2 --algo AES-256 --key-name <key_name> --auth '[{"""ADMIN""": """<admin_key_file>#<password3>"""}]'
```
{: codeblock}

For [Windows]{: tag-windows} CMD:

```sh
ibmcloud kp crypto-unit master-key generate --keyshare-files"[\"<keyshare_file_1>#<password1>\", \"<keyshare_file_2>#<password2>\"]" --keyshare-minimum 2 --algo AES-256 --key-name <key_name> --auth "[{\"ADMIN\": \"<admin_key_file>#<password3>\"}]"
```
{: codeblock}

Where:

* `<keyshare_file_1>#<password1>` is the location of one of the keyshares, along with a passphrase for the file that is created. The passphrase is mandatory and must be between 6-255 characters. Omit `#<password1>` to be prompted to enter a passphrase.
* `<keyshare_file_2>#<password2>` is the location of another keyshare, along with a passphrase for the file that is created. The passphrase is mandatory and must be between 6-255 characters. Omit `#<password2>` to be prompted to enter a passphrase.
* `<key_name>` is the name of your master key.
* `<admin_key_file>#<password3>` is the location of your admin and its passphrase you generated earlier (if you are not bringing your own identity). Omit `#<password3>` to be prompted to enter a passphrase.

The `keyshare-minimum` flag, which defaults to `2` but can be increased, represents the minimum number of keyshares you must specify by their file locations.

To upload your master key to the crypto units of your instance, issue the command on one of the three supported operating systems.

For [macOS]{: tag-macos}:

```sh
ibmcloud kp crypto-unit master-key import --keyshare-files '["<keyshare_file_1>#<password1>", "<keyshare_file_2>#<password2>"]' --auth '[{"ADMIN": "<admin_key_file>#<password3>"}]'
```
{: pre}

For [Windows]{: tag-windows} PowerShell:

```powershell
ibmcloud kp crypto-unit master-key import --keyshare-files '["""<keyshare_file_1>#<password1>""","""<keyshare_file_2>#<password2>"""]' --auth '[{"""ADMIN""": """<admin_key_file>#<password3>"""}]'
```
{: codeblock}

For [Windows]{: tag-windows} CMD:

```sh
ibmcloud kp crypto-unit master-key import --keyshare-files "[\"<keyshare_file_1>#<password1>\", \"<keyshare_file_2>#<password2>\"]" --auth "[{\"ADMIN\": \"<admin_key_file>#<password3>\"}]"
```
{: codeblock}

Where:

* `<keyshare_file_1>#<password1>` is the location of one of the keyshares, along with a passphrase for the file that is created. The passphrase is mandatory and must be between 6-255 characters. Omit `#<password1>` to be prompted to enter a passphrase.
* `<keyshare_file_2>#<password2>` is the location of another keyshare, along with a passphrase for the file that is created. The passphrase is mandatory and must be between 6-255 characters. Omit `#<password2>` to be prompted to enter a passphrase.
* `<admin_key_file>#<password3>` is the location of your admin and its passphrase you generated earlier (if you are not bringing your own identity). Omit `#<password3>` to be prompted to enter a passphrase.

After your master key is created, you must allow the {{site.data.keyword.keymanagementserviceshort}} service to perform actions on your crypto units (for example, to create keys). The level of permissions granted to {{site.data.keyword.keymanagementserviceshort}} is less than that of an admin. Issue the command using one of the three supported operating systems.

For [macOS]{: tag-macos}:

```sh
ibmcloud kp crypto-unit user add --type kmsCryptoUser --auth '[{"ADMIN": "<admin_key_file>#<password>"}]'
```
{: pre}

For [Windows]{: tag-windows} PowerShell:

```powershell
ibmcloud kp crypto-unit user add --type kmsCryptoUser --auth '[{"""ADMIN""": """<admin_key_file>#<password>"""}]'
```
{: codeblock}

For [Windows]{: tag-windows} CMD:

```sh
ibmcloud kp crypto-unit user add --type kmsCryptoUser --auth "[{\"ADMIN\": \"<admin_key_file>#<password>\"}]"
```
{: codeblock}

Where:

* `<admin_key_file>#<password>` is the location of your admin key file and its passphrase as generated earlier (if you are not bringing your own identity). Omit `#<password>` to be prompted to enter a passphrase.

This command can also add admins to your crypto units by setting `--type` to `admin` and adding `--name` and `--credential` that point to an admin identity you possess. Do not add `--name` or `--credential` when adding `kmsCryptoUser`. For example:

```sh
ibmcloud kp crypto-unit user add --type admin --name <username> --credential "<username_key_file>" --auth '[{"ADMIN": "<admin_key_file>#<pwd>"}]'
```
{: pre}

Where:

* `<username>` is the name of the admin identity you are adding.
* `<username_key_file>` is the file path of the credential to associate with the new user.
* `<admin_key_file>#<pwd>` is the location of your existing admin and its passphrase you generated earlier (if you are not bringing your own identity). Omit `#<pwd>` to be prompted to enter a passphrase.

Do not add `--name` or `--credential` when adding `kmsCryptoUser` as an admin.
{: important}

Your instance is now fully initialized.

It might take up to 5 to 10 minutes before you can use your instance.
{: note}

If you have any issues during initialization, see [Troubleshooting](/docs/key-protect?topic=key-protect-troubleshooting-init) section.

## Initialize your dedicated instance with Terraform
{: #getting-started-generate-admin-terraform}
{: terraform}

You can fully initialize your cryptounit instance using the following Terraform resource:

```hcl
resource "ibm_kms_cryptounits" "st-dedicated" {
   # region = ibm_resource_instance.key_protect_instance.location
   # instance_id = ibm_resource_instance.key_protect_instance.guid
  url = ibm_resource_instance.key_protect_instance.extensions["endpoints.public"]

  signature_key {
    filepath   = "terraform-tim-test-kp-st.key"
    passphrase = ""
  }

  master_key  {
    keysharefile {
      filepath = "<your_master_file>-1.key"
      token    = "<yourpass>"
    }
    keysharefile {
      filepath = "<your_master_file>-2.key"
      token    = "<yourpass>"
    }
    keyname = "<your_master_key_name>"
  }
}
```
{: codeblock}

- The `signature_key` becomes the admin credentials. The resource references a file path. If the specified file path finds an existing file, it uses that file. If the file is not found, it creates your admin credentials at that file path.
- The `keyshare` field creates a signature key or use an existing one found in the filepaths of the `keysharefile`. You need provide two or more key share file configurations. Each key share file represents a part of the master key.  

## Create encryption keys
{: #getting-started-create-keys}

After your instance is initialized, you can create encryption keys to protect your data. {{site.data.keyword.keymanagementserviceshort}} supports two types of keys:

- **Root keys**: Symmetric key-wrapping keys used for envelope encryption
- **Standard keys**: Symmetric keys used to directly encrypt data

### Creating a root key in the console
{: #getting-started-create-root-key-console}
{: ui}

1. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned Dedicated instance of {{site.data.keyword.keymanagementserviceshort}}.

2. Click **Add** to create a new key.

3. Select **Create a key**.

4. Specify the key details:

   | Setting | Description |
   | ------- | ----------- |
   | Type | Select **Root key** |
   | Key name | Enter a human-readable name (2-90 characters). Avoid personally identifiable information (PII). |
   | Key description | Optional. Add a description (2-240 characters) to help identify the key's purpose. |
   | Key ring | Optional. Select a key ring to organize your keys. If not specified, the key is placed in the `default` key ring. |
   | Rotation policy | Optional. Set an automatic rotation schedule for the key. |
   {: caption="Root key settings" caption-side="bottom"}

5. Click **Add** to confirm.

Keys created in the service are symmetric 256-bit keys, supported by the AES-GCM algorithm. For added security, keys are generated by FIPS 140-3 Level 4 HSMs (submitted for NIST certification) in your dedicated crypto units.

### Creating a root key with CLI
{: #getting-started-create-root-key-cli}
{: cli}

1. [Retrieve your authentication credentials](/docs/key-protect?topic=key-protect-set-up-cli).

2. Create a root key by running the following command:

   ```sh
   ibmcloud kp key create <key_name> -i <instance_ID>
   ```
   {: pre}

   Replace the variables in the example request according to the following table:

   | Variable | Description |
   |----------|-------------|
   | `key_name` | A human-readable name for your key. The name must be 2–90 characters in length. Avoid entering personally identifiable information (PII) as part of the key name. |
   | `instance_ID` | The unique identifier (GUID) assigned to your Dedicated instance. |
   {: caption="Variables for creating a root key with CLI" caption-side="bottom"}

   A successful response returns the ID and name of your new root key.

3. Optional: Verify that the key was created by listing the keys in your instance.

   ```sh
   ibmcloud kp keys -i <instance_ID>
   ```
   {: pre}

Keys created in the service are symmetric 256-bit keys, supported by the AES-GCM algorithm. For added security, keys are generated by FIPS 140-3 Level 4 HSMs (submitted for NIST certification) in your dedicated crypto units.

### Creating a root key with Terraform
{: #getting-started-create-root-key-terraform}
{: terraform}

You can create a root key using Terraform.

Terraform cannot be used to initialize Dedicated instances. You must initialize your instance using the CLI before using Terraform to manage keys.
{: important}

1. Configure Terraform to use your Dedicated instance by setting the `IBMCLOUD_KP_API_ENDPOINT` environment variable:

   ```sh
   export IBMCLOUD_KP_API_ENDPOINT=https://<region>.kms.cloud.ibm.com
   ```
   {: pre}

2. Add the following to your `main.tf` file:

   ```terraform
   resource "ibm_kms_key" "root_key" {
     instance_id  = "<dedicated_instance_guid>"
     key_name     = "my-root-key"
     standard_key = false
     force_delete = true
   }

   output "root_key_id" {
     value       = ibm_kms_key.root_key.key_id
     description = "The ID of the root key"
   }
   ```
   {: codeblock}

3. Apply the configuration:

   ```terraform
   terraform apply
   ```
   {: pre}

For more advanced configurations:

```terraform
resource "ibm_kms_key" "root_key_advanced" {
  instance_id = "<dedicated_instance_guid>"
  key_name    = "my-root-key-advanced"
  standard_key = false
  
  # Optional: Add to a specific key ring
  key_ring_id = ibm_kms_key_rings.ring.key_ring_id
  
  # Optional: Set rotation policy
  rotation {
    interval_month = 3
  }
  
  # Optional: Set dual authorization policy
  dual_auth_delete {
    enabled = true
  }
}

# Create a key ring first
resource "ibm_kms_key_rings" "ring" {
  instance_id = "<dedicated_instance_guid>"
  key_ring_id = "my-key-ring"
}
```
{: codeblock}

For more information, see [Creating root keys](/docs/key-protect?topic=key-protect-create-root-keys) and [Setting up Terraform](/docs/key-protect?topic=key-protect-terraform-setup).

## Import your own keys (optional)
{: #getting-started-import-keys}

With Dedicated {{site.data.keyword.keymanagementserviceshort}}, you have complete Keep Your Own Key (KYOK) capabilities. You can import your existing encryption keys to maintain full control over your key material.

### Importing a key in the console
{: #getting-started-import-console}
{: ui}

1. From your Dedicated instance dashboard, click **Add**.

2. Select **Import your own key**.

3. Specify the key details:

   | Setting | Description |
   |---------|-------------|
   | Key type | Select **Root key** or **Standard key** |
   | Key name | Enter a human-readable name for your key |
   | Key material | The base64-encoded key material (16, 24, or 32 bytes) |
   | Key description | Optional. Add a description for the key |
   | Key ring | Optional. Select a key ring to organize your keys |
   {: caption="Import key settings" caption-side="bottom"}

4. Click **Add** to confirm.

### Importing a key with CLI
{: #getting-started-import-cli}
{: cli}

1. [Retrieve your authentication credentials](/docs/key-protect?topic=key-protect-set-up-cli).

2. Prepare your key material. Your key material must be a base64-encoded value that is 16, 24, or 32 bytes (128, 192, or 256 bits) in length. To generate a random 32-byte key material, run:

   ```sh
   KEY_MATERIAL=$(openssl rand -base64 32)
   ```
   {: pre}

3. Import the key by running the following command:

   ```sh
   ibmcloud kp key create <key_name> -i <instance_ID> -k $KEY_MATERIAL
   ```
   {: pre}

   Replace the variables in the example request according to the following table:

   | Variable | Description |
   |----------|-------------|
   | `key_name` | A human-readable name for your key. The name must be 2–90 characters in length. Avoid entering personally identifiable information (PII) as part of the key name. |
   | `instance_ID` | The unique identifier (GUID) assigned to your Dedicated instance. |
   | `KEY_MATERIAL` | The base64-encoded key material to import. You can also pass the value directly using the `-k` flag. |
   {: caption="Variables for importing a key with CLI" caption-side="bottom"}

   A successful response returns the ID and name of your imported key.

4. Optional: Verify that the key was imported by listing the keys in your instance.

   ```sh
   ibmcloud kp keys -i <instance_ID>
   ```
   {: pre}

For enhanced security, use an import token to encrypt your key material before importing. An import token ensures that your key material is protected in transit.

1. Create an import token:

   ```sh
   ibmcloud kp import-token create -i <instance_ID> -e 300 -m 10
   ```
   {: pre}

   Where `-e` sets the expiration time in seconds and `-m` sets the maximum number of retrievals.

2. Extract the nonce and public key from the import token:

   ```sh
   NONCE=$(ibmcloud kp import-token show -i <instance_ID> | jq -r '.["nonce"]')
   PUBLIC_KEY=$(ibmcloud kp import-token show -i <instance_ID> | jq -r '.["payload"]')
   ```
   {: pre}

3. Encrypt the key material and nonce using the public key:

   ```sh
   ibmcloud kp import-token key-encrypt -k $KEY_MATERIAL -p $PUBLIC_KEY
   ibmcloud kp import-token nonce-encrypt -k $KEY_MATERIAL -n $NONCE
   ```
   {: pre}

4. Import the root key using the encrypted values:

   ```sh
   ibmcloud kp key create <key_name> -i <instance_ID> -k $ENCRYPTED_KEY -n $ENCRYPTED_NONCE -v $IV
   ```
   {: pre}

For more information about import tokens, see [Using import tokens](/docs/key-protect?topic=key-protect-create-import-tokens).

### Importing a key with Terraform
{: #getting-started-import-terraform}
{: terraform}

You can import a key using Terraform.

```terraform
resource "ibm_kms_key" "imported_root_key" {
  instance_id  = "<dedicated_instance_guid>"
  key_name     = "my-imported-root-key"
  standard_key = false
  payload      = "<base64_encoded_key_material>"
  force_delete = true
}

output "imported_key_id" {
  value       = ibm_kms_key.imported_root_key.key_id
  description = "The ID of the imported root key"
}
```
{: codeblock}

For enhanced security, you can use an import token to encrypt your key material before importing. For more information, see [Using import tokens](/docs/key-protect?topic=key-protect-create-import-tokens) and [Importing root keys](/docs/key-protect?topic=key-protect-import-root-keys).

## Protect your data with envelope encryption
{: #getting-started-envelope-encryption}

After creating a root key, you can use it to protect your data encryption keys (DEKs) through envelope encryption. With Dedicated {{site.data.keyword.keymanagementserviceshort}}, all cryptographic operations are performed in your dedicated HSM partitions.

### Wrapping a data encryption key with CLI
{: #getting-started-wrap-key-cli}
{: cli}

Use the `kp key wrap` command to generate a new data encryption key (DEK) and protect it with a root key. {{site.data.keyword.keymanagementserviceshort}} performs the wrap operation in your dedicated HSM partitions.

You cannot wrap a standard key. Only root keys can be used for wrapping.
{: note}

1. [Retrieve your authentication credentials](/docs/key-protect?topic=key-protect-set-up-cli).

2. Retrieve the ID of the root key you want to use for wrapping:

   ```sh
   ibmcloud kp keys -i <instance_ID>
   ```
   {: pre}

3. Wrap a new DEK with the root key. If you omit the `-p` flag, {{site.data.keyword.keymanagementserviceshort}} generates a new DEK and returns it as ciphertext:

   ```sh
   ibmcloud kp key wrap <root_key_ID> -i <instance_ID>
   ```
   {: pre}

   To wrap an existing base64-encoded DEK, supply it using the `-p` flag:

   ```sh
   PLAINTEXT=$(openssl rand -base64 32)
   ibmcloud kp key wrap <root_key_ID> -i <instance_ID> -p $PLAINTEXT
   ```
   {: pre}

   Replace the variables in the example request according to the following table:

   | Variable | Description |
   |----------|-------------|
   | `root_key_ID` | The ID or alias of the root key to use for wrapping. |
   | `instance_ID` | The unique identifier (GUID) assigned to your Dedicated instance. |
   | `PLAINTEXT` | Optional. A base64-encoded DEK to wrap. If omitted, a new DEK is generated. |
   {: caption="Variables for wrapping a DEK with CLI" caption-side="bottom"}

   A successful response returns the ciphertext. Store the ciphertext alongside your encrypted data — it is required to retrieve the original DEK later.

   Do not save the plaintext DEK to persistent storage. Store only the ciphertext and use `kp key unwrap` to retrieve the DEK when needed.
   {: important}

### Unwrapping a data encryption key with CLI
{: #getting-started-unwrap-key-cli}
{: cli}

Use the `kp key unwrap` command to decrypt a previously wrapped DEK and recover the original plaintext key.

1. Unwrap the ciphertext using the same root key that was used for wrapping:

   ```sh
   ibmcloud kp key unwrap <root_key_ID> <ciphertext> -i <instance_ID>
   ```
   {: pre}

   Replace the variables in the example request according to the following table:

   | Variable | Description |
   |----------|-------------|
   | `root_key_ID` | The ID or alias of the root key that was used for the original wrap operation. |
   | `ciphertext` | The encrypted DEK returned by the wrap operation. |
   | `instance_ID` | The unique identifier (GUID) assigned to your Dedicated instance. |
   {: caption="Variables for unwrapping a DEK with CLI" caption-side="bottom"}

   A successful response returns the plaintext DEK in base64-encoded format.

2. Optional: To add extra protection, supply additional authentication data (AAD) during both wrap and unwrap. The same AAD values must be provided in the same order:

   ```sh
   ibmcloud kp key wrap <root_key_ID> -i <instance_ID> -a "my-app,production"
   ibmcloud kp key unwrap <root_key_ID> <ciphertext> -i <instance_ID> -a "my-app,production"
   ```
   {: pre}

   If you supply AAD on wrap, save it to a secure location. You must provide the same AAD in the same order on every subsequent unwrap request.
   {: important}

### Using envelope encryption with Terraform
{: #getting-started-envelope-terraform}
{: terraform}

While Terraform does not directly support wrap/unwrap operations, you can use the {{site.data.keyword.keymanagementserviceshort}} API within your application code to perform envelope encryption with keys created by Terraform.

```terraform
# Create a root key for envelope encryption
resource "ibm_kms_key" "envelope_root_key" {
  instance_id  = "<dedicated_instance_guid>"
  key_name     = "envelope-encryption-key"
  standard_key = false
  force_delete = true
}

# Output the key ID for use in your application
output "envelope_key_id" {
  value       = ibm_kms_key.envelope_root_key.key_id
  description = "Root key ID for envelope encryption operations"
  sensitive   = true
}

# Output the instance ID for API calls
output "kp_instance_id" {
  value       = "<dedicated_instance_guid>"
  description = "Dedicated Key Protect instance ID for API calls"
}
```
{: codeblock}

Your application can then use these outputs to perform wrap and unwrap operations via the {{site.data.keyword.keymanagementserviceshort}} API or SDKs.

For more information about envelope encryption, see [Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption), [Wrapping keys](/docs/key-protect?topic=key-protect-wrap-keys), and [Unwrapping keys](/docs/key-protect?topic=key-protect-unwrap-keys).

## Additional Terraform examples
{: #getting-started-terraform-examples}
{: terraform}

### Managing key rings
{: #getting-started-key-rings-terraform}

Create and manage key rings in your Dedicated instance:

```terraform
# Create a key ring
resource "ibm_kms_key_rings" "production_ring" {
  instance_id = "<dedicated_instance_guid>"
  key_ring_id = "production-keys"
}

# Create a key in the key ring
resource "ibm_kms_key" "prod_key" {
  instance_id  = "<dedicated_instance_guid>"
  key_name     = "production-root-key"
  standard_key = false
  key_ring_id  = ibm_kms_key_rings.production_ring.key_ring_id
}
```
{: codeblock}

### Setting key policies
{: #getting-started-key-policies-terraform}

Configure rotation and dual authorization policies:

```terraform
resource "ibm_kms_key" "key_with_policies" {
  instance_id  = "<dedicated_instance_guid>"
  key_name     = "key-with-policies"
  standard_key = false
  
  # Set rotation policy
  rotation {
    interval_month = 3
  }
  
  # Enable dual authorization for deletion
  dual_auth_delete {
    enabled = true
  }
}
```
{: codeblock}

### Creating key aliases
{: #getting-started-key-aliases-terraform}

Add aliases to your keys:

```terraform
resource "ibm_kms_key_alias" "key_alias" {
  instance_id = "<dedicated_instance_guid>"
  alias       = "my-key-alias"
  key_id      = ibm_kms_key.root_key.key_id
}
```
{: codeblock}

### Managing IAM policies
{: #getting-started-iam-terraform}

Grant access to your Dedicated {{site.data.keyword.keymanagementserviceshort}} instance:

```terraform
# Grant user access to Dedicated Key Protect instance
resource "ibm_iam_user_policy" "kp_user_policy" {
  ibm_id = "user@example.com"
  roles  = ["Manager", "Writer"]

  resources {
    service              = "kms"
    resource_instance_id = "<dedicated_instance_guid>"
  }
}

# Grant service-to-service authorization
resource "ibm_iam_authorization_policy" "cos_kp_policy" {
  source_service_name         = "cloud-object-storage"
  target_service_name         = "kms"
  target_resource_instance_id = "<dedicated_instance_guid>"
  roles                       = ["Reader"]
}
```
{: codeblock}

### Complete Terraform example
{: #getting-started-complete-terraform}

Here's a complete example for managing keys in a Dedicated instance:

```terraform
terraform {
  required_providers {
    ibm = {
      source  = "IBM-Cloud/ibm"
      version = "~> 1.59"
    }
  }
}

provider "ibm" {
  ibmcloud_api_key = var.ibmcloud_api_key
  region           = var.region
}

variable "ibmcloud_api_key" {
  description = "IBM Cloud API key"
  type        = string
  sensitive   = true
}

variable "region" {
  description = "IBM Cloud region"
  type        = string
  default     = "us-south"
}

variable "dedicated_instance_guid" {
  description = "GUID of the Dedicated Key Protect instance"
  type        = string
}

# Create key rings
resource "ibm_kms_key_rings" "production" {
  instance_id = var.dedicated_instance_guid
  key_ring_id = "production"
}

resource "ibm_kms_key_rings" "development" {
  instance_id = var.dedicated_instance_guid
  key_ring_id = "development"
}

# Create root keys
resource "ibm_kms_key" "prod_root_key" {
  instance_id  = var.dedicated_instance_guid
  key_name     = "prod-root-key"
  standard_key = false
  key_ring_id  = ibm_kms_key_rings.production.key_ring_id
  force_delete = true
  
  rotation {
    interval_month = 3
  }
  
  dual_auth_delete {
    enabled = true
  }
}

resource "ibm_kms_key" "dev_root_key" {
  instance_id  = var.dedicated_instance_guid
  key_name     = "dev-root-key"
  standard_key = false
  key_ring_id  = ibm_kms_key_rings.development.key_ring_id
  force_delete = true
}

# Create key aliases
resource "ibm_kms_key_alias" "prod_alias" {
  instance_id = var.dedicated_instance_guid
  alias       = "production-encryption-key"
  key_id      = ibm_kms_key.prod_root_key.key_id
}

# Outputs
output "prod_key_id" {
  value       = ibm_kms_key.prod_root_key.key_id
  description = "Production root key ID"
  sensitive   = true
}

output "dev_key_id" {
  value       = ibm_kms_key.dev_root_key.key_id
  description = "Development root key ID"
  sensitive   = true
}
```
{: codeblock}

For more Terraform examples and resources, see [Setting up Terraform](/docs/key-protect?topic=key-protect-terraform-setup) and the [IBM Cloud Terraform provider documentation](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs){: external}.


## Unsupported features
{: #getting-started-unsupported}

The following features are not supported by Dedicated {{site.data.keyword.keymanagementserviceshort}}:

- [Creating import tokens](/docs/key-protect?topic=key-protect-create-import-tokens)
- [Secure import key](/docs/key-protect?topic=key-protect-tutorial-import-keys&q=secure+import&tags=key-protect&offset=20#tutorial-import-retrieve-token) with import tokens
- `allowed_network policy`
- Deploying your instance anywhere but `us-south`
- PKCS#11 keystores
- Failover crypto units

## Next steps
{: #getting-started-next-steps}

Now that you have set up Dedicated {{site.data.keyword.keymanagementserviceshort}} and created your first keys, you can:

- [Integrate](/docs/key-protect?topic=key-protect-integrate-services) Dedicated {{site.data.keyword.keymanagementserviceshort}} with {{site.data.keyword.cloud_notm}} services 
- Learn about [key rotation](/docs/key-protect?topic=key-protect-key-rotation) to enhance security
- Explore [key rings](/docs/key-protect?topic=key-protect-grouping-keys) to organize your keys
- Set up [dual authorization](/docs/key-protect?topic=key-protect-manage-dual-auth) for key deletion
- Review [best practices](/docs/key-protect?topic=key-protect-key-protect-cli-reference#best-practices) for using {{site.data.keyword.keymanagementserviceshort}}
- Check out the [{{site.data.keyword.keymanagementserviceshort}} API reference](/apidocs/key-protect){: external}
- Explore the [IBM Cloud Terraform provider for Key Protect](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/kms_key){: external}

### Understanding Dedicated Key Protect
{: #getting-started-dedicated-features}

For more information about Dedicated {{site.data.keyword.keymanagementserviceshort}} features and capabilities, see [About Standard and Dedicated {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-about).

