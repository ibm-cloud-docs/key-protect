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

# Configurazione della CLI
{: #set-up-cli}

Puoi utilizzare il plug-in della CLI {{site.data.keyword.keymanagementservicelong_notm}} per un ausilio nella creazione, importazione e gestione di chiavi di crittografia.

Per ulteriori informazioni sull'utilizzo della CLI, consulta la [documentazione di riferimento della CLI {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/cli-reference.html).
{: tip}

## Installazione del plug-in della CLI {{site.data.keyword.keymanagementserviceshort}}
{: #install-cli}

Prima di configurare il plug-in della CLI {{site.data.keyword.keymanagementserviceshort}}, installa la CLI [{{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli/index.html#overview){: new_window}. 

Per installare le CLI:

1. Installa la [CLI {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli/index.html#overview){: new_window}.

    Dopo che hai installato la CLI, puoi eseguire i comandi `ibmcloud` per interagire con i tuoi servizi cloud.

2. Accedi a {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Nota:** se l'accesso non riesce, esegui il comando `ibmcloud login --sso` e prova di nuovo. Il parametro `--sso` è obbligatorio quando
accedi con un ID federato. Se viene utilizzata questa opzione, vai al link elencato nell'output della CLI
per generare una passcode monouso.

3. Per iniziare a gestire le chiavi di crittografia, installa il plug-in della CLI {{site.data.keyword.keymanagementserviceshort}}.

    ```sh
    ibmcloud plugin install key-protect -r 'IBM Cloud'
    ```
    {: pre}

4. Facoltativo: verifica che il plug-in sia stato installato correttamente.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## Aggiornamento del plug-in della CLI {{site.data.keyword.keymanagementserviceshort}}
{: #update-cli}

Potresti voler aggiornare la CLI periodicamente per utilizzare le nuove funzioni.

Per aggiornare la CLI:

1. Accedi a {{site.data.keyword.cloud_notm}} con la [CLI {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli/index.html#overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Nota:** se l'accesso non riesce, esegui il comando `ibmcloud login --sso` e prova di nuovo. Il parametro `--sso` è obbligatorio quando
accedi con un ID federato. Se viene utilizzata questa opzione, vai al link elencato nell'output della CLI
per generare una passcode monouso.

2. Installa l'aggiornamento dal repository dei plug-in.

    ```sh
    ibmcloud plugin update key-protect -r 'IBM Cloud'
    ```
    {: pre}

3. Facoltativo: verifica che il plug-in sia stato aggiornato correttamente.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## Disinstallazione del plug-in della CLI {{site.data.keyword.keymanagementserviceshort}}
{: #uninstall-cli}

1. Accedi a {{site.data.keyword.cloud_notm}} con la [CLI {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli/index.html#overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Nota:** se l'accesso non riesce, esegui il comando `ibmcloud login --sso` e prova di nuovo. Il parametro `--sso` è obbligatorio quando
accedi con un ID federato. Se viene utilizzata questa opzione, vai al link elencato nell'output della CLI
per generare una passcode monouso.

2. Installa l'aggiornamento dal repository dei plug-in.

    ```sh
    ibmcloud plugin uninstall key-protect
    ```
    {: pre}

3. Facoltativo: verifica che il plug-in sia stato disinstallato correttamente.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}
