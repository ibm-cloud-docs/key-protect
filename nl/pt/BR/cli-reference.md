---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: Key Protect CLI plug-in, CLI reference

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

# Referência da CLI do {{site.data.keyword.keymanagementserviceshort}}
{: #cli-reference}

É possível usar o plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}} para gerenciar chaves em
sua instância do {{site.data.keyword.keymanagementserviceshort}}.
{:shortdesc}

Para instalar o plug-in da CLI, consulte [Configurando a CLI](/docs/services/key-protect?topic=key-protect-set-up-cli). 

Quando você efetuar login na [CLI do {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external}, será notificado quando as atualizações estiverem disponíveis. Certifique-se de manter a CLI atualizada para que seja possível usar os comandos e sinalizadores que estão disponíveis
para o plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}}.
{: tip}

## Comandos ibmcloud kp
{: #ibmcloud-kp-commands}

É possível especificar um dos comandos a seguir:

<table summary="Comandos para gerenciar chaves"> 
    <thead>
        <th colspan="5">Comandos para gerenciar chaves</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-create">kp create</a></td>
            <td><a href="#kp-delete">kp delete</a></td>
            <td><a href="#kp-list">kp list</a></td>
            <td><a href="#kp-get">kp get</a></td>
            <td><a href="#kp-rotate">kp rotate</a></td>
        </tr>
        <tr>
            <td><a href="#kp-unwrap">kp unwrap</a></td>
            <td><a href="#kp-wrap">kp wrap</a></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">Tabela 1. Comandos para gerenciar chaves</caption> 
 </table>

 <table summary="Comandos para gerenciamento de políticas de chave"> 
    <thead>
        <th colspan="5">Comandos para gerenciamento de políticas de chave</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-policy-list">kp policy list</a></td>
            <td><a href="#kp-policy-get">kp policy get</a></td>
            <td><a href="#kp-policy-set">kp policy set</a></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">Tabela 2. Comandos para gerenciamento de políticas de chave</caption> 
 </table>

## kp create
{: #kp-create}

[Crie uma chave raiz](/docs/services/key-protect?topic=key-protect-create-root-keys) na instância de serviço do {{site.data.keyword.keymanagementserviceshort}} que você especifica. 

```
ibmcloud kp create KEY_NAME -i INSTANCE_ID | $INSTANCE_ID
                   [-k, --key-material KEY_MATERIAL] 
                   [-s, --standard-key]
                   [--output FORMAT]
```
{:pre}

```sh
$ ibmcloud kp create sample-root-key -i $KP_INSTANCE_ID
SUCCESS

Key ID                                 Key Name
3df42bc2-a991-41cb-acc2-3f9eab64a63f   sample-root-key
```
{:screen}

### Parâmetros necessários
{: #create-req-params}

<dl>
    <dt><code>KEY_NAME</code></dt>
        <dd>Um alias exclusivo legível para atribuir à sua chave.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>O ID da instância do {{site.data.keyword.cloud_notm}} que identifica a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parâmetros opcionais
{: #create-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>O material de chave codificado base64 que você deseja armazenar e gerenciar no serviço. Para importar uma chave existente, forneça uma chave de 256 bits. Para gerar uma nova chave, omita o parâmetro <code>--key-material</code>.</dd>
    <dt><code>-s, --standard-key</code></dt>
        <dd>Configure o parâmetro somente se você desejar criar uma <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">chave padrão</a>. Para criar uma chave raiz, omita o parâmetro <code>--standard-key</code>.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Configure o formato de saída da CLI. Por padrão, todos os comandos são impressos no formato de tabela. Para mudar o formato de saída para JSON, use <code>--output json</code>.</dd>
</dl>

## kp delete
{: #kp-delete}

[Exclua uma chave](/docs/services/key-protect?topic=key-protect-delete-keys) que está armazenada em seu serviço do {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp delete KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

```sh
$ ibmcloud kp delete 584fb0d9-dec2-47b8-bde5-50f05fd66261 -i $KP_INSTANCE_ID
Deleting key: 584fb0d9-dec2-47b8-bde5-50f05fd66261, from instance: 98d39ab8-cf44-4517-9583-2ad05c7e9bd5...

CONCLUÍDO

Deleted Key
584fb0d9-dec2-47b8-bde5-50f05fd66261
```
{: screen}

### Parâmetros necessários
{: #delete-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
   <dd>O ID da chave que você deseja excluir. Para recuperar uma lista de suas chaves disponíveis, execute o comando
<a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>O ID da instância do {{site.data.keyword.cloud_notm}} que identifica a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

## kp list
{: #kp-list}

Liste as 200 últimas chaves que estão disponíveis em sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp list -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

```sh
$ ibmcloud kp list -i $KP_INSTANCE_ID
Retrieving keys...

SUCCESS

Key ID                                 Key Name
3df42bc2-a991-41cb-acc2-3f9eab64a63f   sample-root-key
92e5fab3-00e8-40e9-8a2d-864de334b043   sample-imported-root-key
```
{: screen}

### Parâmetros necessários
{: #list-req-params}

<dl>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>O ID da instância do {{site.data.keyword.cloud_notm}} que identifica a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parâmetros opcionais
{: #list-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Configure o formato de saída da CLI. Por padrão, todos os comandos são impressos no formato de tabela. Para mudar o formato de saída para JSON, use <code>--output json</code>.</dd>
</dl>

## kp get
{: #kp-get}

Recuperar detalhes sobre uma chave, como os metadados da chave e o material da chave.

Se a chave tiver sido designada como uma chave raiz, o sistema não poderá retornar o material da chave para essa chave.

```
ibmcloud kp get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

```sh
$ ibmcloud kp get 3df42bc2-a991-41cb-acc2-3f9eab64a63f -i $KP_INSTANCE_ID
Grabbing info for key id: 3df42bc2-a991-41cb-acc2-3f9eab64a63f...

SUCCESS

Key ID                                 Key Name          Description     Creation Date                   Expiration Date
3df42bc2-a991-41cb-acc2-3f9eab64a63f   sample-root-key   A sample key.   2019-04-02 16:42:47 +0000 UTC   Key does not expire
```
{:screen}

### Parâmetros necessários
{: #get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>O ID da chave que você deseja recuperar. Para recuperar uma lista de suas chaves disponíveis, execute o comando
<a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>O ID da instância do {{site.data.keyword.cloud_notm}} que identifica a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parâmetros opcionais
{: #get-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Configure o formato de saída da CLI. Por padrão, todos os comandos são impressos no formato de tabela. Para mudar o formato de saída para JSON, use <code>--output json</code>.</dd>
</dl>

## kp rotate
{: #kp-rotate}

[Gire uma chave raiz](/docs/services/key-protect?topic=key-protect-wrap-keys) que esteja armazenada no serviço do {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp rotate KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-k, --key-material KEY_MATERIAL] 
```
{: pre}

```sh
$ ibmcloud kp rotate 3df42bc2-a991-41cb-acc2-3f9eab64a63f -i $KP_INSTANCE_ID
Rotating root key...

SUCCESS
```
{:screen}

### Parâmetros necessários
{: #rotate-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>O ID da chave raiz que você deseja girar.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>O ID da instância do {{site.data.keyword.cloud_notm}} que identifica a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parâmetros opcionais
{: #rotate-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>O material da chave codificada em Base64 que você deseja usar para girar uma chave raiz existente. Para girar uma chave
que foi importada inicialmente para o serviço, forneça uma nova chave de 256 bits. Para girar uma chave que foi gerada
inicialmente no {{site.data.keyword.keymanagementserviceshort}}, omita o parâmetro <code>--key-material</code>.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Configure o formato de saída da CLI. Por padrão, todos os comandos são impressos no formato de tabela. Para mudar o formato de saída para JSON, use <code>--output json</code>.</dd>
</dl>

## kp wrap
{: #kp-wrap}

[Agrupe uma chave de criptografia de dados](/docs/services/key-protect?topic=key-protect-wrap-keys) usando uma chave raiz que está armazenada na instância de serviço do {{site.data.keyword.keymanagementserviceshort}} que
você especifica.

```
ibmcloud kp wrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --plaintext DATA_KEY] 
                 [-a, --aad ADDITIONAL_DATA]
```
{: pre}


### Parâmetros necessários
{: #wrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>O ID da chave raiz que você deseja usar para agrupamento.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>O ID da instância do {{site.data.keyword.cloud_notm}} que identifica a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parâmetros opcionais
{: #wrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd>Os dados de autenticação adicionais (AAD) que são usados para proteção adicional de uma chave. Se fornecidos no
agrupamento, deverão ser fornecidos no desagrupamento.</dd>
    <dt><code>-p, --plaintext</code></dt>
        <dd>A chave de criptografia de dados codificada em Base64 (DEK) que você deseja gerenciar e proteger. Para importar uma chave existente, forneça uma chave de 256 bits. Para
gerar e agrupar uma nova DEK, omita o parâmetro <code>--plaintext</code>.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Configure o formato de saída da CLI. Por padrão, todos os comandos são impressos no formato de tabela. Para mudar o formato de saída para JSON, use <code>--output json</code>.</dd>
</dl>

## kp unwrap
{: #kp-unwrap}

[Desagrupe uma chave de criptografia de dados](/docs/services/key-protect?topic=key-protect-unwrap-keys) usando
uma chave raiz que está armazenada na instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp unwrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID 
                   CIPHERTEXT_FROM_WRAP
                   [-a, --aad ADDITIONAL_DATA, ..]
```
{: pre}

### Parâmetros necessários
{: #unwrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>O ID da chave raiz que você usou para a solicitação de agrupamento inicial.</dd>
    <dt><code>CIPHERTEXT_FROM_WRAP</code></dt>
        <dd>A chave de dados criptografados que foi retornada durante a operação de agrupamento inicial.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>O ID da instância do {{site.data.keyword.cloud_notm}} que identifica a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parâmetros opcionais
{: #unwrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd><p>Os dados de autenticação adicionais (AAD) que foram usados para proteção adicional de uma chave. É possível fornecer até
255 sequências, cada uma delas delimitada por uma vírgula. Se você forneceu o AAD no agrupamento, deverá especificar o
mesmo AAD no desagrupamento.</p><p><b>Importante:</b> o serviço do {{site.data.keyword.keymanagementserviceshort}} não salva dados de autenticação adicionais. Se você fornecer um AAD, salve os dados em um local seguro para assegurar que você possa acessar e fornecer o mesmo AAD durante as solicitações de desagrupamento subsequentes.</p></dd>
    <dt><code>-o, --output</code></dt>
        <dd>Configure o formato de saída da CLI. Por padrão, todos os comandos são impressos no formato de tabela. Para mudar o formato de saída para JSON, use <code>--output json</code>.</dd>
</dl>

## kp policy list
{: #kp-policy-list}

Lista as políticas associadas à chave raiz especificada.

```
ibmcloud kp policy list KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Parâmetros necessários
{: #policy-list-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>O ID da chave raiz que você deseja consultar. Para recuperar uma lista de suas chaves disponíveis, execute o comando
<a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>O ID da instância do {{site.data.keyword.cloud_notm}} que identifica a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parâmetros opcionais
{: #policy-list-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Configure o formato de saída da CLI. Por padrão, todos os comandos são impressos no formato de tabela. Para mudar o formato de saída para JSON, use <code>--output json</code>.</dd>
</dl>

## kp policy get
{: #kp-policy-get}

Recupera detalhes sobre uma política de chave, tais como o intervalo de rotação automática da chave.

```
ibmcloud kp policy get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Parâmetros necessários
{: #policy-get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>O ID da chave que você deseja consultar. Para recuperar uma lista de suas chaves disponíveis, execute o comando
<a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>O ID da instância do {{site.data.keyword.cloud_notm}} que identifica a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parâmetros opcionais
{: #policy-get-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Configure o formato de saída da CLI. Por padrão, todos os comandos são impressos no formato de tabela. Para mudar o formato de saída para JSON, use <code>--output json</code>.</dd>
</dl>

## kp policy set
{: #kp-policy-set}

Cria ou substitui a política associada à chave raiz especificada.

```
ibmcloud kp policy set KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 --set-type POLICY_TYPE 
                 [--policy INTERVAL]
```
{: pre}

### Parâmetros necessários
{: #policy-set-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>O ID da chave que você deseja consultar. Para recuperar uma lista de suas chaves disponíveis, execute o comando
<a href="#kp-list">kp list</a>.</dd>
   <dt><code>--set-type</code></dt>
        <dd>Especifica o tipo de política que você deseja configurar. Para configurar uma política de rotação, use <code>--set-type rotation</code>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>O ID da instância do {{site.data.keyword.cloud_notm}} que identifica a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parâmetros opcionais
{: #policy-set-opt-params}

<dl>
   <dt><code>-p, --policy</code></dt>
        <dd>Especifica o intervalo de tempo de rotação (em meses) para uma chave. O valor padrão é 1.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Configure o formato de saída da CLI. Por padrão, todos os comandos são impressos no formato de tabela. Para mudar o formato de saída para JSON, use <code>--output json</code>.</dd>
</dl>

