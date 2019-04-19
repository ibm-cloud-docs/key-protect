---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: install CLI plug-in, install CLI plugin, update CLI plug-in, update CLI plugin, uninstall CLI plug-in, uninstall CLI plugin, Key Protect CLI plug-in, Key Protect CLI plugin, KMS plug-in, KMS plugin

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

# Configurando a CLI
{: #set-up-cli}

É possível usar o plug-in da CLI do {{site.data.keyword.keymanagementservicelong_notm}} para ajudar a criar, importar e gerenciar as chaves de criptografia.

Para saber mais sobre como usar o plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}}, confira o [doc de referência da CLI do {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-cli-reference).
{: tip}

## Instalando o plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}}
{: #install-cli}

Antes de configurar o plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}}, instale a CLI do [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}. 

Para instalar as CLIs:

1. Instale a [CLI do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}.

    Após instalar a CLI, será possível executar os comandos `ibmcloud` para interagir com os serviços de nuvem.

2. Efetue login no {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Se o login falhar, execute o comando `ibmcloud login --sso` para tentar novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.
    {: note}

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

1. Efetue login no {{site.data.keyword.cloud_notm}} com a [CLI do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Se o login falhar, execute o comando `ibmcloud login --sso` para tentar novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.
    {: note}

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

1. Efetue login no {{site.data.keyword.cloud_notm}} com a [CLI do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Se o login falhar, execute o comando `ibmcloud login --sso` para tentar novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.
    {: note}

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
