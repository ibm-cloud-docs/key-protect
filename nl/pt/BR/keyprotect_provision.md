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

# Fornecendo o serviço
{: #provision}

É possível criar uma instância de {{site.data.keyword.keymanagementservicefull}} usando o console do {{site.data.keyword.cloud_notm}} ou a CLI do {{site.data.keyword.cloud_notm}}.
{: shortdesc}

## Provisionando por meio do console do {{site.data.keyword.cloud_notm}}
{: #gui}

Para provisionar uma instância do {{site.data.keyword.keymanagementserviceshort}} por meio do console do {{site.data.keyword.cloud_notm}}, conclua as etapas a seguir.

1. [Efetue login em sua conta do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/){: new_window}.
2. Clique em **Catálogo** para visualizar a lista de serviços que estão disponíveis no {{site.data.keyword.cloud_notm}}.
3. Na área de janela de navegação Todas as categorias, role para **Plataforma** e clique na categoria
**Segurança**.
4. Na lista de serviços, clique no azulejo **{{site.data.keyword.keymanagementserviceshort}}**.
5. Selecione um plano de serviço e clique em **Criar** para provisionar uma instância do
{{site.data.keyword.keymanagementserviceshort}} na conta, região e grupo de recursos no qual você efetuou login.

## Provisionando por meio da CLI do {{site.data.keyword.cloud_notm}}
{: #cli}

É possível provisionar uma instância do {{site.data.keyword.keymanagementserviceshort}} usando a CLI do {{site.data.keyword.cloud_notm}}. 

### Criando uma instância de serviço em sua conta
{: #provision_acct_lvl}

Para simplificar o acesso às suas chaves de criptografia com funções do [{{site.data.keyword.iamlong}}](/docs/iam/users_roles.html#iamusermanrol), é possível criar uma ou mais instâncias do serviço do {{site.data.keyword.keymanagementserviceshort}} dentro de uma conta, sem precisar especificar uma organização e um espaço. 

1. Efetue login no {{site.data.keyword.cloud_notm}} por meio do [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/bluemix_cli/get_started.html){: new_window}.

    ```sh ibmcloud login 
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
    ibmcloud resource service-instance-create <instance_name> kms tiered-pricing
    ```
    {: pre}

    Substitua `<instance_name>` com um único alias para sua instância de serviço.

4. Opcional: verifique se a instância do serviço foi criada com êxito.

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

### Criando uma instância de serviço dentro de uma organização e espaço
{: #provision_space_lvl}

Para gerenciar o acesso às suas chaves de criptografia com funções do [Cloud
Foundry](/docs/iam/cfaccess.html), é possível criar uma instância do serviço do {{site.data.keyword.keymanagementserviceshort}} dentro de
uma organização e espaço especificados.  

1. Efetue login no {{site.data.keyword.cloud_notm}} por meio do [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/bluemix_cli/get_started.html){: new_window}.

    ```sh ibmcloud login 
    ```
    {: pre}
    **Observação:** se o login falhar, execute o comando `ibmcloud login --sso` e tente novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.

2. Selecione a conta, a região, a organização e o espaço onde você gostaria de criar uma instância de serviço do
{{site.data.keyword.keymanagementserviceshort}}.

    É possível usar o comando a seguir para configurar sua região de destino, organização e espaço.

    ```sh
    ibmcloud target -r <region> -o <organization_name> -s <space_name>
    ```
    {: pre}

3. Provisione uma instância do {{site.data.keyword.keymanagementserviceshort}} dentro dessa conta, região,
organização e espaço.

    ```sh
    ibmcloud service create kms tiered-pricing <instance_name>
    ```
    {: pre}

    Substitua `<instance_name>` com um único alias para sua instância de serviço.

4. Opcional: verifique se a instância do serviço foi criada com êxito.

    ```sh
    ibmcloud service list
    ```
    {: pre}


### O que vem a seguir

- Para ver um exemplo de como chaves armazenadas no {{site.data.keyword.keymanagementserviceshort}} podem funcionar para criptografar e decriptografar dados, [consulte o aplicativo de amostra no GitHub ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/IBM-Bluemix/key-protect-helloworld-python){: new_window}.
- Para saber mais sobre como gerenciar as suas chaves programaticamente, [verifique o documento de referência da API do {{site.data.keyword.keymanagementserviceshort}} para amostras de código ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/apidocs/639){: new_window}.
