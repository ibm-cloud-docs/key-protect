---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: IBM, activity tracker, LogDNA, event, security, KMS API calls, monitor KMS events

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

<!-- Include your AT events file in the Reference nav group in your toc file. -->

<!-- Make sure that the AT events file has the H1 ID set to: {: #at_events} -->

# Activity Tracker-Ereignisse
{: #at-events}

Als Sicherheitsbeauftragter, Auditor oder Manager können Sie den Activity Tracker-Service verwenden, um zu verfolgen, wie Benutzer und Anwendungen mit {{site.data.keyword.keymanagementservicefull}} interagieren.
{: shortdesc}

<!-- There are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones.--> 

<!-- Scenario 3. Add if your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker  -->

Activity Tracker zeichnet vom Benutzer initiierte Aktivitäten auf, die den Status eines Service in {{site.data.keyword.cloud_notm}} ändern. Sie können diesen Service verwenden, um abnormale Aktivitäten und kritische Aktionen zu untersuchen und regulatorische Prüfvorschriften zu erfüllen. Außerdem können Sie über Aktionen benachrichtigt werden, wenn sie geschehen. Die erfassten Ereignisse entsprechen dem CADF-Standard (Cloud Auditing Data Federation). 

Es gibt derzeit zwei Activity Tracker-Services, die im {{site.data.keyword.cloud_notm}}-Katalog verfügbar sind. {{site.data.keyword.keymanagementserviceshort}} sendet Ereignisse an beide Services und Sie können jeden der beiden Services verwenden, um Ihre {{site.data.keyword.keymanagementserviceshort}}-Aktivität in {{site.data.keyword.cloud_notm}} zu überwachen. {{site.data.keyword.cloudaccesstrailfull}} ist jedoch veraltet und es können keine neuen Instanzen erstellt werden, und die Unterstützung für vorhandene Serviceinstanzen ist nur bis zum 30. September 2019 verfügbar.

Weitere Informationen finden Sie unter:
* [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)
* [{{site.data.keyword.cloudaccesstrailfull}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) (veraltet)

<!-- If you have multiple events that might not be related, you can create different sections to group them. -->

## Ereignisliste
{: #at-actions}

In der folgenden Tabelle sind die {{site.data.keyword.keymanagementserviceshort}}-Aktionen aufgeführt, die ein Ereignis generieren:

| Aktion                   | Beschreibung                 |
| ------------------------ | --------------------------- |
| `kms.secrets.create`     | Schlüssel erstellen                |
| `kms.secrets.read`       | Schlüssel anhand der ID abrufen        |
| `kms.secrets.delete`     | Schlüssel anhand der ID löschen          |
| `kms.secrets.list`       | Liste mit Schlüsseln abrufen     |
| `kms.secrets.head`       | Anzahl der Schlüssel abrufen |
| `kms.secrets.wrap`       | Wrapping für einen Schlüssel durchführen                  |
| `kms.secrets.unwrap`     | Wrapping für einen Schlüssel aufheben                |
| `kms.policies.read`      | Richtlinie für einen Schlüssel anzeigen     |
| `kms.policies.write`     | Richtlinie für einen Schlüssel festlegen      |
{: caption="Tabelle 1. {{site.data.keyword.keymanagementserviceshort}}-Aktionen, die Activity Tracker-Ereignisse generieren" caption-side="top"}

## Ereignisse anzeigen
{: #at-ui}

Sie können die Activity Tracker-Ereignisse anzeigen, die Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zugeordnet sind, indem Sie [{{site.data.keyword.at_full_notm}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started) oder [{{site.data.keyword.cloudaccesstrailfull_notm}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) (veraltet) verwenden.

<!-- As in the previous section, there are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones. --> 

<!-- Scenario 3: If your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker, add the information that is relevant from scenario 1 and scenario 2. -->

<!-- Option 2: Location based service: A location-based service generates events in the same location where the service instance is provisioned. For example, Certificate Manager. -->

### {{site.data.keyword.at_full_notm}} verwenden
{: #at-ui-logdna}

Ereignisse, die von einer Instanz von {{site.data.keyword.keymanagementserviceshort}} generiert werden, werden automatisch an die {{site.data.keyword.at_full_notm}}-Serviceinstanz weitergeleitet, die am selben Standort verfügbar ist. 

Für {{site.data.keyword.at_full_notm}} kann es nur eine Instanz pro Standort geben. Um Ereignisse anzuzeigen, müssen Sie auf die Webbenutzerschnittstelle des {{site.data.keyword.at_full_notm}}-Service am selben Standort zugreifen, an dem Ihre Serviceinstanz verfügbar ist. Weitere Informationen finden Sie unter [Webbenutzerschnittstelle über die IBM Cloud-Benutzerschnittstelle starten](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2).

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

### {{site.data.keyword.cloudaccesstrailfull_notm}} verwenden (veraltet)
{: #at-ui-legacy}

{{site.data.keyword.cloudaccesstrailshort}}-Ereignisse stehen in der {{site.data.keyword.cloudaccesstrailshort}}-**Kontodomäne** zur Verfügung, die in der {{site.data.keyword.cloud_notm}}-Region verfügbar ist, in der die Ereignisse generiert werden. Weitere Informationen finden Sie unter [Ereignisse anzeigen](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-getting-started#gs_step4).


## Ereignisse analysieren
{: #at-events-analyze}

<!-- Provide information about the events in your service that add additional information in requestData and responseData. See the IAM Events topic for a sample topic that includes this section: https://cloud.ibm.com/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-at_events_iam.  -->

Da es sich bei den Informationen für einen Verschlüsselungsschlüssel um sensible Daten handelt, enthält das Ereignis, das als Ergebnis eines API-Aufrufs an den {{site.data.keyword.keymanagementserviceshort}}-Service generiert wird, keine Details zum Schlüssel. Das Ereignis enthält eine Korrelations-ID, die Sie verwenden können, um den Schlüssel intern in Ihrer Cloudumgebung zu identifizieren. Bei der Korrelations-ID handelt es sich um ein Feld, das als Teil des Feldes `responseBody.content` zurückgegeben wird. Sie können diese Informationen für die Korrelation des {{site.data.keyword.keymanagementserviceshort}}-Schlüssels mit den Informationen der durch das {{site.data.keyword.cloudaccesstrailshort}}-Ereignis gemeldeten Aktion verwenden.
