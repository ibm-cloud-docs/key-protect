---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: encryption key states, encryption key lifecycle, manage key lifecycle

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

# Lebenszyklus von Verschlüsselungsschlüsseln überwachen
{: #key-states}

{{site.data.keyword.keymanagementservicefull}} orientiert sich an den Sicherheitsrichtlinien von [NIST SP 800-57 für die Schlüsselstatus](https://www.nist.gov/publications/recommendation-key-management-part-1-general-0){: external}.
{: shortdesc}

## Schlüsselstatus und -übergänge
{: #key-transitions}

Verschlüsselungsschlüssel durchlaufen in ihrem Lebenszyklus mehrere Status, die davon abhängig sind, wie lange die Schlüssel bestehen und ob Daten geschützt werden. 

{{site.data.keyword.keymanagementserviceshort}} stellt eine grafische Benutzerschnittstelle und eine REST-API für die Verfolgung von Schlüsseln während der verschiedenen Phasen ihres Lebenszyklus bereit. Das folgende Diagramm veranschaulicht die Status, die ein Schlüssel vom Generieren bis zum Löschen durchläuft.

![Das Diagramm zeigt dieselben Komponenten, die auch in der folgenden Definitionstabelle beschrieben sind.](../images/key-states_min.svg)

| Status | Beschreibung |
| --- | --- |
| Vor Aktivierung | Die Schlüssel werden zu Beginn im Status _Vor Aktivierung_ erstellt. Ein Schlüssel in diesem Status kann nicht für den Verschlüsselungsschutz von Daten verwendet werden.|
| Aktiv | Die Schlüssel werden sofort in den Status _Aktiv_ versetzt, wenn das Aktivierungsdatum erreicht wird. Dieser Übergang kennzeichnet den Beginn der Verschlüsselungsperiode eines Schlüssels. Schlüssel ohne Aktivierungsdatum werden sofort aktiviert und bleiben so lange aktiv, bis sie ablaufen oder gelöscht werden. |
| Inaktiviert | Ein Schlüssel wird in den Status _Inaktiviert_ versetzt, wenn ein Ablaufdatum zugewiesen wurde und dieses Ablaufdatum erreicht wird. In diesem Status kann der Schlüssel nicht mehr zum Schutz von Daten durch Verschlüsselung verwendet werden und kann nur noch in den Status _Gelöscht_ versetzt werden.|
| Gelöscht | Gelöschte Schlüssel befinden sich im Status _Gelöscht_. Schlüssel, die sich in diesem Status befinden, sind nicht wiederherstellbar. Die zugehörigen Metadaten für den Schlüssel werden in der {{site.data.keyword.keymanagementserviceshort}}-Datenbank aufbewahrt. |
{: caption="Tabelle 1. Beschreibt die Schlüsselzustände und -übergänge." caption-side="top"}

Nachdem Sie dem Service einen Schlüssel hinzugefügt haben, verwenden Sie das {{site.data.keyword.keymanagementserviceshort}}-Dashboard oder die {{site.data.keyword.keymanagementserviceshort}}-REST-APIs, um den Verlauf und die Konfiguration des Schlüsselstatus anzuzeigen. Sie können zu Prüfzwecken die Aktivitätenprüfliste für einen Schlüssel überwachen, indem Sie {{site.data.keyword.keymanagementserviceshort}} in {{site.data.keyword.cloudaccesstrailfull}} integrieren. Wenn beide Services bereitgestellt sind und ausgeführt werden, werden Aktivitätsereignisse generiert und in einem {{site.data.keyword.cloudaccesstrailshort}}-Protokoll automatisch erfasst, wenn Sie in {{site.data.keyword.keymanagementserviceshort}} Schlüssel erstellen und löschen. 

Weitere Informationen finden Sie unter [{{site.data.keyword.keymanagementserviceshort}}-Aktivität überwachen](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-kp){: external}.
