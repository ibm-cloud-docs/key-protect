---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: import root key, upload root key, import key-wrapping key, upload key-wrapping key, import CRK, import CMK, upload CRK, upload CMK, import customer key, upload customer key, key-wrapping key, root key API examples

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

# Rootschlüssel importieren
{: #import-root-keys}

Sie können mit {{site.data.keyword.keymanagementservicefull}} Rootschlüssel mithilfe der {{site.data.keyword.keymanagementserviceshort}}-GUI oder programmgesteuert mit der {{site.data.keyword.keymanagementserviceshort}}-API schützen.
{: shortdesc}

Rootschlüssel sind symmetrische Key-Wrapping-Schlüssel, die die Sicherheit verschlüsselter Daten in der Cloud gewährleisten. Weitere Informationen zum Importieren von Rootschlüsseln in {{site.data.keyword.keymanagementserviceshort}} finden Sie in [Eigene Verschlüsselungsschlüssel in der Cloud verwenden](/docs/services/key-protect?topic=key-protect-importing-keys).

## Rootschlüssel mit der GUI importieren
{: #import-root-key-gui}

[Führen Sie nach dem Erstellen einer Instanz dieses Services](/docs/services/key-protect?topic=key-protect-provision) die folgenden Schritte aus, um einen vorhandenen Rootschlüssel mit der {{site.data.keyword.keymanagementserviceshort}}-GUI hinzuzufügen.

1. [Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-Konsole ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/){: new_window} an.
2. Rufen Sie **Menü** &gt; **Ressourcenliste** auf, um eine Liste Ihrer Ressourcen anzuzeigen.
3. Wählen Sie in der {{site.data.keyword.cloud_notm}}-Ressourcenliste die bereitgestellte Instanz von {{site.data.keyword.keymanagementserviceshort}} aus.
4. Um einen Schlüssel zu importieren, klicken Sie auf **Schlüssel hinzufügen** und wählen Sie das Fenster **Eigenen Schlüssel importieren** aus.

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
      <tr>
        <td>Schlüsselinformationen</td>
        <td>
          <p>Die base64-codierten Schlüsselinformationen (z. B. der vorhandene Key-Wrapping-Schlüssel), den Sie im Service speichern und verwalten möchten.</p>
          <p>Stellen Sie sicher, dass die Schlüsselinformationen die folgenden Anforderungen erfüllen:</p>
          <p>
            <ul>
              <li>Der Schlüssel muss eine Größe von 128, 192 oder 256 Bit haben.</li>
              <li>Die Byte der Daten (z. B. 32 Byte oder 256 Bit) müssen mit der base64-Codierung verschlüsselt werden.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Einstellungen für <b>Eigenen Schlüssel importieren</b></caption>
    </table>

5. Geben Sie die Details zum Schlüssel ein und klicken Sie anschließend zum Bestätigen auf **Schlüssel importieren**. 

## Rootschlüssel mit der API importieren
{: #import-root-key-api}

Sie können die Rootschlüssel in den Service importieren, indem Sie die {{site.data.keyword.keymanagementserviceshort}}-API verwenden.

Planen Sie für den Import von Schlüsseln voraus, indem Sie die Informationen zu Ihren [Optionen für die Erstellung und Verschlüsselung von Schlüsselinformationen](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead) lesen. Für zusätzliche Sicherheit können Sie den sicheren Import der Schlüsselinformationen ermöglichen, indem Sie Ihre Schlüsselinformationen mit einem [Transportschlüssel](/docs/services/key-protect?topic=key-protect-importing-keys#transport-keys) verschlüsseln, bevor Sie sie in der Cloud verwenden. Wenn Sie einen Rootschlüssel lieber ohne Transportschlüssel importieren, fahren Sie mit [Schritt 4](#import-root-key) fort.
{: note}

### Schritt 1: Transportschlüssel erstellen
{: #create-transport-key}

Transportschlüssel sind derzeit ein Beta-Feature. Beta-Features können jederzeit geändert werden. Mit zukünftigen Aktualisierungen werden möglicherweise Änderungen eingeführt, die mit der neuesten Version nicht kompatibel sind.
{: important}

Erstellen Sie einen Transportschlüssel für Ihre Serviceinstanz, indem Sie einen `POST`-Aufruf an den folgenden Endpunkt absetzen.

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
      <caption style="caption-side:bottom;">Tabelle 2. Beschreibung der Variablen, die zum Erstellen eines Transportschlüssels mit der {{site.data.keyword.keymanagementserviceshort}}-API erforderlich sind</caption>
  </table>

  Mit der erfolgreichen Antwort `POST api/v2/lockers` werden der ID-Wert für Ihren Transportschlüssel sowie andere Metadaten zurückgegeben. Die ID ist eine eindeutige Kennung, die Ihrem Transportschlüssel zugeordnet ist und für nachfolgende Aufrufe an die {{site.data.keyword.keymanagementserviceshort}}-API verwendet wird.

### Schritt 2: Abrufen des Transportschlüssels und des Importtokens
{: #retrieve-transport-key}

Erstellen Sie einen Transportschlüssel und ein Importtoken, indem Sie einen `GET` an den folgenden Endpunkt absetzen.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers/<key_id>
```
{: codeblock}

1. Rufen Sie die [{{site.data.keyword.keymanagementserviceshort}}-API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/key-protect){: new_window} mit dem folgenden cURL-Befehl auf.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/locker/<locker_id> \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json'
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
        <td><varname>locker_ID</varname></td>
        <td><strong>Erforderlich.</strong> Die ID für den Transportschlüssel, der in <a href="#create-transport-key">Schritt 1</a> erstellt wurde.</td>
      </tr>
        <caption style="caption-side:bottom;">Tabelle 3. Beschreibung der Variablen, die zum Abrufen eines Transportschlüssels mit der {{site.data.keyword.keymanagementserviceshort}}-API erforderlich sind</caption>
    </table>

    Eine erfolgreiche Antwort `GET api/v2/lockers/{id}` gibt einen 4096-Bit- und DER-codierten öffentlichen Verschlüsselungsschlüssel im PKIX-Format zurück, mit dem Sie Ihre Rootschlüsselinformationen verschlüsseln können, sowie ein Importtoken, mit dem die Integrität des Transportschlüssels überprüft wird.

### Schritt 3: Verschlüsseln Ihrer Schlüsselinformationen
{: #encrypt-root-key}

Nachdem Sie den Transportschlüssel abgerufen haben, verschlüsseln Sie mit dem Schlüssel die Schlüsselinformationen, die Sie in {{site.data.keyword.keymanagementserviceshort}} importieren möchten.  

<!-- TODO: Add link to tutorial that uses OpenSSL for key generation and encryption (in progress)-->

Für die lokale Generierung der Schlüsselinformationen lesen Sie die Informationen zu Ihren [Optionen zur Erstellung von Schlüsseln für die symmetrische Verschlüsselung](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead). Sie können beispielsweise das interne Schlüsselmanagementsystem Ihrer Organisation verwenden, das von einem lokalen Sicherheitsmodul (HSM) gesichert wird, um die Schlüsselinformationen zu erstellen und zu exportieren.
{: note}

So verschlüsseln Sie Ihre Schlüsselinformationen:

1. Exportieren Sie die 256-Bit-Schlüsselinformationen im Binärformat aus Ihrem lokalen Schlüsselmanagementsystem.

    Informationen zum Erstellen und Exportieren von Schlüsselinformationen finden Sie in der Dokumentation zu Ihrem lokalen HSM oder zum Schlüsselmanagementsystem.

2. Verwenden Sie den [abgerufenen Transportschlüssel](#retrieve-transport-key) aus Schritt 2, um die Schlüsselinformationen zu verschlüsseln.

   Zum Verschlüsseln Ihrer Schlüsselinformationen verwenden Sie das Verschlüsselungsschema `RSAES_OAEP_SHA_256`. Dies ist das Standardschema, das {{site.data.keyword.keymanagementserviceshort}} verwendet, um den Transportschlüssel zu erstellen. Zur Vermeidung von Entschlüsselungsproblemen in {{site.data.keyword.keymanagementserviceshort}} schließen Sie nicht den optionalen Parameter `label` ein, wenn Sie die Verschlüsselung RSAES_OAEP für Schlüsselinformationen ausführen. Informationen zum Ausführen der RSA-Verschlüsselung für Ihre Schlüsselinformationen finden Sie in der Dokumentation zu Ihrem lokalen HSM oder zum Schlüsselmanagementsystem.

3. Stellen Sie sicher, dass die verschlüsselten Schlüsselinformationen Base64-codiert sind, bevor Sie mit dem nächsten Schritt fortfahren.

### Schritt 4: Schlüsselinformationen importieren
{: #import-root-key}

[Nach der Verschlüsselung und Base64-Codierung der Schlüsselinformationen](#encrypt-root-key) importieren Sie den Rootschlüssel nach {{site.data.keyword.keymanagementserviceshort}}, indem Sie einen `POST`-Aufruf an den folgenden Endpunkt absetzen.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. Rufen Sie die [{{site.data.keyword.keymanagementserviceshort}}-API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/key-protect){: new_window} mit dem folgenden cURL-Befehl auf.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
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
       "payload": "<key_material>",
       "extractable": <key_type>,
       "encryptionAlgorithm": "<encryption_algorithm>",
       "importToken": "<import_token>"
       }
     ]
    }'
    ```
    {: codeblock}

    Ersetzen Sie die Variablen in der Beispielanforderung mithilfe der Angaben in der folgenden Tabelle.
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
        <td><strong>Erforderlich.</strong> Ein eindeutiger, lesbarer Name zur einfachen Identifikation Ihres Schlüssels. Aus Datenschutzgründen dürfen keine personenbezogenen Daten als Metadaten für den Schlüssel gespeichert werden.</td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>Eine erweiterte Beschreibung des Schlüssels. Aus Datenschutzgründen dürfen keine personenbezogenen Daten als Metadaten für den Schlüssel gespeichert werden.</td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>Der Zeitpunkt (Datum und Uhrzeit), zu dem der Schlüssel im System abläuft, im RFC-3339-Format. Wenn das Attribut <code>expirationDate</code> fehlt, bleibt der Schlüssel unbegrenzt gültig.</td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>Die base64-codierten Schlüsselinformationen (z. B. der vorhandene Key-Wrapping-Schlüssel), den Sie im Service speichern und verwalten möchten.</p>
          <p>Stellen Sie sicher, dass die Schlüsselinformationen die folgenden Anforderungen erfüllen:</p>
          <p>
            <ul>
              <li>Der Schlüssel muss eine Größe von 128, 192 oder 256 Bit haben.</li>
              <li>Die Byte der Daten (z. B. 32 Byte oder 256 Bit) müssen mit der base64-Codierung verschlüsselt werden.</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>Ein boolescher Wert, der bestimmt, ob die Schlüsselinformationen den Service verlassen dürfen.</p>
          <p>Wenn das Attribut <code>extractable</code> auf <code>false</code> festgelegt wird, bezeichnet der Service einen Rootschlüssel, der für die Operationen <code>wrap</code> oder <code>unwrap</code> verwendet werden kann.</p>
        </td>
      </tr>
      <tr>
        <td><varname>encryption_algorithm</varname></td>
        <td>Das Verschlüsselungsschema, das Sie zum <a href="#encrypt-root-key">Verschlüsseln der Schlüsselinformationen</a> verwendet haben. Derzeit wird <code>RSAES_OAEP_SHA_256</code> unterstützt. Wenn Sie die Rootschlüsselinformationen ohne Verwendung eines Transportschlüssels und Importtokens importieren möchten, lassen Sie das Attribut <code>encryption_algorithm</code> aus.</td>
      </tr>
      <tr>
        <td><varname>import_token</varname></td>
        <td>Das Importtoken, mit dem der Aktivitätszustand und die Integrität eines Transportschlüssels überprüft werden. Zur Verschlüsselung Ihrer Schlüsselinformationen mit einem Transportschlüssel müssen Sie dasselbe Importtoken angeben, das Sie in <a href="#retrieve-transport-key">Schritt 2</a> abgerufen haben. Zum Importieren der Rootschlüsselinformationen ohne Transportschlüssel und Importtoken lassen Sie das Attribut <code>importToken</code> aus.</td>
      </tr>
        <caption style="caption-side:bottom;">Tabelle 4. Beschreibung der Variablen, die zum Hinzufügen eines Rootschlüssels mit der {{site.data.keyword.keymanagementserviceshort}}-API erforderlich sind.</caption>
    </table>

    Vermeiden Sie zum Schutz der Vertraulichkeit Ihrer persönlichen Daten die Eingabe von personenbezogenen Informationen (PII) beim Hinzufügen von Schlüsseln zum Service. Hierzu gehören beispielsweise Namen oder Standortangaben. Weitere Beispiele für personenbezogene Daten finden Sie in Abschnitt 2.2 des Dokuments [NIST Special Publication 800-122 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: new_window}.
    {: important}

    Mit der erfolgreichen Antwort `POST api/v2/keys` werden der ID-Wert für Ihren Schlüssel sowie andere Metadaten zurückgegeben. Die ID ist eine eindeutige Kennung, die Ihrem Schlüssel zugeordnet ist und die für alle nachfolgenden Aufrufe für die {{site.data.keyword.keymanagementserviceshort}}-API verwendet wird.

2. Optional: Stellen Sie sicher, dass der Schlüssel hinzugefügt wurde, indem Sie den folgenden Aufruf ausführen, um die Schlüssel in Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zu durchsuchen.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## Weitere Schritte
{: #import-root-key-next-steps}

- Weitere Informationen zum Schutz von Schlüsseln mit der Envelope-Verschlüsselung finden Sie in [Schlüssel einschließen](/docs/services/key-protect?topic=key-protect-wrap-keys).
- Weitere Informationen zur programmgesteuerten Verwaltung von Schlüsseln [finden Sie in der {{site.data.keyword.keymanagementserviceshort}}-API-Referenzdokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/key-protect){: new_window}.
