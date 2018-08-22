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

# Visualizzazione delle chiavi
{: #viewing-keys}

{{site.data.keyword.keymanagementservicefull}}
fornisce un sistema centralizzato per visualizzare, gestire e controllare le tue chiavi di crittografia. Controlla le tue chiavi e le restrizioni di accesso
alle chiavi per assicurare la protezione alle tue risorse.
{: shortdesc}

Controlla regolarmente la configurazione delle tue chiavi:

- Esamina quando vengono create le chiavi e determina se è il momento di ruotare la chiave.
- [Monitorare le chiamate API a{{site.data.keyword.keymanagementserviceshort}} con {{site.data.keyword.cloudaccesstrailshort}} ![Icona di link esterno](../../icons/launch-glyph.svg "Icona di link esterno")](/docs/services/cloud-activity-tracker/tutorials/manage_events_cli.html){: new_window}.
- Ispeziona quali utenti dispongono dell'accesso alle chiavi e se il livello di accesso è appropriato.

Per ulteriori informazioni sul controllo dell'accesso alle risorse, consulta [Gestione dell'accesso utente con Cloud IAM](/docs/services/keymgmt/keyprotect_manage_access.html).

## Visualizzazione delle chiavi con la GUI
{: #gui}

Se preferisci ispezionare le chiavi nel tuo servizio con un'interfaccia grafica, puoi utilizzare il dashboard {{site.data.keyword.keymanagementserviceshort}}.

[Dopo aver creato o importato le tue chiavi esistenti nel servizio](/docs/services/keymgmt/keyprotect_create_root.html), completa la seguente procedura per visualizzare la tue chiavi.

1. [Accedi alla console {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/).
2. Dal tuo dashboard {{site.data.keyword.cloud_notm}}, seleziona l'istanza di cui è stato eseguito il provisioning di {{site.data.keyword.keymanagementserviceshort}}.
3. Sfoglia le caratteristiche generali delle tue chiavi dal dashboard {{site.data.keyword.keymanagementserviceshort}}:

    <table>
      <tr>
        <th>Colonna</th>
        <th>Descrizione</th>
      </tr>
      <tr>
        <td>Nome</td>
        <td>L'alias leggibile dall'utente e univoco che è stato assegnato alla tua chiave.</td>
      </tr>
      <tr>
        <td>ID</td>
        <td>Un ID della chiave univoco che è stato assegnato alla tua chiave dal servizio {{site.data.keyword.keymanagementserviceshort}}. Puoi utilizzare il valore ID per effettuare chiamate al servizio con l'[API {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/apidocs/639).</td>
      </tr>
      <tr>
        <td>Stato</td>
        <td>Lo [stato della chiave](/docs/services/keymgmt/concepts/keyprotect_states.html) basato su [NIST Special Publication 800-57, Recommendation for Key Management ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf). Questi stati includono <i>Pre-attivata</i>, <i>Attiva</i>, <i>Disattivata</i> e <i>Distrutta</i>.</td>
      </tr>
      <tr>
        <td>Tipo</td>
        <td>Il [tipo di chiave](/docs/services/keymgmt/concepts/keyprotect_envelope.html#key_types) che descrive lo scopo di progettazione della tua chiave all'interno del servizio.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabella 1. Descrive la tabella <b>Chiavi</b></caption>
    </table>

## Visualizzazione delle chiavi con l'API
{: #api}

Puoi richiamare i contenuti delle tue chiavi utilizzando l'API {{site.data.keyword.keymanagementserviceshort}}.

### Richiamo di un elenco delle tue chiavi
{: #retrieve_keys_api}

Per una vista di alto livello, puoi sfogliare le chiavi che sono gestite nella tua istanza fornita di {{site.data.keyword.keymanagementserviceshort}} effettuando una chiamata `GET` al seguente endpoint.

```
https://keyprotect.<region>.bluemix.net/api/v2/keys
```
{: codeblock}

1. [Richiama le tue credenziali del servizio e di autenticazione per utilizzare le chiavi nel servizio](/docs/services/keymgmt/keyprotect_authentication.html).

2. Esegui il seguente comando cURL per visualizzare le caratteristiche generali delle chiavi.

    ```cURL
    curl -X GET \
    https://keyprotect.<region>.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <IAM_token>' \
    -H 'bluemix-instance: <instance_ID>' \
    -H 'correlation-id: <correlation_ID>' \
    ```
    {: codeblock}

    Per utilizzare le chiavi in un'organizzazione o uno spazio Cloud Foundry nel tuo account, sostituisci `Bluemix-Instance` con le intestazioni `Bluemix-org` e `Bluemix-space` appropriate. [Consulta la documentazione di riferimento dell'API {{site.data.keyword.keymanagementserviceshort}} per gli esempi di codice ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/apidocs/639){: new_window}.
    {: tip}

    Sostituisci le variabili nella richiesta di esempio in base alla seguente tabella.
    <table>
      <tr>
        <th>Variabile</th>
        <th>Descrizione</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td>L'abbreviazione della regione, come <code>us-south</code> o <code>eu-gb</code>, che rappresenta l'area geografica in cui si trova la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/keymgmt/keyprotect_regions.html#endpoints">Endpoint di servizio regionali</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td>Il tuo token di accesso {{site.data.keyword.cloud_notm}}. Includi il contenuto completo del token <code>IAM</code>, compreso il valore Bearer, nella richiesta cURL. Per ulteriori informazioni, vedi <a href="/docs/services/keymgmt/keyprotect_authentication.html#retrieve_token">Richiamo di un token di accesso</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td>L'identificativo univoco che viene assegnato alla tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/keymgmt/keyprotect_authentication.html#retrieve_instance_ID">Retrieving an instance ID</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>Facoltativo: l'identificativo univoco utilizzato per tracciare e correlare le transazioni.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabella 2. Descrive le variabili necessarie per visualizzare le chiavi con l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Una richiesta `GET /v2/keys` con esito positivo restituisce una raccolta di chiavi disponibili nella tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.

    ```
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.collection+json",
        "collectionTotal": 2
      },
    "resources": [
      {
          "id": "...",
          "type": "application/vnd.ibm.kms.key+json",
          "name": "Standard key",
          "description": "...",
          "state": 1,
          "crn": "...",
          "algorithmType": "AES",
          "createdBy": "...",
          "creationDate": "YYYY-MM-DDTHH:MM:SSZ",
          "lastUpdateDate": "YYYY-MM-DDTHH:MM:SSZ",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "GCM"
          },
          "extractable": true
        },
        {
          "id": "...",
          "type": "application/vnd.ibm.kms.key+json",
          "name": "Root key",
          "description": "...",
          "state": 1,
          "crn": "...",
          "algorithmType": "AES",
          "createdBy": "...",
          "creationDate": "YYYY-MM-DDTHH:MM:SSZ",
          "lastUpdateDate": "YYYY-MM-DDTHH:MM:SSZ",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "GCM"
          },
          "extractable": false
        }
      ]
    }
    ```
    {:screen}

    Per impostazione predefinita, `GET /keys` restituisce le tue prime 2000 chiavi, ma puoi modificare questo limite utilizzando il parametro `limit` al momento della query. Per ulteriori informazioni sui parametri `limit` e `offset`, vedi [Richiamo di un sottoinsieme di chiavi](#retrieve_subset_keys_api).
    {: tip}

### Richiamo di un sottoinsieme di chiavi
{: #retrieve_subset_keys_api}

Specificando i parametri `limit` e `offset` nel momento della query, puoi richiamare un sottoinsieme delle tue chiavi, a partire dal valore di `offset` che specifichi. 

Ad esempio, potresti avere 3000 chiavi totali memorizzate nella tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}, ma vuoi richiamare le chiavi da 200 a 300 quando effettui una richiesta `GET /keys`. 

Puoi utilizzare la seguente richiesta di esempio per richiamare una diverso insieme di chiavi.

  ```cURL
  curl -X GET \
  https://keyprotect.<region>.bluemix.net/api/v2/keys?offset=<offset>&limit=<limit> \
  -H 'accept: application/vnd.ibm.collection+json' \
  -H 'authorization: Bearer <IAM_token>' \
  -H 'bluemix-instance: <instance_ID>' \
  -H 'correlation-id: <correlation_ID>' \
  ```
  {: codeblock}

  Sostituisci le variabili `limit` e `offset` nella tua richiesta in base alla seguente tabella.
  <table>
    <tr>
      <th>Variabile</th>
      <th>Descrizione</th>
    </tr>
    <tr>
      <td><p><varname>offset</varname></p></td>
      <td>
        <p>Facoltativo: il numero di chiavi da ignorare.</p> 
        <p>Ad esempio, se nella tua istanza hai 50 chiavi e vuoi elencare le chiavi da 26 a 50, utilizza
            <code>../keys?offset=25</code>. Puoi anche accoppiare <code>offset</code> con <code>limit</code> per sfogliare le risorse disponibili.</p>
      </td>
    </tr>
    <tr>
      <td><p><varname>limit</varname></p></td>
      <td>
        <p>Facoltativo: il numero di chiavi da richiamare.</p> 
        <p>Ad esempio, se nella tua istanza hai 100 chiavi e vuoi elencare solo 10 chiavi, utilizza
            <code>../keys?limit=10</code>. Il valore massimo per <code>limit</code> è 5000.</p>
      </td>
    </tr>
    <caption style="caption-side:bottom;">Tabella 2. Descrive le variabili <code>limit</code> e <code>offset</code></caption>
  </table>

Per le note di utilizzo, consulta i seguenti esempi per l'impostazione dei parametri di query `limit` e `offset`.

<table>
  <tr>
    <th>URL</th>
    <th>Descrizione</th>
  </tr>
  <tr>
    <td><code>.../keys</code></td>
    <td>Elenca tutte le tue risorse disponibili, fino alle prime 2000 chiavi.</td>
  </tr>
  <tr>
    <td><code>.../keys?limit=10</code></td>
    <td>Elenca le prime 10 chiavi.</td>
  </tr>
  <tr>
    <td><code>.../keys?offset=25&limit=50</code></td>
    <td>Elenca le chiavi da 26 a 50.</td>
  </tr>
  <tr>
    <td><code>.../keys?offset=3000&limit=50</code></td>
    <td>Elenca le chiavi da 3001 a 3050.</td>
  </tr>
  <caption style="caption-side:bottom;">Tabella 3. Fornisce le note di utilizzo per i parametri di query limit e offset</caption>
</table>

L'offset è la posizione di una determinata chiave in un dataset. Il valore di `offset` è a base zero, il che significa che la decima chiave di crittografia in un dataset è all'offset 9.
{: tip}

### Richiamo di una chiave per ID
{: #retrieve_key_api}

Per visualizzare informazioni dettagliate su una chiave specifica, puoi effettuare una chiamata `GET` al seguente endpoint.

```
https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>
```
{: codeblock}

1. [Richiama le tue credenziali del servizio e di autenticazione per utilizzare le chiavi nel servizio](/docs/services/keymgmt/keyprotect_authentication.html).

2. Richiama l'ID della chiave che desideri gestire o a cui vuoi accedere.

    Il valore ID viene utilizzato per accedere a informazioni dettagliate sulla chiave, come
il materiale della chiave stesso. Puoi richiamare l'ID di una chiave specificata effettuando una richiesta `GET /v2/keys` o accedendo alle tue chiavi nella GUI {{site.data.keyword.keymanagementserviceshort}}.

3. Esegui il seguente comando cURL per ottenere i dettagli sulla tua chiave e il materiale relativo.

    ```cURL
    curl -X GET \
      https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID> \
      -H 'accept: application/vnd.ibm.kms.key+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
    ```
    {: codeblock}

    Sostituisci le variabili nella richiesta di esempio in base alla seguente tabella.

    <table>
      <tr>
        <th>Variabile</th>
        <th>Descrizione</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td>L'abbreviazione della regione, come <code>us-south</code> o <code>eu-gb</code>, che rappresenta l'area geografica in cui si trova la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, consulta <a href="/docs/services/keymgmt/keyprotect_regions.html#endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td>Il tuo token di accesso {{site.data.keyword.cloud_notm}}. Includi il contenuto completo del token <code>IAM</code>, compreso il valore Bearer, nella richiesta cURL. Per ulteriori informazioni, vedi <a href="/docs/services/keymgmt/keyprotect_authentication.html#retrieve_token">Richiamo di un token di accesso</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td>L'identificativo univoco che viene assegnato alla tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/keymgmt/keyprotect_authentication.html#retrieve_instance_ID">Retrieving an instance ID</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>Facoltativo: l'identificativo univoco utilizzato per tracciare e correlare le transazioni.</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td>L'identificativo della chiave che hai richiamato nel [passo 1](#retrieve_keys_api).</td>
      </tr>
      <caption style="caption-side:bottom;">Tabella 4. Descrive le variabili necessarie per visualizzare una chiave specificata con l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Una risposta `GET v2/keys/<key_ID>` con esito positivo restituisce i dettagli sulla tua chiave e il materiale della chiave. Il seguente oggetto JSON mostra un valore restituito di esempio per una chiave standard.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.key+json"
        },
    "resources": [
      {
            "id": "...",
            "type": "application/vnd.ibm.kms.key+json",
            "name": "Standard key",
            "description": "...",
            "state": 1,
            "crn": "...",
            "algorithmType": "AES",
            "payload": "...",
            "createdBy": "...",
            "creationDate": "YYYY-MM-DDTHH:MM:SSZ",
            "lastUpdateDate": "YYYY-MM-DDTHH:MM:SSZ",
            "algorithmMetadata": {
                "bitLength": "256",
                "mode": "GCM"
            },
            "extractable": true
        }
      ]
    }
    ```
    {:screen}

    Per una descrizione dettagliata dei parametri disponibili, consulta
{{site.data.keyword.keymanagementserviceshort}} [REST API reference doc ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/apidocs/639){: new_window}.
