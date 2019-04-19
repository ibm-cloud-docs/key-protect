---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: service instance, create service instance, KMS service instance, Key Protect service instance

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

# Fornecendo o serviço
{: #provision}

É possível criar uma instância de {{site.data.keyword.keymanagementservicefull}} usando o console do {{site.data.keyword.cloud_notm}} ou a CLI do {{site.data.keyword.cloud_notm}}.
{: shortdesc}

## Provisionando por meio do console do {{site.data.keyword.cloud_notm}}
{: #provision-gui}

Para provisionar uma instância do {{site.data.keyword.keymanagementserviceshort}} por meio do console do {{site.data.keyword.cloud_notm}}, conclua as etapas a seguir.

1. [Efetue login em sua conta do {{site.data.keyword.cloud_notm}}![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}){: new_window}.
2. Clique em **Catálogo** para visualizar a lista de serviços que estão disponíveis no {{site.data.keyword.cloud_notm}}.
3. Na área de janela de navegação Todas as categorias, clique na categoria **Segurança e identidade**.
4. Na lista de serviços, clique no azulejo **{{site.data.keyword.keymanagementserviceshort}}**.
5. Selecione um plano de serviço e clique em **Criar** para provisionar uma instância do
{{site.data.keyword.keymanagementserviceshort}} na conta, região e grupo de recursos no qual você efetuou login.

## Provisionando por meio da CLI do {{site.data.keyword.cloud_notm}}
{: #provision-cli}

Também é possível provisionar uma instância do {{site.data.keyword.keymanagementserviceshort}} usando a CLI do {{site.data.keyword.cloud_notm}}. 

1. Efetue login no {{site.data.keyword.cloud_notm}} por meio do [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}
    **Observação:** se o login falhar, execute o comando `ibmcloud login --sso` e tente novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.

2. Selecione a conta, a região e grupo de recursos no qual você gostaria de criar uma instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.

    É possível usar o comando a seguir para configurar sua região de destino e grupo de recursos.

    ```sh
    ibmcloud target -r <region_name> -g <resource_group_name>
    ```
    {: pre}

3. Provisione uma instância do {{site.data.keyword.keymanagementserviceshort}} dentro dessa conta e grupo de recursos.

    ```sh
    ibmcloud resource service-instance-create < instance_name > kms tiered-pricing
    ```
    {: pre}

    Substitua `<instance_name>` com um único alias para sua instância de serviço.

4. Opcional: verifique se a instância do serviço foi criada com êxito.

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

## O que vem a seguir
{: #provision-service-next-steps}

Para descobrir mais sobre como gerenciar programaticamente as suas chaves, [consulte o doc de referência da API do {{site.data.keyword.keymanagementserviceshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/key-protect){: new_window}.
