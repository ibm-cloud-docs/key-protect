---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: create transport encryption key, secure import, key-wrapping key, transport key API examples

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

# Transportschlüssel erstellen
{: #create-transport-keys}

Sie können den sicheren Import von Rootschlüsselinformationen in die Cloud ermöglichen, indem Sie zuerst einen Transportverschlüsselungs-Schlüssel für Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz erstellen.
{: shortdesc}

Transportschlüssel werden zum Verschlüsseln und sicheren Importieren von Rootschlüsselinformationen in {{site.data.keyword.keymanagementserviceshort}} auf Grundlage der von Ihnen angegebenen Richtlinien verwendet. Weitere Informationen zum sicheren Importieren von Schlüsseln in die Cloud finden Sie unter [Eigene Verschlüsselungsschlüssel in der Cloud verwenden](/docs/services/key-protect/concepts?topic=key-protect-importing-keys).

Transportschlüssel sind derzeit ein Beta-Feature. Beta-Features können jederzeit geändert werden. Mit zukünftigen Aktualisierungen werden möglicherweise Änderungen eingeführt, die mit der neuesten Version nicht kompatibel sind.
{: important}

## Transportschlüssel mit der API erstellen
{: #create-transport-key-api}

Erstellen Sie einen Transportschlüssel, der Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zugeordnet ist, indem Sie einen `POST`-Aufruf an den folgenden Endpunkt absetzen.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers
```
{: codeblock}

1. [Rufen Sie Ihren Service- und Authentifizierungsnachweis ab, um mit den Schlüsseln im Service zu arbeiten.](/docs/services/key-protect?topic=key-protect-set-up-api)

2. Legen Sie eine Richtlinie für Ihren Transportschlüssel fest, indem Sie die [{{site.data.keyword.keymanagementserviceshort}}-API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{/apidocs/key-protect){: new_window} aufrufen.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/json' \
      -d '{
     "expiration": <expiration_time>,  \
     "maxAllowedRetrievals": <use_count>  \
    }'
    ```
    {: codeblock}

    Ersetzen Sie die Variablen in der Beispielanforderung anhand der Angaben in der folgenden Tabelle.

      <table>
        <tr>
          <th>Variable</th>
          <th>Beschreibung</th>
        </tr>
        <tr>
          <td><varname>region</varname></td>
          <td><strong>Erforderlich.</strong> Die Regionsabkürzung, z. B. <code>us-south</code> oder <code>eu-gb</code>, die den geografischen Bereich darstellt, in dem sich Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz befindet. Weitere Informationen finden Sie in <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Regionale Serviceendpunkte</a>.</td>
        </tr>
        <tr>
          <td><varname>IAM_token</varname></td>
          <td><strong>Erforderlich.</strong> Ihr {{site.data.keyword.cloud_notm}}-Zugriffstoken. Nehmen Sie den vollständigen Inhalt des <code>IAM</code>-Tokens einschließlich des Werts für Bearer in die cURL-Anforderung auf. Weitere Informationen finden Sie in <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Zugriffstoken abrufen</a>.</td>
        </tr>
        <tr>
          <td><varname>instance_ID</varname></td>
          <td><strong>Erforderlich.</strong> Die eindeutige ID, die Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zugewiesen ist. Weitere Informationen finden Sie in <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Instanz-ID abrufen</a>.</td>
        </tr>
        <tr>
          <td><varname>expiration_time</varname></td>
          <td>
            <p>Die Zeit in Sekunden ab der Erstellung eines Transportschlüssels, die die Gültigkeitsdauer des Schlüssels festlegt.</p>
            <p>Der Mindestwert beträgt 300 Sekunden (5 Minuten), der Maximalwert 86.400 (24 Stunden). Der Standardwert ist 600 (10 Minuten).</p>
          </td>
        </tr>
        <tr>
          <td><varname>use_count</varname></td>
          <td>Die Häufigkeit, mit der ein Transportschlüssel innerhalb seiner Ablaufzeit abgerufen werden kann, bis er nicht mehr zugänglich ist. Der Standardwert ist 1.</td>
        </tr>
          <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Variablen, die zum Hinzufügen eines Rootschlüssels mit der {{site.data.keyword.keymanagementserviceshort}}-API erforderlich sind.</caption>
      </table>

    Eine erfolgreiche Anforderung `POST api/v2/lockers` erstellt einen Transportschlüssel für Ihre Serviceinstanz und gibt den zugehörigen ID-Wert zusammen mit anderen Metadaten zurück. Die ID ist eine eindeutige Kennung, die Ihrem Transportschlüssel zugeordnet ist und für nachfolgende Aufrufe an die {{site.data.keyword.keymanagementserviceshort}}-API verwendet wird.

3. Optional: Stellen Sie sicher, dass der Transportschlüssel erstellt wurde, indem Sie den folgenden Aufruf ausführen, mit dem Metadaten zu Ihrer Serviceinstanz abgerufen werden.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## Weitere Schritte
{: #create-transport-key-next-steps}

- Weitere Informationen über die Verwendung eines Transportschlüssels zum Importieren von Rootschlüsseln in den Service finden Sie unter [Rootschlüssel importieren](/docs/services/key-protect?topic=key-protect-import-root-keys).
- Weitere Informationen zur programmgesteuerten Verwaltung von Schlüsseln [finden Sie in der {{site.data.keyword.keymanagementserviceshort}}-API-Referenzdokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/key-protect){: new_window}.
