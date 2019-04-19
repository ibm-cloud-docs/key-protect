---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: set up API, use Key Protect API, use KMS API, access Key Protect API, access KMS API

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

# API einrichten
{: #set-up-api}

{{site.data.keyword.keymanagementservicefull}} stellt eine REST-API bereit, die mit jeder Programmiersprache verwendet werden kann, um Verschlüsselungsschlüssel zu speichern, abzurufen und zu generieren.
{: shortdesc}

## {{site.data.keyword.cloud_notm}}-Berechtigungsnachweise abrufen
{: #retrieve-credentials}

Um mit der API zu arbeiten, müssen Sie eigene Service- und Authentifizierungsnachweise generieren. 

So erhalten Sie Ihre Berechtigungsnachweise:

1. [Generieren Sie ein {{site.data.keyword.cloud_notm}} IAM-Zugriffstoken](/docs/services/key-protect?topic=key-protect-retrieve-access-token).
2. [Rufen Sie die Instanz-ID ab, die Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz eindeutig identifiziert](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID).

## API-Anforderung formulieren
{: #form-api-request}

Wenn Sie einen API-Aufruf für den Service absetzen, strukturieren Sie Ihre API-Anforderung auf dieselbe Weise, wie Sie Ihre ursprüngliche Instanz von {{site.data.keyword.keymanagementserviceshort}} bereitgestellt haben. 

Um Ihre Anforderung zu erstellen, kombinieren Sie einen [regionalen Serviceendpunkt](/docs/services/key-protect?topic=key-protect-regions) mit dem entsprechenden Authentifizierungsnachweis. Beispiel: Wenn Sie eine Serviceinstanz für die Region `us-south` erstellen, verwenden Sie den folgenden Endpunkt und diese API-Header, um die Schlüssel in Ihrem Service zu durchsuchen:

```cURL
curl -X GET \
    https://us-south.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-instance: <instance_ID>'
```
{: codeblock} 

Ersetzen Sie `<access_token>` und `<instance_ID>` durch Ihre abgerufenen Service- und Authentifizierungsnachweise.

Möchten Sie Ihre API-Anfragen verfolgen können, wenn etwas schiefgeht? Wenn Sie das Flag `-v` als Teil der cURL-Anforderung angeben, erhalten Sie in den Antwortheadern einen Wert für `correlation-id`. Sie können diesen Wert verwenden, um die Anforderung zu Debugzwecken zu korrelieren und zu verfolgen.
{: tip} 

## Weitere Schritte
{: #set-up-api-next-steps}

Nun sind Sie dafür vorbereitet, Ihre Verschlüsselungsschlüssel in Key Protect zu verwalten. Weitere Informationen zur programmgesteuerten Verwaltung von Schlüsseln [finden Sie in der {{site.data.keyword.keymanagementserviceshort}}-API-Referenzdokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/key-protect){: new_window}.
