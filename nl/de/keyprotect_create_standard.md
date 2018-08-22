---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Standardschlüssel erstellen
{: #create_standard_keys}

Sie können einen Standardverschlüsselungsschlüssel mit der {{site.data.keyword.keymanagementserviceshort}}-GUI oder programmgesteuert mit der {{site.data.keyword.keymanagementserviceshort}}-API erstellen.
{: shortdesc}

## Standardschlüssel mit der GUI erstellen
{: #create_standard_key_GUI}

[Führen Sie nach dem Erstellen einer Instanz dieses Services](/docs/services/keymgmt/keyprotect_provision.html) die folgenden Schritte aus, um einen Standardschlüssel mit der {{site.data.keyword.keymanagementserviceshort}}-GUI zu erstellen.

1. [Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-Konsole ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/){: new_window} an.
2. Wählen Sie im {{site.data.keyword.cloud_notm}}-Dashboard die bereitgestellte Instanz von {{site.data.keyword.keymanagementserviceshort}} aus.
3. Um einen neuen Schlüssel zu erstellen, klicken Sie auf **Schlüssel hinzufügen** und wählen Sie das Fenster **Neuen Schlüssel generieren** aus.

    Geben Sie die Schlüsseldetails an:

    <table>
      <tr>
        <th>Einstellung</th>
        <th>Beschreibung</th>
      </tr>
      <tr>
        <td>Name</td>
        <td>
          <p>Ein eindeutiger, lesbarer Alias zur einfachen Identifikation Ihres Schlüssels.</p>
          <p>Stellen Sie aus Datenschutzgründen sicher, dass der Schlüsselname keine personenbezogenen Daten (PII) wie den Namen oder den Standort enthält.</p>
        </td>
      </tr>
      <tr></tr>
        <td>Schlüsseltyp</td>
        <td>Der <a href="/docs/services/keymgmt/concepts/keyprotect_envelope.html#key_types">Schlüsseltyp</a>, den Sie in {{site.data.keyword.keymanagementserviceshort}} verwalten möchten. Wählen Sie aus der Liste der Schlüsseltypen <b>Standardschlüssel</b> aus.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Einstellungen für <b>Neuen Schlüssel generieren</b></caption>
    </table>

4. Geben Sie die Details zum Schlüssel ein und klicken Sie dann zum Bestätigen auf **Schlüssel generieren**. 

## Standardschlüssel mit der API erstellen
{: #create_standard_key_API}

Erstellen Sie einen Standardschlüssel, indem Sie einen `POST`-Aufruf zum folgenden Endpunkt absetzen.

```
https://keyprotect.<region>.bluemix.net/api/v2/keys
```
{: codeblock}

1. [Rufen Sie Ihren Service- und Authentifizierungsnachweis ab, um mit den Schlüsseln im Service zu arbeiten.](/docs/services/keymgmt/keyprotect_authentication.html)

2. Rufen Sie die [{{site.data.keyword.keymanagementserviceshort}}-API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/apidocs/639){: new_window} mit dem folgenden cURL-Befehl auf.

    ```cURL
    curl -X POST \
      https://keyprotect.<region>.bluemix.net/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'prefer: <return_preference>' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.key+json",
       "collectionTotal": 1
     },
    "resources": [
      {
       "type": "application/vnd.ibm.kms.key+json",
       "name": "<key_alias>",
       "description": "<key_description>",
       "expirationDate": "<YYYY-MM-DDTHH:MM:SS.SSZ>",
       "extractable": <key_type>
       }
     ]
    }'
    ```
    {: codeblock}

    Um mit Schlüsseln in bestimmten Cloud Foundry-Organisationen und -Bereichen zu arbeiten, ersetzen Sie `Bluemix-Instance` durch die entsprechenden Header `Bluemix-org` und `Bluemix-space`. [Siehe auch die Codebeispiele in der {{site.data.keyword.keymanagementserviceshort}}-API-Referenzdokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/apidocs/639){: new_window}.
    {: tip}

    Ersetzen Sie die Variablen in der Beispielanforderung mithilfe der Angaben in der folgenden Tabelle.
    <table>
      <tr>
        <th>Variable</th>
        <th>Beschreibung</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td>Die Regionsabkürzung, z. B. <code>us-south</code> oder <code>eu-gb</code>, die den geografischen Bereich darstellt, in dem sich Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz befindet. Weitere Informationen finden Sie in <a href="/docs/services/keymgmt/keyprotect_regions.html#endpoints">Regionale Serviceendpunkte</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td>Ihr {{site.data.keyword.cloud_notm}}-Zugriffstoken. Nehmen Sie den vollständigen Inhalt des <code>IAM</code>-Tokens einschließlich des Werts für Bearer in die cURL-Anforderung auf. Weitere Informationen finden Sie in <a href="/docs/services/keymgmt/keyprotect_authentication.html#retrieve_token">Zugriffstoken abrufen</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td>Die eindeutige ID, die Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zugewiesen ist. Weitere Informationen finden Sie in <a href="/docs/services/keymgmt/keyprotect_authentication.html#retrieve_instance_ID">Instanz-ID abrufen</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>Die eindeutige ID, die zum Überwachen und Korrelieren von Transaktionen verwendet wird.</td>
      </tr>
      <tr>
        <td><varname>return_preference</varname></td>
        <td><p>Optional: Ein Header, der das Serververhalten für <code>POST</code>- und <code>DELETE</code>-Operationen ändert.</p><p>Wenn Sie die Variable <em>return_preference</em> auf <code>return=minimal</code> setzen, werden im Entitätshauptteil der Antwort nur die Metadaten des Schlüssels zurückgegeben, wie zum Beispiel Schlüsselname und ID-Wert. Wenn Sie für die Variable <code>return=representation</code> festlegen, werden sowohl die Schlüsselinformationen als auch die Metadaten des Schlüssels zurückgegeben.</p></td>
      </tr>
      <tr>
        <td><varname>key_alias</varname></td>
        <td>
          <p>Ein eindeutiger, lesbarer Name zur einfachen Identifikation Ihres Schlüssels.</p>
          <p>Wichtig: Aus Datenschutzgründen dürfen keine personenbezogenen Daten als Metadaten für den Schlüssel gespeichert werden.</p>
        </td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>
          <p>Optional: Eine erweiterte Beschreibung des Schlüssels.</p>
          <p>Wichtig: Aus Datenschutzgründen dürfen keine personenbezogenen Daten als Metadaten für den Schlüssel gespeichert werden.</p>
        </td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>Optional: Der Zeitpunkt (Datum und Uhrzeit), zu dem der Schlüssel im System abläuft, im RFC-3339-Format. Wenn das Attribut <code>expirationDate</code> fehlt, bleibt der Schlüssel unbegrenzt gültig. </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>Ein boolescher Wert, der bestimmt, ob die Schlüsselinformationen den Service verlassen dürfen.</p>
          <p>Wenn das Attribut <code>extractable</code> auf <code>true</code> festgelegt wird, erstellt der Service einen Standardschlüssel, den Sie in Ihren Apps oder Services speichern können.</p>
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabelle 2. Beschreibung der Variablen, die zum Hinzufügen eines Standardschlüssels mit der {{site.data.keyword.keymanagementserviceshort}}-API erforderlich sind.</caption>
    </table>

    Vermeiden Sie zum Schutz der Vertraulichkeit Ihrer persönlichen Daten die Eingabe von personenbezogenen Informationen (PII) beim Hinzufügen von Schlüsseln zum Service. Hierzu gehören beispielsweise Namen oder Standortangaben. Weitere Beispiele für personenbezogene Daten finden Sie in Abschnitt 2.2 des Dokuments [NIST Special Publication 800-122 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-122.pdf){: new_window}.
    {: tip}

    Mit der erfolgreichen Antwort `POST /v2/keys` werden der ID-Wert für Ihren Schlüssel sowie andere Metadaten zurückgegeben. Die ID ist eine eindeutige Kennung, die Ihrem Schlüssel zugeordnet ist und die für alle nachfolgenden Aufrufe für die {{site.data.keyword.keymanagementserviceshort}}-API verwendet wird.

3. Optional: Stellen Sie sicher, dass der Schlüssel durch Ausführen des folgenden Aufrufs zum Abrufen der Schlüssel in Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz erstellt wurde.

    ```cURL
    curl -X GET \
      https://keyprotect.us-south.bluemix.net/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
    ```
    {: codeblock}


### Weitere Schritte

- Weitere Informationen zum programmgesteuerten Management Ihrer Schlüssel [finden Sie in der API-Referenzdokumentation von {{site.data.keyword.keymanagementserviceshort}} für Codebeispiele ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/apidocs/639){: new_window}.
- Zur Anzeige eines Beispiels zur Art und Weise, in der Schlüsselspeicher in {{site.data.keyword.keymanagementserviceshort}} eingesetzt werden können, um Daten zu ver- und entschlüsseln, [überprüfen Sie die Beispielapp in GitHub ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/IBM-Bluemix/key-protect-helloworld-python){: new_window}.
