---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

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
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}

# Fehlerbehebung
{: #troubleshooting}

Zu den allgemeinen Problemen bei der Verwendung von {{site.data.keyword.keymanagementservicefull}} kann beispielsweise die Angabe korrekter Header oder Berechtigungsnachweise bei der Interaktion mit der API gehören. In vielen Fällen können Sie diese Probleme beheben, indem Sie eine Reihe einfacher Schritte ausführen.
{: shortdesc}

## Schlüssel können nicht erstellt oder gelöscht werden
{: #unable-to-create-keys}

Beim Zugriff auf die {{site.data.keyword.keymanagementserviceshort}}-Benutzerschnittstelle werden die Optionen für das Hinzufügen oder Löschen von Schlüsseln nicht angezeigt.

Wählen Sie im {{site.data.keyword.cloud_notm}}-Dashboard Ihre Instanz des {{site.data.keyword.keymanagementserviceshort}}-Service aus.
{: tsSymptoms}

Eine Liste mit Schlüsseln wird angezeigt, nicht jedoch die Optionen zum Hinzufügen oder Löschen von Schlüsseln. 

Sie verfügen nicht über die korrekte Berechtigung für die Durchführung von {{site.data.keyword.keymanagementserviceshort}}-Aktionen.
{: tsCauses} 

Wenden Sie sich an den zuständigen Administrator und prüfen Sie, ob Ihnen die korrekte Rolle in der entsprechenden Ressourcengruppe oder Serviceinstanz zugewiesen ist. Weitere Informationen zu Rollen finden Sie in [Rollen und Berechtigungen](/docs/services/key-protect?topic=key-protect-manage-access#roles).
{: tsResolve}

## Hilfe und Unterstützung anfordern
{: #getting-help}

Wenn bei der Verwendung von {{site.data.keyword.keymanagementserviceshort}} Probleme auftreten oder Fragen entstehen, können Sie den Status von {{site.data.keyword.cloud_notm}} überprüfen, nach Informationen suchen oder Fragen in einem Forum stellen. Sie haben auch die Möglichkeit, ein Support-Ticket zu öffnen.
{: shortdesc}

Sie können überprüfen, ob {{site.data.keyword.cloud_notm}} verfügbar ist, indem Sie die [Statusseite](https://{DomainName}/status?tags=platform,runtimes,services){: external} aufrufen.

Sie können die Foren überprüfen, um festzustellen, ob bei anderen Benutzern dasselbe Problem aufgetreten ist. Wenn Sie in den Foren eine Frage stellen, versehen Sie Ihre Frage mit einem Tag, sodass sie von den {{site.data.keyword.cloud_notm}}-Entwicklungsteams gesehen wird.

- Bei technischen Fragen zu {{site.data.keyword.keymanagementserviceshort}} posten Sie Ihre Frage in [Stack Overflow](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: external} und kennzeichnen Sie sie mit den Tags "ibm-cloud" und "key-protect".
- Verwenden Sie bei Fragen zum Service und zu den Anweisungen für die ersten Schritte das Forum [IBM developerWorks dW Answers](https://developer.ibm.com/answers/topics/key-protect/){: external}. Geben Sie die Tags "ibm-cloud"
und "key-protect" an.

Unter [Support anfordern](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: external} finden Sie weitere Details zur Verwendung der Foren.

Weitere Informationen zum Öffnen eines {{site.data.keyword.IBM_notm}} Support-Tickets oder zu Support-Leveln und zur Priorität von Tickets finden Sie unter [Kontaktaufnahme mit dem Support](/docs/get-support?topic=get-support-getting-customer-support){: external}.
