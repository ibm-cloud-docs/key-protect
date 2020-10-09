---

copyright:
  years: 2017, 2020
lastupdated: "2020-10-09"

keywords: quantum safe cryptography, quantum cryptography, post quantum cryptography, quantum resistant, quantum safe TLS

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

# Introduction to Quantum-safe Cryptography in TLS
{: #quantum-safe-cryptography-tls-introduction}

You can use {{site.data.keyword.keymanagementservicefull}} to protect your in-transit data 
from quantum computer attacks.
{: #shortdesc}

## What is Quantum Safe Cryptography?
{: #quantum-safe-cryptography}

Quantum safe Cryptography, also known as post Quantum Cryptography, is a new generation of 
the public-key cryptographic system that is undergoing NIST evaluation. These new Quantum 
Cryptographic algorithms are based on hard mathematical problems that even large quantum computers 
cannot break.

When these Quantum Cryptographic algorithms are used for TLS communication, the security of the 
key exchange negotiated between the client and server are expected to have higher security levels 
than the current RSA and ECC algorithms. However NIST hast not standardized the algorithms, until then 
{{site.data.keyword.keymanagementserviceshort}} will adopt a hybrid method to use both Quantum Safe
and current ECC algorithms combined.

## Why is Quantum Safe TLS important?
{: #quantum-safe-cryptography-importance}

As quantum computing continues to evolve and advance, a large quantum computer will be 
able to run a "SHOR" algorithm and break the public cryptographic algorithms (RSA/ECC) 
used to secure TLS communication in a matter of minutes. While large quantum computers 
are not available today, TLS data-in-transit can be snooped, stored, and breached when 
these large quantum computers are made available. Data has longer life so it is critical 
that {{site.data.keyword.keymanagementserviceshort}} supports quantum safe
cryptographic algorithms to secure TLS communications.

To keep your in-transit data resilient, {{site.data.keyword.keymanagementserviceshort}} has 
introduced the ability to use a quantum safe enabled TLS connection to ensure that 
your data is secure during the key exchange process.

## What are the considerations of Quantum Safe Cryptography?
{: #qsc-considerations}

Before configuring your application to send requests to {{site.data.keyword.keymanagementserviceshort}} 
through a quantum safe enabled {{site.data.keyword.keymanagementserviceshort}} endpoint, please keep in
mind the following considerations:

- **The National Institute for Standards and Technology (NIST) is in the process of [standardizing quantum safe algorithms](https://csrc.nist.gov/Projects/Post-Quantum-Cryptography){: external}.**
    NIST is currently evaluating candidate approaches to quantum safe
    cryptography and isn't expected to complete the standardization process
    until after 2023. {{site.data.keyword.keymanagementserviceshort}} uses
    the Kyber algorithm, which is one of the third round candidates under evaluation.
    If NIST's research reveals that the Kyber algorithm is not quantum
    safe, the key exchange mechanism key is still supported by the AES algorithm.

- **Performance results may vary from traditional key algorithms.**
  The bandwidth requirements of the quantum safe algorithms vary from classic
  algorithms. Quantum safe algorithm performance can also be affected by network
  profile, CPU speed, and API call rates.

- **Quantum Safe Cryptography implemented in TLS only protects data in transit, not at rest.**
  The Quantum Safe algorithms utilized by
  {{site.data.keyword.keymanagementserviceshort}} protect your data from breach as it
  travels to a {{site.data.keyword.keymanagementserviceshort}} endpoint. Imported root 
  keys(including their associated payloads) are encrypted by TLS session keys.

- **{{site.data.keyword.keymanagementserviceshort}} currently only supports Quantum Safe Cryptography for Linux Platforms.**
  {{site.data.keyword.keymanagementserviceshort}} will provide Quantum Safe Cryptography
  support to additional operating systems in the future.

- **Quantum Safe Cryptography is only supported through the {{site.data.keyword.keymanagementserviceshort}} software development kit (SDK).**
  Quantum Safe Cryptography support will be added to the command line interface
  (CLI) in the future. To find out more about accessing the
  {{site.data.keyword.keymanagementserviceshort}} SDK, check out
  [Setting up the SDK](/docs/key-protect?topic=key-protect-set-up-api).


## Using Quantum Safe Cryptography with {{site.data.keyword.keymanagementserviceshort}}
{: #how-to-use-qsc}

You can choose between hybrid and non-hybrid quantum safe TLS
connection modes when sending requests to
{{site.data.keyword.keymanagementserviceshort}}.

### Quantum Safe Mode vs Hybrid Mode
{: #quantum-safe-modes}

{{site.data.keyword.keymanagementserviceshort}} supports two modes that protect
your keys during a TLS connection: Quantum Safe Mode and Hybrid mode.

- **Hybrid Mode**:
  Hybrid mode uses a combination of a quantum safe algorithm and classic key exchange 
  algorithms to protect your data while in transit. When you make a request using this mode, 
  the classic elliptic algorithm and the quantum safe algorithm will be used in a key exchange
  mechanisms to cryptographically protect your data as it makes its way to the 
  {{site.data.keyword.keymanagementserviceshort}} service. 

  Hybrid mode supports the hybrid Kyber algorithm with the following parameter sets(key sizes):
  
  - `p256_kyber512`
  - `p384_kyber768`
  - `p521_kyber1024`

The hybrid Kyber algorithms is recommended by the Open Quantum Safe (OQS) project community. For more information
about the algorithm and its associated key sizes, see
[Limitations and Security](https://github.com/open-quantum-safe/liboqs#limitations-and-security).
{: note}

- **Quantum Safe Mode**:
  Quantum safe mode uses a quantum safe algorithm to protect your data while in transit. When you 
  make a request using this mode, the quantum safe algorithm will be used in a key exchange
  mechanism to cryptographically protect your data as it makes its way to the 
  {{site.data.keyword.keymanagementserviceshort}} service. 

  Quantum Safe mode supports the Kyber algorithm with the following parameter sets(key sizes):

  - `kyber512`
  - `kyber768`
  - `kyber1024`

The Kyber algorithm is recommended by {{site.data.keyword.cloud_notm}}. To find out more about the algorithm and 
its associated key sizes, see [CRYSTALS-Kyber](https://github.com/open-quantum-safe/liboqs/blob/master/docs/algorithms/kem/kyber.md){: external}.
{: note}

### Quantum Safe Enabled Endpoints
{: #quantum-safe-endpoints}

{{site.data.keyword.keymanagementservicefull}} has quantum safe enabled
endpoints for 3 regions; `US-South`, `EU-GB`, and `EU-DE`. See the following
table to determine which quantum safe enabled endpoints to use when sending
requests to the {{site.data.keyword.keymanagementservicefull}} service.

| Region        | Public endpoints                         |
| ------------- | ---------------------------------------- |
| Staging       | `qsc.qa.us-south.kms.test.cloud.ibm.com` |
| Dallas        | `qsc.us-south.kms.cloud.ibm.com`         | 
| London        | `qsc.eu-gb.kms.cloud.ibm.com`            |
| Frankfurt     | `qsc.eu-de.kms.cloud.ibm.com`            |
{: caption="Table 1. Lists quantum safe enabled public endpoints for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs over IBM Cloud's public network" caption-side="top"}
{: #table-1}
{: tab-title="Public"}
{: class="comparison-tab-table"}
{: row-headers}

| Region        | Private endpoints                               |
| ------------- | ----------------------------------------------- |
| Staging        | `qsc.private.qa.us-south.kms.test.cloud.ibm.com`|
| Dallas        | `qsc.private.us-south.kms.cloud.ibm.com`        |
| London        | `qsc.private.eu-gb.kms.cloud.ibm.com`           |
| Frankfurt     | `qsc.private.eu-de.kms.cloud.ibm.com`           |
{: caption="Table 2. Lists quantum safe enabled private endpoints for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs over IBM Cloud's private network" caption-side="top"}
{: #table-2}
{: tab-title="Private"}
{: class="comparison-tab-table"}
{: row-headers}

The classic {{site.data.keyword.keymanagementserviceshort}} service endpoints
are not quantum safe enabled.
{: note}

## Configure Quantum Safe TLS with {{site.data.keyword.keymanagementserviceshort}} via the SDK
{: #configure-qsc-sdk}

### Prerequisites
{: #qsc-pre-reqs-sdk}

Before setting up your application to work with the SDK, follow these steps:

1. Download the {site.data.keyword.keymanagementserviceshort}} script. This script will install and 
  build all necessary dependencies(`liboqs`, `openssl`, and `libcurl`) into your HOME directory folder 
  (`$HOME/opt/oqssa/`). 

2. Compile the {{site.data.keyword.keymanagementserviceshort}} script and run the following command:
   ```sh
      bash configure-quantum-safe-ibm-kms.sh
  ```

  The script will install the following additional dependencies needed to utilize the 
  {{site.data.keyword.keymanagementserviceshort}} quantum safe enabled endpoints. You will need `sudo` 
  permissions in order to install the dependency packages.

  Debian (Ubuntu) dependencies:
    `libtool automake autoconf cmake(3.5 and above) make openssl libssl-dev build-essential git wget golang patch perl diffutils`
      
  RHEL (Centos/Fedora) dependencies:
    `libtool automake autoconf cmake(3.5 and above) make openssl  ncurses-devel gcc-c++ glibc-locale-source glibc-langpack-enopenssl-devel git wget golang patch perl diffutils`

### Configuring the {{site.data.keyword.keymanagementserviceshort}} SDK with your application
{: #qsc-sdk-application-steps}

Once you have the prerequisites installed, follow these steps to configure the
{{site.data.keyword.keymanagementserviceshort}} SDK with your application:
  
1. Navigate to the folder where the go client resides by running the following command:
    
    ```sh
       cd $HOME/keyprotect-go-client
    ```
    {: pre}

2. Set the Kyber algorithm in the initialization of the {{site.data.keyword.keymanagementserviceshort}} 
   client in your application code. If you do not specify an algorithm, your application will default 
   to using the `p384_kyper768` algorithm. Use the following code as an example of algorithm configuration:
    
    ```go
    qscConfig := kp.ClientQSCConfig{
        AlgorithmID: kp.KP_QSC_ALGO_p384_KYBER768,
    }
    ```
    {: pre}


3. Compile the {{site.data.keyword.keymanagementserviceshort}} SDK by running
   the following command:
    ```sh
    LD_LIBRARY_PATH=$HOME/opt/oqssa/lib PKG_CONFIG_PATH=$HOME/opt/oqssa/lib/pkgconfig go build –tags quantum
    ```
    {: pre}

## Using Quantum Safe {{site.data.keyword.keymanagementserviceshort}} endpoints via CURL
{: #configure-qsc-curl}

### Prerequisites
{: #qsc-pre-reqs-curl}

Before make a curl request to a {{site.data.keyword.keymanagementserviceshort}} quantum 
safe enabled endpoint, follow these steps:

1. Download the {site.data.keyword.keymanagementserviceshort}} script. This script will install and 
  build all necessary dependencies(`liboqs`, `openssl`, and `libcurl`) into your HOME directory folder 
  (`$HOME/opt/oqssa/`). 

2. Compile the {{site.data.keyword.keymanagementserviceshort}} script and run the following command:
   ```sh
      bash configure-quantum-safe-ibm-kms.sh
  ```

  The script will install the following additional dependencies needed to utilize the 
  {{site.data.keyword.keymanagementserviceshort}} quantum safe enabled endpoints. You will need `sudo` 
  permissions in order to install the dependency packages.

  Debian (Ubuntu) dependencies:
    `libtool automake autoconf cmake(3.5 and above) make openssl libssl-dev build-essential git wget golang patch perl diffutils`
      
  RHEL (Centos/Fedora) dependencies:
    `libtool automake autoconf cmake(3.5 and above) make openssl  ncurses-devel gcc-c++ glibc-locale-source glibc-langpack-enopenssl-devel git wget golang patch perl diffutils`

### Making a CURL request to a quantum safe enabled endpoint
{: #qsc-curl-steps}

When making a call to the the a quantum safe enabled endpoint via curl
request, you will need to suse specific to ensure that the request
successfully goes through. The following table contains a list of flags that are
required when making a quantum safe curl request.
<table>
  <tr>
    <th>Flag</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>
      <varname>-tlsv1.3</varname>
    </td>
    <td>
      This flag enforces that the curl connects to a TLS v1.3 server.
    </td>
  </tr>
  <tr>
    <td>
      <varname>--curves</varname>
    </td>
    <td>
      This flag will specify which quantum safe algorithm should be used in the 
      TLSv1.3 key exchange mechanism.. If you do not specify an algorithm, the 
      flag will default to the `p384_kyper768` algorithm.
    </td>
  </tr>
  <caption style="caption-side:bottom;">
    Table 1. Describes the flags needed to make curl request to the
    {{site.data.keyword.keymanagementserviceshort}} service.
  </caption>
</table>
You can use the following example request to retrieve a list of keys for your
{{site.data.keyword.keymanagementserviceshort}} instance via a quantum safe
enabled endpoint.
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
