---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: import standard encryption key, upload standard encryption key, import secret, persist secret, store secret, upload secret, store encryption key, standard key API examples

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

# Importando chaves padrão
{: #import-standard-keys}

É possível incluir suas chaves de criptografia existentes com a GUI do {{site.data.keyword.keymanagementserviceshort}} ou programaticamente com a API do {{site.data.keyword.keymanagementserviceshort}}.

## Importando chaves padrão com a GUI
{: #import-standard-key-gui}

[Depois de criar uma instância do serviço](/docs/services/key-protect?topic=key-protect-provision), conclua as etapas a seguir para inserir sua chave padrão existente com a GUI do {{site.data.keyword.keymanagementserviceshort}}.

1. [Efetue login no console do {{site.data.keyword.cloud_notm}}](https://{DomainName}/){: external}.
2. Acesse **Menu** &gt; **Lista de recursos** para visualizar uma lista de seus recursos.
3. Em sua lista de recursos do {{site.data.keyword.cloud_notm}}, selecione a sua instância provisionada do {{site.data.keyword.keymanagementserviceshort}}.
4. Para importar uma nova chave, clique em **Incluir chave** e selecione a janela **Importar sua própria chave**.

    Especifique os detalhes da chave:

    <table>
      <tr>
        <th>Configuração</th>
        <th>Descrição</th>
      </tr>
      <tr>
        <td>Nome</td>
        <td>
          <p>Um alias exclusivo, legível para fácil identificação de sua chave.</p>
          <p>Para proteger sua privacidade, assegure-se de que o nome da chave não contenha informações pessoalmente identificáveis (PII), como seu nome ou local.</p>
        </td>
      </tr>
      <tr>
        <td>Tipo de chave</td>
        <td>O <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">tipo de chave</a> que você gostaria de gerenciar no {{site.data.keyword.keymanagementserviceshort}}. Na lista de tipos de chave, selecione <b>Chave padrão</b>.</td>
      </tr>
      <tr>
        <td>Material de chave</td>
        <td>
          <p>O material de chave com codificação base64, como uma chave simétrica, que você deseja gerenciar no serviço.</p>
          <p>Certifique-se de que o material da chave atenda aos seguintes requisitos:</p>
          <p><ul>
              <li>A chave pode ter até 10.000 bytes de tamanho.</li>
              <li>Os bytes de dados devem ter codificação base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Tabela 1. Descreve as configurações <b>Importar a sua própria chave</b></caption>
    </table>

5. Quando você tiver concluído o preenchimento dos detalhes da chave, clique em **Importar chave** para confirmar. 

## Importando chaves padrão com a API
{: #import-standard-key-api}

Importe uma chave padrão fazendo uma chamada `POST` para o terminal a seguir:

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Recupere suas credenciais de serviço e autenticação para trabalhar com chaves no serviço](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Chame a [{{site.data.keyword.keymanagementserviceshort}}API](https://{DomainName}/apidocs/key-protect){: external} com o comando cURL a seguir.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'prefer: <return_preference>' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.key+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.key+json", "name": "<key_alias>", "description": "<key_description>", "expirationDate": "<YYYY-MM-DDTHH:MM:SS.SSZ>", "payload": "<key_material>", "extractable": <key_type> }
     ]
    }'
    ```
    {: codeblock}

    Substitua as variáveis na solicitação de exemplo de acordo com a tabela a seguir.
    <table>
      <tr>
        <th>Variável</th>
        <th>Descrição</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Necessário.</strong> A abreviação da região, como <code>us-south</code> ou <code>eu-gb</code>, que representa a área geográfica na qual reside sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">Terminais regionais em serviço</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Necessário.</strong> Seu token de acesso do {{site.data.keyword.cloud_notm}}. Inclua o conteúdo integral do token <code>IAM</code>, incluindo valor Bearer, na solicitação cURL. Para obter mais informações, veja <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Recuperando um token de acesso</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Necessário.</strong> O identificador exclusivo que é designado para sua instância de serviço {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, veja <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Recuperando um ID da instância</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>O identificador exclusivo que é usado para rastrear e correlacionar transações.</td>
      </tr>
      <tr>
        <td><varname>return_preference</varname></td>
        <td><p>Um cabeçalho que altera o comportamento do servidor para as operações <code>POST</code> e <code>DELETE</code>.</p><p>Quando você configurar a variável <em>return_preference</em> para <code>return=minimal</code>, o serviço retornará somente os metadados da chave, como o nome da chave e o valor de ID, no corpo da entidade de resposta. Quando você configura a variável para <code>return=representation</code>, o serviço retorna tanto o material da chave quanto os metadados da chave.</p></td>
      </tr>
      <tr>
        <td><varname>key_alias</varname></td>
        <td><strong>Necessário.</strong> Um nome exclusivo legível para fácil identificação da sua chave. Para proteger a sua privacidade, não armazene os seus dados pessoais como metadados para a sua chave.</td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>Uma descrição estendida de sua chave. Para proteger a sua privacidade, não armazene os seus dados pessoais como metadados para a sua chave.</td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>A data e hora em que a chave expira no sistema, em formato RFC 3339. Se o atributo <code>expirationDate</code> for omitido, a chave não expirará. </td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>O material de chave com codificação base64, como uma chave simétrica, que você deseja gerenciar no serviço.</p>
          <p>Certifique-se de que o material da chave atenda aos seguintes requisitos:</p>
          <p><ul>
              <li>A chave pode ter até 10.000 bytes de tamanho.</li>
              <li>Os bytes de dados devem ter codificação base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>Um valor booleano determina se o material de chave pode sair do serviço.</p>
          <p>Ao configurar o atributo <code>extractable</code> para <code>true</code>, o serviço designa a chave como uma chave padrão que você pode armazenar em seus aplicativos ou serviços.</p>
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabela 2. Descreve as variáveis que são necessárias para incluir uma chave padrão com a API do {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Para proteger a confidencialidade de seus dados pessoais, evite inserir informações pessoalmente identificáveis (PII), como seu nome ou local, ao incluir chaves no serviço. Para obter mais exemplos de PII, consulte a seção 2.2 da [NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
    {: important}

    Uma resposta `POST api/v2/keys` bem-sucedida retorna o valor de ID para a sua chave, junto com outros metadados. O ID é um identificador exclusivo que é designado para sua chave e é usado para chamadas subsequentes para a API do {{site.data.keyword.keymanagementserviceshort}}.

3. Opcional: verifique se a chave foi incluída executando a chamada a seguir para obter as chaves em sua instância de serviço {{site.data.keyword.keymanagementserviceshort}}.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}


## O que vem a seguir
{: #import-standard-key-next-steps}

- Para descobrir mais sobre como gerenciar programaticamente as suas chaves, [confira o doc de referência da API do {{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/apidocs/key-protect){: external}.
