---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Risoluzione dei problemi
{: #troubleshooting}

I problemi generali con l'utilizzo di {{site.data.keyword.keymanagementservicefull}} potrebbero includere il fornire intestazioni o credenziali corrette quando interagisci con l'API. In molti casi, puoi risolvere questi problemi seguendo pochi semplici passi.
{: shortdesc}

## Non riesco a eliminare la mia istanza del servizio Cloud Foundry
{: #unable-to-delete-service}

Quando provi a eliminare la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}, l'eliminazione del servizio non riesce come previsto.

Dal dashboard {{site.data.keyword.cloud_notm}}, vai a **Servizi Cloud Foundry** e selezioni la tua istanza di {{site.data.keyword.keymanagementserviceshort}}. Fai clic sull'icona ⋮ per aprire un elenco di opzioni per l'offerta di servizi e fai clic su **Elimina servizio**.
{: tsSymptoms}

L'eliminazione del servizio non riesce e vedi il seguente errore: 
```
403 Forbidden: This action cannot be completed because you have existing secrets in your Key Protect service. You first need to delete the secrets before you can remove the service.
```
{: screen}

Il 15 dicembre 2017, {{site.data.keyword.keymanagementserviceshort}} è passato dall'utilizzo di organizzazioni, spazi e ruoli Cloud Foundry all'utilizzo di gruppi di risorse e IAM. Puoi ora eseguire il provisioning del servizio {{site.data.keyword.keymanagementserviceshort}} all'interno di un gruppo di risorse senza dover specificare un'organizzazione e uno spazio Cloud Foundry.
{: tsCauses}

Queste modifiche hanno avuto ripercussioni sulla modalità di funzionamento dell'annullamento del provisioning per le istanze meno recenti del servizio. Se hai creato la tua istanza di {{site.data.keyword.keymanagementserviceshort}} prima del 28 settembre 2017, l'eliminazione del servizio potrebbe non funzionare come previsto.

Per eliminare un'istanza del servizio {{site.data.keyword.keymanagementserviceshort}} meno recente, devi prima eliminare le tue chiavi esistenti utilizzando l'endpoint `https://ibm-key-protect.edge.bluemix.net` legacy per interagire con il servizio {{site.data.keyword.keymanagementserviceshort}}.
{: tsResolve}

Per eliminare le tue chiavi e la tua istanza del servizio:

1. Accedi a {{site.data.keyword.cloud_notm}} con la CLI {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login 
    ```
    {: codeblock}

    **Nota:** se l'accesso non riesce, esegui il comando `bx login --sso` e riprova. Il parametro `--sso` è obbligatorio quando
accedi con un ID federato. Se viene utilizzata questa opzione, vai al link elencato nell'output della CLI
per generare una passcode monouso.

2. Seleziona la regione, l'organizzazione e lo spazio {{site.data.keyword.cloud_notm}} che contengono la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.

    Prendi nota dei nomi dei tuoi spazio e organizzazione nell'output della CLI. Puoi anche eseguire `ibmcloud cf target` per indicare come destinazione la tua organizzazione e il tuo spazio Cloud Foundry.

    ```sh
    ibmcloud cf target -o <organization_name> -s <space_name>
    ```
    {: codeblock}

3. Richiama i tuoi GUID dell'organizzazione e dello spazio {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud iam org <organization_name> --guid
    ibmcloud iam space <space_name> --guid
    ```
    {: codeblock}
    Sostituisci `<organization_name>` e `<space_name>` con gli alias univoci che hai assegnato alla tua organizzazione e al tuo spazio.

4. Richiama il tuo token di accesso.

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

5. Elenca le chiavi archiviate nella tua istanza del servizio eseguendo il seguente comando cURL.

    ```cURL
    curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    Sostituisci `<access_token>`, `<organization_GUID>` e `<space_GUID>` con i valori che hai richiamato nei passi 3 - 4. 

6. Copia il valore ID per ciascuna chiave archiviata nella tua istanza del servizio.

7. Esegui il seguente comando cURL per eliminare in modo permanente una chiave e il suo contenuto.

    ```cURL
    curl -X DELETE \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys/<key_ID> \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    Sostituisci `<access_token>`, `<organization_GUID>`, `<space_GUID>` e `<key_ID>` con i valori che hai richiamato nei passi 3 - 5. Ripeti il comando per ciascuna chiave.    

8. Verifica che le tue chiavi siano state eliminate eseguendo il eseguente comando cURL.

    ```cURL
    curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    Sostituisci `<access_token>`, `<organization_GUID>` e `<space_GUID>` con i valori che hai richiamato nei passi 3 - 4. 

9. Elimina la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.

    ```sh
    ibmcloud cf delete-service "<service_instance_name>"
    ```
    {: codeblock}

10. Facoltativo: verifica che la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}} sia stata eliminata passando al tuo dashboard {{site.data.keyword.cloud_notm}}.

    Puoi anche elencare i tuoi servizi Cloud Foundry disponibili nello spazio indicato come destinazione eseguendo questo comando.

    ```sh
    ibmcloud cf service list
    ```
    {: codeblock}


## Impossibile accedere all'interfaccia utente
{: #unable-to-access-ui}

Quando accedi all'interfaccia utente {{site.data.keyword.keymanagementserviceshort}}, l'IU non viene caricata come previsto.

Dal dashboard {{site.data.keyword.cloud_notm}}, seleziona la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

Viene visualizzato il seguente errore: 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

Il 15 dicembre 2017, abbiamo aggiunto nuove funzioni, come la [crittografia envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption), al servizio {{site.data.keyword.keymanagementserviceshort}}. Puoi ora eseguire il provisioning del servizio {{site.data.keyword.keymanagementserviceshort}} all'interno di un gruppo di risorse senza dover specificare un'organizzazione e uno spazio Cloud Foundry.
{: tsCauses}

Queste modifiche hanno interessato l'interfaccia utente per le istanze più vecchie del servizio. Se hai creato la tua istanza di {{site.data.keyword.keymanagementserviceshort}} prima del 28 settembre 2017, l'interfaccia utente potrebbe non funzionare come previsto.

Stiamo lavorando per risolvere questo problema. Come soluzione temporanea, puoi continuare a gestire le tue chiavi utilizzando l'API {{site.data.keyword.keymanagementserviceshort}}.
{: tsResolve}

Puoi utilizzare l'endpoint `https://ibm-key-protect.edge.bluemix.net` legacy per interagire con il servizio {{site.data.keyword.keymanagementserviceshort}}.

**Richiesta di esempio**

```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
```
{: codeblock}

## Impossibile creare o eliminare chiavi
{: #unable-to-create-keys}

Quando accedi all'interfaccia utente {{site.data.keyword.keymanagementserviceshort}}, non vedi le opzioni per aggiungere o eliminare le chiavi.

Dal dashboard {{site.data.keyword.cloud_notm}}, seleziona la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

Puoi vedere un elenco di chiavi, ma non vedi le opzioni per aggiungere o eliminare le chiavi. 

Non hai l'autorizzazione corretta per eseguire le azioni di {{site.data.keyword.keymanagementserviceshort}}.
{: tsCauses} 

Verifica con il tuo amministratore che ti sia stato assegnato il ruolo corretto nel gruppo di risorse o nell'istanza del servizio applicabile. Per ulteriori informazioni sui ruoli, vedi [Ruoli e autorizzazioni](/docs/services/key-protect?topic=key-protect-manage-access#roles).
{: tsResolve}

## Come ottenere aiuto e supporto
{: #getting-help}

Se hai dei problemi o delle domande quando utilizzi {{site.data.keyword.keymanagementserviceshort}}, puoi controllare in {{site.data.keyword.cloud_notm}} oppure ottenere aiuto cercando informazioni o facendo domande attraverso un forum. Puoi inoltre aprire un ticket di supporto.
{: shortdesc}

Puoi controllare se {{site.data.keyword.cloud_notm}} è disponibile andando alla [pagina sugli stati ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/status?tags=platform,runtimes,services).

Puoi rivedere i forum per controllare se altri utenti hanno riscontrato lo stesso problema. Quando stai utilizzando i forum per fare una domanda, contrassegna con una tag la tua domanda in modo che sia visualizzabile dai team di sviluppo
{{site.data.keyword.cloud_notm}}.

- Se hai domande tecniche su {{site.data.keyword.keymanagementserviceshort}}, inserisci la tua domanda in [Stack Overflow ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} e contrassegnala con le tag "ibm-cloud" e "key-protect".
- Per domande sul servizio e sulle istruzioni introduttive, utilizza il forum [IBM developerWorks dW Answers ![Icona di link esterno](../../icons/launch-glyph.svg "Icona di link esterno")](https://developer.ibm.com/answers/topics/key-protect/){: new_window}. Includi le tag "ibm-cloud"
e "key-protect".

Per ulteriori dettagli sull'utilizzo dei forum, consulta il documento relativo a [Richiesta di supporto ![Icona di link esterno](../../icons/launch-glyph.svg "Icona di link esterno")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: new_window}.

Per ulteriori informazioni sull'apertura di un ticket di supporto {{site.data.keyword.IBM_notm}} o sui livelli di supporto e sulla gravità dei ticket, vedi [Come contattare il supporto ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/get-support?topic=get-support-getting-customer-support){: new_window}.
