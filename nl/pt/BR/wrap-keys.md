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

# Chaves de quebra
{: #wrap-keys}

É possível gerenciar e proteger suas chaves de criptografia com uma chave raiz usando a API do {{site.data.keyword.keymanagementservicelong}}, se você for um usuário privilegiado.
{: shortdesc}

Ao agrupar uma chave de criptografia de dados (DEK) com uma chave raiz, o {{site.data.keyword.keymanagementserviceshort}} combina a força de múltiplos algoritmos para proteger a privacidade e a integridade de seus dados criptografados.  

Para saber como a quebra de chave ajuda a controlar a segurança de dados em descanso na nuvem, consulte [Criptografia de envelope](/docs/services/key-protect/concepts/envelope-encryption.html).

## Agrupando chaves usando a API
{: #api}

É possível proteger uma chave de criptografia de dados especificada (DEK) com uma chave raiz que você gerencia em {{site.data.keyword.keymanagementserviceshort}}.

Ao fornecer uma chave raiz para agrupamento, assegure-se de que a chave raiz seja de 256, 384 ou 512 bits para que a
chamada de agrupamento possa ser bem-sucedida. Se você criar uma chave raiz no serviço, o {{site.data.keyword.keymanagementserviceshort}} gerará uma chave de 256 bits de seus HSMs, suportada pelo algoritmo AES-GCM.
{: note}

[Após você designar uma chave raiz no serviço](/docs/services/key-protect/create-root-keys.html), poderá agrupar uma DEK com criptografia avançada fazendo uma chamada `POST` para o terminal a seguir.

```
https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=wrap
```
{: codeblock}

1. [Recuperar suas credenciais de autenticação e serviço para que funcionem com chaves no serviço.](/docs/services/key-protect/access-api.html)

2. Copie o material de chave do DEK que você deseja gerenciar e proteger.

    Se você tiver privilégios de gerenciador ou de gravador para sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}, [será possível recuperar o material da chave para uma chave específica fazendo uma solicitação `GET /v2/keys/<key_ID>`](/docs/services/key-protect/view-keys.html#api).

3. Copie o ID da chave raiz que você deseja usar para agrupamento.

4. Execute o comando cURL a seguir para proteger a chave com uma operação de agrupamento.

    ```cURL
    curl -X POST \ 'https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=wrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "plaintext": "<data_key>",
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
        <td><strong>Necessário.</strong> O identificador exclusivo para a chave raiz que você deseja usar para agrupamento.</td>
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
        <td><varname>data_key</varname></td>
        <td>O material de chave da DEK que você deseja gerenciar e proteger. O valor <code>plaintext</code> deve ser codificado em base64. Para gerar um novo DEK, omita o atributo <code>plaintext</code>. O serviço gera um texto sem formatação aleatório (32 bytes), agrupa esse valor e, em seguida, retorna os valores gerados e agrupados na resposta.</td>
      </tr>
      <tr>
        <td><varname>additional_data</varname></td>
        <td>Os dados de autenticação adicional (AAD) que são usados para proteger a chave ainda mais. Cada sequência pode ter até 255 caracteres. Se você fornecer AAD quando fizer uma chamada de agrupamento para o serviço, deverá especificar o mesmo AAD durante a chamada de desagrupamento subsequente.<br></br>Importante: o serviço do {{site.data.keyword.keymanagementserviceshort}} não salva dados de autenticação adicionais. Se você fornecer um AAD, salve os dados em um local seguro para assegurar que você possa acessar e fornecer o mesmo AAD durante as solicitações de desagrupamento subsequentes.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabela 1. Descreve as variáveis que são necessárias para agrupar uma chave especificada em {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    Sua chave de criptografia de dados agrupados contendo o material de chave codificado em Base64 é retornada no corpo da entidade de resposta. O objeto JSON a seguir mostra um valor retornado de exemplo.

    ```
    {
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}
    
    Se você omitir o atributo `plaintext` ao fazer a solicitação de agrupamento, o serviço retornará a chave de criptografia de dados gerada (DEK) e a DEK agrupada no formato codificado em Base64.

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg==",
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}

    O valor <code>plaintext</code> representa a DEK desagrupada e o valor <code>ciphertext</code> representa a DEK agrupada.
    
    Se você desejar que o {{site.data.keyword.keymanagementserviceshort}} gere uma nova chave de criptografia de dados (DEK) em seu nome, também será possível passar um corpo vazio em uma solicitação de agrupamento. Sua DEK gerada, contendo o material de chave codificado em Base64, é retornada no corpo da entidade de resposta juntamente com a DEK agrupada.
    {: tip}
    
