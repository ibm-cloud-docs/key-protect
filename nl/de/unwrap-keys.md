---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: unwrap key, decrypt key, decrypt data encryption key, access data encryption key, envelope encryption API examples

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

# Wrapping von Schlüssel aufheben
{: #unwrap-keys}

Sie können das Wrapping eines Datenverschlüsselungsschlüssels (DEK) mithilfe der {{site.data.keyword.keymanagementservicefull}}-API aufheben, um auf seine Inhalte zuzugreifen. Beim Unwrapping wird der DEK entschlüsselt. Außerdem wird die Integrität seiner Inhalte geprüft. Die ursprünglichen Schlüsselinformationen werden an Ihren {{site.data.keyword.cloud_notm}}-Datenservice zurückgegeben.
{: shortdesc}

Informationen darüber, wie Key-Wrapping Sie unterstützt, die Sicherheit ruhender Daten in der Cloud zu gewährleisten, finden Sie in [Daten mit Envelope-Verschlüsselung schützen](/docs/services/key-protect?topic=key-protect-envelope-encryption).

## Wrapping von Schlüsseln mithilfe der API aufheben
{: #unwrap-key-api}

[Nach dem Absetzen eines Wrapping-Aufrufs für den Service](/docs/services/key-protect?topic=key-protect-wrap-keys) können Sie das Wrapping eines bestimmten Datenverschlüsselungsschlüssels (DEK) aufheben, um auf seine Inhalte mithilfe eines `POST`-Aufrufs zum folgenden Endpunkt zuzugreifen.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>?action=unwrap
```
{: codeblock}

1. [Rufen Sie Ihren Service- und Authentifizierungsnachweis ab, um mit den Schlüsseln im Service zu arbeiten.](/docs/services/key-protect?topic=key-protect-set-up-api)

2. Kopieren Sie die ID des Rootschlüssels, mit dem Sie die ursprüngliche Wrapping-Anforderung durchgeführt haben.

    Sie können die ID für einen Schlüssel abrufen, indem Sie die Anforderung `GET /v2/keys` absetzen oder indem Sie die Schlüssel in der {{site.data.keyword.keymanagementserviceshort}}-GUI anzeigen.

3. Kopieren Sie den `ciphertext`-Wert, der während der ursprünglichen Wrapping-Anforderung zurückgegeben wurde.

4. Führen Sie den folgenden cURL-Befehl aus, um die Schlüsselinformationen zu entschlüsseln und zu authentifizieren.

    ```cURL
    curl -X POST \
      'https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=unwrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "ciphertext": "<encrypted_data_key>",
      "aad": ["<additional_data>", "<additional_data>"]
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
        <td><varname>key_ID</varname></td>
        <td><strong>Erforderlich.</strong> Die eindeutige ID für den Rootschlüssel der ursprünglichen Wrapping-Anforderung.</td>
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
        <td><varname>additional_data</varname></td>
        <td>Die zusätzlichen Authentifizierungsdaten (AAD), die für die weitere Sicherung des Schlüssels verwendet werden. Jede Zeichenfolge kann bis zu 255 Zeichen enthalten. Wenn AAD bei einer Wrapping-Aufruf für den Service angegeben wird, müssen Sie dieselben AAD während des Unwrap-Aufrufs verwenden.</td>
      </tr>
      <tr>
        <td><varname>encrypted_data_key</varname></td>
        <td><strong>Erforderlich.</strong> Der <code>ciphertext</code>-Wert, der während der Wrapping-Operation zurückgegeben wurde.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Variablen, die zum Aufheben des Key-Wrappings in {{site.data.keyword.keymanagementserviceshort}} erforderlich sind.</caption>
    </table>

    Die ursprünglichen Schlüsselinformationen werden im Entitätshauptteil der Antwort zurückgegeben. Das folgende JSON-Objekt zeigt ein Beispiel für einen zurückgegebenen Wert.

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg=="
    }
    ```
    {:screen}

    Wenn {{site.data.keyword.keymanagementserviceshort}} feststellt, dass Sie für den Rootschlüssel, der zum Aufheben des Wrappings und Zugreifen auf Ihre Daten verwendet wird, eine Rotation durchgeführt haben, gibt der Service auch einen neuen Datenverschlüsselungsschlüssel, für den ein Wrapping durchgeführt wurde (`ciphertext`), im Antworthauptteil des Unwrappings zurück. Speichern Sie den neuen `ciphertext`-Wert und verwenden Sie ihn bei zukünftigen Envelope-Verschlüsselungsoperationen, sodass Ihre Daten durch den neuesten Rootschlüssel geschützt werden.
    {: note}
