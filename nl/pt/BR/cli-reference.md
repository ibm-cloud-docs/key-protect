---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Referência da CLI do {{site.data.keyword.keymanagementserviceshort}}
{: #key-protect-cli}

É possível usar o plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}} para gerenciar chaves em
sua instância do {{site.data.keyword.keymanagementserviceshort}}.
{:shortdesc}

Para instalar o plug-in da CLI, consulte [Configurando a CLI](/docs/services/key-protect/set-up-cli.html). 

Ao efetuar login na CLI do [{{site.data.keyword.cloud_notm}}
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/index.html#overview){: new_window}, você será notificado quando as atualizações estiverem disponíveis. Certifique-se de manter a CLI atualizada para que seja possível usar os comandos e sinalizadores que estão disponíveis
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
            <td><a href="#kp-rotate">kp rotate</a></td>
            <td><a href="#kp-unwrap">kp unwrap</a></td>
        </tr>
        <tr>
            <td><a href="#kp-wrap">kp wrap</a></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">Tabela 1. Comandos para gerenciar chaves</caption> 
 </table>

## kp create
{: #kp-create}

[Crie uma chave raiz](/docs/services/key-protect/create-root-keys.html) na instância de serviço do {{site.data.keyword.keymanagementserviceshort}} que você especifica. 

```sh
ibmcloud kp create KEY_NAME -i INSTANCE_ID | $INSTANCE_ID
                   [-k, --key-material KEY_MATERIAL] 
                   [-s, --standard-key]
```
{:pre}

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
        <dd>Configure o parâmetro somente se você desejar criar uma <a href="/docs/services/key-protect/concepts/envelope-encryption.html#key-types">chave padrão</a>. Para criar uma chave raiz, omita o parâmetro <code>--standard-key</code>.</dd>
</dl>

## kp delete
{: #kp-delete}

[Exclua uma chave](/docs/services/key-protect/delete-keys.html) que está armazenada em seu serviço do {{site.data.keyword.keymanagementserviceshort}}.

```sh
ibmcloud kp delete KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

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

```sh
ibmcloud kp list -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Parâmetros necessários
{: #list-req-params}

<dl>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>O ID da instância do {{site.data.keyword.cloud_notm}} que identifica a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

## kp rotate
{: #kp-rotate}

[Gire uma chave raiz](/docs/services/key-protect/wrap-keys.html) que esteja armazenada no serviço do {{site.data.keyword.keymanagementserviceshort}}.

```sh
ibmcloud kp rotate KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --key-material KEY_MATERIAL]
```
{: pre}

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
</dl>

## kp wrap
{: #kp-wrap}

[Agrupe uma chave de criptografia de dados](/docs/services/key-protect/wrap-keys.html) usando uma chave raiz que está armazenada na instância de serviço do {{site.data.keyword.keymanagementserviceshort}} que
você especifica.

```sh
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
</dl>

## kp unwrap
{: #kp-unwrap}

[Desagrupe uma chave de criptografia de dados](/docs/services/key-protect/unwrap-keys.html) usando
uma chave raiz que está armazenada na instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.

```sh
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
</dl>



