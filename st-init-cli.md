---

copyright:
  years: 2026
lastupdated: "2026-04-24"

keywords: single-tenant deploy, single-tenant-initialize, dedicated

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Initializing Dedicated Key Protect by creating an instance, credentials, and a master key
{: #st-init-cli}

For Dedicated {{site.data.keyword.keymanagementserviceshort}} to function, you must first [provision an instance](#st-init-cli-provision), then [generate admin credentials](#st-init-cli-generate-admin) used to operate your crypto units, and then [create and load the master key](#st-init-cli-crypto-units-master-key), which permits {{site.data.keyword.keymanagementserviceshort}} to perform cryptographic operations against the crypto units on your behalf.
{: shortdesc}

For more information about the key concepts for the Dedicated {{site.data.keyword.keymanagementserviceshort}} service, check out [About Standard and Dedicated {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-about).

## Before you begin
{: #st-init-cli-before-begin}

If you do not have the latest version of the [{{site.data.keyword.cloud_notm}} CLI](/docs/key-protect?topic=key-protect-set-up-cli), you might not be able to initialize your instance. Help ensure your initialization succeeds by updating to the latest version of the CLI plugin.

You must use the latest version of the CLI to complete the initialization, even if you deploy your instance using the console.
{: note}

## Provisioning your instance in the console
{: #st-init-cli-provision-console}

To provision your instance in the console, follow the instructions [here](/docs/key-protect?topic=key-protect-provision) and select the "Dedicated" tile in the catalog. The provisioning process can take several minutes.

Once your instance is provisioned, you are ready to [generate admin credentials and claim your crypto units](#st-init-cli-generate-admin).

If you do not specify a number of crypto units, your instance is provisioned with two. You can also specify three crypto units using the drop down. Whether you specify two or three crypto units, note that the value cannot be changed later.
{: note}

## Provisioning your instance in the CLI
{: #st-init-cli-provision}

Before crypto units can be created and your instance initialized, your instance must be created. First, set a resource group to target by issuing:

```{: pre}
ibmcloud target -c <resource-group>
```
{: codeblock}

If you don't know your resource group, you can find out which ones you have by issuing:

```{: pre}
ibmcloud resource groups
```
{: codeblock}

After you have your resource group set, create the instance by issuing:

```{: pre}
ibmcloud resource service-instance-create <INSTANCE_NAME> kms dedicated us-south 
ibmcloud resource service-instance-create <INSTANCE_NAME> kms dedicated us-south 
```
{: codeblock}

Where:

* `<INSTANCE_NAME>` is the name you give your instance.

Note that this command defaults you to two crypto units. You can specify three crypto units by issuing:

```{: pre}
ibmcloud resource service-instance-create <INSTANCE_NAME> kms dedicated us-south -p '{"crypto_units": 3}'
ibmcloud resource service-instance-create <INSTANCE_NAME> kms dedicated us-south -p '{"crypto_units": 3}'
```
{: codeblock}

If you specify a number of crypto units other than `2` or `3`, an error is returned. You cannot change the number of crypto units later.
{: important}

Provisioning a Dedicated instance can take several minutes. You can check on the status on your instance by issuing:

```{: pre}
ibmcloud resource service-instance <INSTANCE_NAME>
```
{: codeblock}

Where:

* `<INSTANCE_NAME>` is the name you gave your instance in the previous step.

The instance can have one of two states, **active** or **in progress**. Note that an **active** instance has nevertheless not yet been initialized, as that requires completing the remaining steps in this topic. Until you initialize your instance, it cannot be used, as your identities have not yet been configured with your crypto units to create the master key.

## Getting the endpoint
{: #st-init-cli-get-endpoint}

Once your instance is active, get the endpoint and GUID by issuing:

```{: pre}
ibmcloud resource service-instance <INSTANCE_NAME> -o json 
```
{: codeblock}

Where:  

* `<INSTANCE_NAME>` is the name you gave your instance in the previous step.

The endpoint is the `public` parameter value in the `endpoints` stanza of the json output above.  It takes the format `https://<instance-id>.api.<region>.kms.appdomain.cloud`. The GUID is the `GUID` parameter value in the output above. It takes the format of `UUID`.

You can get the endpoint by issuing: `ibmcloud resource service-instance <\kp-instance-id\> --output json | jq -r '.[].extensions.endpoints'`.
{: tip}

Save the full endpoint as an environment variable by issuing:

```{: pre}
export KP_TARGET_ADDR=<ST_INSTANCE_ENDPOINT>
```
{: codeblock}

And:

```{: pre}
export KP_INSTANCE_ID=<GUID>
```
  
Where:  

* `<ST_INSTANCE_ENDPOINT>` is the full endpoint of your instance, taking the format of `https://<instance-id>.api.<region>.kms.appdomain.cloud`.
* `<GUID>` is the instance ID from above output.

You are now ready to generate admin credentials.

You might have to wait for a few minutes after provisioning before your crypto units are available.
{: note}

For more information on the states a crypto unit can be in, check out [Crypto unit states](/docs/key-protect?topic=key-protect-crypto-unit-states).
{: tip}

## Generating admin credentials and claiming your crypto units
{: #st-init-cli-generate-admin}

A crypto unit is managed by an admin or admins, which means you either need to have identities available or create them. If you have properly formatted admin identities (a symmetric 256-bit AES key using RSA-2048), you can skip down to [Create the master key](#st-init-cli-crypto-units-master-key).

### Generating admin credentials
{: #st-init-cli-generate-admin-credentials}

If you need to create an admin credential, issue:

```{: pre}
ibmcloud kp crypto-unit sig-key generate --file <ADMIN_KEY_FILE> --passphrase <PWD> --algo RSA-2048 
```
{: codeblock}

Where:

* `<ADMIN_KEY_FILE>` is the location on your machine where the identity is created (for example, `admin-keyfile.key`).
* `<PWD>` is an optional password or passphrase that is used to encrypt the file at rest. 

Save a copy of this keyfile and remember the passphrase. It is required for all authenticated commands when interacting with the crypto units.
{: tip}

### Claiming your crypto units
{: #st-init-cli-generate-admin-claim}

For more information on the states a crypto unit can be in, check out [Crypto unit states](/docs/key-protect?topic=key-protect-crypto-unit-states).
{: tip}

Crypto units that are assigned to a user start in a [cleared state](/docs/key-protect?topic=key-protect-crypto-unit-states). All crypto units in a service instance need to be configured the same. If one availability zone in the region where your instance is located can't be accessed, the operational crypto units can be used interchangeably for load balancing or for high availability.
  
The master key in all crypto units in a single service instance must be set the same. The same set of administrators must be added in all crypto units, and all crypto units must initialize at the same time.  
  
To display the service instances and crypto units in the target resource group under the current user account, use the following command:

```{: pre}
ibmcloud kp crypto-units
```
{: codeblock}

The following output is an example that is displayed. The ID column in the output table identifies the crypto units that are targeted by later administrative commands that are issued by the KP CLI plug-in.

```bash
*******************************************************  
Id                                     InstanceID                             State  
6e0aead3-9d44-4c92-a4c4-f7a1ab415420   c28a8939-3980-4697-a80c-50b1f8bbf160   reserved  
3bb363fc-b1f9-4237-b37b-2c9e07784e3c   c28a8939-3980-4697-a80c-50b1f8bbf160   reserved  
*******************************************************  
```
  
The public part of the RSA key pair is placed in a certificate that is installed in the target crypto unit to define a crypto unit administrator. Use the claim command to upload it as the default admin of your crypto units, issue: 

```{: pre}
ibmcloud kp crypto-unit claim --credential <ADMIN_KEY_FILE>
```
{: codeblock}

Where: 

* `<ADMIN_KEY_FILE>` is the file where the identity was stored.

All `crypto-unit` commands apply to all of the crypto units. They are effectively clones of each other.
{: tip}

## Generating and importing the master key
{: #st-init-cli-crypto-units-master-key}

Because you are importing your master key credentials, {{site.data.keyword.keymanagementserviceshort}} has no access or backups of that key. Keep records of your master key in a safe location.
{: tip}

Now that you have created your instance and your admin identity, you can use them to create your master key. The master key, also known as HSM master key, is used to encrypt the service instance for key storage. It is a symmetric 256-bit AES key. With the master key, you take the ownership of the cloud HSM and own the root of trust that encrypts the entire hierarchy of encryption keys, including root keys and standard keys in the key management keystore. One service instance can have only one master key. If you delete the master key of the service instance, you can effectively crypto-shred all data that was encrypted with the keys that are managed in the service.

Dedicated {{site.data.keyword.keymanagementserviceshort}} uses the process of "key spliting", in which a cryptographic key is split into multiple pieces to enhance security. At least `2` "keyshares" must be created, though more can be used depending on the use case.
{: important}

To generate the master key locally, issue:

```{: pre}
ibmcloud kp crypto-unit mk generate --keyshare-files '["<KEYSHARE_FILE_1>#<PASSWORD1>", "<KEYSHARE_FILE_2>#<PASSWORD2>"]' --keyshare-minimum 2 --algo AES-256 --key-name <KEY_NAME> --auth '[{"ADMIN": "<ADMIN_KEY_FILE>#<PASSOWRD3>"}]' 
```
{: codeblock}

Where:

* `<KEYSHARE_FILE_1>#<PASSWORD1>` is the location of one of the keyshares, along with a passphrase for the file that is created. The passphrase is mandatory and must be between 6-255 characters.
* `<KEYSHARE_FILE_2>#<PASSWORD2>` is the location of another keyshare, along with a passphrase for the file that is created. Note that the passphrase is optional but if one is set, it must have at least six characters.
* `<KEY_NAME>` is the name of your master key.
* `<ADMIN_KEY_FILE>#<PASSOWRD3>` is the location of your admin and its passphrase you generated earlier (if you are not bringing your own identity).

Note that the `keyshare-minimum`, which is set to `2` by default but can be increased, represents the minimum number of keyshares (by their locations) you must specify.

To upload your master key to the crypto units of your instance, issue:

```{: pre}
ibmcloud kp crypto-unit mk import --keyshare-files '["<KEYSHARE_FILE_1>#<PASSWORD1>", "<KEYSHARE_FILE_2>#<PASSWORD2"]' --auth '[{"ADMIN": "<ADMIN_KEY_FILE>#<PASSWORD3>"}]'
```
{: codeblock}

Where:

* `<KEYSHARE_FILE_1>#<PASSWORD1>` is the location of one of the keyshares, along with a passphrase for the file that will be created. Note that the passphrase is optional but if one is set, it must have at least six characters.
* `<KEYSHARE_FILE_2>#<PASSWORD2` is the location of another keyshare, along with a passphrase for the file that will be created. Note that the passphrase is optional but if one is set, it must have at least six characters.
* `<ADMIN_KEY_FILE>#<PASSWORD3>` is the location of your admin and its passphrase you generated earlier (if you are not bringing your own identity).

Now that your master key has been created, you need to allow the Key Protect service the ability to perform actions on your crypto units (for example, to create keys). Note that the level of permissions granted Key Protect is lower than that of an admin.

```{: pre}
ibmcloud kp crypto-unit user add --type kmsCryptoUser --auth '[{"ADMIN": "<ADMIN_KEY_FILE>#<PASSWORD>"}]' 
```
{: codeblock}

Where:

* `<ADMIN_KEY_FILE>#<PASSWORD>` is the location of your admin key file and its passphrase as generated earlier (if you are not bringing your own identity).

This command can also be used to add admins to your crypto units by making your `--type` `admin` and adding a `--name` and `--file` that point to an admin identity you possess. Do not add `--name` or `--file` when adding `kmsCryptoUser`. For example:


```{: pre}
ibmcloud kp crypto-unit user add --type admin --name <USERNAME> --credential "<USERNAME_KEY_FILE>" --auth '[{"ADMIN": "<ADMIN_KEY_FILE>#<PWD>"}]'
```
{: codeblock}

Where: 

* `<USERNAME>` is the name of the admin identity you are adding.
* `<USERNAME_KEY_FILE>` is the file path of the credential to associate with the new user.
* `<ADMIN_KEY_FILE>#<PWD>` is the location of your existing admin and its passphrase you generated earlier (if you are not bringing your own identity).

Do not add `--name` or `--file` when adding `kmsCryptoUser` as an admin.
{: important}

Congratulations. Your instance has been fully initialized. 

It may take unto 5 to 10 minutes before you can use your instance.
{: note}

## Next steps
{: #st-init-cli-next-steps}

Now that your instance has been created, you have admin identities that can be used to operate it, and have created your master key and given Key Protect access to perform actions on your instance, you are ready to do things like:

* [Create a root key](/docs/key-protect?topic=key-protect-create-root-keys). You can have a maximum of 500 root or standard keys that are in any state, including `Destroyed`.
* [Create key rings](/docs/key-protect?topic=key-protect-grouping-keys). You can have a maximum of 50 key rings per service instance.
* [Set a rotation policy](/docs/key-protect?topic=key-protect-set-rotation-policy).

Import tokens are not supported by {{site.data.keyword.keymanagementserviceshort}} Dedicated.
{: important}

## Unsupported features
{: #st-init-cli-known-unsupported}

* [Creating import tokens](/docs/key-protect?topic=key-protect-create-import-tokens).
* [Secure import key](/docs/key-protect?topic=key-protect-tutorial-import-keys&q=secure+import&tags=key-protect&offset=20#tutorial-import-retrieve-token) with import tokens.
* `allowed_network policy`.
* Deploying your instance anywhere but `us-south`.
* PKCS#11 keystores.
* Failover crypto units.

## Troubleshooting
{: #st-init-cli-troubleshooting}

- Calls to {{site.data.keyword.keymanagementserviceshort}} [operations](/apidocs/key-protect) return HTTP 503 `no healthy upstream`: no crypto units are in kms-initialized state at this time. 

Possible causes:
    - User has not yet done Dedicated initialzation steps.
    - User has done Dedication initialization steps, but needs to give a new minutes for {{site.data.keyword.keymanagementserviceshort}} to pick up the newly kms-initialized crypto units.
    - User has only one crypto unit in kms-initialized state, and that crypto unit is down for maintenance.
    - User has uploaded mismatched MKs to one or more crypto units.

- CLI commands return `context deadline exceeded (Client.Timeout exceeded while awaiting headers)`: User has set `KP_TARGET_ADDR` to private endpoint. 

Fixes:
    - Use the public endpoint.
    - If user knows what private endpoint is and is using it intentionally, check out [Private endpoints](/docs/key-protect?topic=key-protect-regions#connectivity-options-private) for how to make calls against private endpoint.
- User was following the instructions in this topic, but any of `crypto-unit claim`, `crypto-unit mk import`, `crypto-unit user add --type kmsCryptoUser` cmds failed to apply to all crypto units.
    - Example:
    
    ```
    Executing operation Generate Master Key against CryptoUnit with ID fadedbee-0000-0000-0000-1234567890ab
    OK
    Executing operation Generate Master Key against CryptoUnit with ID addedace-0000-0000-0000-1234567890ab
    FAILED
    ```
    
    - By default, `claim`, `mk import`, `user add` cmds will attempt to apply to all crypto units. If these cmds were only partially successful (applied to only a subset of the crypto units in the instance), recommendation is to retry the command only against the crypto unit(s) that returned failure. Each of these cmds can be configured to target specific crypto units(s). To determine how to do so, append `-h` to any `crypto-unit` cmd to view helptext or refer to [CLI reference](/docs/key-protect?topic=key-protect-key-protect-cli-reference)
    - Call `kp crypto-units` cmd in the [CLI reference](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-crypto-units) to confirm all CUs are in the same state.
        - If CU states are mismatched, follow the crypto unit states guide (link to it here?)
        - If any crypto unit is in `maintenance` state, any `kp crypto-unit` cmds against the instance should be tried again at a later time.
