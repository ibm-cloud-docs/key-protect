---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: Activity tracker events, KMS API calls, monitor KMS events

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.cloudaccesstrailshort}}-Ereignisse
{: #activity-tracker-events}

Der {{site.data.keyword.cloudaccesstrailfull}}-Service kann dazu verwendet werden, die Interaktion von Benutzern und Anwendungen mit {{site.data.keyword.keymanagementservicefull}} zu verfolgen. 
{: shortdesc}

Der {{site.data.keyword.cloudaccesstrailfull_notm}}-Service zeichnet vom Benutzer initiierte Aktivitäten auf, die den Status eines Service in {{site.data.keyword.cloud_notm}} ändern. 

Weitere Informationen finden Sie in der [{{site.data.keyword.cloudaccesstrailshort}}-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started#getting-started){: new_window}.

## Ereignisliste
{: #list-activity-tracker-events}

In der folgenden Tabelle sind die Aktionen aufgeführt, die ein Ereignis generieren:

| Aktion               | Beschreibung                 |
| -------------------- | --------------------------- |
| `kms.secrets.create` | Schlüssel erstellen                |
| `kms.secrets.read`   | Schlüssel anhand der ID abrufen        |
| `kms.secrets.delete` | Schlüssel anhand der ID löschen          |
| `kms.secrets.list`   | Liste mit Schlüsseln abrufen     |
| `kms.secrets.head`   | Anzahl der Schlüssel abrufen |
| `kms.secrets.wrap`   | Wrapping für einen Schlüssel durchführen                  |
| `kms.secrets.unwrap` | Wrapping für einen Schlüssel aufheben                |
| `kms.policies.read`  | Richtlinie für einen Schlüssel anzeigen     |
| `kms.policies.write` | Richtlinie für einen Schlüssel festlegen      |
{: caption="Tabelle 1. Aktionen, die {{site.data.keyword.cloudaccesstrailfull_notm}}-Ereignisse generieren" caption-side="top"}

## Ereignis anzeigen
{: #view-activity-tracker-events}

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

{{site.data.keyword.cloudaccesstrailshort}}-Ereignisse stehen in der {{site.data.keyword.cloudaccesstrailshort}}-**Kontodomäne** zur Verfügung, die in der {{site.data.keyword.cloud_notm}}-Region verfügbar ist, in der die Ereignisse generiert werden.

Beispiel: Wenn Sie einen Schlüssel in {{site.data.keyword.keymanagementserviceshort}} erstellen, importieren, löschen oder lesen, wird ein {{site.data.keyword.cloudaccesstrailshort}}-Ereignis generiert. Diese Ereignisse werden automatisch an den {{site.data.keyword.cloudaccesstrailshort}}-Service in derselben Region weitergeleitet, in der auch der {{site.data.keyword.keymanagementserviceshort}}-Service bereitgestellt wird.

Zur Überwachung der API-Aktivität müssen Sie den {{site.data.keyword.cloudaccesstrailshort}}-Service in einem Bereich bereitstellen, der in derselben Region verfügbar ist, in der auch der {{site.data.keyword.keymanagementserviceshort}}-Service bereitgestellt wird. Dann können Sie Ereignisse über die Kontoansicht in der {{site.data.keyword.cloudaccesstrailshort}}-Benutzerschnittstelle anzeigen, wenn Sie einen Lite-Plan verwenden; oder Sie können sie über Kibana anzeigen, wenn Sie einen Premium-Plan verwenden.

## Zusätzliche Informationen
{: #activity-tracker-info}

Da es sich bei den Informationen für einen Verschlüsselungsschlüssel um sensible Daten handelt, enthält das Ereignis, das als Ergebnis eines API-Aufrufs an den {{site.data.keyword.keymanagementserviceshort}}-Service generiert wird, keine Details zum Schlüssel. Das Ereignis enthält eine Korrelations-ID, die Sie verwenden können, um den Schlüssel intern in Ihrer Cloudumgebung zu identifizieren. Bei der Korrelations-ID handelt es sich um ein Feld, das als Teil des Felds `responseHeader.content` zurückgegeben wird. Sie können diese Informationen für die Korrelation des {{site.data.keyword.keymanagementserviceshort}}-Schlüssels mit den Informationen der durch das {{site.data.keyword.cloudaccesstrailshort}}-Ereignis gemeldeten Aktion verwenden.
