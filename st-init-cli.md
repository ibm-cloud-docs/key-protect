---

copyright:
  years: 2025
lastupdated: "2025-10-15"

keywords: single-tenant deploy, single-tenant-initialize

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Initializing single-tenant Key Protect by creating an instance, credentials, and a master key
{: #st-init-cli}

This topic is in progress.
{: important}

For single-tenant {{site.data.keyword.keymanagementserviceshort}} to function, you must first [provision an instance](#st-init-cli-provision), then [generate admin credentials](#st-init-cli-generate-admin) used to operate your crypto units, and then [create and load the master key](#st-init-cli-crypto-units-master-key), which ties together your crypto units and created admin or admins.
{: shortdesc}

For more information about the key concepts for the single-tenant {{site.data.keyword.keymanagementserviceshort}} service, check out [Single-tenant important concepts](/docs/key-protect?topic=key-protect-about-st#about-st-concepts).

## Before you begin
{: #st-init-cli-before-begin}

If you do not have the latest version of the {{site.data.keyword.cloud_notm}} CLI, you might not be able to initialize your instance. Help ensure your initialization succeeds by [updating to the latest version](/docs/key-protect?topic=key-protect-set-up-cli#update-cli) and logging into the CLI in staging:

   ```sh
   ibmcloud login -a test.cloud.ibm.com
   ```
   {: pre}

Now you are ready to provision your instance.

If your attempt to update the CLI fails, check out the [Known issues](#st-init-cli-known-issues) for potential solutions to try.
{: tip}

## Provisioning your instance in the console
{: #st-init-cli-provision-console}

To provision your instance in the console, follow the instructions [here](/docs/key-protect?topic=key-protect-provision) and select the "Single-tenant" tile in the catalog. The provisioning process can take several minutes.

Once your instance is provisioned, you are ready to [generate admin credentials and claim your crypto units](#st-init-cli-generate-admin).

Your instance has two crypto units. This value cannot be changed.
{: note}

## Provisioning your instance in the CLI
{: #st-init-cli-provision}

Before crypto units can be created and your instance initialized, your instance must be created. To do that in the CLI, issue:

```{: pre}
$ ibmcloud resource service-instance-create <INSTANCE_NAME> kms premium us-south 
```
{: codeblock}

Where:

* `<INSTANCE_NAME>` is the name you give your instance.

For the limited beta, you must use `premium` (the name of the plan) and `us-south`, the only region where it can be deployed.
{: note}

Provisioning a single-tenant instance can take several minutes. You can check on the status on your instance by issuing:

```{: pre}
$ ibmcloud resource service-instance <INSTANCE_NAME>
```
{: codeblock}

Where:

* `<INSTANCE_NAME>` is the name you gave your instance in the previous step.

The instance can have one of two states, **active** or **in progress**. Note that an **active** instance has nevertheless not yet been initialized, as that requires completing the remaining steps in this topic. Until you initialize your instance, it cannot be used, as your identities have not yet been configured with your crypto units to create the master key.

Once your instance is active, get the endpoint and GUID by issuing:

```{: pre}
$ ibmcloud resource service-instance <INSTANCE_NAME> -o json 
```
{: codeblock}

Where:  

* `<INSTANCE_NAME>` is the name you gave your instance in the previous step.

The endpoint is the `public` parameter value in the `endpoints` stanza of the json output above.  It takes the format `https://<instance-id>.api.<region>.kms.appdomain.cloud`. The GUID is the `GUID` parameter value in the output above. It takes the format of `UUID`.

Save the full endpoint as an environment variable by issuing:

```{: pre}
$ export KP_PRIVATE_ADDR=<ST_INSTANCE_ENDPOINT>
```
{: codeblock}

And:

```{: pre}
$ export KP_INSTANCE_ID=<GUID>
```
  
Where:  

* `<ST_INSTANCE_ENDPOINT>` is the full endpoint of your instance, taking the format of `https://<instance-id>.api.<region>.kms.appdomain.cloud`.
* `<GUID>` is the `guid` value from above output.

You are now ready to generate admin credentials.

Your instance has two crypto units. This value cannot be changed.
{: note}

## Generating admin credentials and claiming your crypto units
{: #st-init-cli-generate-admin}

A crypto unit is managed by an admin or admins, which means you either need to have identities available or create them. If you have properly formatted admin identities (a symmetric 256-bit AES key using RSA-2048), you can skip down to [Create the master key](#st-init-cli-crypto-units-master-key). To learn more about RSA master keys, check out [RSA signature authentication keys](/docs/key-protect?topic=key-protect-about-st#about-st-concepts-rsa).

### Generating admin credentials
{: #st-init-cli-generate-admin-credentials}

If you need to create an admin credential, issue:

```{: pre}
$ ibmcloud kp crypto-unit sig-key generate --file <ADMIN_KEY_FILE> --passphrase <PWD> --algo RSA-2048 
```
{: codeblock}

Where:

* `<ADMIN_KEY_FILE>` is the location on your machine where the identity is created (for example, `admin-keyfile.key`).
* `<PWD>` is an optional password or passphrase that is used to encrypt the file at rest. 

Save a copy of this keyfile and remember the passphrase. It is required for all authenticated commands when interacting with the crypto units.
{: tip}

### Claiming your crypto units
{: #st-init-cli-generate-admin-claim}

Crypto units that are assigned to a user start in a cleared state. All crypto units in a service instance need to be configured the same. If one availability zone in the region where your instance is located can't be accessed, the operational crypto units can be used interchangeably for load balancing or for high availability.
  
The master key in all crypto units in a single service instance must be set the same. The same set of administrators must be added in all crypto units, and all crypto units must initialize at the same time.  
  
To display the service instances and crypto units in the target resource group under the current user account, use the following command:

```{: pre}
$ ibmcloud kp crypto-units
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

For beta, the state of each of the crypto unit shows reserved even after you claim the crypto-units in later stages. All crypto-unit specific operations are applied in parallel to maintain the same configurations.
{: note}
  
The public part of the RSA key pair is placed in a certificate that is installed in the target crypto unit to define a crypto unit administrator. Use the claim command to upload it as the default admin of your crypto units, issue: 

```{: pre}
$ ibmcloud kp crypto-unit claim --credential <ADMIN_KEY_FILE> -i <INSTANCE_GUID>
```
{: codeblock}

Where: 

* `<ADMIN_KEY_FILE>` is the file where the identity was stored.
* `<INSTANCE_GUID>` is the ID of your instance.

## Creating the master key
{: #st-init-cli-crypto-units-master-key}

Now that you have created your instance and your admin identity, you can use them to create your master key. The master key, also known as HSM master key, is used to encrypt the service instance for key storage. It is a symmetric 256-bit AES key. With the master key, you take the ownership of the cloud HSM and own the root of trust that encrypts the entire hierarchy of encryption keys, including root keys and standard keys in the key management keystore. One service instance can have only one master key. If you delete the master key of the service instance, you can effectively crypto-shred all data that was encrypted with the keys that are managed in the service.

Single-tenant Key Protect uses the process of "key spliting", in which a cryptographic key is split into multiple pieces to enhance security. At least `2` "keyshares" must be created, though more can be used depending on the use case. For the beta, the maximum number of keyshares supported are 3.
{: important}

To generate the master key locally, issue:

```{: pre}
$ ibmcloud kp crypto-unit mk generate --keyshare-files '["<KEYSHARE_FILE_1>$<PASSWORD1>", "<KEYSHARE_FILE_2>#<PASSWORD2>"]' --keyshare-minimum 2 --algo AES-256 --keyname <KEY_NAME> --auth '[{"ADMIN": "<ADMIN_KEY_FILE>#<PASSOWRD3>"}]' -i $KP_INSTANCE_ID 
```
{: codeblock}

Where:

* `<KEYSHARE_FILE_1>$<PASSWORD1>` is the location of one of the keyshares, along with a passphrase for the file that is created. Note that the passphrase is optional but if one is set, it must have at least six characters.
* `<KEYSHARE_FILE_2>#<PASSWORD2>` is the location of another keyshare, along with a passphrase for the file that is created. Note that the passphrase is optional but if one is set, it must have at least six characters.
* `<KEY_NAME>` is the name of your master key.
* `<ADMIN_KEY_FILE>#<PASSOWRD3>` is the location of your admin and its passphrase you generated earlier (if you are not bringing your own identity).
* `$KP_INSTANCE_ID` is the environment variable holding the GUID of your instance.

Note that the `keyshare-minimum`, which is set to `2` by default but can be increased, represents the minimum number of keyshares (by their locations) you must specify. For the beta, it must match the # of key share files you specify.

To upload your master key to the crypto units of your instance, issue:

```{: pre}
$ ibmcloud kp crypto-unit mk import --keyshare-files '["<KEYSHARE_FILE_1>#<PASSWORD1>", "<KEYSHARE_FILE_2>#<PASSWORD2"]' --auth '["ADMIN": "<ADMIN_KEY_FILE>#<PASSWORD3>"]' -i $KP_INSTANCE_ID
```
{: codeblock}

Where:

* `<KEYSHARE_FILE_1>#<PASSWORD1>` is the location of one of the keyshares, along with a passphrase for the file that will be created. Note that the passphrase is optional but if one is set, it must have at least six characters.
* `<KEYSHARE_FILE_2>#<PASSWORD2` is the location of another keyshare, along with a passphrase for the file that will be created. Note that the passphrase is optional but if one is set, it must have at least six characters.
* `<ADMIN_KEY_FILE>#<PASSWORD3>` is the location of your admin and its passphrase you generated earlier (if you are not bringing your own identity).
* `$KP_INSTANCE_ID` is the environment variable holding the GUID of your instance.

Now that your master key has been created, you need to allow the Key Protect service the ability to perform actions on your crypto units (for example, to create keys). Note that the level of permissions granted Key Protect is lower than that of an admin.

```{: pre}
$ ibmcloud kp crypto-unit user add --type kmsCryptoUser --credential '["ADMIN": "<ADMIN_KEY_FILE>#<PASSWORD>"]' -i $KP_INSTANCE_ID 
```
{: codeblock}

Where:

* `<ADMIN_KEY_FILE>#<PASSWORD>` is the location of your admin key file and its passphrase as generated earlier (if you are not bringing your own identity).
* `$KP_INSTANCE_ID` is the environment variable holding the `GUID` of your instance.

This command can also be used to add admins to your crypto units by making your `--type` `admin` and adding a `--name` and `--file` that point to an admin identity you possess. Do not add `--name` or `--file` when adding `kmsCryptoUser`. For example:

```{: pre}
$ ibmcloud kp crypto-unit user add --type admin --name <USERNAME> --credential “<USERNAME_KEY_FILE>#<USER_KEY_FILE_PWD>” --auth '[{"ADMIN": “<ADMIN_KEY_FILE>#<PWD>”}]’ -i $KP_INSTANCE_ID
```
{: codeblock}

Where: 

* `<USERNAME>` is the name of the admin identity you are adding.
* `<USERNAME_KEY_FILE>#<USER_KEY_FILE_PWD>` is the file path of the credential to associate with the new user.
* `<ADMIN_KEY_FILE>#<PWD>` is the location of your existing admin and its passphrase you generated earlier (if you are not bringing your own identity).

Do not add `--name` or `--file` when adding `kmsCryptoUser` as an admin.
{: important}

Congratulations. Your instance has been fully initialized. 

It may take unto 5 to 10 minutes before your crypto units can be used.
{: note}

## Next steps
{: #st-init-cli-next-steps}

Now that your instance has been created, you have admin identities that can be used to operate it, and have created your master key and given Key Protect access to perform actions on your instance, you are ready to do things like:

* [Create a root key](/docs/key-protect?topic=key-protect-create-root-keys).
* [Create key rings](/docs/key-protect?topic=key-protect-grouping-keys).
* [Set a rotation policy](/docs/key-protect?topic=key-protect-set-rotation-policy).

## Known issues
{: #st-init-cli-known-issues}

1. CLI commands don't work at all. Make sure you are:

* On the IBM Global Protect VPN.
* Using a CLI installed in a Linux environment.
* An internal IBM employee who can access `test.cloud`.
* If your attempt to upgrade does not work, try upgrading by using this command: `ibmcloud plugin install https://plugins.test.cloud.ibm.com/downloads/bluemix-plugins/key-protect/0.11.0/key-protect-linux-386-0.11.0`.
  
2. Unable to create a signature file. Invalid file name.

```bash
FAILED
command failed with error code: b9061003
```

3. You can only claim crypto using once. Once successfully claimed, you will get following error.

```bash
FAILED  
returned error status code 500
```

4. Authentication failiue due to token expiration. Perform `ibmcloud login` again. 

```bash
FAILED
list cryptounits failed: error calling list crypto units endpoint: returned error status code 400
```

5. Instance ID cannot be empty. Specify instance id.

```bash
FAILED
instance id cannot be empty
```
