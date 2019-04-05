---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: automatic key rotation, set rotation policy, policy-based, key rotation

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

# Configurazione di una politica di rotazione
{: #set-rotation-policy}

Puoi configurare una politica di rotazione automatica per una chiave root utilizzando {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

Quando configuri una politica di rotazione automatica per una chiave root, ne abbrevi il ciclo di vita ad intervalli regolari e limiti la quantità di informazioni da essa protetta.

Puoi creare una politica di rotazione solo per le chiavi root generate in {{site.data.keyword.keymanagementserviceshort}}. Se hai importato la chiave root inizialmente, devi fornire del nuovo materiale della chiave con codifica base64 per eseguire la rotazione della chiave. Per ulteriori informazioni, vedi [Rotazione delle chiavi root su richiesta](/docs/services/key-protect?topic=key-protect-rotate-root-keys).
{: note}

Vuoi saperne di più sulle tue opzioni di rotazione della chiave in {{site.data.keyword.keymanagementserviceshort}}? Per ulteriori informazioni, vedi [Confronto delle tue opzioni di rotazione della chiave](/docs/services/key-protect?topic=key-protect-compare-key-rotation-options).
{: tip}

## Gestione delle politiche di rotazione nella GUI
{: #manage-policies-gui}

Se preferisci gestire le politiche per le tue chiavi root utilizzando un'interfaccia grafica, puoi utilizzare la GUI {{site.data.keyword.keymanagementserviceshort}}.

1. [Accedi alla console {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/){: new_window}.
2. Vai a **Menu** &gt; **Resource List** per visualizzare un elenco delle tue risorse.
3. Dal tuo elenco risorse {{site.data.keyword.cloud_notm}}, seleziona la tua istanza di cui è stato eseguito il provisioning di {{site.data.keyword.keymanagementserviceshort}}.
4. Nella pagina dei dettagli dell'applicazione, utilizza la tabella **Keys** per sfogliare le chiavi nel tuo servizio.
5. Fai clic sull'icona ⋯ per aprire un elenco di opzioni per una chiave specifica.
6. Dal menu delle opzioni, fai clic su **Manage policy** per gestire la politica di rotazione per la chiave.
7. Dall'elenco delle opzioni di rotazione, seleziona una frequenza per la rotazione in mesi.

    Se la tua chiave ha una politica di rotazione esistente, l'interfaccia visualizza il periodo di rotazione esistente della chiave.

8. Fai clic su **Create policy** per configurare la politica per la chiave.

Quando è il momento di ruotare la chiave in base all'intervallo di rotazione che hai specificato, {{site.data.keyword.keymanagementserviceshort}} sostituisce automaticamente la chiave root con il nuovo materiale della chiave.

## Gestione delle politiche di rotazione con l'API
{: #manage-rotation-policies-api}

### Visualizzazione di una politica di rotazione
{: #view-rotation-policy-api}

Per una vista di alto livello, puoi sfogliare le politiche associate a una chiave root effettuando una chiamata `GET` al seguente endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Richiama le tue credenziali del servizio e di autenticazione](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Richiama la politica di rotazione per una chiave specificata eseguendo il seguente comando cURL. 

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

    Sostituisci le variabili nella richiesta di esempio in base alla seguente tabella.
    <table>
      <tr>
        <th>Variabile</th>
        <th>Descrizione</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Obbligatorio</strong> L'identificativo univoco per la chiave root che ha una politica di rotazione esistente. </td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Obbligatorio</strong> L'abbreviazione della regione, come <code>us-south</code> o <code>eu-gb</code>, che rappresenta l'area geografica in cui si trova la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Endpoint di servizio regionali</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Obbligatorio</strong> Il tuo token di accesso {{site.data.keyword.cloud_notm}}. Includi il contenuto completo del token <code>IAM</code>, compreso il valore Bearer, nella richiesta cURL. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Richiamo di un token di accesso</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Obbligatorio</strong> L'identificativo univoco che viene assegnato alla tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Richiamo di un ID istanza</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>L'identificativo univoco utilizzato per tracciare e correlare le transazioni.</td>
      </tr>
        <caption style="caption-side:bottom;">Tabella 1. Descrive le variabili necessarie per creare una politica di rotazione con l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Una risposta `GET api/v2/keys/{id}/policies` con esito positivo restituisce i dettagli della politica associati alla tua chiave. Il seguente oggetto JSON mostra una risposta di esempio per una chiave root che ha una politica di rotazione esistente. 

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

    Il valore `interval_month` indica la frequenza di rotazione della chiave in mesi.

### Creazione di una politica di rotazione
{: #create-rotation-policy-api}

Crea una politica di rotazione per la tua chiave root effettuando una chiamata `PUT` al seguente endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Richiama le tue credenziali del servizio e di autenticazione](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Crea una politica di rotazione per una chiave specificata eseguendo il seguente comando cURL. 

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

    Sostituisci le variabili nella richiesta di esempio in base alla seguente tabella.
    <table>
      <tr>
        <th>Variabile</th>
        <th>Descrizione</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Obbligatorio</strong> L'identificativo univoco della chiave root per cui vuoi creare una politica di rotazione.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Obbligatorio</strong> L'abbreviazione della regione, come <code>us-south</code> o <code>eu-gb</code>, che rappresenta l'area geografica in cui si trova la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Endpoint di servizio regionali</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Obbligatorio</strong> Il tuo token di accesso {{site.data.keyword.cloud_notm}}. Includi il contenuto completo del token <code>IAM</code>, compreso il valore Bearer, nella richiesta cURL. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Richiamo di un token di accesso</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Obbligatorio</strong> L'identificativo univoco che viene assegnato alla tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Richiamo di un ID istanza</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>L'identificativo univoco utilizzato per tracciare e correlare le transazioni.</td>
      </tr>
      <tr>
        <td><varname>rotation_interval</varname></td>
        <td><strong>Obbligatorio</strong> Un valore intero che determina il tempo di intervallo della rotazione della chiave in mesi. Il valore minimo è <code>1</code> e il massimo è <code>12</code>.
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabella 1. Descrive le variabili necessarie per creare una politica di rotazione con l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Una risposta `PUT api/v2/keys/{id}/policies` con esito positivo restituisce i dettagli della politica associati alla tua chiave. Il seguente oggetto JSON mostra una risposta di esempio per una chiave root che ha una politica di rotazione esistente. 

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

### Aggiornamento di una politica di rotazione
{: #update-rotation-policy-api}

Aggiorna una politica esistente per una chiave root effettuando una chiamata `PUT` al seguente endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Richiama le tue credenziali del servizio e di autenticazione](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Sostituisci la politica di rotazione per una chiave specificata eseguendo il seguente comando cURL. 

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

    Sostituisci le variabili nella richiesta di esempio in base alla seguente tabella.
    <table>
      <tr>
        <th>Variabile</th>
        <th>Descrizione</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Obbligatorio</strong> L'identificativo univoco della chiave root per cui vuoi sostituire una politica di rotazione.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Obbligatorio</strong> L'abbreviazione della regione, come <code>us-south</code> o <code>eu-gb</code>, che rappresenta l'area geografica in cui si trova la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Endpoint di servizio regionali</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Obbligatorio</strong> Il tuo token di accesso {{site.data.keyword.cloud_notm}}. Includi il contenuto completo del token <code>IAM</code>, compreso il valore Bearer, nella richiesta cURL. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Richiamo di un token di accesso</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Obbligatorio</strong> L'identificativo univoco che viene assegnato alla tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Richiamo di un ID istanza</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>L'identificativo univoco utilizzato per tracciare e correlare le transazioni.</td>
      </tr>
      <tr>
        <td><varname>new_rotation_interval</varname></td>
        <td><strong>Obbligatorio</strong> Un valore intero che determina il tempo di intervallo della rotazione della chiave in mesi. Il valore minimo è <code>1</code> e il massimo è <code>12</code>.
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabella 1. Descrive le variabili necessarie per creare una politica di rotazione con l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Una risposta `PUT api/v2/keys/{id}/policies` con esito positivo restituisce i dettagli della politica aggiornati associati alla tua chiave. Il seguente oggetto JSON mostra una risposta di esempio per una chiave root con una politica di rotazione aggiornata. 

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

    I valori `interval_month` e `updatedat` vengono aggiornati nei dettagli della politica per la chiave. Se un utente diverso aggiorna una politica per una chiave da te creata inizialmente, anche il valore `updatedby` viene modificato per mostrare l'identificativo della persona che ha inviato la richiesta.
