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

# Befehlszeilenschnittstelle einrichten
{: #set-up-cli}

Mit den Plug-in der Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementservicelong_notm}} können Sie Verschlüsselungsschlüssel erstellen, importieren und verwalten. 

Weitere Informationen zur Verwendung der Befehlszeilenschnittstelle finden Sie in der [Referenzdokumentation zur Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/cli-reference.html).
{: tip}

## Plug-in der Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}} installieren
{: #install-cli}

Bevor Sie das Plug-in der Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}} einrichten können, müssen Sie die [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli/index.html#overview){: new_window} installieren.  

Gehen Sie wie folgt vor, um die Befehlszeilenschnittstellen zu installieren: 

1. Installieren Sie die [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli/index.html#overview){: new_window}. 

    Nach der Installation der Befehlszeilenschnittstelle können Sie `ibmcloud`-Befehle ausführen, um mit den Cloud-Services zu interagieren. 

2. Melden Sie sich bei {{site.data.keyword.cloud_notm}} an. 

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Hinweis:** Wenn die Anmeldung fehlschlägt, führen Sie den Befehl `ibmcloud login --sso` aus, um es erneut zu versuchen. Der Parameter `--sso` ist für die Anmeldung mit einer eingebundenen ID erforderlich. Rufen Sie bei Verwendung dieser Option den in der CLI-Ausgabe aufgeführten Link auf, um einen einmaligen Kenncode zu generieren.

3. Wenn Sie mit der Verwaltung von Verschlüsselungsschlüsseln beginnen möchten, installieren Sie das Plug-in der Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}}. 

    ```sh
    ibmcloud plugin install key-protect -r 'IBM Cloud'
    ```
    {: pre}

4. Optional: Überprüfen Sie, ob das Plug-in erfolgreich installiert wurde. 

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## Plug-in der Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}} aktualisieren
{: #update-cli}

Sie können die Befehlszeilenschnittstelle regelmäßig aktualisieren, um neue Features zu verwenden. 

Gehen Sie wie folgt vor, um die Befehlszeilenschnittstelle zu aktualisieren: 

1. Melden Sie sich bei {{site.data.keyword.cloud_notm}} über die [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli/index.html#overview){: new_window} an.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Hinweis:** Wenn die Anmeldung fehlschlägt, führen Sie den Befehl `ibmcloud login --sso` aus, um es erneut zu versuchen. Der Parameter `--sso` ist für die Anmeldung mit einer eingebundenen ID erforderlich. Rufen Sie bei Verwendung dieser Option den in der CLI-Ausgabe aufgeführten Link auf, um einen einmaligen Kenncode zu generieren.

2. Installieren Sie die Aktualisierung über das Plug-in-Repository. 

    ```sh
    ibmcloud plugin update key-protect -r 'IBM Cloud'
    ```
    {: pre}

3. Optional: Überprüfen Sie, ob das Plug-in erfolgreich aktualisiert wurde. 

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## Plug-in der Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}} deinstallieren
{: #uninstall-cli}

1. Melden Sie sich bei {{site.data.keyword.cloud_notm}} über die [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli/index.html#overview){: new_window} an.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Hinweis:** Wenn die Anmeldung fehlschlägt, führen Sie den Befehl `ibmcloud login --sso` aus, um es erneut zu versuchen. Der Parameter `--sso` ist für die Anmeldung mit einer eingebundenen ID erforderlich. Rufen Sie bei Verwendung dieser Option den in der CLI-Ausgabe aufgeführten Link auf, um einen einmaligen Kenncode zu generieren.

2. Deinstallieren Sie die Aktualisierung über das Plug-in-Repository. 

    ```sh
    ibmcloud plugin uninstall key-protect
    ```
    {: pre}

3. Optional: Überprüfen Sie, ob das Plug-in erfolgreich deinstalliert wurde. 

    ```sh
    ibmcloud plugin list
    ```
    {: pre}
