---

copyright:
  years: 2026
lastupdated: "2026-10-06"

keywords: PKCS11, PKCS #11, manage keystore, create keystore, list keystore, delete keystore, list objects, view object, delete object, Key Protect Dedicated

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Managing a keystore
{: #pkcs11-manage-keystore}
{: help}
{: support}

After you create and initialize a Dedicated {{site.data.keyword.keymanagementservicefull}} instance, you can use the {{site.data.keyword.cloud_notm}} console or the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in to create and manage PKCS #11 keystores, manage keystore users, and view or delete cryptographic objects such as keys and certificates.
{: shortdesc}

Each Dedicated instance supports a maximum of five PKCS #11 keystores.
{: important}

## Before you begin
{: #pkcs11-manage-keystore-prereqs}

Before you manage PKCS #11 keystores, ensure that you meet the following prerequisites:

- You have a provisioned and initialized {{site.data.keyword.keymanagementserviceshort}} Dedicated instance. If you do not have one, see [Creating a Dedicated {{site.data.keyword.keymanagementserviceshort}} instance](/docs/key-protect?topic=key-protect-provision-ded-instance).
- Your identity has the required IAM roles and policies on the Dedicated instance. To learn more, see [Roles and Cloud Identity and Access Management policies](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles-policies).
- The {{site.data.keyword.keymanagementserviceshort}} CLI plug-in is installed and configured to target your Dedicated instance (required for CLI operations). If it is not, see [Setting up the CLI](/docs/key-protect?topic=key-protect-set-up-cli).

### Required IAM actions
{: #pkcs11-manage-keystore-iam-actions}

The following table lists the IAM actions required for each keystore and object management task:

| Task | Required IAM action | Scope |
| --- | --- | --- |
| Create a keystore | `kms.keystore.create` | Instance |
| List keystores | `kms.keystore.list` | Instance |
| Delete a keystore | `kms.crypto-unit.send` | Instance |
| List objects in a keystore | `kms.keystore.list`, `kms.keystore.pkcs11-op` | Instance and keystore |
| View object details | `kms.keystore.pkcs11-op` | Instance and keystore |
| Delete objects | `kms.keystore.pkcs11-op` | Instance and keystore |
{: caption="Required IAM actions for PKCS #11 keystore operations" caption-side="bottom"}

## Managing keystores and objects
{: #pkcs11-manage-keystore-tabs}

Select the interface that you want to use to manage your keystores.




### Create a keystore
{: #pkcs11-create-keystore-ui}
{: ui}

Creating a PKCS #11 keystore provisions a dedicated cryptographic token within your {{site.data.keyword.keymanagementserviceshort}} Dedicated instance. Each keystore is identified by a unique UUID that you use when adding PKCS #11 users and configuring the PKCS #11 client. Each Dedicated instance supports a maximum of five keystores.

Keystore creation is supported through the CLI only. For step-by-step instructions, see [Create a keystore using the CLI](#pkcs11-create-keystore-cli).

### List keystores by using the UI
{: #pkcs11-list-keystores-ui}
{: ui}

To view all PKCS #11 keystores that are associated with your Dedicated instance by using the {{site.data.keyword.cloud_notm}} console, complete the following steps:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. In the navigation pane, click **PKCS #11 keystores**.

   The keystores table lists each keystore with its name, UUID, label, and creation date.

5. Note the keystore UUID. You need it when you add PKCS #11 users and configure the PKCS #11 client.

### Delete keystores by using the UI
{: #pkcs11-delete-keystore-ui}
{: ui}

Deleting a keystore is permanent and cannot be undone. All PKCS #11 objects stored in the keystore, including keys and certificates, are permanently deleted and cannot be recovered.
{: important}

To delete a PKCS #11 keystore by using the {{site.data.keyword.cloud_notm}} console, complete the following steps:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. In the navigation pane, click **PKCS #11 keystores**.

5. In the keystores table, expand the **Actions** menu next to the keystore that you want to delete, then select **Delete keystore**.

6. Review the confirmation message, then click **Delete** to confirm.

### List objects within a keystore by using the UI
{: #pkcs11-list-objects-ui}
{: ui}

You can view all PKCS #11 objects (such as keys and certificates) stored within a keystore by completing the following steps using the {{site.data.keyword.cloud_notm}} console:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. In the navigation pane, click **PKCS #11 keystores**.

5. Click the name of the keystore that you want to inspect.

   The objects table lists each object with its type, label, and identifier.

### View object details by using the UI
{: #pkcs11-view-object-ui}
{: ui}

To view the full attribute details for a specific PKCS #11 object by using the {{site.data.keyword.cloud_notm}} console, complete the following steps:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. In the navigation pane, click **PKCS #11 keystores**.

5. Click the name of the keystore that contains the object you want to view.

6. In the objects table, click the object label or identifier.

   A side panel opens and displays the full set of attributes for the object, including its class, key type (where applicable), and any user-defined attributes.

You must enter the credential of a user who is authorized to access the keystore before you can view the object details in the panel. This is required once per browser session.
{: note}

### Delete objects by using the UI
{: #pkcs11-delete-object-ui}
{: ui}

Deleting a PKCS #11 object is permanent. The object and its underlying key material are destroyed and cannot be recovered.
{: important}

To delete a PKCS #11 object by using the {{site.data.keyword.cloud_notm}} console, complete the following steps:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. In the navigation pane, click **PKCS #11 keystores**.

5. Click the name of the keystore that contains the object you want to delete.

6. In the objects table, expand the **Actions** menu next to the object that you want to delete, then select **Delete object**.

7. Review the confirmation message, then click **Delete** to confirm.

You must enter the credential of the user for the keystore to view the objects in the panel before you can see the details. This is required once per browser session.
{: note}

### Create a keystore by using the CLI
{: #pkcs11-create-keystore-cli}
{: cli}

To create a PKCS #11 keystore, run the following {{site.data.keyword.keymanagementserviceshort}} CLI command:

```sh
ibmcloud kp keystore create --name <keystore-name> --label <keystore-label>
```
{: codeblock}

Replace the variables according to the following table.

| Variable | Description |
| -------- | ----------- |
| `keystore-name` | **Required**. A unique name for the keystore within the instance. |
| `keystore-label` | **Required**. A label to associate with the keystore. |
{: caption="Variables for the create keystore CLI command" caption-side="bottom"}

Both `--name` and `--label` are required. The command fails with `required flag(s) "label" not set` or `required flag(s) "name" not set` if either is omitted.

**Common errors:**

| Error message | Cause |
| ------------- | ----- |
| `required flag(s) "label" not set` | The `--label` flag was not provided. |
| `required flag(s) "name" not set` | The `--name` flag was not provided. |
| `The quota for keystores in this instance has been reached and keystores cannot be created` | The Dedicated instance already has the maximum number of keystores (five). Delete an existing keystore before creating a new one. |
| `A keystore with this name already exists for this instance.` | A keystore with the specified name already exists. Use a different name. |
{: caption="Common errors for the create keystore CLI command" caption-side="bottom"}

### List keystores by using the CLI
{: #pkcs11-list-keystores-cli}
{: cli}

To list all PKCS #11 keystores in your Dedicated instance, run the following command:

```sh
ibmcloud kp keystores -i <instance-id>
```
{: codeblock}

Where `<instance-id>` is the unique identifier of your {{site.data.keyword.keymanagementserviceshort}} Dedicated instance. You can also set this value by using the `KP_INSTANCE_ID` environment variable.

The output lists all keystores that are associated with your Dedicated instance, including the keystore name, UUID, and label. Note the keystore UUID, because you need it when you add PKCS #11 users and configure the PKCS #11 client.

### Add users to the keystore by using the CLI
{: #pkcs11-add-users-cli}
{: cli}

Before you can make PKCS #11 API calls against a keystore, you must create a PKCS #11 Security Officer (SO) user and a normal user for that keystore. User and token management through the PKCS #11 protocol itself is not supported. You must manage users by using the CLI.
{: important}

#### Step 1: Identify the keystore UUID
{: #pkcs11-add-users-identify-cli}

Run the list keystores command to find the UUID of the keystore that you want to target:

```sh
ibmcloud kp keystores -i <instance-id>
```
{: codeblock}

Note the keystore UUID from the output. You need it in the steps that follow.

#### Step 2: Create the Security Officer
{: #pkcs11-create-so-cli}

The Security Officer (SO) user has elevated privileges for keystore administration, such as initializing the keystore, managing user credentials, and setting keystore-level policies. To create the SO user, run the following command:

```sh
ibmcloud kp crypto-unit user add \
  --type keystoreSO \
  --keystore-id <keystore-id> \
  --credential <temp-hmac-password> \
  --auth '[{"ADMIN": "<admin_key_file>#<password>"}]'
```
{: codeblock}

Replace the variables according to the following table.

| Variable | Description |
| -------- | ----------- |
| `keystore-id` | **Required**. The UUID of the keystore that you want to target. |
| `temp-hmac-password` | **Required**. The temporary password to assign to the SO user at creation. Omit `--credential` to be prompted for the password interactively. |
| `admin_key_file` | **Required**. The file path to your admin key file. |
| `password` | **Required**. The passphrase for the admin key file. Omit `#<password>` to be prompted for the passphrase interactively. |
{: caption="Variables for the create Security Officer command" caption-side="bottom"}

Because the SO user credential was handled by a crypto-unit admin during creation, the keystore will not permit use of this user until the confidentiality of the credential is restored. The SO user must change its own credential immediately after creation.

**Restore SO user credential confidentiality:**

1. List the crypto-unit users to identify the SO user name:

    ```sh
    ibmcloud kp crypto-unit users
    ```
    {: codeblock}

2. Run the following command in your instance (runs against all crypto units at once):

    ```sh
    ibmcloud kp crypto-unit user update-password \
      --auth "<SO_XXXX>#<temp-hmac-password>"
    ```
    {: codeblock}

    Replace `<SO_XXXX>` with the name of the SO user associated with your keystore, and `<temp-hmac-password>` with the credential that was assigned during creation.

3. If you want to update the password at the crypto unit level, run the following command against each crypto unit in your instance:

    ```sh
    ibmcloud kp crypto-unit user update-password \
      --cu '[{"CryptoUnitId": "fadedbee-0000-0000-0000-1234567890ab", "Auth": [{"SO_0184": "myCurrentP@ss"}]}]'
    ```
    {: codeblock}

#### Step 3: Create the normal user
{: #pkcs11-create-normal-user-cli}

The normal user performs standard cryptographic operations against the keystore. To create the normal user, run the following command:

```sh
ibmcloud kp crypto-unit user add \
  --type keystoreUser \
  --keystore-id <keystore-id> \
  --credential <temp-hmac-password> \
  --auth '[{"ADMIN": "<admin_key_file>#<password>"}]'
```
{: codeblock}

Replace the variables according to the following table.

| Variable | Description |
| -------- | ----------- |
| `keystore-id` | **Required**. The UUID of the keystore that you want to target. |
| `temp-hmac-password` | **Required**. The temporary password to assign to the normal user at creation. Omit `--credential` to be prompted for the password interactively. |
| `admin_key_file` | **Required**. The file path to your admin key file. |
| `password` | **Required**. The passphrase for the admin key file. Omit `#<password>` to be prompted for the passphrase interactively. |
{: caption="Variables for the create normal user command" caption-side="bottom"}

**Restore normal user credential confidentiality:**

1. List the crypto-unit users to identify the normal user name:

    ```sh
    ibmcloud kp crypto-unit users
    ```
    {: codeblock}

2. Run the following command against each crypto unit in your instance:

    ```sh
    ibmcloud kp crypto-unit user update-password \
      --auth '<USR_XXXX>#<temp-hmac-password>'
    ```
    {: codeblock}

    Replace `<USR_XXXX>` with the name of the normal user associated with your keystore, and `<temp-hmac-password>` with the credential that was assigned during creation. To be prompted for the old password interactively instead, use `--auth '<USR_XXXX>'`.

### Create objects in the keystore by using the CLI
{: #pkcs11-create-objects-cli}
{: cli}

PKCS #11 objects cannot be created through the {{site.data.keyword.keymanagementserviceshort}} REST API or CLI. Objects must be created by using the PKCS #11 library through a standard-compliant PKCS #11 client.
{: note}

The PKCS #11 client library is now hosted in a new location. Download the latest release from the [{{site.data.keyword.keymanagementserviceshort}} PKCS #11 releases page](https://github.com/IBM/keyprotect-go-client/releases#release-pkcs11-0.1.0){: external}.
{: note}

#### Set up the PKCS #11 configuration file
{: #pkcs11-create-objects-config-cli}

The PKCS #11 library requires a `client.toml` configuration file to locate your keystore and authenticate requests. Create `client.toml` in the same directory as your PKCS #11 client application, or specify a custom path by exporting the environment variable:

```sh
export IBM_PKCS11_GRPC_CLIENT_CFG_PATH=/path/to/custom/client.toml
```
{: codeblock}

Populate `client.toml` based on the following example:

```toml
### Logging and Retrying
[logging]
loglevel = "info"  # Supported levels: 'error', 'warn', 'info', 'debug', 'trace'. Default: 'warn'
logpath = ""       # The full path of your logging file

### IAM profiles
[iam_profiles.so_user]
api_key = "<IAM API key>"
endpoint = "https://iam.cloud.ibm.com"

[iam_profiles.normal_user]
api_key = "<IAM API key>"
endpoint = "https://iam.cloud.ibm.com"

[iam_profiles.anon_user]
api_key = "<IAM API key>"
endpoint = "https://iam.cloud.ibm.com"

### TLS profiles
[tls_profiles.tls_enabled]
enabled = true  # Required. TLS is required for all PKCS #11 requests.

### Slot configuration
[[slots]]
virtual_slot_id = 0
keystore_uuid = "<keystore-uuid>"
server_address = "<pkcs11-endpoint>:50011"
so_user_iam_profile = "so_user"
norm_user_iam_profile = "normal_user"
anon_user_iam_profile = "anon_user"
tls_profile = "tls_enabled"
```
{: codeblock}

Replace the variables according to the following table.

| Variable | Description |
| -------- | ----------- |
| `IAM API key` | **Required**. The IAM API key for the SO user, normal user, or anonymous user profile, as applicable. Tokens from this API key are used to authenticate PKCS #11 requests. The `iam_profile` used is based on the login status of the PKCS #11 session (that is, which user type is logged in). The API key must have permission to call `kms.keystore.pkcs11-op` and, optionally, any `pkcsOperation` that the given type of PKCS #11 session is going to call. |
| `keystore-uuid` | **Required**. The UUID of the keystore that this slot targets. |
| `pkcs11-endpoint` | **Required**. The PKCS #11 endpoint for your Dedicated instance. Determine your endpoint from the instance's endpoint list. The default endpoint type is `p11_public`. If your client runs from a device that meets private endpoint requirements, you may use `p11_private`. |
{: caption="Variables for the client.toml configuration file" caption-side="bottom"}

#### Supported object types
{: #pkcs11-create-objects-types-cli}

The `C_CreateObject` function supports the following object types:

- Secret key objects
- Private key objects
- Public key objects
- Data objects
- X.509 Public Key Certificate objects
- WTLS public key certificate objects
- X.509 Attribute Certificate objects

To generate keys, use the following PKCS #11 functions:

| Function | Purpose |
| -------- | ------- |
| `C_GenerateKey` | Generates a symmetric key (for example, an AES key). |
| `C_GenerateKeyPair` | Generates an asymmetric key pair (for example, RSA or EC). |
| `C_CreateObject` | Creates an object directly from provided key material. |
{: caption="PKCS #11 functions for object creation" caption-side="bottom"}

For the full list of supported functions and mechanisms, see [PKCS #11 capabilities and support in Key Protect](/docs/key-protect?topic=key-protect-pkcs11-functions).

#### Making PKCS #11 API calls
{: #pkcs11-create-objects-calls-cli}

After the configuration file is in place, initialize the PKCS #11 library in your client and call the standard PKCS #11 functions to generate, store, and list keys.

If the connection between your PKCS #11 client and the Dedicated instance's PKCS #11 service is lost for any reason, all further requests from the client will return `CKR_CRYPTOKI_NOT_INITIALIZED`. To re-establish the connection, call `C_Finalize` and then reinitialize the Cryptoki library.
{: important}

### Delete keystores by using the CLI
{: #pkcs11-delete-keystore-cli}
{: cli}

Deleting a keystore is permanent and cannot be undone. All PKCS #11 objects stored in the keystore, including keys and certificates, are permanently deleted and cannot be recovered.
{: important}

Before you delete a keystore, confirm that:

- The keystore and all objects it contains are no longer required. Deletion cannot be undone.
- All PKCS #11 users have been deleted from the keystore. For more information, see [Remove user association with a keystore](/docs/key-protect?topic=key-protect-pkcs11-manage-keystore#pkcs11-add-users-cli).

#### Step 1: Identify the keystore to delete
{: #pkcs11-identify-keystore-cli}

To list all keystores in your Dedicated instance and identify the UUID of the keystore that you want to delete, run the following command:

```sh
ibmcloud kp keystores -i <instance-id>
```
{: codeblock}

Where `<instance-id>` is the unique identifier of your {{site.data.keyword.keymanagementserviceshort}} Dedicated instance. You can also set this value by using the `KP_INSTANCE_ID` environment variable.

Note the keystore UUID from the output.

#### Step 2: Delete the keystore
{: #pkcs11-run-delete-cli}

To delete the keystore, run the following {{site.data.keyword.keymanagementserviceshort}} CLI command:

```sh
ibmcloud kp keystore delete <keystore-id> [--force]
```
{: codeblock}

Replace the variables according to the following table.

| Variable | Description |
| -------- | ----------- |
| `keystore-id` | **Required**. The UUID of the keystore that you want to delete. |
{: caption="Variables for the delete keystore CLI command" caption-side="bottom"}

The `--force` (`-f`) flag is optional. When specified, the keystore is deleted even if it still contains PKCS #11 objects. All PKCS #11 objects in the keystore are permanently deleted. The `--force` flag does not delete PKCS #11 users. You must delete all users before you delete the keystore regardless of whether you use `--force`.
{: note}

**Examples:**

Delete an empty keystore:

```sh
ibmcloud kp keystore delete 00000000-0000-0000-0000-000000000001
```
{: codeblock}

You can force delete a keystore and all of its objects by issuing:

```sh
ibmcloud kp keystore delete 00000000-0000-0000-0000-000000000001 --force
```
{: codeblock}

#### Step 3: Verify keystore deletion
{: #pkcs11-verify-delete-cli}

After you run the command, verify that the keystore was deleted successfully by listing your keystores again:

```sh
ibmcloud kp keystores -i <instance-id>
```
{: codeblock}

The deleted keystore should no longer appear in the output.

## What's next
{: #pkcs11-manage-keystore-next}

- For the full list of PKCS #11 functions and mechanisms that are supported by {{site.data.keyword.keymanagementserviceshort}}, see [PKCS #11 capabilities and support](/docs/key-protect?topic=key-protect-pkcs11-functions).
- To Activity Tracker Event for PKCS #11 operations through {{site.data.keyword.logs_full_notm}}, see [Activity Tracker Event Routing](/docs/key-protect?topic=key-protect-at-events).
- If you need a new keystore, keep in mind that each Dedicated instance supports a maximum of five keystores.
