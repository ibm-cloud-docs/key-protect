---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: instance ID, instance GUID, get instance ID, get instance GUID, instance ID API, instance ID CLI

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

# Recuperando o ID de sua instância
{: #retrieve-instance-ID}

É possível destinar uma instância de serviço individual do {{site.data.keyword.keymanagementservicelong}} para operações, incluindo seu identificador exclusivo ou ID da instância, em solicitações de API para o serviço.
{: shortdesc}

## Visualizando o ID da instância no console do {{site.data.keyword.cloud_notm}}
{: #view-instance-ID}

É possível visualizar o ID da instância que está associado à sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}} navegando para a sua lista de recursos do {{site.data.keyword.cloud_notm}}.

1. [Efetue login no console do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}){: new_window}.
2. Acesse **Menu** &gt; **Lista de recursos** e, em seguida, clique em **Serviços** para procurar uma lista de seus serviços de nuvem.
3. Clique na linha da tabela que descreve sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.
4. Na visualização de detalhes do serviço, copie o valor **GUID**.

    Esse valor **GUID** representa o ID da instância que identifica exclusivamente a instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.

## Recuperando um ID da instância com a CLI
{: #retrieve-instance-ID-cli}

Também é possível recuperar o ID da instância para sua instância de serviço usando a [CLI do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}.

1. Efetue login no {{site.data.keyword.cloud_notm}} com a [CLI do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Se o login falhar, execute o comando `ibmcloud login --sso` para tentar novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.
    {: note}

2. Selecione a conta, a região e o grupo de recursos que contêm sua instância provisionada do {{site.data.keyword.keymanagementserviceshort}}.

3. Recupere o Cloud Resource Name (CRN) que identifica exclusivamente sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. 

    ```sh
    ibmcloud resource service-instance < instance_name> -- id
    ```
    {: pre}

    Substitua `<instance_name>` pelo alias exclusivo que você designou para a instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. O exemplo truncado a seguir mostra a saída da CLI.

    ```
    crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:42454b3b-5b06-407b-a4b3-34d9ef323901:: 42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

    O valor _42454b3b-5b06-407b-a4b3-34d9ef323901_ é um ID da instância de exemplo.


## Recuperando um ID da instância com a API
{: #retrieve-instance-ID-api}

Talvez você queira recuperar o ID da instância programaticamente para ajudar a construir e conectar seu aplicativo. É possível chamar a [API do {{site.data.keyword.cloud_notm}} Resource Controller ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/resource-controller) e, em seguida, canalizar a saída JSON para `jq` a fim de extrair esse valor.

1. [Recupere um token de acesso do IAM do {{site.data.keyword.cloud_notm}}](/docs/services/key-protect?topic=key-protect-retrieve-access-token).
2. Chame a API do [Resource Controller ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/resource-controller) para recuperar o ID da instância.

    ```sh
    curl -X GET \
    https://resource-controller.cloud.ibm.com/v2/resource_instances \
    -H 'Authorization: Bearer <access_token>' | jq -r '.resources[] | select(.name | contains("<instance_name>")) | .guid'
    ```
    {: codeblock}

    Substitua `<instance_name>` pelo alias exclusivo que você designou para a instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. A saída a seguir mostra um ID de instância de exemplo:

    ```
    42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}
