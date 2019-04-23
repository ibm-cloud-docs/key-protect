---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: import root key, upload root key, import key-wrapping key, upload key-wrapping key, import CRK, import CMK, upload CRK, upload CMK, import customer key, upload customer key, key-wrapping key, root key API examples

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Importing root keys
{: #import-root-keys}

You can use {{site.data.keyword.keymanagementservicefull}} to secure your existing root keys by using the {{site.data.keyword.keymanagementserviceshort}} GUI, or programmatically with the {{site.data.keyword.keymanagementserviceshort}} API.
{: shortdesc}

Root keys are symmetric key-wrapping keys that are used to protect the security of encrypted data in the cloud. For more information about importing root keys into {{site.data.keyword.keymanagementserviceshort}}, see [Bringing your encryption keys to the cloud](/docs/services/key-protect?topic=key-protect-importing-keys).

## Importing root keys with the GUI
{: #import-root-key-gui}

[After you create an instance of the service](/docs/services/key-protect?topic=key-protect-provision), complete the following steps to add your existing root key with the {{site.data.keyword.keymanagementserviceshort}} GUI.

1. [Log in to the {{site.data.keyword.cloud_notm}} console ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/){: new_window}.
2. Go to **Menu** &gt; **Resource List** to view a list of your resources.
3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.
4. To import a key, click **Add key** and select the **Import your own key** window.

    Specify the key's details:

    <table>
      <tr>
        <th>Setting</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>Name</td>
        <td>
          <p>A unique, human-readable alias for easy identification of your key.</p>
          <p>To protect your privacy, ensure that the key name does not contain personally identifiable information (PII), such as your name or location.</p>
        </td>
      </tr>
      <tr>
        <td>Key type</td>
        <td>The <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">type of key</a> that you would like to manage in {{site.data.keyword.keymanagementserviceshort}}. From the list of key types, select <b>Root key</b>.</td>
      </tr>
      <tr>
        <td>Key material</td>
        <td>
          <p>The base64 encoded key material, such as an existing key-wrapping key, that you want to store and manage in the service.</p>
          <p>Ensure that the key material meets the following requirements:</p>
          <p>
            <ul>
              <li>The key must be 128, 192, or 256 bits.</li>
              <li>The bytes of data, for example 32 bytes for 256 bits, must be encoded by using base64 encoding.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Table 1. Describes the <b>Import your own key</b> settings</caption>
    </table>

5. When you are finished filling out the key's details, click **Import key** to confirm. 

## Importing root keys with the API
{: #import-root-key-api}

You can import your root keys into the service by using the {{site.data.keyword.keymanagementserviceshort}} API.

Plan ahead for importing keys by [reviewing your options for creating and encrypting key material](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead). For added security, you can enable the secure import of the key material by using a [transport key](/docs/services/key-protect?topic=key-protect-importing-keys#transport-keys) to encrypt your key material before you bring it to the cloud. If you prefer to import a root key without using a transport key, skip to [step 4](#import-root-key).
{: note}

### Step 1: Create a transport key
{: #create-transport-key}

Transport keys are currently a beta feature. Beta features can change at any time, and future updates might introduce changes are incompatible with the latest version.
{: important}

Create a transport key for your service instance by making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers
```
{: codeblock}

1. [Retrieve your service and authentication credentials to work with keys in the service](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Set a policy for your transport key by calling the [{{site.data.keyword.keymanagementserviceshort}} API ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/key-protect){: new_window}.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/json' \
      -d '{
     "expiration": <expiration_time>,  \
     "maxAllowedRetrievals": <use_count>  \
    }'
    ```
    {: codeblock}

 Replace the variables in the example request according to the following table.

  <table>
    <tr>
      <th>Variable</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><varname>region</varname></td>
      <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
    </tr>
    <tr>
      <td><varname>IAM_token</varname></td>
      <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
    </tr>
    <tr>
      <td><varname>instance_ID</varname></td>
      <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
    </tr>
    <tr>
      <td><varname>expiration_time</varname></td>
      <td>
        <p>The time in seconds from the creation of a transport key that determines how long the key remains valid.</p>
        <p>The minimum value is 300 seconds (5 minutes), and the maximum value is 86400 (24 hours). The default value is 600 (10 minutes).</p>
      </td>
    </tr>
    <tr>
      <td><varname>use_count</varname></td>
      <td>The number of times that a transport key can be retrieved within its expiration time before it is no longer accessible. The default value is 1.</td>
    </tr>
      <caption style="caption-side:bottom;">Table 2. Describes the variables that are needed to create a transport key {{site.data.keyword.keymanagementserviceshort}} API</caption>
  </table>

  A successful `POST api/v2/lockers` response returns the ID value for your transport key, along with other metadata. The ID is a unique identifier that is associated with a transport key and is used for subsequent calls to the {{site.data.keyword.keymanagementserviceshort}} API.

### Step 2: Retrieve the transport key and import token
{: #retrieve-transport-key}

Retrieve a transport key and import token by making a `GET` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers/<key_id>
```
{: codeblock}

1. Call the [{{site.data.keyword.keymanagementserviceshort}} API ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/key-protect){: new_window} with the following cURL command.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/locker/<locker_id> \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.
    
    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <tr>
        <td><varname>locker_ID</varname></td>
        <td><strong>Required.</strong> The identifier for the transport key that you created in <a href="#create-transport-key">step 1</a>.</td>
      </tr>
        <caption style="caption-side:bottom;">Table 3. Describes the variables that are needed to retrieve a transport key with the {{site.data.keyword.keymanagementserviceshort}} API</caption>
    </table>

    A successful `GET api/v2/lockers/{id}` response returns a 4096-bit, DER encoded public encryption key in PKIX format that you can use to encrypt your root key material, along with an import token that's used to verify the integrity of the transport key.

### Step 3: Encrypt your key material
{: #encrypt-root-key}

After you retrieve your transport key, use the key to encrypt the key material that you want to import into {{site.data.keyword.keymanagementserviceshort}}.  

<!-- TODO: Add link to tutorial that uses OpenSSL for key generation and encryption (in progress)-->

To generate key material on-premises, [review your options for creating symmetric encryption keys](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead). For example, you might want to use your organization's internal key management system, backed by an on-premises hardware security module (HSM), to create and export the key material.
{: note}

To encrypt your key material:

1. Export the 256-bit key material in binary format from your on-premises key management system.

    To learn how to to create and export key material, see the documentation for your on-premises HSM or key management system.

2. Use the [retrieved transport key](#retrieve-transport-key) from step 2 to encrypt the key material.

   When you encrypt your key material, use the `RSAES_OAEP_SHA_256` encryption scheme. This is the default scheme that {{site.data.keyword.keymanagementserviceshort}} uses to create the transport key. To avoid decryption issues in {{site.data.keyword.keymanagementserviceshort}}, do not include the optional `label` parameter when you run RSAES_OAEP encryption on key material. To learn how to run RSA encryption on your key material, see the documentation for your on-premises HSM or key management system.

3. Ensure that the encrypted key material is base64 encoded before you continue to the next step.

### Step 4: Import the key material
{: #import-root-key}

[After you encrypt and base64 encode the key material](#encrypt-root-key), import the root key to {{site.data.keyword.keymanagementserviceshort}} by making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. Call the [{{site.data.keyword.keymanagementserviceshort}} API ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/key-protect){: new_window} with the following cURL command.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.key+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.key+json",
       "name": "<key_alias>",
       "description": "<key_description>",
       "expirationDate": "<YYYY-MM-DDTHH:MM:SS.SSZ>",
       "payload": "<key_material>",
       "extractable": <key_type>,
       "encryptionAlgorithm": "<encryption_algorithm>",
       "importToken": "<import_token>"
       }
     ]
    }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.
    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>The unique identifier that is used to track and correlate transactions.</td>
      </tr>
      <tr>
        <td><varname>key_alias</varname></td>
        <td><strong>Required.</strong> A unique, human-readable name for easy identification of your key. To protect your privacy, do not store your personal data as metadata for your key.</td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>An extended description of your key. To protect your privacy, do not store your personal data as metadata for your key.</td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>The date and time that the key expires in the system, in RFC 3339 format. If the <code>expirationDate</code> attribute is omitted, the key does not expire.</td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>The base64 encoded key material, such as an existing key-wrapping key, that you want to store and manage in the service.</p>
          <p>Ensure that the key material meets the following requirements:</p>
          <p>
            <ul>
              <li>The key must be 128, 192, or 256 bits.</li>
              <li>The bytes of data, for example 32 bytes for 256 bits, must be encoded by using base64 encoding.</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>A boolean value that determines whether the key material can leave the service.</p>
          <p>When you set the <code>extractable</code> attribute to <code>false</code>, the service designates the key as a root key that you can use for <code>wrap</code> or <code>unwrap</code> operations.</p>
        </td>
      </tr>
      <tr>
        <td><varname>encryption_algorithm</varname></td>
        <td>The encryption scheme that you used to <a href="#encrypt-root-key">encrypt the key material</a>. Currently, <code>RSAES_OAEP_SHA_256</code> is supported. To import root key material without using a transport key and import token, omit the <code>encryption_algorithm</code> attribute.</td>
      </tr>
      <tr>
        <td><varname>import_token</varname></td>
        <td>The import token that is used to verify the liveliness and integrity of a transport key. If you encrypt your key material by using a transport key, you must supply the same import token that you retrieved in <a href="#retrieve-transport-key">step 2</a>. To import root key material without using a transport key and import token, omit the <code>importToken</code> attribute.</td>
      </tr>
        <caption style="caption-side:bottom;">Table 4. Describes the variables that are needed to add a root key with the {{site.data.keyword.keymanagementserviceshort}} API</caption>
    </table>

    To protect the confidentiality of your personal data, avoid entering personally identifiable information (PII), such as your name or location, when you add keys to the service. For more examples of PII, see section 2.2 of the [NIST Special Publication 800-122 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: new_window}.
    {: important}

    A successful `POST api/v2/keys` response returns the ID value for your key, along with other metadata. The ID is a unique identifier that is assigned to your key and is used for subsequent calls to the {{site.data.keyword.keymanagementserviceshort}} API.

2. Optional: Verify that the key was added by running the following call to browse the keys in your {{site.data.keyword.keymanagementserviceshort}} service instance.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## What's next
{: #import-root-key-next-steps}

- To find out more about protecting keys with envelope encryption, check out [Wrapping keys](/docs/services/key-protect?topic=key-protect-wrap-keys).
- To find out more about programmatically managing your keys, [check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/key-protect){: new_window}.