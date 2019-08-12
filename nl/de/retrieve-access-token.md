---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: access token, IAM token, generate access token, generate IAM token, get access token, get IAM token, IAM token API, IAM token CLI

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

# Ein Zugriffstoken abrufen
{: #retrieve-access-token}

Beginnen Sie Ihre Arbeit mit den {{site.data.keyword.keymanagementservicelong}}-APIs, indem Sie Ihre Anforderungen an den Service mit einem IAM-Zugriffstoken ({{site.data.keyword.iamlong}}) authentifizieren.
{: shortdesc}

## Zugriffstoken mit der Befehlszeilenschnittstelle abrufen
{: #retrieve-token-cli}

Mit der [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle](/docs/cli?topic=cloud-cli-getting-started){: external} können Sie umgehend Ihr persönliches Cloud-IAM-Zugriffstoken generieren.

1. Melden Sie sich bei {{site.data.keyword.cloud_notm}} über die [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle](/docs/cli?topic=cloud-cli-getting-started){: external} an.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Wenn die Anmeldung fehlschlägt, führen Sie den Befehl `ibmcloud login --sso` aus, um es erneut zu versuchen. Der Parameter `--sso` ist für die Anmeldung mit einer eingebundenen ID erforderlich. Rufen Sie bei Verwendung dieser Option den in der CLI-Ausgabe aufgeführten Link auf, um einen einmaligen Kenncode zu generieren.
    {: note}

2. Wählen Sie das Konto, die Region und die Ressourcengruppe aus, die Ihre bereitgestellte Instanz von {{site.data.keyword.keymanagementserviceshort}} enthalten.

3. Führen Sie den folgenden Befehl aus, um Ihr Cloud IAM-Zugriffstoken abzurufen.

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    Im folgenden gekürzten Beispiel sehen Sie ein abgerufenes IAM-Token.

    ```sh
    IAM token:  Bearer eyJraWQiOiIyM...
    ```
    {: screen}

## Zugriffstoken mit der API abrufen
{: #retrieve-token-api}

Sie können Ihr Zugriffstoken auch programmgesteuert abrufen, indem Sie zuerst einen [Service-ID-API-Schlüssel](/docs/iam?topic=iam-serviceidapikeys) für Ihre Anwendung erstellen und anschließend Ihren API-Schlüssel durch ein {{site.data.keyword.cloud_notm}} IAM-Token ersetzen.

1. Melden Sie sich bei {{site.data.keyword.cloud_notm}} über die [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle](/docs/cli?topic=cloud-cli-getting-started){: external} an.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Wenn die Anmeldung fehlschlägt, führen Sie den Befehl `ibmcloud login --sso` aus, um es erneut zu versuchen. Der Parameter `--sso` ist für die Anmeldung mit einer eingebundenen ID erforderlich. Rufen Sie bei Verwendung dieser Option den in der CLI-Ausgabe aufgeführten Link auf, um einen einmaligen Kenncode zu generieren.
    {: note}

2. Wählen Sie das Konto, die Region und die Ressourcengruppe aus, die Ihre bereitgestellte Instanz von {{site.data.keyword.keymanagementserviceshort}} enthalten.

3. Erstellen Sie eine [Service-ID](/docs/iam?topic=iam-serviceids#creating-a-service-id) für Ihre Anwendung.

  ```sh
  ibmcloud iam service-id-create SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
  ```
  {: pre}

4. [Weisen Sie der Service-ID eine Zugriffsrichtlinie zu](/docs/iam?topic=iam-serviceidpolicy).

    Sie können Ihrer Service-ID Zugriffsberechtigungen zuweisen, indem Sie die [{{site.data.keyword.cloud_notm}}-Konsole verwenden](/docs/iam?topic=iam-serviceidpolicy#access_new). Informationen zum Zuweisen der Zugriffsrollen _Manager_, _Schreibberechtigter_ und _Leseberechtigter_ zu bestimmten Aktionen des {{site.data.keyword.keymanagementserviceshort}}-Service finden Sie unter [Rollen und Berechtigungen](/docs/services/key-protect?topic=key-protect-manage-access#roles).
    {: tip}

5. Erstellen Sie einen [API-Schlüssel für die Service-ID](/docs/iam?topic=iam-serviceidapikeys).

  ```sh
  ibmcloud iam service-api-key-create API_KEY_NAME SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
                     [--file FILE_NAME]
  ```
  {: pre}

  Ersetzen Sie `<service_ID_name>` durch den eindeutigen Alias, den Sie Ihrer Service-ID im vorherigen Schritt zugewiesen haben. Speichern Sie Ihren API-Schlüssel, indem Sie ihn an eine sichere Position herunterladen. 

6. Rufen Sie die [API für den IAM-Identitätsservice](https://{DomainName}/apidocs/iam-identity-token-api) auf, um Ihr Zugriffstoken abzurufen.

    ```cURL
    curl -X POST \
      "https://iam.cloud.ibm.com/identity/token" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -H "Accept: application/json" \
      -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>" > token.json
    ```
    {: codeblock}

    Ersetzen Sie in der Anforderung `<API_KEY>` durch den API-Schlüssel, den Sie im vorherigen Schritt erstellt haben. Im folgenden gekürzten Beispiel wird der Inhalt der Datei `token.json` dargestellt:

    ```
    {
    "access_token": "eyJraWQiOiIyM...",
    "expiration": 1512161390,
    "expires_in": 3600,
    "refresh_token": "...",
    "token_type": "Bearer"
    }
    ```
    {: screen}

    Verwenden Sie den vollständigen Wert `access_token` mit dem Präfixtyp _Trägertoken_, um die Schlüssel für Ihren Service programmgesteuert mit der {{site.data.keyword.keymanagementserviceshort}}-API zu verwalten. Ein Beispiel für eine {{site.data.keyword.keymanagementserviceshort}}-API-Anforderung finden Sie unter [API-Anforderung formulieren](/docs/services/key-protect?topic=key-protect-set-up-api#form-api-request).

    Zugriffstokens sind für einen Zeitraum von 1 Stunde gültig, können jedoch bei Bedarf neu generiert werden. Generieren Sie das Zugriffstoken für Ihren API-Schlüssel regelmäßig neu, indem Sie die [API für den IAM-Identitätsservice](https://{DomainName}/apidocs/iam-identity-token-api) aufrufen, damit Ihr Zugriff auf den Service erhalten bleibt.   
    {: note }

    <!--You can also pipe the output to `jq`, and then grab only the `access_token` value `| jq .access_token-->

    <!--You use IBM® Cloud Identity and Access Management (IAM) tokens to make authenticated requests to IBM Watson™ services without embedding service credentials in every call. IAM authentication uses access tokens for authentication, which you acquire by sending a request with an API key.-->
