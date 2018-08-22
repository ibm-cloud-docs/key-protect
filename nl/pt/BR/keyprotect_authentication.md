---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Acessando a API
{: #access-api}

O {{site.data.keyword.keymanagementservicefull}} fornece uma API de REST que pode ser usada com qualquer
linguagem de programação para armazenar, recuperar e gerar chaves.
{: shortdesc}

Para trabalhar com a API, é necessário gerar suas credenciais de serviço e autenticação. 

## Recuperando um token de acesso
{: #retrieve_token}

É possível autenticar com o {{site.data.keyword.keymanagementserviceshort}} ao recuperar um token de acesso do
{{site.data.keyword.iamshort}}. Com um [ID de serviço](/docs/iam/serviceid.html#serviceids), é possível trabalhar
com a API do {{site.data.keyword.keymanagementserviceshort}} em nome do seu serviço ou aplicativo ou fora do
{{site.data.keyword.cloud_notm}}, sem precisar compartilhar suas credenciais do usuário pessoais.  

Se você desejar se autenticar com as suas credenciais do usuário, poderá recuperar o seu token executando `ibmcloud iam oauth-tokens` na CLI do [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/reference/bluemix_cli/get_started.html){: new_window}.
{: tip}

Conclua as seguintes etapas para recuperar um token de acesso:

1. No console do {{site.data.keyword.cloud_notm}}, acesse **Gerenciar** &gt;
**Segurança** &gt; **Identidade e acesso** &gt; **IDs de serviço**. Siga o processo para [criar um ID do serviço](/docs/iam/serviceid.html#creating-a-service-id){: new_window}.
2. Use o menu **Ações** para [definir uma política de acesso para o seu novo ID do serviço](/docs/iam/serviceidaccess.html){: new_window}.
    
    Para obter mais informações sobre como gerenciar o acesso para seus recursos do
{{site.data.keyword.keymanagementserviceshort}}, consulte [Funções e permissões](/docs/services/keymgmt/keyprotect_manage_access.html#roles).
3. Use a seção **Chaves API** para [criar uma chave API para associar com o ID do serviço](/docs/iam/serviceid_keys.html#serviceidapikeys){: new_window}. Salve sua chave API por meio de seu download para um local seguro.
4. Chame a API do {{site.data.keyword.iamshort}} para recuperar seu token de acesso.

    ```cURL
    curl -X POST \
      "https://iam.bluemix.net/identity/token" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -H "Accept: application/json" \
      -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>" \ 
    ```
    {: codeblock}

    Na solicitação, substitua `<API_KEY>` pela chave API que você criou na etapa 3. O exemplo truncado
a seguir mostra a saída de token:

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
programaticamente para seu serviço usando a API do {{site.data.keyword.keymanagementserviceshort}}. 

    Os tokens de acesso são válidos por 1 hora, mas é possível gerá-los novamente conforme necessário. Para manter o acesso ao serviço, atualize o token de acesso para a sua chave API em uma base regular chamando a API do {{site.data.keyword.iamshort}}.   
    {: tip }

## Recuperando o ID de sua instância
{: #retrieve_instance_ID}

É possível recuperar as informações de identificação para a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}} usando a CLI do [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/reference/bluemix_cli/get_started.html){: new_window}. Use um ID de instância para gerenciar suas chaves de criptografia dentro de uma instância especificada de
{{site.data.keyword.keymanagementserviceshort}} em sua conta. 

1. Efetue login no {{site.data.keyword.cloud_notm}} com a CLI do [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/reference/bluemix_cli/get_started.html){: new_window}.

    ```sh ibmcloud login 
    ```
    {: pre}

    **Observação:** se o login falhar, execute o comando `ibmcloud login --sso` e tente novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.

2. Selecione a conta que contém sua instância provisionada do {{site.data.keyword.keymanagementserviceshort}}.

    É possível executar `ibmcloud resource service-instances` para listar todas as instâncias de serviço que são fornecidas em sua conta.

3. Recupere o Cloud Resource Name (CRN) que identifica exclusivamente sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. 

    ```sh
    ibmcloud resource service-instance <instance_name> --id
    ```
    {: pre}

    Substitua `<instance_name>` com o alias exclusivo que você designou para sua instância do
{{site.data.keyword.keymanagementserviceshort}}. O exemplo truncado a seguir mostra a saída da CLI. O valor _42454b3b-5b06-407b-a4b3-34d9ef323901_ é um ID da instância de exemplo.

    ```
    crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:42454b3b-5b06-407b-a4b3-34d9ef323901::
    ```
    {: screen}

## Formando sua solicitação de API
{: #form_api_request}

Ao fazer uma chamada API para o serviço, estruture sua solicitação de API de acordo com a maneira como você inicialmente
provisionou sua instância do {{site.data.keyword.keymanagementserviceshort}}. 

Para construir sua solicitação, emparelhe um [terminal em serviço
regional](/docs/services/keymgmt/keyprotect_regions.html) com as credenciais de autenticação apropriadas. Por exemplo, se você criou uma instância de serviço para a região us-south, use o terminal a seguir e os cabeçalhos de API para procurar chaves em seu serviço:

```cURL
curl -X GET \
    https://keyprotect.us-south.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-instance: <instance_ID>' \
```
{: codeblock} 

### O que vem a seguir

- Para saber mais sobre como gerenciar as suas chaves programaticamente, [verifique o documento de referência da API do {{site.data.keyword.keymanagementserviceshort}} para amostras de código ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/apidocs/639){: new_window}.
- Para ver um exemplo de como chaves armazenadas no {{site.data.keyword.keymanagementserviceshort}} podem funcionar para criptografar e decriptografar dados, [consulte o aplicativo de amostra no GitHub ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/IBM-Bluemix/key-protect-helloworld-python){: new_window}.
