---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: access token, IAM token, generate access token, generate IAM token, get access token, get IAM token, IAM token API, IAM token CLI

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

# Recuperando um token de acesso
{: #retrieve-access-token}

Comece com as APIs do {{site.data.keyword.keymanagementservicelong}} autenticando as suas solicitações para o serviço com um token de acesso do {{site.data.keyword.iamlong}} (IAM).
{: shortdesc}

## Recuperando um token de acesso com a CLI
{: #retrieve-token-cli}

É possível usar a [CLI do {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external} para gerar rapidamente o seu token de acesso do Cloud IAM pessoal.

1. Efetue login no {{site.data.keyword.cloud_notm}} com a [CLI do {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Se o login falhar, execute o comando `ibmcloud login --sso` para tentar novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.
    {: note}

2. Selecione a conta, a região e o grupo de recursos que contêm sua instância provisionada do {{site.data.keyword.keymanagementserviceshort}}.

3. Execute o comando a seguir para recuperar o token de acesso do Cloud IAM.

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    O exemplo truncado a seguir mostra um token do IAM recuperado.

    ```sh
    Token do IAM:  Bearer eyJraWQiOiIyM...
    ```
    {: screen}

## Recuperando um token de acesso com a API
{: #retrieve-token-api}

Também é possível recuperar seu token de acesso programaticamente primeiro criando uma [chave de API do ID do serviço](/docs/iam?topic=iam-serviceidapikeys) para seu aplicativo e, em seguida, trocando sua chave de API por um token do {{site.data.keyword.cloud_notm}} IAM.

1. Efetue login no {{site.data.keyword.cloud_notm}} com a [CLI do {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Se o login falhar, execute o comando `ibmcloud login --sso` para tentar novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.
    {: note}

2. Selecione a conta, a região e o grupo de recursos que contêm sua instância provisionada do {{site.data.keyword.keymanagementserviceshort}}.

3. Crie um [ID de serviço](/docs/iam?topic=iam-serviceids#creating-a-service-id) para seu aplicativo.

  ```sh
  ibmcloud iam service-id-create SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
  ```
  {: pre}

4. [Designe uma política de acesso](/docs/iam?topic=iam-serviceidpolicy) ao ID do serviço.

    É possível designar permissões de acesso para seu ID de serviço [usando o console do {{site.data.keyword.cloud_notm}}](/docs/iam?topic=iam-serviceidpolicy#access_new). Para saber como as funções de acesso _Gerente_, _Gravador_ e _Leitor_ são mapeadas para ações de serviço do {{site.data.keyword.keymanagementserviceshort}} específicas, consulte [Funções e permissões](/docs/services/key-protect?topic=key-protect-manage-access#roles).
    {: tip}

5. Crie uma [chave de API do ID de serviço](/docs/iam?topic=iam-serviceidapikeys).

  ```sh
  ibmcloud iam service-api-key-create API_KEY_NAME SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
                     [--file FILE_NAME]
  ```
  {: pre}

  Substitua `<service_ID_name>` pelo alias exclusivo que você designou para o seu ID de serviço na etapa anterior. Salve sua chave API por meio de seu download para um local seguro. 

6. Chame a [API de serviços de identidade do IAM](https://{DomainName}/apidocs/iam-identity-token-api) para recuperar seu token de acesso.

    ```cURL
    curl -X POST \
      "https://iam.cloud.ibm.com/identity/token" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -H "Accept: application/json" \
      -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>" > token.json
    ```
    {: codeblock}

    Na solicitação, substitua `<API_KEY>` pela chave de API que você criou na etapa anterior. O exemplo truncado a seguir mostra os conteúdos do arquivo `token.json`:

    ```
    {
    "access_token": "eyJraWQiOiIyM...",
    "expiration": 1512161390,
    "expires_in": 3600,
    "refresh_token": "...",
    "token_type": "Bearer"
    }
    ```
    {: screen}

    Use o valor integral `access_token`, prefixado pelo tipo de token _Bearer_ para gerenciar chaves
programaticamente para seu serviço usando a API do {{site.data.keyword.keymanagementserviceshort}}. Para ver um exemplo de solicitação de API do {{site.data.keyword.keymanagementserviceshort}}, confira [Formando sua solicitação de API](/docs/services/key-protect?topic=key-protect-set-up-api#form-api-request).

    Os tokens de acesso são válidos por 1 hora, mas é possível gerá-los novamente conforme necessário. Para manter o acesso ao serviço, gere novamente o token de acesso para sua chave de API regularmente chamando a [API de serviços de identidade do IAM](https://{DomainName}/apidocs/iam-identity-token-api).   
    {: note }

    <!--You can also pipe the output to `jq`, and then grab only the `access_token` value `| jq .access_token-->

    <!--You use IBM® Cloud Identity and Access Management (IAM) tokens to make authenticated requests to IBM Watson™ services without embedding service credentials in every call. IAM authentication uses access tokens for authentication, which you acquire by sending a request with an API key.-->
