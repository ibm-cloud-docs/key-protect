---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: create root key, create key-wrapping key, create CRK, create CMK, create customer key, create root key in Key Protect, create key-wrapping key in Key Protect, create customer key in Key Protect, key-wrapping key, root key API examples

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

# Rootschlüssel erstellen
{: #create-root-keys}

Sie können mit {{site.data.keyword.keymanagementservicefull}} Rootschlüssel mithilfe der {{site.data.keyword.keymanagementserviceshort}}-GUI oder programmgesteuert mit der {{site.data.keyword.keymanagementserviceshort}}-API erstellen.
{: shortdesc}

Rootschlüssel sind symmetrische Key-Wrapping-Schlüssel, die die Sicherheit verschlüsselter Daten in der Cloud gewährleisten. Weitere Informationen zu Rootschlüsseln finden Sie in [Daten mit Envelope-Verschlüsselung schützen](/docs/services/key-protect?topic=key-protect-envelope-encryption). 

## Rootschlüssel mit der GUI erstellen
{: #create-root-key-gui}

[Führen Sie nach dem Erstellen einer Instanz dieses Services](/docs/services/key-protect?topic=key-protect-provision) die folgenden Schritte aus, um einen Rootschlüssel mit der {{site.data.keyword.keymanagementserviceshort}}-GUI zu erstellen.

1. [Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-Konsole ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}){: new_window} an.
2. Rufen Sie **Menü** &gt; **Ressourcenliste** auf, um eine Liste Ihrer Ressourcen anzuzeigen.
3. Wählen Sie in der {{site.data.keyword.cloud_notm}}-Ressourcenliste die bereitgestellte Instanz von {{site.data.keyword.keymanagementserviceshort}} aus.
4. Wenn Sie einen neuen Schlüssel erstellen möchten, klicken Sie auf **Schlüssel hinzufügen** und wählen Sie das Fenster **Schlüssel erstellen** aus.

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
      <tr>
        <td>Schlüsseltyp</td>
        <td>Der <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Schlüsseltyp</a>, den Sie in {{site.data.keyword.keymanagementserviceshort}} verwalten möchten. Wählen Sie aus der Liste der Schlüsseltypen <b>Rootschlüssel</b> aus.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Einstellungen für <b>Schlüssel generieren</b></caption>
    </table>

5. Geben Sie die Details zum Schlüssel ein und klicken Sie anschließend zum Bestätigen auf **Schlüssel erstellen**. 

Die im Service generierten Schlüssel sind symmetrische 256-Bit-Schlüssel, die vom AES-GCM-Algorithmus unterstützt werden. Um eine höhere Sicherheit zu erhalten, werden die Schlüssel von FIPS 140-2 Level 2-zertifizierten Hardwaresicherheitsmodulen (HSMs) generiert, die sich in sicheren {{site.data.keyword.cloud_notm}}-Datenzentren befinden. 

## Rootschlüssel mit der API erstellen
{: #create-root-key-api}

Erstellen Sie einen Rootschlüssel, indem Sie einen `POST`-Aufruf an den folgenden Endpunkt absetzen.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Rufen Sie Ihren Service- und Authentifizierungsnachweis ab, um mit den Schlüsseln im Service zu arbeiten.](/docs/services/key-protect?topic=key-protect-set-up-api)

2. Rufen Sie die [{{site.data.keyword.keymanagementserviceshort}}-API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/key-protect){: new_window} mit dem folgenden cURL-Befehl auf.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -H 'correlation-id: <correlation_ID>' \
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
        <td><varname>correlation_ID</varname></td>
        <td>Die eindeutige ID, die zum Überwachen und Korrelieren von Transaktionen verwendet wird.</td>
      </tr>
      <tr>
        <td><varname>key_alias</varname></td>
        <td>
          <p><strong>Erforderlich.</strong> Ein eindeutiger, lesbarer Name zur einfachen Identifikation Ihres Schlüssels.</p>
          <p>Wichtig: Aus Datenschutzgründen dürfen keine personenbezogenen Daten als Metadaten für den Schlüssel gespeichert werden.</p>
        </td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>
          <p>Eine erweiterte Beschreibung des Schlüssels.</p>
          <p>Wichtig: Aus Datenschutzgründen dürfen keine personenbezogenen Daten als Metadaten für den Schlüssel gespeichert werden.</p>
        </td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>Der Zeitpunkt (Datum und Uhrzeit), zu dem der Schlüssel im System abläuft, im RFC-3339-Format. Wenn das Attribut <code>expirationDate</code> fehlt, bleibt der Schlüssel unbegrenzt gültig. </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>Ein boolescher Wert, der bestimmt, ob die Schlüsselinformationen den Service verlassen dürfen.</p>
          <p>Wenn das Attribut <code>extractable</code> auf <code>false</code> festgelegt wird, erstellt der Service einen Rootschlüssel, der für die Operationen <code>wrap</code> oder <code>unwrap</code> verwendet werden kann.</p>
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabbelle 1. Beschreibung der Variablen, die zum Hinzufügen eines Rootschlüssels mit der {{site.data.keyword.keymanagementserviceshort}}-API erforderlich sind.</caption>
    </table>

    Vermeiden Sie zum Schutz der Vertraulichkeit Ihrer persönlichen Daten die Eingabe von personenbezogenen Informationen (PII) beim Hinzufügen von Schlüsseln zum Service. Hierzu gehören beispielsweise Namen oder Standortangaben. Weitere Beispiele für personenbezogene Daten finden Sie in Abschnitt 2.2 des Dokuments [NIST Special Publication 800-122 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-122.pdf){: new_window}.
    {: important}

    Mit der erfolgreichen Antwort `POST api/v2/keys` werden der ID-Wert für Ihren Schlüssel sowie andere Metadaten zurückgegeben. Die ID ist eine eindeutige Kennung, die Ihrem Schlüssel zugeordnet ist und die für alle nachfolgenden Aufrufe für die {{site.data.keyword.keymanagementserviceshort}}-API verwendet wird.

3. Optional: Stellen Sie sicher, dass der Schlüssel erstellt wurde, indem Sie den folgenden Aufruf ausführen, um die Schlüssel in Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zu durchsuchen.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

    Nachdem Sie einen Rootschlüssel mit dem Service erstellt haben, ist der Schlüssel an {{site.data.keyword.keymanagementserviceshort}} gebunden und die Schlüsselinformationen können nicht abgerufen werden.
    {: note} 

## Weitere Schritte
{: #create-root-key-next-steps}

- Weitere Informationen zum Schutz von Schlüsseln mit der Envelope-Verschlüsselung finden Sie in [Schlüssel einschließen](/docs/services/key-protect?topic=key-protect-wrap-keys).
- Weitere Informationen zur programmgesteuerten Verwaltung von Schlüsseln [finden Sie in der {{site.data.keyword.keymanagementserviceshort}}-API-Referenzdokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/key-protect){: new_window}.
