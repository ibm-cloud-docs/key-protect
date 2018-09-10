---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Accesso all'API
{: #access-api}

{{site.data.keyword.keymanagementservicefull}} fornisce un'API REST
che può essere utilizzata con qualsiasi linguaggio di programmazione per archiviare, richiamare e generare le chiavi.
{: shortdesc}

Per utilizzare l'API, devi generare le tue credenziali del servizio e di autenticazione. 

## Richiamo di un token di accesso
{: #retrieve-token}

Puoi autenticarti con {{site.data.keyword.keymanagementserviceshort}} richiamando un token di accesso da {{site.data.keyword.iamshort}}. Con un [ID del servizio](/docs/iam/serviceid.html#serviceids), puoi utilizzare l'API {{site.data.keyword.keymanagementserviceshort}} al posto del tuo servizio o applicazione all'interno o all'esterno di {{site.data.keyword.cloud_notm}}, senza il bisogno di condividere le tue credenziali utente personali.  

Se vuoi autenticarti con le tue credenziali utente, puoi richiamare il tuo token eseguendo `ibmcloud iam oauth-tokens` nella [CLI {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli/index.html#overview){: new_window}.
{: tip}

Completa la seguente procedura per richiamare un token di accesso:

1. Nella console {{site.data.keyword.cloud_notm}}, vai a **Manage** &gt; **Security** &gt; **Identity and Access** &gt; **Service IDs**. Segui il processo per [creare un ID del servizio](/docs/iam/serviceid.html#creating-a-service-id){: new_window}.
2. Utilizza il menu **Actions** per [definire una politica di accesso per il tuo nuovo ID servizio](/docs/iam/serviceidaccess.html){: new_window}.
    
    Per ulteriori informazioni sulla gestione dell'accesso alle tue risorse {{site.data.keyword.keymanagementserviceshort}}, consulta [Ruoli e autorizzazioni](/docs/services/key-protect/manage-access.html#roles).
3. Utilizza la sezione **API keys** per [creare una chiave API da associare all'ID servizio](/docs/iam/serviceid_keys.html#serviceidapikeys){: new_window}. Salva la tua chiave API scaricandola in un luogo sicuro.
4. Richiama l'API {{site.data.keyword.iamshort}} per richiamare il tuo token di accesso.

    ```cURL
    curl -X POST \
      "https://iam.bluemix.net/identity/token" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -H "Accept: application/json" \
      -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>" \ 
    ```
    {: codeblock}

    Nella richiesta, sostituisci `<API_KEY>` con la chiave API che hai creato nel passo 3. Il seguente esempio troncato mostra l'output del token:

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

    Utilizza il valore `access_token` completo, preceduto dal tipo di token _Bearer_, per gestire in modo programmatico le chiavi per il tuo servizio utilizzando l'API {{site.data.keyword.keymanagementserviceshort}}. 

    I token di accesso sono validi per 1 ora, ma puoi rigenerarli quando necessario. Per mantenere l'accesso al servizio, aggiorna regolarmente il token di accesso per la tua chiave API richiamando l'API {{site.data.keyword.iamshort}}.   
    {: tip }

## Richiamo del tuo ID dell'istanza
{: #retrieve-instance-ID}

Puoi richiamare le informazioni di identificazione per la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}} utilizzando la [CLI {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli/index.html#overview){: new_window}. Utilizza un ID dell'istanza per gestire le tue chiavi di crittografia in un'istanza specificata di {{site.data.keyword.keymanagementserviceshort}} nel tuo account. 

1. Accedi a {{site.data.keyword.cloud_notm}} con la [CLI {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli/index.html#overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Nota:** se l'accesso non riesce, esegui il comando `ibmcloud login --sso` e prova di nuovo. Il parametro `--sso` è obbligatorio quando
accedi con un ID federato. Se viene utilizzata questa opzione, vai al link elencato nell'output della CLI
per generare una passcode monouso.

2. Seleziona l'account che contiene la tua istanza fornita di {{site.data.keyword.keymanagementserviceshort}}.

    Puoi eseguire `ibmcloud resource service-instances` per elencare tutte le istanze del servizio che sono state fornite nel tuo account.

3. Richiama il CRN (Cloud Resource Name) che identifica in modo univoco la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. 

    ```sh
    ibmcloud resource service-instance <instance_name> --id
    ```
    {: pre}

    Sostituisci `<instance_name>` con l'alias univoco che hai assegnato alla tua istanza di {{site.data.keyword.keymanagementserviceshort}}. Il seguente esempio troncato mostra l'output della CLI. Il valore _42454b3b-5b06-407b-a4b3-34d9ef323901_ è un ID dell'istanza di esempio.

    ```
    crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:42454b3b-5b06-407b-a4b3-34d9ef323901::
    ```
    {: screen}

## Creazione della tua richiesta API
{: #form-api-request}

Quando effettui una chiamata API al servizio, struttura la tua richiesta API in base a come hai inizialmente eseguito il provisioning della tua istanza di {{site.data.keyword.keymanagementserviceshort}}. 

Per creare la tua richiesta, accoppia un [endpoint del servizio regionale](/docs/services/key-protect/regions.html) con le credenziali di autenticazione appropriate. Ad esempio, se hai creato un'istanza del servizio per la regione `us-south`, utilizza il seguente endpoint e le intestazioni API per sfogliare le chiavi nel tuo servizio:

```cURL
curl -X GET \
    https://keyprotect.us-south.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-instance: <instance_ID>' \
```
{: codeblock} 

### Operazioni successive

- Per ulteriori informazioni sulla gestione a livello programmatico delle tue chiavi, [consulta la documentazione di riferimento API {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/apidocs/kms){: new_window}.
- Per visualizzare un esempio di come è possibile utilizzare le chiavi archiviate in {{site.data.keyword.keymanagementserviceshort}} per crittografare e decrittografare i dati, [controlla l'applicazione di esempio in GitHub ![Icona di link esterno](../../icons/launch-glyph.svg "Icona di link esterno")](https://github.com/IBM-Bluemix/key-protect-helloworld-python){: new_window}.
