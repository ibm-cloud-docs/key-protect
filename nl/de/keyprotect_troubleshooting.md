---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Fehlerbehebung
{: #troubleshooting}

Zu den allgemeinen Problemen bei der Verwendung von {{site.data.keyword.keymanagementservicefull}} kann beispielsweise die Angabe korrekter Header oder Berechtigungsnachweise bei der Interaktion mit der API gehören. In vielen Fällen können Sie diese Probleme beheben, indem Sie eine Reihe einfacher Schritte ausführen.
{: shortdesc}

## Zugriff auf die Benutzerschnittstelle nicht möglich
{: #unabletoaccessUI}

Beim Zugriff auf die {{site.data.keyword.keymanagementserviceshort}}-Benutzerschnittstelle wird diese nicht wie erwartet geladen.

Wählen Sie im {{site.data.keyword.cloud_notm}}-Dashboard Ihre Instanz des {{site.data.keyword.keymanagementserviceshort}}-Service aus.
{: tsSymptoms}

Der folgende Fehler wird angezeigt: 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

Am 15. Dezember 2017 wurden neue Features, wie z. B. die [Envelope-Verschlüsselung](/docs/services/keymgmt/concepts/keyprotect_envelope.html), zum {{site.data.keyword.keymanagementserviceshort}}-Service hinzugefügt. Sie können nun den {{site.data.keyword.keymanagementserviceshort}}-Service global bereitstellen, ohne dass eine Cloud Foundry-Organisation und ein Cloud Foundry-Bereich angegeben werden müssen.
{: tsCauses}

Diese Änderungen haben sich auf die Benutzerschnittstelle für ältere Instanzen des Service ausgewirkt. Wenn Sie Ihre {{site.data.keyword.keymanagementserviceshort}}-Instanz vor dem 28. September 2017 erstellt haben, wird die Benutzerschnittstelle möglicherweise nicht wie erwartet ausgeführt.

An der Behebung dieses Problems wird gearbeitet. Als temporäre Lösung können Sie zur weiteren Verwaltung Ihrer Schlüssel die {{site.data.keyword.keymanagementserviceshort}}-API verwenden.
{: tsResolve}

Sie können den traditionellen `https://ibm-key-protect.edge.bluemix.net`-Endpunkt verwenden, um mit dem {{site.data.keyword.keymanagementserviceshort}}-Service zu interagieren.

**Beispielanforderung**

```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
```
{: codeblock}

## Schlüssel können nicht erstellt oder gelöscht werden
{: #unabletomanagekeys}

Beim Zugriff auf die {{site.data.keyword.keymanagementserviceshort}}-Benutzerschnittstelle werden die Optionen für das Hinzufügen oder Löschen von Schlüsseln nicht angezeigt.

Wählen Sie im {{site.data.keyword.cloud_notm}}-Dashboard Ihre Instanz des {{site.data.keyword.keymanagementserviceshort}}-Service aus.
{: tsSymptoms}

Eine Liste mit Schlüsseln wird angezeigt, nicht jedoch die Optionen zum Hinzufügen oder Löschen von Schlüsseln. 

Sie verfügen nicht über die korrekte Berechtigung für die Durchführung von {{site.data.keyword.keymanagementserviceshort}}-Aktionen.
{: tsCauses} 

Wenden Sie sich an den zuständigen Administrator und prüfen Sie, ob Ihnen die korrekte Rolle in der entsprechenden Ressourcengruppe oder Serviceinstanz zugewiesen ist. Weitere Informationen zu Rollen finden Sie in [Rollen und Berechtigungen](/docs/services/keymgmt/keyprotect_manage_access.html#roles).
{: tsResolve}

## Hilfe und Unterstützung anfordern
{: #getting_help}

Wenn bei der Verwendung von {{site.data.keyword.keymanagementserviceshort}} Probleme auftreten oder Fragen entstehen, können Sie den Status von {{site.data.keyword.cloud_notm}} überprüfen, nach Informationen suchen oder Fragen in einem Forum stellen. Sie haben auch die Möglichkeit, ein Support-Ticket zu öffnen.
{: shortdesc}

Sie können überprüfen, ob {{site.data.keyword.cloud_notm}} verfügbar ist, indem Sie die [Statusseite![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/status?tags=platform,runtimes,services) aufrufen.

Sie können die Foren überprüfen, um festzustellen, ob bei anderen Benutzern dasselbe Problem aufgetreten ist. Wenn Sie in den Foren eine Frage stellen, versehen Sie Ihre Frage mit einem Tag, sodass sie von den {{site.data.keyword.cloud_notm}}-Entwicklungsteams gesehen wird.

- Bei technischen Fragen zu {{site.data.keyword.keymanagementserviceshort}} posten Sie Ihre Fragen in [Stack Overflow ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} und kennzeichnen Sie die Fragen mit den Tags "ibm-cloud" und "key-protect".
- Antworten auf Fragen zum Service und Anweisungen für den Einstieg erhalten Sie über das Forum [IBM developerWorks dW Answers ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window}. Geben Sie die Tags "ibm-cloud"
und "key-protect" an.

Weitere Details zur Nutzung der Foren finden Sie unter [Hilfe anfordern ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/docs/support/index.html#getting-help){: new_window}.

Weitere Informationen zum Öffnen eines {{site.data.keyword.IBM_notm}} Support-Tickets bzw. zu Supportebenen und zur Priorität von Tickets finden Sie in [Kontaktaufnahme mit dem Support![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/docs/support/index.html#contacting-support){: new_window}.
