---

copyright:
  years: 2017, 2020
lastupdated: "2020-09-28"

keywords: quantum safe cryptography, quantum cryptography, post quantum, quantum resistant

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

# Introduction to quantum safe cryptography
{: #quantum-safe-cryptography-introduction}

You can protect your in-transit data from quantum computer attacks by using {{site.data.keyword.keymanagementservicefull}}'s quantum safe enabled service endpoints. 
{: #shortdesc}

## What is quantum safe cryptography?
{: #quantum-safe-cryptography}

Quantum safe cryptography is a technology that uses quantum mechanics to transfer an encryption key between two locations. When you send data to {{site.data.keyword.keymanagementserviceshort}}, that data is passed over a Transport Layer Security(TLS) connection that is supported by classic {{site.data.keyword.keymanagementserviceshort}} algorithms. These algorithms protect your data from brute force attacks during the key exchange process. However, as quantum computing 
continues to grow, there is a future possibility of large scale quantum computer having the ability potentially break those classic algorithms and cause widespread 
problems. To mitigate the risk of being attacked by a quantum computer, it is important to be prepared for the future.

To keep your TLS connection resilient, {{site.data.keyword.keymanagementserviceshort}} has introduced new endpoints that are supported by quantum safe algorithms to 
ensure that your TLS connection is stable and secure during the key exchange process.

Quantum safe cryptography is only supported through the {{site.data.keyword.keymanagementserviceshort}} 
software development kit (SDK). Quantum safe cryptography support will be
added to the command line interface (CLI) in the future. To find out more about
accessing the {{site.data.keyword.keymanagementserviceshort}} SDK, check out
[Setting up the SDK](/docs/key-protect?topic=key-protect-set-up-api).

## What are the considerations of Quantum Safe Cryptography?
{: #considerations}

Before configuring your application to send requests through a quantum safe enabled {{site.data.keyword.keymanagementserviceshort}} endpoint, please keep in mind the 
following considerations:

- **The National Institute for Standards and Technology (NIST) is in the process of [standardizing quantum safe algorithms](https://csrc.nist.gov/Projects/Post-Quantum-Cryptography){: external}.**
    NIST is currently evaluating candidate approaches to quantum safe cryptography and isn't expected to complete the standardization process until after 2023. 
- **Performance results may vary from traditional key algorithms.**
    The bandwidth requirements of the quantum safe algorithms vary from classic algorithms. Quantum safe algorithm performance can also be affected by network profile, 
    CPU speed, and API call rates. 
- **Quantum Safe Cryptography only protects data in transit, not at rest.**
    The Quantum Safe algorithms utilized by {{site.data.keyword.keymanagementserviceshort}} protect your data as it travels to a {{site.data.keyword.keymanagementserviceshort}} endpoint. Quantum safe algorithms are not used to encrypt data associated with root keys.
- **{{site.data.keyword.keymanagementserviceshort}} currently only supports Quantum Safe Cryptography for Linux Platforms.**
    Quantum safe cryptography support will be added to additional operating systems in the future.

## Using Quantum Safe Cryptography with Key Protect
{: #how-to-use-qsc}

You can choose from multiple algorithms between two different quantum safe TLS connection modes when sending requests to {{site.data.keyword.keymanagementserviceshort}}. 

### Quantum Safe Mode vs Hybrid Mode
{: #quantum-safe-modes} 

{{site.data.keyword.keymanagementserviceshort}} has two modes that will protect your keys under quantum attacks: Quantum Safe Mode and Hybrid mode. 

- **Hybrid Mode**:
    Hybrid mode uses a combination of quantum safe algorithms and classic algorithms to protect your data.
- **Quantum Safe Mode**:
    Quantum safe mode only uses quantum safe algorithms to protect your data.  


### Algorithm Types
{#quantum-safe-algorithms}

When configuring the sdk, you will have the choice to select one of six algorithms to protect your data while in transit: `kyber512`, `kyber768`, `kyber1024`, 
`p256_kyber512`, `p384_kyber768`, `p521_kyber1024`. Hybrid mode supports the `p256_kyber512`, `p384_kyber768`, and `p521_kyber1024` algorithms. Quantum Safe mode 
supports the `kyber512`, `kyber768`, and `kyber1024` algorithms. 

These kyber algorithm is recommended by the Open Quantum Safe(OQS) project community. To find out more see, [CRYSTALS-Kyber](https://github.com/open-quantum-safe/liboqs/blob/master/docs/algorithms/kem/kyber.md){: external}.
{: note}

### Quantum Safe Enabled Endpoints 
{#quantum-safe-endpoints}

{{site.data.keyword.keymanagementservicefull}} has quantum safe enabled endpoints for 3 regions; `US-South`, `EU-GB`, and `EU-DE`. See the following table to 
determine which quantum safe enabled endpoints to use when sending requests to the {{site.data.keyword.keymanagementservicefull}} service.

| Region        | Public endpoints             |
| ------------- | ---------------------------- |
| Dallas        | `qsc.us-south.kms.cloud.ibm.com` |
| London        | `qsc.eu-gb.kms.cloud.ibm.com`    |
| Frankfurt     | `qsc.eu-de.kms.cloud.ibm.com`    |
{: caption="Table 1. Lists quantum safe enabled public endpoints for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs over IBM Cloud's public network" caption-side="top"}
{: #table-1}
{: tab-title="Public"}
{: class="comparison-tab-table"}
{: row-headers}

| Region        | Private endpoints                    |
| ------------- | ------------------------------------ |
| Dallas        | `qsc.private.us-south.kms.cloud.ibm.com` |
| London        | `qsc.private.eu-gb.kms.cloud.ibm.com`    |
| Frankfurt     | `qsc.private.eu-de.kms.cloud.ibm.com`    |
{: caption="Table 2. Lists quantum safe enabled private endpoints for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs over IBM Cloud's private network" caption-side="top"}
{: #table-2}
{: tab-title="Private"}
{: class="comparison-tab-table"}
{: row-headers}

The classic {{site.data.keyword.keymanagementserviceshort}} service endpoints are not quantum safe enabled.
{: note}

## Using Quantum Safe {{site.data.keyword.keymanagementserviceshort}} endpoints via the SDK
{: #configure-qsc-sdk}

### Prerequisites
{: #qsc-pre-reqs}

Before setting up your application to work with the SDK, follow these steps:

1. Install [Golang](https://golang.org/).
2. Install and configure [Git](https://git-scm.com/).
3. Install pkg-config by running the following command:
    ```sh
    apt-get install -y pkg-config
    ```
    {: pre}
4. Install [OQS Openssl](https://github.com/IBM/oqssa) and the Curl deb package from IBM site by running the following command:
    ```sh
    curl https://api.github.com/repos/IBM/oqssa/releases/latest | grep "browser_download_url" | cut -d '"' -f 4 | wget -i -dpkg -i oqssa-0.1.0_amd64.deb
    ```
    {: pre}

### Configuring the Key Protect SDK with your application
{: #qsc-application-steps}

Once you have the prerequisites installed, follow these steps to configure the Key Protect SDK your application:

1. Clone the Key Protect Go client by running the following command:
    ```sh
    git clone git@github.com:IBM/keyprotect-go-client.git
    ```
    {: pre}

2. Navigate to the folder where the client resides by running the following command:
    ```sh
    cd ~/go/src/github.com/IBM/keyprotect-go-client
    ```
    {: pre}

3. Compile the Key Protect SDK by running the following command:
    ```sh
    CPATH=/opt/oqssa/include/ PKG_CONFIG_PATH=/opt/oqssa/lib/pkgconfig go build –tags quantum
    ```
    {: pre}

4. Set the chosen quantum safe algorithm in the initialization of the Key Protect client in your application code. Use the following code as an example of algorithm 
    configuration:
    ```go
    qscConfig := kp.ClientQSCConfig{
		AlgorithmID: kp.KP_QSC_ALGO_p384_KYBER768,
	}
    ```
    {: pre}

    For a full example of how to initialize the Key Protect quantum safe client in your application, see [QSC Demo](https://github.ibm.com/jfeng/qsc-sdk-samples/blob/master/qsc-demo.go#13){: external}.
    {: note}

4. Compile your application code by running the following command:
    ```sh
    go build –tags quantum
    ```
    {: pre}

5. Run your application by running the following command:
    ```sh
    export LD_LIBRARY_PATH=/opt/oqssa/lib:$LD_LIBRARY_PATHexamples/test-app
    ```
    {: pre}

## Using Quantum Safe {{site.data.keyword.keymanagementserviceshort}} endpoints via CURL
{: #configure-qsc-curl}

When you making a call to the the a quantum safe enabled endpoint via curl request, you will
need to specify a couple of flags to ensure that the request successfully goes through. The following table
contains a list of flags that are required when making a quantum safe curl request.

<table>
  <tr>
    <th>Flag</th>
    <th>Description</th>
  </tr>

  <tr>
    <td>
      <varname>-k</varname>
    </td>
    <td>
      <p>
        The flag will bypass any certificate errors during a connection to HTTPS.
      </p>
    </td>
  </tr>

  <tr>
    <td>
      <varname>-tlsv1.3</varname>
    </td>
    <td>
      <p>
        This flag enforces that the curl connects to a TLS v1.3 server.
      </p>
    </td>
  </tr>

  <tr>
    <td>
      <varname>--curves</varname>
    </td>
    <td>
      <p>
        This flag will specify which quantum safe algorithm that {{site.data.keyword.keymanagementserviceshort}} should use to protect your data while in transit.
      </p>
    </td>
  </tr>

  <caption style="caption-side:bottom;">
    Table 1. Describes the flags needed to make curl request to the {{site.data.keyword.keymanagementserviceshort}} service.
  </caption>
</table>


You can use the following example request to retrieve a list of keys for your
{{site.data.keyword.keymanagementserviceshort}} instance via a quantum safe enabled endpoint.

```sh
$ curl -k --tlsv1.3 --curves <qsc_algorithm> -X GET \
    "https://qsc.<region>.kms.cloud.ibm.com/api/v2/keys" \
    -H "accept: application/vnd.ibm.kms.key+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Replace the variables in your request according to the following table.

<table>
  <tr>
    <th>Variable</th>
    <th>Description</th>
  </tr>

  <tr>
    <td>
      <varname>qsc_algorithm</varname>
    </td>
    <td>
      <p>
        <strong>Required.</strong> The quantum safe algorithm that will be 
        used to protect your data in transit. 
      </p>
      <p>
        For more information on available algorithms, see
        [Algorithm Types](#quantum-safe-algorithms).
      </p>
    </td>
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

  <caption style="caption-side:bottom;">
    Table 2. Describes the variables needed to make a list keys request through
    a quantum safe endpoint.
  </caption>
</table>