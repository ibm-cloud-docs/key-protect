---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: import standard encryption key, upload standard encryption key, import secret, persist secret, store secret, upload secret, store encryption key, standard key API examples

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

# Standardschlüssel importieren
{: #import-standard-keys}

Sie können Ihre vorhandenen Verschlüsselungsschlüssel mit der {{site.data.keyword.keymanagementserviceshort}}-GUI oder programmgesteuert mit der {{site.data.keyword.keymanagementserviceshort}}-API hinzufügen.

## Standardschlüssel mit der GUI importieren
{: #import-standard-key-gui}

[Führen Sie nach dem Erstellen einer Instanz dieses Services](/docs/services/key-protect?topic=key-protect-provision) die folgenden Schritte aus, um einen vorhandenen Standardschlüssel mit der {{site.data.keyword.keymanagementserviceshort}}-GUI einzugeben.

1. [Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-Konsole an](https://{DomainName}/){: external}.
2. Rufen Sie **Menü** &gt; **Ressourcenliste** auf, um eine Liste Ihrer Ressourcen anzuzeigen.
3. Wählen Sie in der {{site.data.keyword.cloud_notm}}-Ressourcenliste die bereitgestellte Instanz von {{site.data.keyword.keymanagementserviceshort}} aus.
4. Wenn Sie einen neuen Schlüssel importieren möchten, klicken Sie auf **Schlüssel hinzufügen** und wählen Sie das Fenster **Eigenen Schlüssel importieren** aus.

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
        <td>Der <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Schlüsseltyp</a>, den Sie in {{site.data.keyword.keymanagementserviceshort}} verwalten möchten. Wählen Sie aus der Liste der Schlüsseltypen <b>Standardschlüssel</b> aus.</td>
      </tr>
      <tr>
        <td>Schlüsselinformationen</td>
        <td>
          <p>Die mit Base64 codierten Schlüsselinformationen (z. B. ein symmetrischer Schlüssel), die Sie im Service verwalten möchten.</p>
          <p>Stellen Sie sicher, dass die Schlüsselinformationen die folgenden Anforderungen erfüllen:</p>
          <p><ul>
              <li>Der Schlüssel kann bis zu 10.000 Byte groß sein.</li>
              <li>Die Datenbyte müssen mit Base64 codiert sein.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Einstellungen für <b>Eigenen Schlüssel importieren</b></caption>
    </table>

5. Geben Sie die Details zum Schlüssel ein und klicken Sie anschließend zum Bestätigen auf **Schlüssel importieren**. 

## Standardschlüssel mit der API importieren
{: #import-standard-key-api}

Importieren Sie einen Standardschlüssel, indem Sie einen `POST`-Aufruf an den folgenden Endpunkt vornehmen:

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Rufen Sie Ihren Service- und Authentifizierungsnachweis ab, um mit den Schlüsseln im Service zu arbeiten.](/docs/services/key-protect?topic=key-protect-set-up-api)

2. Rufen Sie die [{{site.data.keyword.keymanagementserviceshort}}-API](https://{DomainName}/apidocs/key-protect){: external} mit dem folgenden cURL-Befehl auf.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
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
       "payload": "<key_material>",
       "extractable": <key_type>
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
        <td><strong>Erforderlich.</strong> Die Regionsabkürzung, z. B. <code>us-south</code> oder <code>eu-gb</code>, die den geografischen Bereich darstellt, in dem sich Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz befindet. Weitere Informationen finden Sie in <a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">Regionale Serviceendpunkte</a>.</td>
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
        <td><varname>return_preference</varname></td>
        <td><p>Ein Header, der das Serververhalten für <code>POST</code>- und <code>DELETE</code>-Operationen ändert.</p><p>Wenn Sie die Variable <em>return_preference</em> auf <code>return=minimal</code> setzen, werden im Entitätshauptteil der Antwort nur die Metadaten des Schlüssels zurückgegeben, wie zum Beispiel Schlüsselname und ID-Wert. Wenn Sie für die Variable <code>return=representation</code> festlegen, werden sowohl die Schlüsselinformationen als auch die Metadaten des Schlüssels zurückgegeben.</p></td>
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
        <td>Der Zeitpunkt (Datum und Uhrzeit), zu dem der Schlüssel im System abläuft, im RFC-3339-Format. Wenn das Attribut <code>expirationDate</code> fehlt, bleibt der Schlüssel unbegrenzt gültig. </td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>Die mit Base64 codierten Schlüsselinformationen (z. B. ein symmetrischer Schlüssel), die Sie im Service verwalten möchten.</p>
          <p>Stellen Sie sicher, dass die Schlüsselinformationen die folgenden Anforderungen erfüllen:</p>
          <p><ul>
              <li>Der Schlüssel kann bis zu 10.000 Byte groß sein.</li>
              <li>Die Datenbyte müssen mit Base64 codiert sein.</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>Ein boolescher Wert, der bestimmt, ob die Schlüsselinformationen den Service verlassen dürfen.</p>
          <p>Wenn das Attribut <code>extractable</code> auf <code>true</code> festgelegt wird, bezeichnet der Service einen Standardschlüssel, den Sie in Ihren Apps oder Services speichern können.</p>
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabelle 2. Beschreibung der Variablen, die zum Hinzufügen eines Standardschlüssels mit der {{site.data.keyword.keymanagementserviceshort}}-API erforderlich sind.</caption>
    </table>

    Vermeiden Sie zum Schutz der Vertraulichkeit Ihrer persönlichen Daten die Eingabe von personenbezogenen Informationen (PII) beim Hinzufügen von Schlüsseln zum Service. Hierzu gehören beispielsweise Namen oder Standortangaben. Weitere Beispiele für personenbezogene Daten finden Sie in Abschnitt 2.2 des Dokuments [NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
    {: important}

    Mit der erfolgreichen Antwort `POST api/v2/keys` werden der ID-Wert für Ihren Schlüssel sowie andere Metadaten zurückgegeben. Die ID ist eine eindeutige Kennung, die Ihrem Schlüssel zugeordnet ist und die für alle nachfolgenden Aufrufe für die {{site.data.keyword.keymanagementserviceshort}}-API verwendet wird.

3. Optional: Stellen Sie sicher, dass der Schlüssel hinzugefügt wurde, indem Sie den folgenden Aufruf ausführen, um die Schlüssel in Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz abzurufen.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}


## Weitere Schritte
{: #import-standard-key-next-steps}

- Weitere Informationen zur programmgesteuerten Verwaltung von Schlüsseln finden Sie in der [{{site.data.keyword.keymanagementserviceshort}}-API-Referenzdokumentation](https://{DomainName}/apidocs/key-protect){: external}.
