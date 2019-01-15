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

# Schlüssel anzeigen
{: #view-keys}

{{site.data.keyword.keymanagementservicefull}} stellt ein zentrales System zum Anzeigen, Verwalten und Prüfen Ihrer Verschlüsselungsschlüssel bereit. Prüfen Sie Ihre Schlüssel und Zugriffsbeschränkungen für Schlüssel, um die Sicherheit Ihrer Ressourcen sicherzustellen.
{: shortdesc}

Prüfen Sie Ihre Schlüsselkonfiguration regelmäßig:

- Untersuchen Sie, wann die Schlüssel erstellt wurden und stellen Sie fest, ob es an der Zeit ist, den Schlüssel zu wechseln.
- [Überwachen Sie API-Aufrufe für {{site.data.keyword.keymanagementserviceshort}} mit {{site.data.keyword.cloudaccesstrailshort}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/cloud-activity-tracker/tutorials/manage_events_cli.html){: new_window}.
- Untersuchen Sie, welche Benutzer auf Schlüssel zugreifen können, und ob entsprechende Zugriffsberechtigungen
erteilt wurden.

Weitere Informationen zum Prüfen des Zugriffs auf Ihre Ressourcen finden Sie in [Benutzerzugriff mit Cloud IAM verwalten](/docs/services/key-protect/manage-access.html).

## Schlüssel mit GUI anzeigen
{: #gui}

Wenn Sie die Überprüfung von Verschlüsselungsschlüssel über eine grafische Oberfläche bevorzugen, dann können Sie hierzu das {{site.data.keyword.keymanagementserviceshort}}-Dashboard verwenden.

[Nach dem Erstellen oder Importieren der vorhandenen Schlüssel in den Service](/docs/services/key-protect/create-root-keys.html) müssen Sie die folgenden Schritte ausführen, um Ihre Schlüssel anzuzeigen.

1. [Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-Konsole ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/) an.
2. Rufen Sie **Menü** &gt; **Ressourcenliste** auf, um eine Liste Ihrer Ressourcen anzuzeigen. 
3. Wählen Sie in der {{site.data.keyword.cloud_notm}}-Ressourcenliste die bereitgestellte Instanz von {{site.data.keyword.keymanagementserviceshort}} aus. 
4. Durchsuchen Sie die allgemeinen Merkmale Ihrer Schlüssel auf der Seite mit den Anwendungsdetails: 

    <table>
      <tr>
        <th>Spalte</th>
        <th>Beschreibung</th>
      </tr>
      <tr>
        <td>Name</td>
        <td>Der eindeutige, lesbare Alias, der Ihrem Schlüssel zugewiesen wurde.</td>
      </tr>
      <tr>
        <td>ID</td>
        <td>Eine eindeutige Schlüssel-ID, die Ihrem Schlüssel vom {{site.data.keyword.keymanagementserviceshort}}-Service zugewiesen wurde. Mit dem ID-Wert können Sie den Service mit der [{{site.data.keyword.keymanagementserviceshort}}-API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/key-protect) aufrufen.</td>
      </tr>
      <tr>
        <td>Status</td>
        <td>Der [Schlüsselstatus](/docs/services/key-protect/concepts/key-states.html) basiert auf der Dokumentation ['NIST Special Publication 800-57, Recommendation for Key Management' ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf). Diese Status sind <i>Vor Aktivierung</i>, <i>Aktiv</i>, <i>Inaktiviert</i> und <i>Gelöscht</i>. </td>
      </tr>
      <tr>
        <td>Typ</td>
        <td>Der [Schlüsseltyp](/docs/services/key-protect/concepts/envelope-encryption.html#key-types), der den Zweck des Schlüssels im Service beschreibt.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Tabelle <b>Schlüssel</b></caption>
    </table>

## Schlüssel mit API anzeigen
{: #api}

Sie können den Inhalt Ihrer Schlüssel abrufen, indem Sie die {{site.data.keyword.keymanagementserviceshort}}-API verwenden.

### Liste Ihrer Schlüssel abrufen
{: #retrieve-keys-api}

Für eine übergeordnete Ansicht können Sie die Schlüssel durchsuchen, die in Ihrer bereitgestellten Instanz von {{site.data.keyword.keymanagementserviceshort}} verwaltet werden, indem Sie einen `GET`-Aufruf zum folgenden Endpunkt absetzen.

```
https://keyprotect.<region>.bluemix.net/api/v2/keys
```
{: codeblock}

1. [Rufen Sie Ihren Service- und Authentifizierungsnachweis ab, um mit den Schlüsseln im Service zu arbeiten.](/docs/services/key-protect/access-api.html)

2. Führen Sie den folgenden cURL-Befehl aus, um allgemeine Merkmale Ihres Schlüssels anzuzeigen.

    ```cURL
    curl -X GET \
    https://keyprotect.<region>.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <IAM_token>' \
    -H 'bluemix-instance: <instance_ID>' \
    -H 'correlation-id: <correlation_ID>' \
    ```
    {: codeblock}

    Um mit Schlüsseln in Cloud Foundry-Organisationen und -Bereichen zu arbeiten, ersetzen Sie `Bluemix-Instance` durch die entsprechenden Header `Bluemix-org` und `Bluemix-space`. [Weitere Informationen finden Sie in der {{site.data.keyword.keymanagementserviceshort}}-API-Referenzdokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/key-protect){: new_window}.
    {: tip}

    Ersetzen Sie die Variablen in der Beispielanforderung anhand der Angaben in der folgenden Tabelle.
    <table>
      <tr>
        <th>Variable</th>
        <th>Beschreibung</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Erforderlich.</strong> Die Regionsabkürzung, z. B. <code>us-south</code> oder <code>eu-gb</code>, die den geografischen Bereich darstellt, in dem sich Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz befindet. Weitere Informationen finden Sie in <a href="/docs/services/key-protect/regions.html#endpoints">Regionale Serviceendpunkte</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Erforderlich.</strong> Ihr {{site.data.keyword.cloud_notm}}-Zugriffstoken. Nehmen Sie den vollständigen Inhalt des <code>IAM</code>-Tokens einschließlich des Werts für Bearer in die cURL-Anforderung auf. Weitere Informationen finden Sie in <a href="/docs/services/key-protect/access-api.html#retrieve-token">Zugriffstoken abrufen</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Erforderlich.</strong> Die eindeutige ID, die Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zugewiesen ist. Weitere Informationen finden Sie in <a href="/docs/services/key-protect/access-api.html#retrieve-instance-ID">Instanz-ID abrufen</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>Die eindeutige ID, die zum Überwachen und Korrelieren von Transaktionen verwendet wird.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 2. Beschreibung der Variablen, die zum Anzeigen von Schlüsseln mit der {{site.data.keyword.keymanagementserviceshort}}-API erforderlich sind.</caption>
    </table>

    Eine erfolgreiche Anforderung `GET api/v2/keys` gibt eine Gruppe von Schlüsseln zurück, die in Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz verfügbar sind. 

    ```
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.collection+json",
        "collectionTotal": 2
      },
    "resources": [
      {
          "id": "...",
          "type": "application/vnd.ibm.kms.key+json",
          "name": "Standard key",
          "description": "...",
          "state": 1,
          "crn": "...",
          "algorithmType": "AES",
          "createdBy": "...",
          "creationDate": "YYYY-MM-DDTHH:MM:SSZ",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "GCM"
          },
          "extractable": true,
          "imported": false
        },
        {
          "id": "...",
          "type": "application/vnd.ibm.kms.key+json",
          "name": "Root key",
          "description": "...",
          "state": 1,
          "crn": "...",
          "algorithmType": "AES",
          "createdBy": "...",
          "creationDate": "YYYY-MM-DDTHH:MM:SSZ",
          "lastUpdateDate": "YYYY-MM-DDTHH:MM:SSZ",
          "lastRotateDate": "YYYY-MM-DDTHH:MM:SSZ",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "GCM"
          },
          "extractable": false,
          "imported": true
        }
      ]
    }
    ```
    {:screen}

    Standardmäßig werden mit `GET api/v2/keys` die ersten 2000 Schlüssel zurückgegeben. Sie können diesen Grenzwert jedoch anpassen, indem Sie den Parameter `limit` bei der Abfrage verwenden. Weitere Informationen zu `limit` und `offset` finden Sie in [Untergruppe von Schlüsseln abrufen](#retrieve_subset_keys_api).
    {: tip}

### Untergruppe von Schlüsseln abrufen
{: #retrieve-subset-keys-api}

Durch die Angabe der Parameter `limit` und `offset` bei der Abfrage können Sie eine Untergruppe Ihrer Schlüssel abrufen, die mit dem Wert beginnt, den Sie für `offset` angeben. 

Beispiel: Sie verfügen über insgesamt 3000 Schlüssel, die in der {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz gespeichert sind, möchten aber mit der Abfrage `GET /keys` nur die Schlüssel 200 bis 300 abrufen. 

Sie können die folgende Beispielanforderung verwenden, um eine andere Gruppe von Schlüsseln abzurufen.

  ```cURL
  curl -X GET \
  https://keyprotect.<region>.bluemix.net/api/v2/keys?offset=<offset>&limit=<limit> \
  -H 'accept: application/vnd.ibm.collection+json' \
  -H 'authorization: Bearer <IAM_token>' \
  -H 'bluemix-instance: <instance_ID>' \
  ```
  {: codeblock}

  Ersetzen Sie die Variablen `limit` und `offset` in der Anforderung, wie in der folgenden Tabelle beschrieben.
  <table>
    <tr>
      <th>Variable</th>
      <th>Beschreibung</th>
    </tr>
    <tr>
      <td><p><varname>offset</varname></p></td>
      <td>
        <p>Die Anzahl der zu überspringenden Schlüssel.</p> 
        <p>Beispiel: Wenn Sie über 50 Schlüssel in Ihrer Instanz verfügen und die Schlüssel 26 - 50 auflisten möchten, verwenden Sie
            <code>../keys?offset=25</code>. Sie können auch <code>offset</code> mit <code>limit</code> kombinieren, um Ihre verfügbaren Ressourcen durchzugehen.</p>
      </td>
    </tr>
    <tr>
      <td><p><varname>limit</varname></p></td>
      <td>
        <p>Die Anzahl der abzurufenden Schlüssel.</p> 
        <p>Beispiel: Wenn Sie über 100 Schlüssel in Ihrer Instanz verfügen und nur 10 Schlüssel auflisten möchten, verwenden Sie
            <code>../keys?limit=10</code>. Der Maximalwert für <code>limit</code> ist 5000.</p>
      </td>
    </tr>
    <caption style="caption-side:bottom;">Tabelle 2. Beschreibung der Variablen <code>limit</code> und <code>offset</code>.</caption>
  </table>

Hinweise zur Verwendung liefern Ihnen die folgenden Beispiele für das Festlegen der Abfrageparameter `limit` und `offset`.

<table>
  <tr>
    <th>URL</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td><code>.../keys</code></td>
    <td>Listet alle verfügbaren Ressourcen bis einschließlich der ersten 2000 Schlüssel auf.</td>
  </tr>
  <tr>
    <td><code>.../keys?limit=10</code></td>
    <td>Listet die ersten 10 Schlüssel auf.</td>
  </tr>
  <tr>
    <td><code>.../keys?offset=25&limit=50</code></td>
    <td>Listet die Schlüssel 26 - 50 auf.</td>
  </tr>
  <tr>
    <td><code>.../keys?offset=3000&limit=50</code></td>
    <td>Listet die Schlüssel 3001 - 3050 auf.</td>
  </tr>
  <caption style="caption-side:bottom;">Tabelle 3. Hinweise zur Verwendung für die Abfrageparameter 'limit' und 'offset'</caption>
</table>

Mit dem Wert für 'offset' wird die relative Position eines bestimmten Schlüssels im Dataset angegeben. Der Wert für `offset` wird mit der Basis null angegeben, d. h., dass sich der zehnte Verschlüsselungsschlüssel in einem Dataset an der Position 9 befindet.
{: tip}

### Schlüssel nach ID abrufen
{: #retrieve-key-api}

Um detaillierte Informationen zu einem bestimmten Schlüssel anzuzeigen, können Sie einen `GET`-Aufruf für den folgenden Endpunkt durchführen.

```
https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>
```
{: codeblock}

1. [Rufen Sie Ihren Service- und Authentifizierungsnachweis ab, um mit den Schlüsseln im Service zu arbeiten.](/docs/services/key-protect/access-api.html)

2. Rufen Sie die ID des Schlüssels ab, auf den Sie zugreifen oder den Sie verwalten möchten. 

    Der ID-Wert wird für den Zugriff auf ausführliche Informationen zu dem Schlüssel (z. B. auf die Schlüsselinformationen selbst) verwendet. Sie können die ID für einen angegebenen Schlüssel abrufen, indem Sie die Anforderung `GET /v2/keys` absetzen oder indem Sie auf die {{site.data.keyword.keymanagementserviceshort}}-GUI zugreifen.

3. Führen Sie den folgenden cURL-Befehl aus, um Details zu Ihrem Schlüssel und die Schlüsselinformationen abzurufen.

    ```cURL
    curl -X GET \
      https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID> \
      -H 'accept: application/vnd.ibm.kms.key+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
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
        <td><strong>Erforderlich.</strong> Die Regionsabkürzung, z. B. <code>us-south</code> oder <code>eu-gb</code>, die den geografischen Bereich darstellt, in dem sich Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz befindet. <a href="/docs/services/key-protect/regions.html#endpoints">Regionale Serviceendpunkte</a> enthält weitere Informationen hierzu.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Erforderlich.</strong> Ihr {{site.data.keyword.cloud_notm}}-Zugriffstoken. Nehmen Sie den vollständigen Inhalt des <code>IAM</code>-Tokens einschließlich des Werts für Bearer in die cURL-Anforderung auf. Weitere Informationen finden Sie in <a href="/docs/services/key-protect/access-api.html#retrieve-token">Zugriffstoken abrufen</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Erforderlich.</strong> Die eindeutige ID, die Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zugewiesen ist. Weitere Informationen finden Sie in <a href="/docs/services/key-protect/access-api.html#retrieve-instance-ID">Instanz-ID abrufen</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>Die eindeutige ID, die zum Überwachen und Korrelieren von Transaktionen verwendet wird.</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Erforderlich.</strong> Die ID für den Schlüssel, der in [Schritt 1](#retrieve-keys-api) abgerufen wurde.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 4. Beschreibung der Variablen, die zum Anzeigen eines angegebenen Schlüssels mit der {{site.data.keyword.keymanagementserviceshort}}-API erforderlich sind.</caption>
    </table>

    Die erfolgreiche Antwort `GET api/v2/keys/<key_ID>` gibt Details zu Ihrem Schlüssel und zu den Schlüsselinformationen zurück. Das folgende JSON-Objekt zeigt ein Beispiel für einen zurückgegebenen Wert für einen Standardschlüssel.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.key+json"
        },
    "resources": [
      {
            "id": "...",
            "type": "application/vnd.ibm.kms.key+json",
            "name": "Standard key",
            "description": "...",
            "state": 1,
            "crn": "...",
            "algorithmType": "AES",
            "payload": "...",
            "createdBy": "...",
            "creationDate": "YYYY-MM-DDTHH:MM:SSZ",
            "algorithmMetadata": {
                "bitLength": "256",
                "mode": "GCM"
            },
            "extractable": true,
            "imported": false
        }
      ]
    }
    ```
    {:screen}

    Eine detaillierte Beschreibung der verfügbaren Parameter finden Sie in der [REST-API-Referenzdokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/key-protect){: new_window} für {{site.data.keyword.keymanagementserviceshort}}. 
