---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Desagrupando chaves
{: #unwrap-keys}

É possível desagrupar uma chave de criptografia de dados (DEK) para acessar seu conteúdo usando a API do {{site.data.keyword.keymanagementservicefull}}, se você for um usuário privilegiado. O desagrupamento de uma DEK decriptograda e verifica a integridade de seu conteúdo, retornando o material de chave original para o serviço de dados do {{site.data.keyword.cloud_notm}}.
{: shortdesc}

Para saber como a quebra de chave ajuda a controlar a segurança de dados em descanso na nuvem, consulte [Criptografia de envelope](/docs/services/key-protect/concepts/envelope-encryption.html).

## Desagrupando chaves usando a API
{: #api}

[Após você fazer uma chamada de agrupamento para o serviço](/docs/services/key-protect/wrap-keys.html), poderá desagrupar uma chave de criptografia de dados (DEK) especificada para acessar os seus conteúdos fazendo uma chamada `POST` para o terminal a seguir.

```
https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_id>?action=unwrap
```
{: codeblock}

1. [Recupere suas credenciais de serviço e autenticação para trabalhar com chaves no serviço](/docs/services/key-protect/access-api.html).

2. Copie o ID da chave raiz que você usou para executar a solicitação de agrupamento inicial.

    É possível recuperar o ID para uma chave fazendo uma solicitação `GET /v2/keys` ou visualizando suas chaves na GUI do {{site.data.keyword.keymanagementserviceshort}}.

3. Copie o valor `ciphertext` que foi retornado durante a solicitação de agrupamento inicial.

4. Execute o comando cURL a seguir para decriptografar e autenticar o material da chave.

    ```cURL
    curl -X POST \ 'https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=unwrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "ciphertext": "<encrypted_data_key>",
      "aad": ["<additional_data>", "<additional_data>"]
    }'
    ```
    {: codeblock}

    Para trabalhar com chaves dentro de uma organização e um espaço do Cloud Foundry em sua conta, substitua `Bluemix-Instance` pelos cabeçalhos `Bluemix-org` e `Bluemix-space` apropriados. [Para obter mais informações, consulte o doc de referência da API do {{site.data.keyword.keymanagementserviceshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/key-protect){: new_window}.
    {: tip}

    Substitua as variáveis na solicitação de exemplo de acordo com a tabela a seguir.
    <table>
      <tr>
        <th>Variável</th>
        <th>Descrição</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Necessário.</strong> A abreviação da região, como <code>us-south</code> ou <code>eu-gb</code>, que representa a área geográfica na qual reside sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, consulte <a href="/docs/services/key-protect/regions.html#endpoints">Terminais regionais em serviço</a>.</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Necessário.</strong> O identificador exclusivo para a chave raiz usada para a solicitação de agrupamento inicial.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Necessário.</strong> Seu token de acesso do {{site.data.keyword.cloud_notm}}. Inclua o conteúdo integral do token <code>IAM</code>, incluindo valor Bearer, na solicitação cURL. Para obter mais informações, veja <a href="/docs/services/key-protect/access-api.html#retrieve-token">Recuperando um token de acesso</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Necessário.</strong> O identificador exclusivo que é designado para sua instância de serviço {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, veja <a href="/docs/services/key-protect/access-api.html#retrieve-instance-ID">Recuperando um ID da instância</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>O identificador exclusivo que é usado para rastrear e correlacionar transações.</td>
      </tr>
      <tr>
        <td><varname>additional_data</varname></td>
        <td>Os dados de autenticação adicional (AAD) que são usados para proteger a chave ainda mais. Cada sequência pode ter até 255 caracteres. Se você fornecer um AAD quando fizer uma chamada de agrupamento para o serviço, deverá especificar o mesmo AAD durante a chamada de desagrupamento.</td>
      </tr>
      <tr>
        <td><varname>encrypted_data_key</varname></td>
        <td><strong>Necessário.</strong> O valor <code>ciphertext</code> que foi retornado durante uma operação de agrupamento.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabela 1. Descreve as variáveis que são necessárias para desagrupar chaves no {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    O material da chave original é retornado no corpo da entidade de resposta. O objeto JSON a seguir mostra um valor retornado de exemplo.

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg=="
    }
    ```
    {:screen}
