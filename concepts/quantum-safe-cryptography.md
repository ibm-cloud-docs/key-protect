---

copyright:
  years: 2017, 2025
lastupdated: "2025-08-14"

keywords: post-quantum cryptography, quantum cryptography, post-quantum TLS

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

# Post quantum cryptography in TLS
{: #quantum-safe-cryptography-tls-introduction}

You can use a post-quantum enabled TLS connection to send requests to a
{{site.data.keyword.keymanagementservicefull}} service endpoint.
{: #shortdesc}

## What is post-quantum cryptography?
{: #quantum-safe-cryptography}

Post-quantum cryptography is a new
generation of the public-key cryptographic system that is undergoing NIST
evaluation. These new quantum cryptographic algorithms are based on hard
mathematical problems that, based on current research, even large quantum
computers cannot break.

When these quantum cryptographic algorithms are used for TLS communication, the
security of the public key exchange between the client and server are expected
to have higher security levels than the current RSA and ECC algorithms. {{site.data.keyword.keymanagementserviceshort}} has adopted a hybrid method that combines both NIST evaluation round 2 post-quantum algorithm and current ECC algorithms to protect in-transit data.

## Why is post-quantum TLS important?
{: #quantum-safe-cryptography-importance}

As quantum computing continues to evolve and advance, a large quantum computer
will be able to run a
["SHOR" algorithm](https://en.wikipedia.org/wiki/Shor%27s_algorithm){: external}
that can break the current TLS communication algorithms (RSA/ECC) in a matter of
minutes. While large quantum computers are not available today, any TLS
data-in-transit that has been snooped and stored can be breached when these
large quantum computers are made available. Data has a long shelf life so it is
critical that {{site.data.keyword.keymanagementserviceshort}} supports quantum
safe cryptographic algorithms to secure TLS communications.

To keep your in-transit data resilient,
{{site.data.keyword.keymanagementserviceshort}} has introduced the ability to
use a post-quantum enabled TLS connection to ensure that your data is secure
during the key exchange process.

## What are the considerations of post-quantum Cryptography?
{: #qsc-considerations}

Before configuring your service to send requests to
{{site.data.keyword.keymanagementserviceshort}} through a post-quantum enabled
{{site.data.keyword.keymanagementserviceshort}} service endpoint, please keep in
mind the following considerations:

- **Performance results may vary from traditional key algorithms.**
    Post-quantum algorithms use a larger key size compared to classic public
    key algorithms, therefore the network bandwidth requirements are higher.
    Post-quantum algorithm performance can also be affected by network profile,
    CPU speed, and API call rates.

- **Post-quantum TLS only protects data in transit, not at rest.**
    The post-quantum algorithms utilized by
    {{site.data.keyword.keymanagementserviceshort}} protects your data from breach
    as it travels to a {{site.data.keyword.keymanagementserviceshort}} service
    endpoint. Imported root keys (including their associated payloads) are
    encrypted by TLS session keys. Data at rest encryption uses symmetric keys and
    AES 256 symmetric keys are safe from large quantum computer attacks.

- **{{site.data.keyword.keymanagementserviceshort}} only supports post-quantum TLS for Linux Platforms.**
    {{site.data.keyword.keymanagementserviceshort}} will provide post-quantum TLS
    connection support to additional operating systems in the future.

- **Post-quantum TLS is only supported through the {{site.data.keyword.keymanagementserviceshort}} software development kit (SDK).**
    To find out more about accessing the
    {{site.data.keyword.keymanagementserviceshort}} SDK, check out
    [Setting up the SDK](/docs/key-protect?topic=key-protect-set-up-api).

## Using post-quantum TLS with {{site.data.keyword.keymanagementserviceshort}}
{: #how-to-use-qsc}

You can choose between hybrid and non-hybrid post-quantum TLS
connection modes when sending requests to
{{site.data.keyword.keymanagementserviceshort}}.

### Post-quantum Mode vs Hybrid Mode
{: #quantum-safe-modes}

{{site.data.keyword.keymanagementserviceshort}} supports two modes that protect
your keys during a TLS connection: post-quantum Mode and Hybrid mode.

- **Hybrid Mode**:
    Hybrid mode uses a combination of a post-quantum algorithm and classic key
    exchange algorithms to protect your data while in transit. When you make a
    request using this mode, the classic elliptic algorithm and the post-quantum
    algorithm will be used in a key exchange mechanism to cryptographically
    protect your data as it makes its way to the
    {{site.data.keyword.keymanagementserviceshort}} service.

    Hybrid mode supports the hybrid Kyber algorithm with the following parameter
    sets (key sizes):

    - `p256_kyber512`: combines kyber512 with ECDH using p_256 curve. It provides
        L1 security.
    - `p384_kyber768`: combines kyber768 with ECDH using p_384 curve. It provides
        L3 security.
    - `p521_kyber1024`: combines kyber1024 with ECDH using p_521 curve. It
        provides L5 security.

The hybrid algorithm is used based on guidance from the post-quantum
project community. For more information about the algorithm and its associated
key sizes, see
[Prototyping post-quantum and hybrid key exchange
and authentication in TLS and SSH](https://csrc.nist.gov/CSRC/media/Events/Second-PQC-Standardization-Conference/documents/accepted-papers/stebila-prototyping-post-quantum.pdf){: external}.
{: note}

- **Post-quantum Mode**:
    Post-quantum mode uses a post-quantum algorithm to protect your data while in
    transit. When you make a request using this mode, the post-quantum algorithm
    is used in a key exchange mechanism to cryptographically protect your
    data as it makes its way to the
    {{site.data.keyword.keymanagementserviceshort}} service.

    Post-quantum mode supports the Kyber algorithm with the following parameter
    sets (key sizes):

    - `kyber512`
    - `kyber768`
    - `kyber1024`

The Kyber algorithm is used based on recommendation from
{{site.data.keyword.cloud_notm}}. To find out more about the algorithm and its
associated key sizes, see
[CRYSTALS-Kyber](https://github.com/open-quantum-safe/liboqs/blob/main/docs/algorithms/kem/kyber.md){: external}.
{: note}

### Post-quantum Enabled Endpoints
{: #quantum-safe-endpoints}

{{site.data.keyword.keymanagementservicefull}} has post-quantum enabled
endpoints for 2 regions; `US-South` and `EU-GB`. See the following
table to determine which post-quantum enabled endpoints to use when sending
requests to the {{site.data.keyword.keymanagementservicefull}} service.



| Region        | Public endpoints                         |
| ------------- | ---------------------------------------- |
| Dallas        | `qsc.us-south.kms.cloud.ibm.com`         |
| London        | `qsc.eu-gb.kms.cloud.ibm.com`            |
| Frankfurt     | `qsc.eu-de.kms.cloud.ibm.com`            |
{: caption="Lists post-quantum enabled public endpoints for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs over IBM Cloud's public network" caption-side="top"}
{: #table-1}
{: tab-title="Public"}
{: class="comparison-tab-table"}
{: row-headers}

| Region        | Private endpoints                                |
| ------------- | ------------------------------------------------ |
| Dallas        | `private-qsc.us-south.kms.cloud.ibm.com`         |
| London        | `private-qsc.eu-gb.kms.cloud.ibm.com`            |
| Frankfurt     | `private-qsc.eu-de.kms.cloud.ibm.com`            |
{: caption="Lists post-quantum enabled private endpoints for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs over IBM Cloud's private network" caption-side="top"}
{: #table-2}
{: tab-title="Private"}
{: class="comparison-tab-table"}


The classic {{site.data.keyword.keymanagementserviceshort}} service endpoints
are not post-quantum enabled.
{: note}

## Configure post-quantum TLS with {{site.data.keyword.keymanagementserviceshort}} via the SDK
{: #configure-qsc-sdk}

### Prerequisites
{: #qsc-pre-reqs-sdk}

Before setting up your application to work with the SDK, follow these steps:

1. Download the
    [Open post-quantum Software Stack (OQSSA) script](https://github.com/IBM/oqssa/blob/master/build-oqssa.sh){: external}.
    This script builds and install all necessary dependencies
    (`liboqs`, `openssl`, and `libcurl`) into your HOME directory folder
    (`$HOME/opt/oqssa/`).

2. Make sure dependent packages required for building OQSSA are installed. You need `sudo` permissions in order to install the dependency packages.

    - Debian (Ubuntu) dependencies:
        `libtool automake autoconf cmake (3.5 or above) make openssl libssl-dev build-essential git wget golang (1.14 or above) patch perl diffutils`

    If you are using a Debian distribution, copy the following code snippet to a
    file and execute it to verify that all necessary packages have been
    installed:

    ```sh
    echo "Starting prerequisites verification"
    CMAKE_VER_REQUIRED="3.*"
    packages="libtool automake autoconf cmake make openssl libssl-dev git wget build-essential golang patch perl diffutils"
    for REQUIRED_PKG in $packages
    do
        PKG_STATUS=$(dpkg-query -W --showformat='${Version},${Status}\n' $REQUIRED_PKG|grep "install ok installed")
        if [ "" = "$PKG_STATUS" ]
        then
          echo "$REQUIRED_PKG is NOT installed"
          #sudo apt-get -y install $REQUIRED_PKG
        else
          PKG_VER=$(echo $PKG_STATUS| cut -d',' -f 1)
          if [ "cmake" == $REQUIRED_PKG ]  && ! [[ $PKG_VER =~ $CMAKE_VER_REQUIRED ]]
          then
            echo "$REQUIRED_PKG Version is: $PKG_VER. OQSSA requires cmake 3.5 and above."
          fi
        fi
    done
    echo "Prerequisites verification completed"
    ```

    - RHEL (Centos/Fedora) dependencies:
        `libtool automake autoconf cmake (3.5 or above) make openssl  ncurses-devel gcc-c++ glibc-locale-source glibc-langpack-enopenssl-devel git wget golang (1.14 or above) patch perl diffutils 'Developement Tools'`

    If you are using a RHEL distribution, copy the following code snippet to a
    file and execute it to verify that all necessary packages have been
    installed:

    ```sh
    echo "Starting prerequisites verification"
    CMAKE_VER_REQUIRED="3.*"
    packages="git libtool automake autoconf cmake make openssl  ncurses-devel gcc-c++ openssl-devel wget glibc-locale-source glibc-langpack-en sudo golang patch perl diffutils"
    for REQUIRED_PKG in $packages
    do
        PKG_STATUS=$(rpm -q --qf '%{VERSION},%{INSTALLTIME}\n' $REQUIRED_PKG)
        if [[ "$PKG_STATUS" == *"not installed"* ]];
        then
        echo "$REQUIRED_PKG is NOT installed"
        #sudo yum -y install $REQUIRED_PKG
        else
          PKG_VER=$(echo $PKG_STATUS| cut -d',' -f 1)
          if [ "cmake" == $REQUIRED_PKG ]  && ! [[ $PKG_VER =~ $CMAKE_VER_REQUIRED ]]
          then
            echo "$REQUIRED_PKG Version is: $PKG_VER. OQSSA requires cmake 3.5 and above."
          fi
        fi
    done
    PKG_STATUS=$(yum grouplist Dev* |grep "Development Tools")
    if [ "" = "$PKG_STATUS" ]
    then
        echo "Developement Tools is NOT installed"
    fi
    echo "Prerequisites verification completed"
    ```

3. Once prerequisite packages are installed and verified, execute script to
    build and install OQSSA:

    ```sh
    bash build-oqssa.sh
    ```

4. Run the following command to set the quantum library path:

    ```sh
    export LD_LIBRARY_PATH=$HOME/opt/oqssa/lib:$LD_LIBRARY_PATH
    ```
    {: pre}

### Configuring the {{site.data.keyword.keymanagementserviceshort}} SDK with your application
{: #qsc-sdk-application-steps}

Once you have the prerequisites installed, follow these steps to configure the
[{{site.data.keyword.keymanagementserviceshort}} SDK](https://github.com/IBM/keyprotect-go-client#usage){: external}
with your application:

1. Navigate to the folder where the go client resides by running the following
    command:

    ```sh
        cd $HOME/keyprotect-go-client
    ```
    {: pre}

2. Set the Kyber algorithm in the initialization of the
    {{site.data.keyword.keymanagementserviceshort}} client in your application
    code. If you do not specify an algorithm, your application will default to
    using the `p384_kyber768` algorithm. Use the following code as an example of
    algorithm configuration:

    ```go
    qscConfig := kp.ClientQSCConfig{
        AlgorithmID: kp.KP_QSC_ALGO_p384_KYBER768,
    }
    ```
    {: pre}

3. Compile the {{site.data.keyword.keymanagementserviceshort}} SDK by running
    the following command:

    ```sh
    LD_LIBRARY_PATH=$HOME/opt/oqssa/lib PKG_CONFIG_PATH=$HOME/opt/oqssa/lib/pkgconfig go build –-tags quantum
    ```
    {: pre}

## Using post-quantum enabled {{site.data.keyword.keymanagementserviceshort}} endpoints via CURL
{: #configure-qsc-curl}

### Prerequisites
{: #qsc-pre-reqs-curl}

Before making a `curl` request to a
{{site.data.keyword.keymanagementserviceshort}} post-quantum enabled endpoint,
follow these steps:

1. Download the
    [Open post-quantum Software Stack (OQSSA) script](https://github.com/IBM/oqssa/blob/master/build-oqssa.sh){: external}.
    This script will build and install all necessary dependencies
    (`liboqs`, `openssl`, and `libcurl`) into your HOME directory folder
    (`$HOME/opt/oqssa/`).

2. Make sure dependent packages required for building OQSSA are installed. You need `sudo` permissions in order to install the dependency packages.

    - Debian (Ubuntu) dependencies:
        `libtool automake autoconf cmake (3.5 or above) make openssl libssl-dev build-essential git wget golang (1.14 or above) patch perl diffutils`

    If you are using a Debian distribution, copy the following code snippet to a
    file and execute it to verify that all necessary packages have been
    installed:

    ```sh
    echo "Starting prerequisites verification"
    CMAKE_VER_REQUIRED="3.*"
    packages="libtool automake autoconf cmake make openssl libssl-dev git wget build-essential golang patch perl diffutils"
    for REQUIRED_PKG in $packages
    do
        PKG_STATUS=$(dpkg-query -W --showformat='${Version},${Status}\n' $REQUIRED_PKG|grep "install ok installed")
        if [ "" = "$PKG_STATUS" ]
        then
          echo "$REQUIRED_PKG is NOT installed"
          #sudo apt-get -y install $REQUIRED_PKG
        else
          PKG_VER=$(echo $PKG_STATUS| cut -d',' -f 1)
          if [ "cmake" == $REQUIRED_PKG ]  && ! [[ $PKG_VER =~ $CMAKE_VER_REQUIRED ]]
          then
            echo "$REQUIRED_PKG Version is: $PKG_VER. OQSSA requires cmake 3.5 and above."
          fi
        fi
    done
    echo "Prerequisites verification completed"
    ```

    - RHEL (Centos/Fedora) dependencies:
        `libtool automake autoconf cmake (3.5 or above) make openssl  ncurses-devel gcc-c++ glibc-locale-source glibc-langpack-enopenssl-devel git wget golang (1.14 or above) patch perl diffutils 'Developement Tools''`

    If you are using a RHEL distribution, copy the following code snippet to a
    file and execute it to verify that all necessary packages have been
    installed:

    ```sh
    echo "Starting prerequisites verification"
    CMAKE_VER_REQUIRED="3.*"
    packages="git libtool automake autoconf cmake make openssl  ncurses-devel gcc-c++ openssl-devel wget glibc-locale-source glibc-langpack-en sudo golang patch perl diffutils"
    for REQUIRED_PKG in $packages
    do
        PKG_STATUS=$(rpm -q --qf '%{VERSION},%{INSTALLTIME}\n' $REQUIRED_PKG)
        if [[ "$PKG_STATUS" == *"not installed"* ]];
        then
        echo "$REQUIRED_PKG is NOT installed"
        #sudo yum -y install $REQUIRED_PKG
        else
          PKG_VER=$(echo $PKG_STATUS| cut -d',' -f 1)
          if [ "cmake" == $REQUIRED_PKG ]  && ! [[ $PKG_VER =~ $CMAKE_VER_REQUIRED ]]
          then
            echo "$REQUIRED_PKG Version is: $PKG_VER. OQSSA requires cmake 3.5 and above."
          fi
        fi
    done
    PKG_STATUS=$(yum grouplist Dev* |grep "Development Tools")
    if [ "" = "$PKG_STATUS" ]
    then
        echo "Developement Tools is NOT installed"
    fi
    echo "Prerequisites verification completed"
    ```

3. Once prerequisite packages are installed and verified, execute the script to
    build and install OQSSA:

    ```sh
    bash build-oqssa.sh
    ```

### Making a CURL request to a post-quantum enabled endpoint
{: #qsc-curl-steps}

When making a call to the the a post-quantum enabled endpoint via `curl`
request, you must be specific to ensure that the request
successfully goes through. The following table contains a list of flags that are
required when making a post-quantum `curl` request.

|Flag|Description|
|--- |--- |
|-tlsv1.3|This flag enforces that curl connects to a TLS v1.3 server.|
|--curves|This flag specifies which post-quantum algorithm should be used in the TLSv1.3 key exchange mechanism. If you do not specify an algorithm, the flag will default to the p384_kyber768 algorithm.|
{: caption="Describes the flags needed to make curl requests to the {{site.data.keyword.keymanagementserviceshort}} service." caption-side="top"}

You can use the following example request to retrieve a list of keys for your
{{site.data.keyword.keymanagementserviceshort}} instance via a post-quantum
enabled endpoint.

```sh
$ curl --tlsv1.3 --curves <qsc_algorithm> -X GET \
    "https://qsc.<region>.kms.cloud.ibm.com/api/v2/keys" \
    -H "accept: application/vnd.ibm.kms.key+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Replace the variables in your request according to the following table.

|Variable|Description|
|--- |--- |
|qsc_algorithm|**Required**. The kyber algorithm in the key size that will be used to protect your data in transit.<br><br>Acceptable algorithm + keysizes: kyber512, kyber768, kyber1024, p256_kyber512, p384_kyber768, and p521_kyber1024.|
|region|**Required**. The region abbreviation, such as us-south or eu-gb, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Describes the variables needed to make a list keys request through a post-quantum endpoint." caption-side="top"}
