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

# Importando chaves raiz
{: #import-root-keys}

É possível usar {{site.data.keyword.keymanagementservicefull}} para proteger suas chaves raiz existentes usando a GUI do {{site.data.keyword.keymanagementserviceshort}} ou programaticamente com a {{site.data.keyword.keymanagementserviceshort}} do API.
{: shortdesc}

Chaves raiz são chaves simétricas de quebra de chaves usadas para proteger a segurança dos dados criptografados na nuvem. Para obter mais informações sobre como importar chaves raiz para o {{site.data.keyword.keymanagementserviceshort}}, consulte [Trazendo suas chaves de criptografia para a nuvem](/docs/services/key-protect?topic=key-protect-importing-keys).

## Importando chaves raiz com a GUI
{: #import-root-key-gui}

[Depois de criar uma instância do serviço](/docs/services/key-protect?topic=key-protect-provision), conclua as etapas a seguir para incluir sua chave raiz existente com a {{site.data.keyword.keymanagementserviceshort}}do GUI.

1. [Efetue login no console do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/){: new_window}.
2. Acesse **Menu** &gt; **Lista de recursos** para visualizar uma lista de seus recursos.
3. Em sua lista de recursos do {{site.data.keyword.cloud_notm}}, selecione a sua instância provisionada do {{site.data.keyword.keymanagementserviceshort}}.
4. Para importar uma chave, clique em **Incluir chave** e selecione a janela **Importar sua própria chave**.

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
        <td>O <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">tipo de chave</a> que você gostaria de gerenciar no {{site.data.keyword.keymanagementserviceshort}}. Na lista de tipos de chave, selecione <b>Chave raiz</b>.</td>
      </tr>
      <tr>
        <td>Material de chave</td>
        <td>
          <p>O material de chave com codificação base64, como uma chave de quebra de chave, que você deseja armazenar e gerenciar no serviço.</p>
          <p>Certifique-se de que o material da chave atenda aos seguintes requisitos:</p>
          <p>
            <ul>
              <li>A chave deve ser 128, 192 ou 256 bits.</li>
              <li>Os bytes de dados, por exemplo, 32 bytes para 256 bits, devem ser codificados usando codificação base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Tabela 1. Descreve as configurações <b>Importar a sua própria chave</b></caption>
    </table>

5. Quando você tiver concluído o preenchimento dos detalhes da chave, clique em **Importar chave** para confirmar. 

## Importando chaves raiz com a API
{: #import-root-key-api}

É possível importar suas chaves raiz para o serviço usando a API do {{site.data.keyword.keymanagementserviceshort}}.

Planeje antecipadamente a importação de chaves [revisando suas opções de criação e criptografia do material de chave](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead). Para segurança adicional, é possível ativar a importação segura do material da chave usando uma [chave de transporte](/docs/services/key-protect?topic=key-protect-importing-keys#transport-keys) para criptografar seu material da chave antes de trazê-la para a nuvem. Se você preferir importar uma chave raiz sem usar uma chave de transporte, pule para a [etapa 4](#import-root-key).
{: note}

### Etapa 1: criar uma chave de transporte
{: #create-transport-key}

As chaves de transporte são atualmente um recurso beta. Os recursos Beta podem mudar a qualquer momento, e as atualizações futuras podem introduzir mudanças que sejam incompatíveis com a versão mais recente.
{: important}

Crie uma chave de transporte para sua instância de serviço fazendo uma chamada `POST` para o terminal a seguir.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers
```
{: codeblock}

1. [Recupere suas credenciais de serviço e autenticação para trabalhar com chaves no serviço](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Configure uma política para sua chave de transporte chamando a API do [{{site.data.keyword.keymanagementserviceshort}}![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/key-protect){: new_window}.

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

 Substitua as variáveis na solicitação de exemplo de acordo com a tabela a seguir.

  <table>
    <tr>
      <th>Variável</th>
      <th>Descrição</th>
    </tr>
    <tr>
      <td><varname>region</varname></td>
      <td><strong>Necessário.</strong> A abreviação da região, como <code>us-south</code> ou <code>eu-gb</code>, que representa a área geográfica na qual reside sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Terminais regionais em serviço</a>.</td>
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
      <td><varname>expiration_time</varname></td>
      <td>
        <p>O tempo em segundos a partir da criação de uma chave de transporte que determina por quanto tempo a chave permanece válida.</p>
        <p>O valor mínimo é 300 segundos (5 minutos), e o máximo é 86.400 (24 horas). O valor padrão é 600 (10 minutos).</p>
      </td>
    </tr>
    <tr>
      <td><varname>use_count</varname></td>
      <td>O número de vezes que uma chave de transporte pode ser recuperada dentro de seu prazo de expiração antes de que ela não esteja mais acessível. O valor padrão é 1.</td>
    </tr>
      <caption style="caption-side:bottom;">Tabela 2. Descreve as variáveis que são necessárias para criar uma API da chave de transporte do {{site.data.keyword.keymanagementserviceshort}}</caption>
  </table>

  Uma resposta `POST api/v2/lockers` bem-sucedida retorna o valor do ID para sua chave de transporte, junto com outros metadados. O ID é um identificador exclusivo que está associado a uma chave de transporte e é usado para chamadas subsequentes para a API do {{site.data.keyword.keymanagementserviceshort}}.

### Etapa 2: recuperar a chave de transporte e o token de importação
{: #retrieve-transport-key}

Recupere uma chave de transporte e o token de importação, fazendo uma chamada `GET` para o terminal a seguir.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers/<key_id>
```
{: codeblock}

1. Chame a API do [{{site.data.keyword.keymanagementserviceshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/key-protect){: new_window} com o comando cURL a seguir.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/locker/<locker_id> \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json'
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
        <td><strong>Necessário.</strong> A abreviação da região, como <code>us-south</code> ou <code>eu-gb</code>, que representa a área geográfica na qual reside sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Terminais regionais em serviço</a>.</td>
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
        <td><varname>locker_ID</varname></td>
        <td><strong>Necessário.</strong> O identificador para a chave de transporte que você criou na <a href="#create-transport-key">etapa 1</a>.</td>
      </tr>
        <caption style="caption-side:bottom;">Tabela 3. Descreve as variáveis que são necessárias para recuperar uma chave de transporte com a API do {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Uma resposta `GET api/v2/lockers/{id}` bem-sucedida retorna uma chave de criptografia pública codificada em DER de 4096 bits em formato PKIX que pode ser usada para criptografar seu material de chave raiz, junto com um token de importação que é usado para verificar a integridade da chave de transporte.

### Etapa 3: criptografar seu material de chave
{: #encrypt-root-key}

Depois de recuperar a chave de transporte, use a chave para criptografar o material da chave que você deseja importar para o {{site.data.keyword.keymanagementserviceshort}}.  

<!-- TODO: Add link to tutorial that uses OpenSSL for key generation and encryption (in progress)-->

Para gerar material de chave no local, [revise suas opções para criar chaves de criptografia simétricas](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead). Por exemplo, você pode desejar usar o sistema de gerenciamento de chave interno de sua organização, suportado por um hardware security module (HSM) no local, para criar e exportar o material da chave.
{: note}

Para criptografar seu material de chave:

1. Exporte o material da chave de 256 bits em formato binário do seu sistema de gerenciamento de chave no local.

    Para aprender como criar e exportar o material da chave, consulte a documentação para seu sistema de gerenciamento de HSM ou chave no local.

2. Use a [chave de transporte recuperada](#retrieve-transport-key) da etapa 2 para criptografar o material da chave.

   Quando você criptografar seu material da chave, use o esquema de criptografia `RSAES_OAEP_SHA_256`. Esse é o esquema padrão que o {{site.data.keyword.keymanagementserviceshort}} usa para criar a chave de transporte. Para evitar problemas de criptografia no {{site.data.keyword.keymanagementserviceshort}}, não inclua o parâmetro opcional `label` ao executar a criptografia RSAES_OAEP no material de chave. Para saber como executar a criptografia RSA em seu material de chave, consulte a documentação para seu HSM no local ou sistema de gerenciamento de chaves.

3. Assegure-se de que o material da chave criptografada seja codificado em base64 antes de continuar com a próxima etapa.

### Etapa 4: importar o material da chave
{: #import-root-key}

[Depois de criptografar e codificar base64 no material da chave](#encrypt-root-key), importe a chave raiz para o {{site.data.keyword.keymanagementserviceshort}} fazendo uma chamada `POST` para o terminal a seguir.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. Chame a API do [{{site.data.keyword.keymanagementserviceshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/key-protect){: new_window} com o comando cURL a seguir.

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

    Substitua as variáveis na solicitação de exemplo de acordo com a tabela a seguir.
    <table>
      <tr>
        <th>Variável</th>
        <th>Descrição</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Necessário.</strong> A abreviação da região, como <code>us-south</code> ou <code>eu-gb</code>, que representa a área geográfica na qual reside sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Terminais regionais em serviço</a>.</td>
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
        <td><varname>key_alias</varname></td>
        <td><strong>Necessário.</strong> Um nome exclusivo legível para fácil identificação da sua chave. Para proteger a sua privacidade, não armazene os seus dados pessoais como metadados para a sua chave.</td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>Uma descrição estendida de sua chave. Para proteger a sua privacidade, não armazene os seus dados pessoais como metadados para a sua chave.</td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>A data e hora em que a chave expira no sistema, em formato RFC 3339. Se o atributo <code>expirationDate</code> for omitido, a chave não expirará.</td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>O material de chave com codificação base64, como uma chave de quebra de chave, que você deseja armazenar e gerenciar no serviço.</p>
          <p>Certifique-se de que o material da chave atenda aos seguintes requisitos:</p>
          <p>
            <ul>
              <li>A chave deve ser 128, 192 ou 256 bits.</li>
              <li>Os bytes de dados, por exemplo, 32 bytes para 256 bits, devem ser codificados usando codificação base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>Um valor booleano determina se o material de chave pode sair do serviço.</p>
          <p>Ao configurar o atributo <code>extractable</code> para <code>false</code>, o serviço designa a chave como uma chave raiz que pode ser usada para operações de <code>wrap</code> ou <code>unwrap</code>.</p>
        </td>
      </tr>
      <tr>
        <td><varname>encryption_algorithm</varname></td>
        <td>O esquema de criptografia que você usou para <a href="#encrypt-root-key">criptografar o material da chave</a>. Atualmente, <code>RSAES_OAEP_SHA_256</code> é suportado. Para importar o material de chave raiz sem usar uma chave de transporte e um token de importação, omita o atributo <code>encryption_algorithm</code>.</td>
      </tr>
      <tr>
        <td><varname>import_token</varname></td>
        <td>O token de importação que é usado para verificar a vivacidade e a integridade de uma chave de transporte. Se você criptografar seu material de chave usando uma chave de transporte, deverá fornecer o mesmo token de importação que recuperou na <a href="#retrieve-transport-key">etapa 2</a>. Para importar o material de chave raiz sem usar uma chave de transporte e um token de importação, omita o atributo <code>importToken</code>.</td>
      </tr>
        <caption style="caption-side:bottom;">Tabela 4. Descreve as variáveis que são necessárias para incluir uma chave raiz com a API do {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Para proteger a confidencialidade de seus dados pessoais, evite inserir informações pessoalmente identificáveis (PII), como seu nome ou local, ao incluir chaves no serviço. Para obter mais exemplos de PII, consulte a seção 2.2 do [NIST Special Publication 800-122 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: new_window}.
    {: important}

    Uma resposta `POST api/v2/keys` bem-sucedida retorna o valor de ID para a sua chave, junto com outros metadados. O ID é um identificador exclusivo que é designado para sua chave e é usado para chamadas subsequentes para a API do {{site.data.keyword.keymanagementserviceshort}}.

2. Opcional: verifique se a chave foi incluída executando a chamada a seguir para procurar as chaves na instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## O que vem a seguir
{: #import-root-key-next-steps}

- Para descobrir mais sobre como proteger chaves com criptografia de envelope, consulte [Chaves de quebra](/docs/services/key-protect?topic=key-protect-wrap-keys).
- Para descobrir mais sobre como gerenciar programaticamente as suas chaves, [consulte o doc de referência da API do {{site.data.keyword.keymanagementserviceshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/key-protect){: new_window}.
