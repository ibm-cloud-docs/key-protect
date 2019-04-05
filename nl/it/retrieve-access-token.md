---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: access token, IAM token, generate access token, generate IAM token, get access token, get IAM token, IAM token API, IAM token CLI

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

# Richiamo di un token di accesso
{: #retrieve-access-token}

Introduzione alle API {{site.data.keyword.keymanagementservicelong}} utilizzando l'autenticazione delle tue richieste al servizio con un token di accesso {{site.data.keyword.iamlong}} (IAM).
{: shortdesc}

## Richiamo di un token di accesso con la CLI
{: #retrieve-token-cli}

Puoi utilizzare la [CLI {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli?topic=cloud-cli-overview){: new_window} per generare velocemente il tuo token di accesso Cloud IAM personale.

1. Accedi a {{site.data.keyword.cloud_notm}} con la [CLI {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli?topic=cloud-cli-overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Se l'accesso non riesce, esegui il comando `ibmcloud login --sso` e prova di nuovo. Il parametro `--sso` è obbligatorio quando
accedi con un ID federato. Se viene utilizzata questa opzione, vai al link elencato nell'output della CLI
per generare una passcode monouso.
    {: note}

2. Seleziona l'account, la regione e il gruppo di risorse che contengono la tua istanza di cui è stato eseguito il provisioning di {{site.data.keyword.keymanagementserviceshort}}.

3. Immetti il seguente comando per richiamare il tuo token di accesso Cloud IAM.

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    Il seguente esempio troncato mostra un token IAM richiamato.

    ```sh
    IAM token:  Bearer eyJraWQiOiIyM...
    ```
    {: screen}

## Richiamo di un token di accesso con l'API
{: #retrieve-token-api}

Puoi anche richiamare il tuo token di accesso in modo programmatico creando prima una [chiave API dell'ID servizio](/docs/iam?topic=iam-serviceidapikeys) per la tua applicazione e poi scambiandola con un token di accesso {{site.data.keyword.cloud_notm}} IAM.

1. Accedi a {{site.data.keyword.cloud_notm}} con la [CLI {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli?topic=cloud-cli-overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Se l'accesso non riesce, esegui il comando `ibmcloud login --sso` e prova di nuovo. Il parametro `--sso` è obbligatorio quando
accedi con un ID federato. Se viene utilizzata questa opzione, vai al link elencato nell'output della CLI
per generare una passcode monouso.
    {: note}

2. Seleziona l'account, la regione e il gruppo di risorse che contengono la tua istanza di cui è stato eseguito il provisioning di {{site.data.keyword.keymanagementserviceshort}}.

3. Crea un [ID servizio](/docs/iam?topic=iam-serviceids#creating-a-service-id) per la tua applicazione.

  ```sh
  ibmcloud iam service-id-create SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
  ```
  {: pre}

4. [Assegna una politica di accesso](/docs/iam?topic=iam-serviceidpolicy) per l'ID servizio.

    Puoi assegnare le autorizzazioni di accesso per il tuo ID servizio [utilizzando la console {{site.data.keyword.cloud_notm}}](/docs/iam?topic=iam-serviceidpolicy#access_new). Per ulteriori informazioni su come i ruoli di accesso _Gestore_, _Scrittore_ e _Lettore_ vengono associati ad azioni {{site.data.keyword.keymanagementserviceshort}} specifiche, vedi [Ruoli e autorizzazioni](/docs/services/key-protect?topic=key-protect-manage-access#roles).
    {: tip}

5. Crea una [chiave API dell'ID servizio](/docs/iam?topic=iam-serviceidapikeys).

  ```sh
  ibmcloud iam service-api-key-create API_KEY_NAME SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
                     [--file FILE_NAME]
  ```
  {: pre}

  Sostituisci `<service_ID_name>` con l'alias univoco che hai assegnato al tuo ID servizio nel passo precedente. Salva la tua chiave API scaricandola in un luogo sicuro. 

6. Richiama l'[API IAM Identity Services](https://{DomainName}/apidocs/iam-identity-token-api) per richiamare il tuo token di accesso.

    ```cURL
    curl -X POST \
      "https://iam.bluemix.net/identity/token" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -H "Accept: application/json" \
      -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>"
    ```
    {: codeblock}

    Nella richiesta, sostituisci `<API_KEY>` con la chiave API che hai creato nel passo precedente. Il seguente esempio troncato mostra l'output del token: 

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

    Utilizza il valore `access_token` completo, preceduto dal tipo di token _Bearer_, per gestire in modo programmatico le chiavi per il tuo servizio utilizzando l'API {{site.data.keyword.keymanagementserviceshort}}. Per vedere un esempio di richiesta API {{site.data.keyword.keymanagementserviceshort}}, consulta [Creazione della tua richiesta API](/docs/services/key-protect?topic=key-protect-set-up-api#form-api-request).

    I token di accesso sono validi per 1 ora, ma puoi rigenerarli quando necessario. Per mantenere l'accesso al servizio, rigenera il token di accesso per la tua chiave API regolarmente richiamando l'[API IAM Identity Services](https://{DomainName}/apidocs/iam-identity-token-api).   
    {: note }

    
