---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

subcollection: key-protect

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
{:note: .note}
{:important: .important}

# Fehlerbehebung
{: #troubleshooting}

Zu den allgemeinen Problemen bei der Verwendung von {{site.data.keyword.keymanagementservicefull}} kann beispielsweise die Angabe korrekter Header oder Berechtigungsnachweise bei der Interaktion mit der API gehören. In vielen Fällen können Sie diese Probleme beheben, indem Sie eine Reihe einfacher Schritte ausführen.
{: shortdesc}

## Die Cloud Foundry-Serviceinstanz kann nicht gelöscht werden.
{: #unable-to-delete-service}

Wenn Sie versuchen, die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zu löschen, kann der Service nicht wie erwartet gelöscht werden.

Navigieren Sie über das {{site.data.keyword.cloud_notm}}-Dashboard zu **Cloud Foundry-Services** und wählen Sie Ihre Instanz von {{site.data.keyword.keymanagementserviceshort}} aus. Klicken Sie auf das Symbol ⋮, um eine Liste der Optionen für das Serviceangebot zu öffnen, und klicken Sie dann auf **Service löschen**.
{: tsSymptoms}

Der Service kann nicht gelöscht werden, und der folgende Fehler wird angezeigt: 
```
403 Nicht zulässig: Diese Aktion kann nicht ausgeführt werden, da im Key Protect-Service geheime Schlüssel vorhanden sind. Bevor Sie den Service entfernen können, müssen Sie zuerst die geheimen Schlüssel löschen.
```
{: screen}

Seit 15. December 2017 werden in {{site.data.keyword.keymanagementserviceshort}} IAM und Ressourcengruppen anstelle von Cloud Foundry-Organisationen, -Bereichen und -Rollen verwendet. Sie können nun den {{site.data.keyword.keymanagementserviceshort}}-Service innerhalb einer Ressourcengruppe bereitstellen, ohne dass eine Cloud Foundry-Organisation und ein Cloud Foundry-Bereich angegeben werden müssen.
{: tsCauses}

Diese Änderungen haben sich auf das Zurücknehmen der Einrichtung bei älteren Instanzen des Service ausgewirkt. Wenn Sie die Instanz von {{site.data.keyword.keymanagementserviceshort}} vor dem 28. September 2017 erstellt haben, funktioniert das Löschen von Services möglicherweise nicht wie erwartet.

Zum Löschen einer älteren {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz müssen Sie zuerst die vorhandenen Schlüssel löschen, indem Sie den traditionellen Endpunkt `https://ibm-key-protect.edge.bluemix.net` verwenden, um mit dem {{site.data.keyword.keymanagementserviceshort}}-Service zu interagieren.
{: tsResolve}

Gehen Sie wie folgt vor, um die Schlüssel und die Serviceinstanz zu löschen:

1. Melden Sie sich über die {{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle bei {{site.data.keyword.cloud_notm}} an.

    ```sh
    ibmcloud login 
    ```
    {: codeblock}

    **Hinweis:** Wenn die Anmeldung fehlschlägt, führen Sie den Befehl `bx login --sso` aus, um es erneut zu versuchen. Der Parameter `--sso` ist für die Anmeldung mit einer eingebundenen ID erforderlich. Rufen Sie bei Verwendung dieser Option den in der CLI-Ausgabe aufgeführten Link auf, um einen einmaligen Kenncode zu generieren.

2. Wählen Sie die Region, die Organisation und den Bereich von {{site.data.keyword.cloud_notm}} aus, in denen die {{site.data.keyword.keymanagementserviceshort}}-Servicesinstanz enthalten ist.

    Notieren Sie den Organisations- und Bereichsnamen in der Ausgabe der Befehlszeilenschnittstelle. Sie können auch `ibmcloud cf target` ausführen, um die Cloud Foundry-Organisation und den Cloud Foundry-Bereich als Ziel anzugeben.

    ```sh
    ibmcloud cf target -o <organization_name> -s <space_name>
    ```
    {: codeblock}

3. Rufen Sie die Organisations- und Bereichs-GUID für {{site.data.keyword.cloud_notm}} ab.

    ```sh
    ibmcloud iam org <organization_name> --guid
    ibmcloud iam space <space_name> --guid
    ```
    {: codeblock}
    Ersetzen Sie `<organization_name>` und `<space_name>` mit den eindeutigen Aliasnamen, die Sie Ihrer Organisation und Ihrem Bereich zugeordnet haben.

4. Rufen Sie das Zugriffstoken ab.

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

5. Listen Sie die Schlüssel auf, die in Ihrer Serviceinstanz gespeichert sind, indem Sie den folgenden cURL-Befehl ausführen.

    ```cURL
    curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    Ersetzen Sie `<access_token>`, `<organization_GUID>` und `<space_GUID>` mit den Werten, die Sie in den Schritten 3 bis 4 abgerufen haben. 

6. Kopieren Sie den ID-Wert für jeden Schlüssel, der in Ihrer Serviceinstanz gespeichert ist.

7. Führen Sie den folgenden cURL-Befehl aus, um einen Schlüssel und seinen Inhalt permanent zu löschen.

    ```cURL
    curl -X DELETE \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys/<key_ID> \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    Ersetzen Sie `<access_token>`, `<organization_GUID>`, `<space_GUID>` und `<key_ID>` mit den Werten, die Sie in den Schritten 3 bis 5 abgerufen haben. Wiederholen Sie den Befehl für jeden Schlüssel.    

8. Stellen Sie sicher, dass die Schlüssel gelöscht wurden, indem Sie den folgenden cURL-Befehl ausführen.

    ```cURL
    curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    Ersetzen Sie `<access_token>`, `<organization_GUID>` und `<space_GUID>` mit den Werten, die Sie in den Schritten 3 bis 4 abgerufen haben. 

9. Löschen Sie die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz.

    ```sh
    ibmcloud cf delete-service "<service_instance_name>"
    ```
    {: codeblock}

10. Optional: Stellen Sie sicher, dass die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz gelöscht wurde, indem Sie zum {{site.data.keyword.cloud_notm}}-Dashboard navigieren.

    Darüber hinaus können Sie die verfügbaren Cloud Foundry-Services in dem als Ziel angegebenen Bereich auflisten, indem Sie den folgenden Befehl ausführen.

    ```sh
    ibmcloud cf service list
    ```
    {: codeblock}


## Zugriff auf die Benutzerschnittstelle nicht möglich
{: #unable-to-access-ui}

Beim Zugriff auf die {{site.data.keyword.keymanagementserviceshort}}-Benutzerschnittstelle wird diese nicht wie erwartet geladen.

Wählen Sie im {{site.data.keyword.cloud_notm}}-Dashboard Ihre Instanz des {{site.data.keyword.keymanagementserviceshort}}-Service aus.
{: tsSymptoms}

Der folgende Fehler wird angezeigt: 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

Am 15. Dezember 2017 wurden neue Features, wie z. B. die [Envelope-Verschlüsselung](/docs/services/key-protect?topic=key-protect-envelope-encryption), zum {{site.data.keyword.keymanagementserviceshort}}-Service hinzugefügt. Sie können nun den {{site.data.keyword.keymanagementserviceshort}}-Service innerhalb einer Ressourcengruppe bereitstellen, ohne dass eine Cloud Foundry-Organisation und ein Cloud Foundry-Bereich angegeben werden müssen.
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

Sie können überprüfen, ob {{site.data.keyword.cloud_notm}} verfügbar ist, indem Sie die [Statusseite![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/status?tags=platform,runtimes,services) aufrufen.

Sie können die Foren überprüfen, um festzustellen, ob bei anderen Benutzern dasselbe Problem aufgetreten ist. Wenn Sie in den Foren eine Frage stellen, versehen Sie Ihre Frage mit einem Tag, sodass sie von den {{site.data.keyword.cloud_notm}}-Entwicklungsteams gesehen wird.

- Bei technischen Fragen zu {{site.data.keyword.keymanagementserviceshort}} posten Sie Ihre Fragen in [Stack Overflow ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} und kennzeichnen Sie die Fragen mit den Tags "ibm-cloud" und "key-protect".
- Antworten auf Fragen zum Service und Anweisungen für den Einstieg erhalten Sie über das Forum [IBM developerWorks dW Answers ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window}. Geben Sie die Tags "ibm-cloud"
und "key-protect" an.

Unter [Support anfordern ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/get-support?topic=get-support-using-avatar){: new_window} erhalten Sie weitere Details zur Verwendung der Foren.

Weitere Informationen zum Öffnen eines {{site.data.keyword.IBM_notm}} Support-Tickets bzw. zu Supportebenen und zur Priorität von Tickets finden Sie in [Kontaktaufnahme mit dem Support![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/get-support?topic=get-support-getting-customer-support){: new_window}.
