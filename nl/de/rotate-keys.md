---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: rotate encryption key, encryption key rotation, rotate key API examples 

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

# Bedarfsgesteuerte Rotation von Schlüsseln
{: #rotate-keys}

Sie können Rootschlüssel mit {{site.data.keyword.keymanagementservicefull}} bedarfsgesteuert turnusmäßig wechseln (Schlüsselrotation).
{: shortdesc}

Durch die Rotation des Rootschlüssels wird die Laufzeit des Schlüssels verkürzt und die Menge der durch den betreffenden Schlüssel geschützten Informationen wird begrenzt.   

Die Schlüsselrotation unterstützt Sie bei der Einhaltung branchenspezifischer Vorgaben und dem Einsatz bewährter Verschlüsselungsverfahren. Informationen hierzu finden Sie in [Rotation für Ihre Verschlüsselungsschlüssel](/docs/services/key-protect?topic=key-protect-key-rotation).

Die Rotation ist nur für Rootschlüssel verfügbar. Weitere Informationen zu den Optionen für Ihre Schlüsselrotation in {{site.data.keyword.keymanagementserviceshort}} finden Sie unter [Vergleich Ihrer Optionen für die Schlüsselrotation](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).
{: note}

## Schlüsselrotation für Rootschlüssel mit der GUI
{: #rotate-key-gui}

Wenn Sie die Rotation von Rootschlüsseln über eine grafische Oberfläche bevorzugen, können Sie hierzu die {{site.data.keyword.keymanagementserviceshort}}-GUI verwenden.

[Nach dem Erstellen oder Importieren der vorhandenen Rootschlüssel in den Service](/docs/services/key-protect?topic=key-protect-create-root-keys) führen Sie die folgenden Schritte aus, um eine Schlüsselrotation durchzuführen:

1. [Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-Konsole an](https://{DomainName}/){: external}.
2. Rufen Sie **Menü** &gt; **Ressourcenliste** auf, um eine Liste Ihrer Ressourcen anzuzeigen.
3. Wählen Sie in der {{site.data.keyword.cloud_notm}}-Ressourcenliste die bereitgestellte Instanz von {{site.data.keyword.keymanagementserviceshort}} aus.
4. Verwenden Sie auf der Seite mit den Anwendungsdetails die Tabelle **Schlüssel**, um die Schlüssel im Service zu durchsuchen.
5. Klicken Sie auf das Symbol ⋯ , um eine Liste der Optionen für den Schlüssel zu öffnen, für den Sie eine Rotation durchführen möchten.
6. Klicken Sie im Auswahlmenü auf **Schlüsselrotation** und bestätigen Sie die Rotation in der nächsten Anzeige.

Wenn Sie den Rootschlüssel ursprünglich importiert haben, müssen Sie neue, mit Base64-Codierung verschlüsselte Schlüsselinformationen für die Schlüsselrotation angeben. Weitere Informationen finden Sie in [Rootschlüssel über die GUI importieren](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-key-gui).
{: note}

## Schlüsselrotation für Rootschlüssel mit der API
{: #rotate-key-api}

[Nach der Angabe eines Rootschlüssels im Service](/docs/services/key-protect?topic=key-protect-create-root-keys) können Sie eine Rotation für den Schlüssel durchführen, indem Sie einen `POST`-Aufruf an den folgenden Endpunkt vornehmen.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=rotate
```
{: codeblock}

1. [Rufen Sie Ihren Service- und Authentifizierungsnachweis ab, um mit den Schlüsseln im Service zu arbeiten.](/docs/services/key-protect?topic=key-protect-set-up-api)

2. Kopieren Sie die ID des Rootschlüssels, für den eine Rotation durchgeführt werden soll.

3. Führen Sie den folgenden cURL-Befehl aus, um den Schlüssel durch neue Schlüsselinformationen zu ersetzen.

    ```cURL
    curl -X POST \
      'https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=rotate' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -d '{
        'payload: <key_material>'
      }'
    ```
    {: codeblock}

    Ersetzen Sie die Variablen in der Beispielanforderung mithilfe der folgenden Tabelle.

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
        <td><strong>Erforderlich.</strong> Die eindeutige ID für den Rootschlüssel, für den eine Rotation durchgeführt werden soll.</td>
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
        <td><varname>key_material</varname></td>
        <td>
          <p>Die mit einer Base64-Codierung verschlüsselten Schlüsselinformationen, die im Service gespeichert und verwaltet werden sollen. Dieser Wert ist erforderlich, wenn die Schlüsselinformationen ursprünglich beim Hinzufügen des Schlüssels zum Service importiert wurden.</p>
          <p>Geben Sie für die Rotation eines Schlüssels, der ursprünglich in {{site.data.keyword.keymanagementserviceshort}} generiert wurde, das Attribut <code>payload</code> nicht an und übergeben Sie einen leeren Entitätshauptteil für die Anforderung. Geben Sie für die Rotation eines importierten Schlüssels Schlüsselinformationen an, die die folgenden Voraussetzungen erfüllen:</p>
          <p>
            <ul>
              <li>Der Schlüssel muss eine Größe von 128, 192 oder 256 Bit haben.</li>
              <li>Die Byte der Daten (z. B. 32 Byte oder 256 Bit) müssen mit der base64-Codierung verschlüsselt werden.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Variablen, die für die Rotation eines bestimmten Schlüssels in {{site.data.keyword.keymanagementserviceshort}} erforderlich sind.</caption>
    </table>

    Eine erfolgreiche Rotationsanforderung gibt die HTTP-Antwort `204 No Content` zurück, die bedeutet, dass der Rootschlüssel durch neue Schlüsselinformationen ersetzt wurde.

4. Optional: Stellen Sie sicher, dass die Schlüsselrotation durchgeführt wurde, indem Sie den folgenden Aufruf ausführen, um die Schlüssel in der {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zu durchsuchen.

    ```cURL
    curl -X GET \
    https://<region>.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <IAM_token>' \
    -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}
  
    Überprüfen Sie den Wert für `lastRotateDate` im Entitätshauptteil der Antwort, um das Datum und die Uhrzeit der letzten Rotation des Schlüssels zu prüfen.
    
