---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-22"

keywords: import symmetric key, upload symmetric key, import root key, upload root key, import key-wrapping key, upload key-wrapping key, import CRK, import CMK, upload CRK, upload CMK, import customer key, upload customer key, key-wrapping key, root key API examples

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
{:term: .term}

# Importing root keys
{: #import-root-keys}

You can use {{site.data.keyword.keymanagementservicefull}} to secure your
existing root keys by using the {{site.data.keyword.keymanagementserviceshort}}
GUI, or programmatically with the
{{site.data.keyword.keymanagementserviceshort}} API.
{: shortdesc}

Root keys are symmetric key-wrapping keys that are used to protect the security
of encrypted data in the cloud. For more information about importing root keys
into {{site.data.keyword.keymanagementserviceshort}}, see
[Bringing your encryption keys to the cloud](/docs/key-protect?topic=key-protect-importing-keys).

Plan ahead for importing keys by
[reviewing your options for creating and encrypting key material](/docs/key-protect?topic=key-protect-importing-keys#plan-ahead).
For added security, you can enable the secure import of the key material by
using an
[import token](/docs/key-protect?topic=key-protect-importing-keys#using-import-tokens)
to encrypt your key material before you bring it to the cloud.
{: note}

## Importing root keys in the console
{: #import-root-key-gui}

[After you create an instance of the service](/docs/key-protect?topic=key-protect-provision),
complete the following steps to import a key with the
{{site.data.keyword.cloud_notm}} console.

If you enable
[dual authorization settings for your {{site.data.keyword.keymanagementserviceshort}} instance](/docs/key-protect?topic=key-protect-manage-dual-auth),
keep in mind that any keys that you add to the service require an authorization
from two users to delete keys.
{: note}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.
2. Go to **Menu** &gt; **Resource List** to view a list of your resources.
3. From your {{site.data.keyword.cloud_notm}} resource list, select your
   provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.
4. To import a key, click **Add key** and select the **Import your own key**
   window.

    Specify the key's details:

    <table>
      <tr>
        <th>Setting</th>
        <th>Description</th>
      </tr>

      <tr>
        <td>Name</td>
        <td>
          <p>
            A human-readable alias for easy identification of your key. Length
            must be within 2 - 90 characters.
          </p>
          <p>
            To protect your privacy, ensure that the key name does not contain
            personally identifiable information (PII), such as your name or
            location.
          </p>
        </td>
      </tr>

      <tr>
        <td>
          Key type
        </td>
        <td>
          The
          [type of key](/docs/key-protect?topic=key-protect-envelope-encryption#key-types)
          that you would like to manage in
          {{site.data.keyword.keymanagementserviceshort}}. From the list of key
          types, select <b>Root key</b>.
        </td>
      </tr>

      <tr>
        <td>
          Key material
        </td>
        <td>
          <p>
            The base64 encoded key material, such as an existing key-wrapping
            key, that you want to store and manage in the service. For more
            information, check out
            [Base64 encoding your key material](#how-to-encode-root-key-material).
          </p>
          <p>
            Ensure that the key material meets the following requirements:
          </p>
          <p>
            <ul>
              <li>
                The key must be 128, 192, or 256 bits.
              </li>
              <li>
                The bytes of data, for example 32 bytes for 256 bits, must be
                encoded by using base64 encoding.
              </li>
            </ul>
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the <b>Import your own key</b> settings
      </caption>
    </table>

5. When you are finished filling out the key's details, click **Import key** to
   confirm.

## Importing root keys with the API
{: #import-root-key-api}

Import symmetric keys to {{site.data.keyword.keymanagementserviceshort}} by
making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Retrieve your service and authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Call the
   [{{site.data.keyword.keymanagementserviceshort}} API](/apidocs/key-protect){: external}
   with the following cURL command.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.key+json" \
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
                        "extractable": <key_type>
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>

      <tr>
        <td>
          <varname>region</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The region abbreviation, such as
            <code>us-south</code> or <code>eu-gb</code>, that represents the
            geographic area where your
            {{site.data.keyword.keymanagementserviceshort}} instance
            resides.
          </p>
          <p>
            For more information, see
            [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>IAM_token</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> Your {{site.data.keyword.cloud_notm}}
            access token. Include the full contents of the <code>IAM</code>
            token, including the Bearer value, in the cURL request.
          </p>
          <p>
            For more information, see
            [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>instance_ID</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The unique identifier that is assigned to
            your {{site.data.keyword.keymanagementserviceshort}} service
            instance.
          </p>
          <p>
            For more information, see
            [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>correlation_ID</varname>
        </td>
        <td>
          The unique identifier that is used to track and correlate
          transactions.
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_alias</varname>
        </td>
        <td>
          <strong>Required.</strong> A unique, human-readable name for easy
          identification of your key. To protect your privacy, do not store your
          personal data as metadata for your key.
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_description</varname>
        </td>
        <td>
          An extended description of your key. To protect your privacy, do not
          store your personal data as metadata for your key.
        </td>
      </tr>

      <tr>
        <td>
          <varname>YYYY-MM-DD</varname>
          <br>
          <varname>HH:MM:SS.SS</varname>
        </td>
        <td>
          The date and time that the key expires in the system, in RFC 3339
          format. If the <code>expirationDate</code> attribute is omitted, the
          key does not expire.
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_material</varname>
        </td>
        <td>
          <p>
            The base64 encoded key material, such as an existing key-wrapping
            key, that you want to store and manage in the service. For more
            information, check out
            [Base64 encoding your key material](#how-to-encode-root-key-material).
          </p>
          <p>
            Ensure that the key material meets the following requirements:
          </p>
          <p>
            <ul>
              <li>
                The key must be 128, 192, or 256 bits.
              </li>
              <li>
                The bytes of data, for example 32 bytes for 256 bits, must be
                encoded by using base64 encoding.
              </li>
            </ul>
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_type</varname>
        </td>
        <td>
          <p>
            A boolean value that determines whether the key material can leave
            the service.
          </p>
          <p>
            When you set the <code>extractable</code> attribute to
            <code>false</code>, the service designates the key as a root key
            that you can use for <code>wrap</code> or <code>unwrap</code>
            operations.
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 4. Describes the variables that are needed to add a root key with
        the {{site.data.keyword.keymanagementserviceshort}} API
      </caption>
    </table>

    To protect the confidentiality of your personal data, avoid entering
    personally identifiable information (PII), such as your name or location,
    when you add keys to the service. For more examples of PII, see section 2.2
    of the
    [NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
    {: important}

    A successful `POST api/v2/keys` response returns the ID value for your key,
    along with other metadata. The ID is a unique identifier that is assigned to
    your key and is used for subsequent calls to the
    {{site.data.keyword.keymanagementserviceshort}} API.

3. Optional: Verify that the key was added by running the following call to
   browse the keys in your {{site.data.keyword.keymanagementserviceshort}}
   instance.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys" \
        -H "accept: application/vnd.ibm.collection+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

## Base64 encoding your key material
{: #how-to-encode-root-key-material}

When importing an existing root key, it is required to include the encrypted key
material that you want to store and manage in the service.

### Using OpenSSL to encrypt existing key material
{: #open-ssl-encoding-root-import}

1. Download and install
   [OpenSSL](https://github.com/openssl/openssl#for-production-use){: external}.

2. Base64 encode your key material string by running the following command:

    ```sh
    openssl base64 -in <infile> -out <outfile>
    ```
    {: pre}

    Replace the variables in the example request according to the following
    table.

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>
          <varname>infile</varname>
        </td>
        <td>
          <p>
            The name of the file where your key material string resides.
          </p>
        </td>
      </tr>
      <tr>
        <td>
          <varname>outfile</varname>
        </td>
        <td>
          <p>
            The name of the file where your base64 encoded key material will be
            outputted once the command has ran.
          </p>
          <p>
            Ensure that the key is 128, 192, or 256 bits in length.
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 3. Describes the variables that are needed to base64 encode your
        key material.
      </caption>
    </table>

  If you want to output the base64 material in the command line directly rather
  than a file, run the command
  `openssl enc -base64 <<< '<key_material_string>'`, where key_material_string
  is the key material input for your imported key.
  {: note}

### Using OpenSSL to create and encode new key material
{: #open-ssl-encoding-root-new-key-material}

1. Download and install
   [OpenSSL](https://github.com/openssl/openssl#for-production-use){: external}.

2. Base64 encode your key material string by running the following command:

    ```sh
    openssl rand <byte_length> -base64
    ```
    {: pre}

    Replace the variable in the example request according to the following
    table.

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>
          <varname>byte_length</varname>
        </td>
        <td>
          <p>
            The length of the key, measured in bytes. You will need to convert the desired bit length into bytes for the command.
          </p>
          <p>
            Acceptable bit lengths: 128, 192, 256. Converted byte lengths are 16, 24, and 32.
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 4. Describes the variable that is needed to create and encode new
        key material.
      </caption>
    </table>

#### Key Material Creation Examples
{: #open-ssl-examples}

1. `openssl rand 16 -base64` will generate 128 bit key material.
2. `openssl rand 24 -base64` will generate 192 bit key material.
3. `openssl rand 32 -base64` will generate 256 bit key material.

## What's next
{: #import-root-key-next-steps}

- To find out more about protecting keys with envelope encryption, check out
  [Wrapping keys](/docs/key-protect?topic=key-protect-wrap-keys).

- To find out more about programmatically managing your keys,
  [check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc](/apidocs/key-protect){: external}.
