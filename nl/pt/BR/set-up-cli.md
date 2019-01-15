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

# Configurando a CLI
{: #set-up-cli}

É possível usar o plug-in da CLI do {{site.data.keyword.keymanagementservicelong_notm}} para ajudar a criar, importar e gerenciar as chaves de criptografia.

Para saber mais sobre como usar a CLI, consulte o doc de referência da CLI do [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/cli-reference.html).
{: tip}

## Instalando o plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}}
{: #install-cli}

Antes de ser possível configurar o plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}}, instale a CLI do [{{site.data.keyword.cloud_notm}}![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/index.html#overview){: new_window}. 

Para instalar as CLIs:

1. Instale a CLI do [{{site.data.keyword.cloud_notm}}![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/index.html#overview){: new_window}.

    Após instalar a CLI, será possível executar os comandos `ibmcloud` para interagir com os serviços de nuvem.

2. Efetue login no {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Observação:** se o login falhar, execute o comando `ibmcloud login --sso` e tente novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.

3. Para iniciar o gerenciamento de chaves de criptografia, instale o plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}}.

    ```sh
    ibmcloud plugin install key-protect -r 'IBM Cloud'
    ```
    {: pre}

4. Opcional: verifique se o plug-in foi instalado com êxito.

    ```sh
    lista de plug-in do ibmcloud
    ```
    {: pre}

## Atualizando o plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}}
{: #update-cli}

Você pode desejar atualizar a CLI periodicamente para usar novos recursos.

Para atualizar a CLI:

1. Efetue login no {{site.data.keyword.cloud_notm}} com a [CLI do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/index.html#overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Observação:** se o login falhar, execute o comando `ibmcloud login --sso` e tente novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.

2. Instale a atualização do repositório de plug-in.

    ```sh
    ibmcloud plugin update key-protect -r 'IBM Cloud'
    ```
    {: pre}

3. Opcional: verifique se o plug-in foi atualizado com êxito.

    ```sh
    lista de plug-in do ibmcloud
    ```
    {: pre}

## Desinstalando o plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}}
{: #uninstall-cli}

1. Efetue login no {{site.data.keyword.cloud_notm}} com a [CLI do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/index.html#overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Observação:** se o login falhar, execute o comando `ibmcloud login --sso` e tente novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.

2. Instale a atualização do repositório de plug-in.

    ```sh
    ibmcloud plugin uninstall key-protect
    ```
    {: pre}

3. Opcional: verifique se o plug-in foi desinstalado com êxito.

    ```sh
    lista de plug-in do ibmcloud
    ```
    {: pre}
