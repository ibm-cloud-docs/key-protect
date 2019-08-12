---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: instance ID, instance GUID, get instance ID, get instance GUID, instance ID API, instance ID CLI

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

# Instanz-ID abrufen
{: #retrieve-instance-ID}

Sie können eine einzelne {{site.data.keyword.keymanagementservicelong}}-Serviceinstanz als Ziel für Operationen festlegen, indem Sie ihre eindeutige ID oder Instanz-ID in API-Anforderungen an den Service einschließen.
{: shortdesc}

## Instanz-ID in der {{site.data.keyword.cloud_notm}}-Konsole anzeigen
{: #view-instance-ID}

Sie können die Instanz-ID anzeigen, die Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zugeordnet ist, indem Sie zu Ihrer {{site.data.keyword.cloud_notm}}-Ressourcenliste navigieren.

1. [Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-Konsole an](https://{DomainName}){: external}.
2. Gehen Sie zu **Menü** &gt; **Ressourcenliste** und klicken Sie auf **Services**, um eine Liste Ihrer Cloud-Services zu durchsuchen.
3. Klicken Sie auf die Tabellenzeile, die Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz beschreibt.
4. Kopieren Sie in der Ansicht für Servicedetails den Wert für **GUID**.

    Dieser Wert für **GUID** stellt die Instanz-ID dar, die Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz eindeutig identifiziert.

## Instanz-ID mit der Befehlszeilenschnittstelle abrufen
{: #retrieve-instance-ID-cli}

Sie können die Instanz-ID für Ihre Serviceinstanz auch über die [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle](/docs/cli?topic=cloud-cli-getting-started){: external} abrufen.

1. Melden Sie sich bei {{site.data.keyword.cloud_notm}} über die [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle](/docs/cli?topic=cloud-cli-getting-started){: external} an.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Wenn die Anmeldung fehlschlägt, führen Sie den Befehl `ibmcloud login --sso` aus, um es erneut zu versuchen. Der Parameter `--sso` ist für die Anmeldung mit einer eingebundenen ID erforderlich. Rufen Sie bei Verwendung dieser Option den in der CLI-Ausgabe aufgeführten Link auf, um einen einmaligen Kenncode zu generieren.
    {: note}

2. Wählen Sie das Konto, die Region und die Ressourcengruppe aus, die Ihre bereitgestellte Instanz von {{site.data.keyword.keymanagementserviceshort}} enthalten.

3. Rufen Sie den Cloudressourcennamen (CRN) ab, der Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz eindeutig identifiziert. 

    ```sh
    ibmcloud resource service-instance <instance_name> --id
    ```
    {: pre}

    Ersetzen Sie `<instance_name>` durch den eindeutigen Alias, den Sie Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zugeordnet haben. Im folgenden gekürzten Beispiel sehen Sie die CLI-Ausgabe.

    ```
    crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:42454b3b-5b06-407b-a4b3-34d9ef323901:: 42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

    Der Wert _42454b3b-5b06-407b-a4b3-34d9ef323901_ ist eine Beispielinstanz-ID.


## Instanz-ID mit der API abrufen
{: #retrieve-instance-ID-api}

Das programmgesteuerte Abrufen der Instanz-ID kann hilfreich sein, um Sie bei der Erstellung und Verbindung Ihrer Anwendung zu unterstützen. Sie können die [{{site.data.keyword.cloud_notm}}-Resourcencontroller-API](https://{DomainName}/apidocs/resource-controller) aufrufen und anschließend die JSON-Ausgabe über eine Pipe an `jq` leiten, um diesen Wert zu extrahieren.

1. [Rufen Sie ein {{site.data.keyword.cloud_notm}} IAM-Zugriffstoken ab](/docs/services/key-protect?topic=key-protect-retrieve-access-token).
2. Rufen Sie die [Resourcencontroller-API](https://{DomainName}/apidocs/resource-controller) auf, um Ihre Instanz-ID abzurufen.

    ```sh
    curl -X GET \
    https://resource-controller.cloud.ibm.com/v2/resource_instances \
    -H 'Authorization: Bearer <access_token>' | jq -r '.resources[] | select(.name | contains("<instance_name>")) | .guid'
    ```
    {: codeblock}

    Ersetzen Sie `<instance_name>` durch den eindeutigen Alias, den Sie Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zugeordnet haben. Die folgende Ausgabe zeigt ein Beispiel für eine Instanz-ID:

    ```
    42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}
