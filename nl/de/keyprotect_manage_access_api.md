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

# Zugriff auf Schlüssel verwalten
{: #managing-access-api}

Mit {{site.data.keyword.iamlong}} können Sie die differenzierte Zugriffssteuerung für Ihre Verschlüsselungsschlüssel aktivieren, indem Sie die Zugriffsrichtlinien erstellen und ändern.
{: shortdesc}

Auf dieser Seite werden Sie Schritt für Schritt durch Szenarios zur Verwaltung des Zugriffs auf Ihre Verschlüsselungsschlüssel mithilfe der [API für das Zugriffsmanagement ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://iampap.ng.bluemix.net/v1/docs/#!/Access_Policies/){: new_window} geführt.

## Vorbereitungen
{: #prereqs}

Generieren Sie zur Verwendung der API Ihre Authentifizierungsberechtigungsnachweise, wie z. B. das [Zugriffstoken](/docs/services/keymgmt/keyprotect_authentication.html#retrieve_token) und die [Instanz-ID](/docs/services/keymgmt/keyprotect_authentication.html#retrieve_instance_ID). Darüber hinaus benötigen Sie die ID des {{site.data.keyword.keymanagementserviceshort}}-Schlüssels, für den Sie den Zugriff verwenden möchten. 

Informationen zum Anzeigen von Schlüssel-IDs finden Sie in [Schlüssel anzeigen](/docs/services/keymgmt/keyprotect_view_keys.html).
{: tip} 

### Konto-ID abrufen
{: #retrieve_account_ID}

Bestimmen Sie nach dem Abrufen der Berechtigungsnachweise den Zugriffsbereich für die neue Zugriffsrichtlinie, indem Sie die ID des Kontos abrufen, das Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz enthält.

Führen Sie die folgenden Schritte aus, um die Konto-ID abzurufen: 

1. Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle an.
    ```sh
    ibmcloud login [--sso]
    ```
    {: codeblock}

    **Anmerkung**: Der Parameter `--sso` ist für die Anmeldung mit einer eingebundenen ID erforderlich. Rufen Sie bei Verwendung dieser Option den in der CLI-Ausgabe aufgeführten Link auf, um einen einmaligen Kenncode zu generieren.

    Als Ergebnis werden die Identifikationsinformationen für Ihr Konto angezeigt.

    ```sh
    Authenticating...
    OK

    Konto auswählen (oder zum Überspringen die Eingabetaste drücken):

    1. Beispielkonto (b6hnh3560ehqjkf89s4ba06i367801e)
    Zahl eingeben> 1
    Zielkonto: Beispielkonto (b6hnh3560ehqjkf89s4ba06i367801e)

    API-Endpunkt:   https://api.ng.bluemix.net (API-Version: 2.75.0)
    Region:         us-south
    Benutzer:       admin
    Konto:          Beispielkonto (b6hnh3560ehqjkf89s4ba06i367801e)
    ```
    {: screen}
    
2. Kopieren Sie den Wert für die eigene Konto-ID. 
  
    Der Wert _b6hnh3560ehqjkf89s4ba06i367801e_ ist ein Beispiel für eine Instanz-ID.

### Benutzer-ID abrufen
{: #retrieve_user_ID}

Rufen Sie die Benutzer-ID des Benutzers ab, dessen Zugriff Sie ändern möchten. 

Führen Sie die folgenden Schritte aus, um die Benutzer-ID abzurufen:

1. [Fordern Sie den Benutzer zur Angabe eines IAM-Tokens auf](/docs/services/keymgmt/keyprotect_authentication.html#retrieve_token).
    Die Struktur des IAM-Tokens lautet wie folgt:

    ```sh
    IAM token: Bearer <value>.<value>.<value>
    ```
    {: screen}

2. Kopieren Sie den mittleren Wert und führen Sie den folgenden Befehl aus:
    ```sh
    echo -n "<value>" | base64 --decode
    ```
    {: codeblock}

    Als Ergebnis wird ein JSON-Objekt angezeigt, das dem folgenden Beispiel ähnelt:
   ```json
   {
        "iam_id":"...",
        "id":"...",
        "realmid":"...",
        "identifier":"...",
        "given_name":"...",
        "family_name":"...",
        "name":"...",
        "email":"...",
        "sub":"...",
        "account":{
            "bss":"..."},
            "iat":...,
            "exp":...,
            "iss":"...",
            "grant_type":"...",
            "scope":"...",
            "client_id":"..."
        }
   }
   ```
   {: screen}

4. Kopieren Sie den Wert der Eigenschaft `id`.

## Zugriffsrichtlinie erstellen
{: #create_policy}

Zur Aktivierung der Zugriffssteuerung für einen bestimmten Schlüssel können Sie eine Anforderung an {{site.data.keyword.iamshort}} senden, indem Sie den folgenden Befehl ausführen. Wiederholen Sie den Befehl für jede Zugriffsrichtlinie.

```cURL
curl -X POST \
  https://iam.bluemix.net/acms/v1/scopes/a%2F<account_ID>/users/<user_ID>/policies \
  -H 'Authorization: Bearer <Admin_IAM_token>' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
  "roles": [
    {
      "id": "crn:v1:bluemix:public:iam::::role:<IAM_role>"
    }
  ],
  "resources": [
    {
      "serviceName": "IBM Key Protect",
      "accountId": "<account_ID>",
      "region": "<region>",
      "serviceInstance": "<instance_ID>",
      "resourceType": "key",
      "resource": "<key_ID>"
    }
  ]
}'
```
{: codeblock}

Wenn Sie den Zugriff auf Schlüssel in bestimmten Cloud Foundry-Organisationen oder -Bereichen verwalten müssen, ersetzen Sie `serviceInstance` durch `organizationId` und `spaceId`. Weitere Informationen finden Sie in der [Referenzdokumentation zur API für das Zugriffsmanagement ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://iampap.ng.bluemix.net/v1/docs/#!/Access_Policies/){: new_window}.
{: tip}

Ersetzen Sie `<user_ID>`, `<Admin_IAM_token>`, `<IAM_role>`, `<region>`, `<account_ID>`, `<instance_ID>` und `<key_ID>` durch die entsprechenden Werte.

**Optional:** Überprüfen Sie, ob die Richtlinie erfolgreich erstellt wurde.

```cURL
curl -X GET \
  https://iam.bluemix.net/acms/v1/scopes/a%2F<account_ID>/users/<user_ID>/policies \
  -H 'Authorization: Bearer <Admin_IAM_token>' \
  -H 'Accept: application/json' \
```
{: codeblock}


## Zugriffsrichtlinie aktualisieren
{: #update_policy}

Sie können eine abgerufene Richtlinien-ID verwenden, um eine vorhandene Richtlinie für einen Benutzer zu ändern. Senden Sie eine Anforderung an {{site.data.keyword.iamshort}}, indem Sie den folgenden Befehl ausführen:

```cURL
curl -X PUT \
  https://iam.bluemix.net/acms/v1/scopes/a%2F<account_ID>/users/<user_ID>/policies/<policy_ID> \
  -H 'Authorization: Bearer <Admin_IAM_token>' \
  -H 'If-Match': <ETag_ID> \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
  "roles": [
    {
      "id": "crn:v1:bluemix:public:iam::::role:<IAM_role>"
    }
  ],
  "resources": [
    {
      "serviceName": "IBM Key Protect",
      "accountId": "<account_ID>",
      "region": "<region>",
      "serviceInstance": "<instance_ID>",
      "resourceType": "key",
      "resource": "<key_ID>"
    }
  ]
}'
```
{: codeblock}

Ersetzen Sie `<user_ID>`, `<Admin_IAM_token>`, `<IAM_role>`, `<region>`, `<account_ID>`, `<instance_ID>` und `<key_ID>` durch die entsprechenden Werte.

**Optional:** Überprüfen Sie, ob die Richtlinie erfolgreich aktualisiert wurde.

```cURL
curl -X GET \
  https://iam.bluemix.net/acms/v1/scopes/a%2F<account_ID>/users/<user_ID>/policies \
  -H 'Authorization: Bearer <Admin_IAM_token>' \
  -H 'Accept: application/json' \
```
{: codeblock}

## Zugriffsrichtlinie löschen
{: #delete_policy}

Sie können eine abgerufene Richtlinien-ID verwenden, um eine vorhandene Richtlinie für einen Benutzer zu löschen. Senden Sie eine Anforderung an {{site.data.keyword.iamshort}}, indem Sie den folgenden Befehl ausführen:

```cURL
curl -X DELETE \
  https://iam.bluemix.net/acms/v1/scopes/a%2F<account_ID>/users/<user_ID>/policies/<policy_ID> \
  -H 'Authorization: Bearer <Admin_IAM_token>' \
  -H 'Accept: application/json' \
```
{: codeblock}

Ersetzen Sie `<account_ID>`, `<user_ID>`, `<policy_ID>` und `<Admin_IAM_token>` durch die entsprechenden Werte.

**Optional:** Überprüfen Sie, ob die Richtlinie erfolgreich gelöscht wurde.

```cURL
curl -X GET \
  https://iam.bluemix.net/acms/v1/scopes/a%2F<account_ID>/users/<user_ID>/policies \
  -H 'Authorization: Bearer <Admin_IAM_token>' \
  -H 'Accept: application/json' \
```
{: codeblock}
