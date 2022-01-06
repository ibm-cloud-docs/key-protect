---

copyright:
  years: 2017, 2022
lastupdated: "2022-01-06"

keywords: tutorial, Key Protect tutorial, secure import

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:preview: .preview}
{:term: .term}

# Tutorial: Creating and importing encryption keys
{: #tutorial-import-keys}

Learn how to create, encrypt, and bring your encryption keys to the cloud by
using {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

As a security professional for your organization, you're always looking for ways
to enhance the security of your at rest data on the cloud.

To comply with strict data governance and regulatory audit requirements, you
want to integrate your apps with a key management service that offers
fine-grained access control to encryption keys, audit trail capabilities, and
flexible options for uploading encryption keys that you generate on-premises.

With {{site.data.keyword.keymanagementserviceshort}}, you can create encryption
keys by using your internal key management system, and then upload those keys
for use on the cloud.

You can choose from
[different options for uploading keys](/docs/key-protect?topic=key-protect-importing-keys#plan-ahead)
based on your ongoing security needs. As you manage the lifecycle of encryption
keys, you control access to resources by using {{site.data.keyword.iamshort}},
and you monitor API activity to the service with {{site.data.keyword.at_short}}.

In this tutorial, you use an
[import token](/docs/key-protect?topic=key-protect-importing-keys#using-import-tokens)
to upload an encryption key to {{site.data.keyword.keymanagementserviceshort}}.
To learn more about your options for importing keys to
{{site.data.keyword.keymanagementserviceshort}}, see
[Planning ahead for importing key material](/docs/key-protect?topic=key-protect-importing-keys#plan-ahead).

To learn about importing a key without an import token, see
[Importing a root key](/docs/key-protect?topic=key-protect-import-root-keys)
{: note}

## Objectives
{: #tutorial-import-objectives}

This tutorial walks you through creating and securely importing encryption keys
into the {{site.data.keyword.keymanagementserviceshort}} service. It's
intended for users who are new to
{{site.data.keyword.keymanagementserviceshort}}, but who might have some
familiarity with key management systems. The following steps should take about
20 minutes to complete.

- Setting up the {{site.data.keyword.keymanagementserviceshort}} API

- Preparing your {{site.data.keyword.keymanagementserviceshort}} service
    instance to begin importing keys

- Creating and encrypting keys using the OpenSSL cryptography toolkit

- Importing an encrypted key to your
    {{site.data.keyword.keymanagementserviceshort}} instance

This tutorial won't incur any charges to your {{site.data.keyword.cloud_notm}}
account.

## Before you begin
{: #tutorial-import-prereqs}

To get started, you need the {{site.data.keyword.cloud_notm}} CLI so that you
can interact with services that you provision on
{{site.data.keyword.cloud_notm}}. You also need the `openssl` and `jq` packages
installed locally on your computer.

1. Create an
    [{{site.data.keyword.cloud_notm}} account](https://{DomainName}/){: external}.

2. Download and install the
    [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}
    for your operating system.

3. Download and install the
    [OpenSSL cryptography library](https://www.openssl.org/source/){: external}.

    You can use `openssl` commands to generate encryption keys on your local
    computer if you're trying out
    {{site.data.keyword.keymanagementserviceshort}} for the first time. This
    tutorial requires OpenSSL version `1.0.2r` or later.

    If you're using a Mac, you can download OpenSSL by using
    [Homebrew](https://brew.sh/){: external}.
    Run `brew install openssl` if you're installing the package for the first
    time, or run `brew upgrade openssl` to upgrade your existing package to the
    latest version.
    {: tip}

4. Download and install [jq](https://stedolan.github.io/jq/){: external}.

    `jq` helps you slice up JSON data. You use `jq` in this tutorial to capture
    specific data that's returned when you call the
    {{site.data.keyword.keymanagementserviceshort}} API.

## Step 1. Create a {{site.data.keyword.keymanagementserviceshort}} instance
{: #tutorial-import-provision-service}

After you set up an {{site.data.keyword.cloud_notm}} account, complete the
following steps to provision a {{site.data.keyword.keymanagementserviceshort}}
instance.

1. In a terminal window, run the following command to log in to
    {{site.data.keyword.cloud_notm}} with the
    [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.

    ```sh
    ibmcloud login
    ```
    {: pre}

    If the login fails, run the `ibmcloud login --sso` command to try again.
    The `--sso` parameter is required when you log in with a federated ID. If
    this option is used, go to the link listed in the CLI output to generate a
    one-time passcode.
    {: note}

2. Select the account and resource group where you would like to create a
    {{site.data.keyword.keymanagementserviceshort}} instance.

    In this tutorial, you interact with the Washington DC region. If you're
    logged into a different region, be sure to set Washington DC as your target
    region by running the following command.

    ```sh
    ibmcloud target -r us-east
    ```
    {: pre}

3. Provision an instance of {{site.data.keyword.keymanagementserviceshort}}
    within that account and resource group.

    ```sh
    ibmcloud resource service-instance-create "import-keys-demo" kms tiered-pricing us-east
    ```
    {: pre}

    This tutorial won't incur any charges to your
    {{site.data.keyword.cloud_notm}} account.
    {: note}

4. Optional: Verify that the {{site.data.keyword.keymanagementserviceshort}}
    instance was created successfully by listing your available
    {{site.data.keyword.keymanagementserviceshort}} instances.

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

    Success! You're now set with the
    {{site.data.keyword.keymanagementserviceshort}} instance where you
    can store and manage your encryption keys. Continue to the next step.

## Step 2. Set up the {{site.data.keyword.keymanagementserviceshort}} API
{: #tutorial-import-configure-api}

Now that you've provisioned an instance of
{{site.data.keyword.keymanagementserviceshort}}, you're ready to start using the
API.

{{site.data.keyword.keymanagementserviceshort}} provides a graphical user
interface and a REST API to create, track, and manage encryption keys. The
{{site.data.keyword.keymanagementserviceshort}} API requires a valid
{{site.data.keyword.cloud_notm}} IAM token and an instance ID to authenticate
with the service.

In this step, you use the {{site.data.keyword.cloud_notm}} CLI to gather the
authentication credentials you need to start interacting with
{{site.data.keyword.keymanagementserviceshort}} APIs. To retrieve and prepare
your credentials for later steps, you also set the credentials as environment
variables in your terminal.

1. In your terminal window, set the
    {{site.data.keyword.keymanagementserviceshort}} API endpoint as an
    environment variable.

    ```sh
    export KP_API_URL=https://<region>.kms.cloud.ibm.com
    ```
    {: pre}

2. Generate an {{site.data.keyword.cloud_notm}} access token using the
    {{site.data.keyword.keymanagementserviceshort}} CLI Plugin, and set it as an
    environment variable.

    The environment variable should begin with the authorization type,
    **`Bearer`**. The CLI command, as shown in the example, will automatically
    include the correct type.
    {: note}

    ```sh
    export ACCESS_TOKEN=`ibmcloud iam oauth-tokens | grep IAM | cut -d \: -f 2 | sed 's/^ *//'`
    ```
    {: pre}

    {{site.data.keyword.cloud_notm}} access tokens are valid for 1 hour, but you
    can regenerate them as needed. To generate a new access token, run the
    `ibmcloud iam oauth-tokens` command. To find out more about retrieving
    {{site.data.keyword.cloud_notm}} access tokens, see
    [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).
    {: note}

3. Retrieve the identifier that is associated with your
    {{site.data.keyword.keymanagementserviceshort}} instance, and then
    set the value as an environment variable.

    ```sh
    export INSTANCE_ID=`ibmcloud resource service-instance "import-keys-demo" --output json | jq -r '.[].guid'`
    ```
    {: pre}

4. Optional: Verify that the environment variables are set correctly by printing
    them to your terminal screen.

    ```sh
    $ echo $KP_API_URL
    https://us-east.kms.cloud.ibm.com
    $ echo $ACCESS_TOKEN
    Bearer eyJraWQiOiIyM...
    $ echo $INSTANCE_ID
    c1cf624b-6bed-4d4d-bd54-8e2534258a88
    ```
    {: screen}

    Success! You're now set with the service credentials that you need to
    authenticate to the {{site.data.keyword.keymanagementserviceshort}} API.
    Continue to the next step.

## Step 3. Create an import token
{: #tutorial-import-create-token}

With your service credentials, you can start interacting with the
{{site.data.keyword.keymanagementserviceshort}} APIs to create and bring your
encryption keys to the service.

In the following step, you create a
[import token](/docs/key-protect?topic=key-protect-importing-keys#using-import-tokens)
for your {{site.data.keyword.keymanagementserviceshort}} instance. By
creating an import token based on a policy that you specify, you enable extra
security for your encryption key while it's in flight to the service.

1. Using your terminal session, change into a new `key-protect-test` directory.

    ```sh
    mkdir key-protect-test && cd key-protect-test
    ```
    {: pre}

    You use this directory to store files for later steps.

2. Create an import token for your
    {{site.data.keyword.keymanagementserviceshort}} instance, and then
    save the response to a JSON file.

    ```sh
    $ curl -X POST \
        "$KP_API_URL/api/v2/import_token" \
        -H "accept: application/vnd.ibm.collection+json" \
        -H "authorization: $ACCESS_TOKEN" \
        -H "bluemix-instance: $INSTANCE_ID" \
        -H "content-type: application/json" \
        -d '{
                "expiration": 1200,
                "maxAllowedRetrievals": 1
            }' > createImportTokenResponse.json
    ```
    {: codeblock}

    In the request body, you can specify a policy on the import token that
    limits its use based on time and usage count. In this example, you set the
    expiration time for the import token to 1200 seconds (20 minutes), and you
    also allow only one retrieval of that token within the expiration time.
    {: tip}

3. View details for the import token.

    ```sh
    jq '.' createImportTokenResponse.json
    ```
    {: pre}

    The output displays the metadata that is associated with your import token,
    such as its creation date and policy details. The following snippet shows
    example output.

    ```json
    {
        "creationDate": "2019-04-08T16:58:29Z",
        "expirationDate": "2019-04-08T17:18:29Z",
        "maxAllowedRetrievals": 1,
        "remainingRetrievals": 1
    }
    ```
    {: screen}

## Step 4. Retrieve the import token
{: #tutorial-import-retrieve-token}

In the previous step, you created an import token and you viewed the metadata
that is associated with the token.

```json
{
    "creationDate": "2019-04-08T16:58:29Z",
    "expirationDate": "2019-04-08T17:18:29Z",
    "maxAllowedRetrievals": 1,
    "remainingRetrievals": 1
}
```
{: screen}

In this step, you retrieve the public encryption key and nonce value that are
associated with the import token. You need the public key to encrypt data in a
later step, and the nonce to verify your secure import request to the
{{site.data.keyword.keymanagementserviceshort}} service.

To retrieve the import token contents:

1. Retrieve the import token that you generated the previous step, and then save
    the response to a JSON file.

    ```sh
    $ curl -X GET \
        "$KP_API_URL/api/v2/import_token" \
        -H "accept: application/vnd.ibm.collection+json" \
        -H "authorization: $ACCESS_TOKEN" \
        -H "bluemix-instance: $INSTANCE_ID" > getImportTokenResponse.json
    ```
    {: codeblock}

2. Optional: Inspect the contents of the import token.

    ```sh
    jq '.' getImportTokenResponse.json
    ```
    {: pre}

    The output displays detailed information about the import token. The
    following snippet shows example output with truncated values.

    ```json
    {
        "creationDate": "2019-04-08T16:58:29Z",
        "expirationDate": "2019-04-08T17:18:29Z",
        "maxAllowedRetrievals": 1,
        "remainingRetrievals": 0,
        "payload": "Rm91ciBzY29yZSBhbmQgc2V2ZW4geWVhcnMgYWdv...",
        "nonce": "8zJE9pKVdXVe/nLb"
    }
    ```
    {: screen}

    The `payload` value represents the public key that is associated with the
    import token. This value is base64 encoded. The `nonce` value is used to
    verify the originality of a request to the service. You need to encrypt and
    provide this value when you import your encryption key in a later step.

3. Decode and save the public key to a file called `PublicKey.pem`.

    ```sh
    jq -r '.payload' getImportTokenResponse.json | base64 -o PublicKey.pem
    ```
    {: pre}

    The public key is now downloaded to your computer in PEM format. Continue to
    the next step.

## Step 5. Create an encryption key
{: #tutorial-import-create-key}

With {{site.data.keyword.keymanagementserviceshort}}, you can enable the
security benefits of Bring Your Own Key (BYOK) by creating and uploading your
own keys for use on {{site.data.keyword.cloud_notm}}.

In the following step, you create a 256-bit AES symmetric key on your local
computer.

This tutorial uses the OpenSSL cryptography toolkit to generate a pseudo-random
key, but you might want to
[explore different options](/docs/key-protect?topic=key-protect-importing-keys#plan-ahead)
for generating stronger keys based on your security needs. For example, you
might want to use your organization's internal key management system, backed by
an on-premises hardware security module (HSM), to create and export keys.
{: note}

1. In a terminal window, run the following `openssl` command to create a 256-bit
    encryption key.

    ```sh
    openssl rand 32 > PlainTextKey.bin
    ```
    {: pre}

    Success! Your encryption key is now saved in a file called
    `PlainTextKey.bin`. Continue to the next step.

## Step 6. Encrypt the nonce
{: #tutorial-import-encrypt-nonce}

To verify that the bits we receive are exactly the same as the bits that you
send as part of a request, {{site.data.keyword.keymanagementserviceshort}}
requires nonce verification when you upload the symmetric key to the service.

In cryptography, a nonce serves as a session token that checks the originality
of a request to protect against malicious attacks and unauthorized calls. By
using the same nonce that was distributed by
{{site.data.keyword.keymanagementserviceshort}}, you help to ensure that your
request to upload a key is valid. The nonce value must be encrypted by using the
same key that you want to import into the service.

To encrypt the nonce value:

1. Encode the key that you generated in the previous step, and set the encoded
    value as an environment variable.

    ```sh
    KEY_MATERIAL=$(base64 PlainTextKey.bin)
    ```
    {: pre}

2. Gather the nonce value that you retrieved in step 4.

    ```sh
    NONCE=$(jq -r '.nonce' getImportTokenResponse.json)
    ```
    {: pre}

3. Execute the following to encrypt the nonce value with the encryption key that you
    generated in step 5. Then, save the response to a file called
    `EncryptedValues.json`.

    ```sh
    ibmcloud kp import-token nonce-encrypt -k $KEY_MATERIAL -n $NONCE --output json > EncryptedValues.json
    ```
    {: pre}

6. Optional: Inspect the contents of the JSON file by using `jq` as shown.

    ```sh
    jq '.' EncryptedValues.json
    ```
    {: pre}

    The output displays the values that you need to provide for the next step.
    The following snippet shows example output with truncated values.

    ```json
    {
        "encryptedNonce": "DVy/Dbk37X8gSVwRA5U6vrHdWQy8T2ej+riIVw==",
        "iv": "puQrzDX7gU1TcTTx"
    }
    ```
    {: screen}

    The `encryptedNonce` value represents the original nonce that is wrapped
    (or encrypted) by the encryption key that you generated using OpenSSL. The
    `iv` value is the initialization vector (IV) that is created by the AES-GCM
    algorithm, and it's required later so that
    {{site.data.keyword.keymanagementserviceshort}} can successfully decrypt the
    nonce.

## Step 7. Encrypt the key
{: #tutorial-import-encrypt-key}

Next, use the public key that was distributed by
{{site.data.keyword.keymanagementserviceshort}} to encrypt the symmetric key
that you generated using OpenSSL.

1. Encrypt the generated key by using the the public key that you retrieved in
    step 4.

    ```sh
    openssl pkeyutl \
        -encrypt \
        -pubin \
        -keyform PEM \
        -inkey PublicKey.pem \
        -pkeyopt rsa_padding_mode:oaep \
        -pkeyopt rsa_oaep_md:sha256 \
        -in PlainTextKey.bin \
        -out EncryptedKey.bin
    ```
    {: pre}

    If you run into a parameter settings error when you run the `openssl`
    command on Mac OSX, you might need to ensure that OpenSSL is properly
    configured for your environment. If you installed OpenSSL by using Homebrew,
    run `brew update` and then `brew install openssl` to get the latest version.
    Then, run
    `export PATH="/usr/local/opt/openssl/bin:$PATH" >> ~/.bash_profile` to
    symlink the package. Open a new terminal session, and then run
    `which openssl && openssl version` to verify that the latest version of
    OpenSSL is available under the `/usr/local/` location. If you continue to
    encounter errors, be sure to use only the parameters that are listed in this
    example.
    {: tip}

    Success! Your encrypted key is now saved to a file called
    `EncryptedKey.bin`. You're all set to upload your encrypted key into
    {{site.data.keyword.keymanagementserviceshort}}. Continue to the next step.

## Step 8. Import the key
{: #tutorial-import-encrypted-key}

You can now import the encrypted key using the
{{site.data.keyword.keymanagementserviceshort}} API.

To import the key:

1. Gather the encrypted key, the encrypted nonce, and the initialization vector
    (IV) values.

    ```sh
    ENCRYPTED_KEY=$(openssl enc -base64 -A -in EncryptedKey.bin)
    ```
    {: pre}

    ```sh
    ENCRYPTED_NONCE=$(jq -r '.encryptedNonce' EncryptedValues.json)
    ```
    {: pre}

    ```sh
    IV=$(jq -r '.iv' EncryptedValues.json)
    ```
    {: pre}

2. Store the encrypted key in your
    {{site.data.keyword.keymanagementserviceshort}} instance by running
    the following `curl` command.

    ```sh
    $ curl -X POST \
        "$KP_API_URL/api/v2/keys" \
        -H "accept: application/vnd.ibm.collection+json" \
        -H "authorization: $ACCESS_TOKEN" \
        -H "bluemix-instance: $INSTANCE_ID" \
        -H "content-type: application/json" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.key+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                        "name": "encrypted-root-key",
                        "type": "application/vnd.ibm.kms.key+json",
                        "payload": "'"$ENCRYPTED_KEY"'",
                        "extractable": false,
                        "encryptionAlgorithm": "RSAES_OAEP_SHA_256",
                        "encryptedNonce": "'"$ENCRYPTED_NONCE"'",
                        "iv": "'"$IV"'"
                    }
                ]
            }' > createRootKeyResponse.json
    ```
    {: codeblock}

    In the request body, you provide the encryption key that you prepared in the
    previous step. You also supply the encrypted nonce and the IV values that
    are required to verify the request. Finally, the `extractable` value set to
    `false` designates your new key as a root key in the service that you can
    use for envelope encryption.

    {{site.data.keyword.keymanagementserviceshort}} receives
    your encrypted packet over the TLS 1.2 or 1.3 protocol. Within a hardware security
    module, the system uses the private key to decrypt the symmetric key.
    Finally, the system uses the symmetric key and the IV to decrypt the nonce
    and verify the request.

    If the API request fails with an import token expired error,
    [return to step 3](#tutorial-import-create-token)
    to create a new import token. Remember that import tokens and their
    associated public keys expire based on the policy that you specify at
    creation time.
    {: tip}

3. View details for the encryption key.

    ```sh
    jq '.' createRootKeyResponse.json
    ```
    {: pre}

    The following snippet shows an example output.

    ```json
    {
        "metadata": {
            "collectionType": "application/vnd.ibm.kms.key+json",
            "collectionTotal": 1
        },
        "resources": [
            {
                "id": "02fd6835-6001-4482-a892-13bd2085f75d",
                "type": "application/vnd.ibm.kms.key+json",
                "name": "encrypted-root-key",
                "state": 1,
                "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
                "extractable": false,
                "imported": true
            }
        ]
    }
    ```
    {: screen}

    The `id` value is a unique identifier that is assigned to your key and is
    used for subsequent calls to the
    {{site.data.keyword.keymanagementserviceshort}} API. The `state` value set
    to 1 indicates that your encryption key is now in the
    [_Active_ key state](/docs/key-protect?topic=key-protect-key-states).
    The `crn` value provides the full scoped path to the key that specifies
    where the resource resides within {{site.data.keyword.cloud_notm}}. Finally,
    the `extractable` and `imported` values describe this resource as a root key
    that you imported to the service.

4. Optional: Navigate to the
    [{{site.data.keyword.keymanagementserviceshort}} dashboard](/docs/key-protect?topic=key-protect-view-keys#view-keys-gui)
    to view and manage your encryption key.

    ![The image shows the {{site.data.keyword.keymanagementserviceshort}} dashboard view.](../images/import-keys-demo.png)

    You can browse the general characteristics of your keys from the application
    details page. Choose from a list of options for managing your key, such as
    [rotating the key](/docs/key-protect?topic=key-protect-rotate-keys#rotate-key-gui)
    or
    [deleting the key](/docs/key-protect?topic=key-protect-delete-keys#delete-key-gui).

## Step 9. Clean up
{: #tutorial-import-clean-up}

1. Gather the identifier for the encryption key that you imported in the
    previous step.

    ```sh
    ROOT_KEY_ID=$(jq -r '.resources[].id' createRootKeyResponse.json)
    ```
    {: pre}

2. Remove the encryption key from your
    {{site.data.keyword.keymanagementserviceshort}} instance.

    ```sh
    $ curl -X DELETE \
        "$KP_API_URL/api/v2/keys/$ROOT_KEY_ID" \
        -H "accept: application/vnd.ibm.collection+json" \
        -H "authorization: $ACCESS_TOKEN" \
        -H "bluemix-instance: $INSTANCE_ID" | jq .
    ```
    {: codeblock}

3. Remove the all the local files associated with this tutorial.

    ```sh
    rm kms-encrypt-nonce *.json *.bin *.pem
    ```
    {: pre}

4. Delete the test directory that you created for this tutorial.

    ```sh
    cd .. && rm -r key-protect-test
    ```
    {: pre}

5. Optional: Remove your {{site.data.keyword.keymanagementserviceshort}} service
    instance.

    ```sh
    ibmcloud resource service-instance-delete import-keys-demo
    ```
    {: pre}

    If you created more test keys in your
    {{site.data.keyword.keymanagementserviceshort}} instance, be sure to
    [remove all encryption keys from your instance](/docs/key-protect?topic=key-protect-delete-keys)
    before you delete or deprovision the instance.
    {: tip}

## Next steps
{: #tutorial-import-next-steps}

In this tutorial, you learned how to set up the
{{site.data.keyword.keymanagementserviceshort}} API, create an encryption key,
and securely import an encrypted key into your
{{site.data.keyword.keymanagementserviceshort}} instance.

- Learn more about
    [using your root key to protect data at rest](/docs/key-protect?topic=key-protect-envelope-encryption).

- Deploy your root key across
    [supported cloud services](/docs/key-protect?topic=key-protect-integrate-services).

- Learn more about the
    [{{site.data.keyword.keymanagementserviceshort}} API](/apidocs/key-protect){: external}.
