---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: automatic key rotation, set rotation policy, policy-based, key rotation

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

# Rotationsrichtlinie festlegen
{: #set-rotation-policy}

Mit {{site.data.keyword.keymanagementservicefull}} können Sie eine automatische Rotationsrichtlinie für einen Rootschlüssel festlegen. 
{: shortdesc}

Durch das Festlegen einer automatischen Rotationsrichtlinie für einen Rootschlüssel wird die Laufzeit des Schlüssels verkürzt und die Menge der durch den Schlüssel geschützten Informationen begrenzt.

Eine Rotationsrichtlinie kann nur für Rootschlüssel festgelegt werden, die in {{site.data.keyword.keymanagementserviceshort}} generiert wurden. Wenn Sie den Rootschlüssel ursprünglich importiert haben, müssen Sie neue, mit Base64-Codierung verschlüsselte Schlüsselinformationen für die Schlüsselrotation angeben. Weitere Informationen finden Sie unter [Bedarfsgesteuerte Rotation von Rootschlüsseln](/docs/services/key-protect?topic=key-protect-rotate-keys#rotate-keys).
{: note}

Möchten Sie mehr über Ihre Optionen zur Schlüsselrotation in {{site.data.keyword.keymanagementserviceshort}} erfahren? Weitere Informationen finden Sie unter [Vergleich Ihrer Optionen für die Schlüsselrotation](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).
{: tip}

## Rotationsrichtlinien in der GUI verwalten
{: #manage-policies-gui}

Wenn Sie die Verwaltung von Richtlinien für Ihre Rootschlüssel über eine grafische Oberfläche bevorzugen, können Sie hierzu die {{site.data.keyword.keymanagementserviceshort}}-GUI verwenden.

1. [Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-Konsole an](https://{DomainName}/){: external}.
2. Rufen Sie **Menü** &gt; **Ressourcenliste** auf, um eine Liste Ihrer Ressourcen anzuzeigen.
3. Wählen Sie in der {{site.data.keyword.cloud_notm}}-Ressourcenliste die bereitgestellte Instanz von {{site.data.keyword.keymanagementserviceshort}} aus.
4. Verwenden Sie auf der Seite mit den Anwendungsdetails die Tabelle **Schlüssel**, um die Schlüssel im Service zu durchsuchen.
5. Klicken Sie auf das Symbol ⋯ , um eine Liste der Optionen für einen bestimmten Schlüssel zu öffnen.
6. Klicken Sie im Auswahlmenü auf **Richtlinie verwalten**, um die Rotationsrichtlinie für den Schlüssel zu verwalten.
7. Wählen Sie in der Liste der Rotationsoptionen die Häufigkeit der Rotation in Monaten aus.

    Wenn für Ihren Schlüssel eine Rotationsrichtlinie vorhanden ist, zeigt die Schnittstelle die vorhandene Rotationsdauer des Schlüssels an.

8. Klicken Sie auf **Richtlinie erstellen**, um die Richtlinie für den Schlüssel festzulegen.

Wenn der Schlüssel auf Basis des von Ihnen angegebenen Rotationsintervalls rotieren soll, ersetzt {{site.data.keyword.keymanagementserviceshort}} den Rootschlüssel automatisch durch die neuen Schlüsselinformationen.

## Rotationsrichtlinien mit der API verwalten
{: #manage-rotation-policies-api}

### Rotationsrichtlinie anzeigen
{: #view-rotation-policy-api}

Für eine übergeordnete Ansicht können Sie die Richtlinien durchsuchen, die einem Rootschlüssel zugeordnet sind, indem Sie einen `GET`-Aufruf an den folgenden Endpunkt absetzen.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Rufen Sie Ihren Service- und Authentifizierungsnachweis ab](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Rufen Sie die Rotationsrichtlinie für einen bestimmten Schlüssel ab, indem Sie den folgenden cURL-Befehl ausführen.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

    Ersetzen Sie die Variablen in der Beispielanforderung anhand der Angaben in der folgenden Tabelle.
    <table>
      <tr>
        <th>Variable</th>
        <th>Beschreibung</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Erforderlich.</strong> Die eindeutige ID für den Rootschlüssel, für den eine Rotationsrichtlinie vorhanden ist.</td>
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
        <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Variablen, die zum Erstellen einer Rotationsrichtlinie mit der {{site.data.keyword.keymanagementserviceshort}}-API erforderlich sind</caption>
    </table>

    Eine erfolgreiche Antwort `GET api/v2/keys/{id}/policies` gibt die Details zur Richtlinie zurück, die Ihrem Schlüssel zugeordnet sind. Das folgende JSON-Objekt zeigt eine Beispielantwort für einen Rootschlüssel mit einer vorhandenen Rotationsrichtlinie.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

    Der Wert für `interval_month` gibt die Häufigkeit der Schlüsselrotation in Monaten an.

### Rotationsrichtlinie erstellen
{: #create-rotation-policy-api}

Erstellen Sie eine Rotationsrichtlinie für Ihren Rootschlüssel, indem Sie einen `PUT`-Aufruf an den folgenden Endpunkt absetzen.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Rufen Sie Ihren Service- und Authentifizierungsnachweis ab](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Erstellen Sie die Rotationsrichtlinie für einen bestimmten Schlüssel, indem Sie den folgenden cURL-Befehl ausführen.

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
    "resources": [
      {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <rotation_interval>
        }
       }
      ]
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
        <td><varname>key_ID</varname></td>
        <td><strong>Erforderlich.</strong> Die eindeutige ID für den Rootschlüssel, für den eine Rotationsrichtlinie erstellt werden soll.</td>
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
        <td><varname>rotation_interval</varname></td>
        <td><strong>Erforderlich.</strong> Ein ganzzahliger Wert, der das Intervall für die Schlüsselrotation in Monaten bestimmt. Der Mindestwert ist <code>1</code>, der Höchstwert <code>12</code>.
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Variablen, die zum Erstellen einer Rotationsrichtlinie mit der {{site.data.keyword.keymanagementserviceshort}}-API erforderlich sind</caption>
    </table>

    Eine erfolgreiche Antwort `PUT api/v2/keys/{id}/policies` gibt die Details zur Richtlinie zurück, die Ihrem Schlüssel zugeordnet sind. Das folgende JSON-Objekt zeigt eine Beispielantwort für einen Rootschlüssel mit einer vorhandenen Rotationsrichtlinie.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

### Rotationsrichtlinie aktualisieren
{: #update-rotation-policy-api}

Aktualisieren Sie eine vorhandene Richtlinie für einen Rootschlüssel, indem Sie einen `PUT`-Aufruf an den folgenden Endpunkt absetzen.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Rufen Sie Ihren Service- und Authentifizierungsnachweis ab](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Ersetzen Sie die Rotationsrichtlinie für einen bestimmten Schlüssel, indem Sie den folgenden cURL-Befehl ausführen.

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
    "resources": [
      {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <new_rotation_interval>
        }
       }
      ]
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
        <td><varname>key_ID</varname></td>
        <td><strong>Erforderlich.</strong> Die eindeutige ID für den Rootschlüssel, für den eine Rotationsrichtlinie ersetzt werden soll.</td>
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
        <td><varname>new_rotation_interval</varname></td>
        <td><strong>Erforderlich.</strong> Ein ganzzahliger Wert, der das Intervall für die Schlüsselrotation in Monaten bestimmt. Der Mindestwert ist <code>1</code>, der Höchstwert <code>12</code>.
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Variablen, die zum Erstellen einer Rotationsrichtlinie mit der {{site.data.keyword.keymanagementserviceshort}}-API erforderlich sind</caption>
    </table>

    Eine erfolgreiche Antwort `PUT api/v2/keys/{id}/policies` gibt die aktualisierten Details zur Richtlinie zurück, die Ihrem Schlüssel zugeordnet sind. Das folgende JSON-Objekt zeigt eine Beispielantwort für einen Rootschlüssel mit einer aktualisierten Rotationsrichtlinie.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 2
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-820DPWINC2",
            "updatedat": "2019-03-10T12:24:22Z"
        }
      ]
    }
    ```
    {:screen}

    Die Werte für `interval_month` und `updatedat` werden in den Richtliniendetails für diesen Schlüssel aktualisiert. Wenn ein anderer Benutzer eine Richtlinie für einen Schlüssel aktualisiert, den Sie ursprünglich erstellt haben, wird der Wert `updatedby` ebenfalls geändert, sodass die ID für die Person angezeigt wird, die die Anforderung gesendet hat.
