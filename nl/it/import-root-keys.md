---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: import root key, upload root key, import key-wrapping key, upload key-wrapping key, import CRK, import CMK, upload CRK, upload CMK, import customer key, upload customer key, key-wrapping key, root key API examples

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

# Importazione delle chiavi root
{: #import-root-keys}

Puoi utilizzare {{site.data.keyword.keymanagementservicefull}} per proteggere le chiavi root utilizzando la GUI {{site.data.keyword.keymanagementserviceshort}}, o in modo programmatico con l'API {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

Le chiavi root sono chiavi simmetriche per l'impacchettamento della chiave che vengono utilizzate per proteggere la sicurezza dei dati crittografati nel cloud. Per ulteriori informazioni sull'importazione delle chiavi root in {{site.data.keyword.keymanagementserviceshort}}, vedi [Utilizzo delle tue chiavi di crittografia sul cloud](/docs/services/key-protect?topic=key-protect-importing-keys).

## Importazione delle chiavi root con la GUI
{: #import-root-key-gui}

[Dopo aver creato un'istanza del servizio](/docs/services/key-protect?topic=key-protect-provision), completa la seguente procedura per aggiungere una chiave root con la GUI {{site.data.keyword.keymanagementserviceshort}}.

1. [Accedi alla console {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/){: new_window}.
2. Vai a **Menu** &gt; **Resource List** per visualizzare un elenco delle tue risorse.
3. Dal tuo elenco risorse {{site.data.keyword.cloud_notm}}, seleziona la tua istanza di cui è stato eseguito il provisioning di {{site.data.keyword.keymanagementserviceshort}}.
4. Per importare una chiave, fai clic su **Add key** e seleziona la finestra **Import your own key**.

    Specifica i dettagli della chiave:

    <table>
      <tr>
        <th>Impostazione</th>
        <th>Descrizione</th>
      </tr>
      <tr>
        <td>Nome</td>
        <td>
          <p>Un alias univoco e leggibile dall'utente per una facile identificazione della tua chiave.</p>
          <p>Per proteggere la tua privacy, assicurati che il nome della chiave non contenga informazioni d'identificazione personale, come il tuo nome o la tua posizione.</p>
        </td>
      </tr>
      <tr>
        <td>Tipo di chiave</td>
        <td>Il <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">tipo di chiave</a> che desideri gestire in {{site.data.keyword.keymanagementserviceshort}}. Dall'elenco dei tipi di chiave, seleziona <b>Root key</b>.</td>
      </tr>
      <tr>
        <td>Materiale della chiave</td>
        <td>
          <p>Il materiale della chiave codificato con base64, come ad esempio una chiave di impacchettamento della chiave esistente, che desideri archiviare e gestire nel servizio.</p>
          <p>Assicurati che il materiale della chiave soddisfi i seguenti requisiti:</p>
          <p>
            <ul>
              <li>La chiave deve essere di 128, 192 o 256 bit.</li>
              <li>I byte di dati, ad esempio 32 byte per 256 bit, devo essere codificati utilizzando la codifica base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Tabella 1. Descrive le impostazioni <b>Import your own key</b></caption>
    </table>

5. Una volta che hai finito di compilare i dettagli della chiave, fai clic su **Import key** per confermare. 

## Importazione delle chiavi root con l'API
{: #import-root-key-api}

Puoi importare le tue chiavi root nel servizio utilizzando l'API {{site.data.keyword.keymanagementserviceshort}}.

Pianifica in anticipo l'importazione delle chiavi [controllando le tue opzioni per la creazione e la crittografia del materiale della chiave](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead). Per aggiungere sicurezza, puoi abilitare l'importazione sicura del materiale della chiave utilizzando una [chiave di trasporto](/docs/services/key-protect?topic=key-protect-importing-keys#transport-keys) per crittografare il tuo materiale della chiave prima di portarlo nel cloud. Se preferisci importare una chiave root senza utilizzare una chiave di trasporto, vai al [passo 4](#import-root-key).
{: note}

### Passo 1: crea una chiave di trasporto
{: #create-transport-key}

Le chiavi di trasporto sono al momento una funzione beta. Le funzioni beta possono venire modificate in qualsiasi momento e aggiornamenti futuri potrebbero introdurre delle modifiche incompatibili con la versione più recente.
{: important}

Crea una chiave di trasporto per la tua istanza del servizio effettuando una chiamata `POST` al seguente endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers
```
{: codeblock}

1. [Richiama le tue credenziali del servizio e di autenticazione per utilizzare le chiavi nel servizio](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Configura una politica per la tua chiave di trasporto richiamando l'[API {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/key-protect){: new_window}.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/json' \
      -d '{
     "expiration": <expiration_time>,  \
     "maxAllowedRetrievals": <use_count>  \
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
      <td><varname>expiration_time</varname></td>
      <td>
        <p>Il tempo in secondi dalla creazione di una chiave di trasporto che determina per quanto tempo la chiave rimane valida.</p>
        <p>Il valore minimo è 300 secondi (5 minuti) e il valore massimo è 86400 (24 ore). Il valore predefinito è 600 (10 minuti).</p>
      </td>
    </tr>
    <tr>
      <td><varname>use_count</varname></td>
      <td>Il numero di volte in cui una chiave di trasporto può essere richiamata entro la sua data di scadenza prima che non sia più accessibile. Il valore predefinito è 1.</td>
    </tr>
      <caption style="caption-side:bottom;">Tabella 2. Descrive le variabili necessarie per creare una chiave di trasporto con l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
  </table>

  Una risposta `POST api/v2/lockers` corretta restituisce il valore dell'ID per la tua chiave di trasporto, insieme ad altri metadati. L'ID è un identificativo univoco associato a una chiave di trasporto e viene utilizzato per le successive chiamate all'API {{site.data.keyword.keymanagementserviceshort}}.

### Passo 2: richiama la chiave di trasporto e il token di importazione
{: #retrieve-transport-key}

Richiama una chiave di trasporto e un token di importazione effettuando una chiamata `GET` al seguente endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers/<key_id>
```
{: codeblock}

1. Richiama l'[API {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/key-protect){: new_window} con il seguente comando cURL.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/locker/<locker_id> \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json'
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
        <td><varname>locker_ID</varname></td>
        <td><strong>Obbligatorio</strong> L'identificativo della chiave di trasporto che hai creato nel <a href="#create-transport-key">passo 1</a>.</td>
      </tr>
        <caption style="caption-side:bottom;">Tabella 3. Descrive le variabili necessarie per richiamare una chiave di trasporto con l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Una risposta `GET api/v2/lockers/{id}` con esito positivo restituisce una chiave di crittografia pubblica codificata DER di 4096 bit nel formato PKIX che puoi utilizzare per crittografare il tuo materiale della chiave root, insieme a un token di importazione utilizzato per verificare l'integrità della chiave di trasporto.

### Passo 3: crittografa il tuo materiale della chiave
{: #encrypt-root-key}

Dopo aver richiamato la tua chiave di trasporto, utilizzala per crittografare il materiale della chiave che vuoi importare in {{site.data.keyword.keymanagementserviceshort}}.  

<!-- TODO: Add link to tutorial that uses OpenSSL for key generation and encryption (in progress)-->

Per generare del materiale della chiave in loco, [controlla le tue opzioni per la creazione delle chiavi di crittografia simmetriche](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead). Ad esempio, potresti voler utilizzare il sistema di gestione delle chiavi interno della tua organizzazione, supportato da un HSM (hardware security module) in loco, per creare ed esportare il materiale della chiave.
{: note}

Per crittografare il tuo materiale della chiave:

1. Esporta il materiale della chiave a 256 bit nel formato binario dal tuo sistema di gestione delle chiavi in loco.

    Per ulteriori informazioni su come creare ed esportare il materiale della chiave, vedi la documentazione per il tuo HSM o il tuo sistema di gestione delle chiavi in loco.

2. Utilizza la [chiave di trasporto richiamata](#retrieve-transport-key) dal passo 2 per crittografare il materiale della chiave.

   Quando crittografi il tuo materiale della chiave, utilizza lo schema di crittografia `RSAES_OAEP_SHA_256`. Questo è lo schema predefinito che {{site.data.keyword.keymanagementserviceshort}} utilizza per creare la chiave di trasporto. Per evitare problemi di decrittografia in {{site.data.keyword.keymanagementserviceshort}}, non includere il parametro `label` facoltativo quando esegui la crittografia RSAES_OAEP sul materiale della chiave. Per ulteriori informazioni su come eseguire la crittografia RSA sul tuo materiale della chiave, vedi la documentazione per il tuo HSM o il tuo sistema di gestione delle chiavi in loco.

3. Assicurati che il materiale della chiave crittografato sia codificato base64 prima di continuare con il passo successivo.

### Passo 4: importa il materiale della chiave
{: #import-root-key}

[Dopo aver crittografato e codificato base64 il materiale della chiave](#encrypt-root-key), importa la chiave root in {{site.data.keyword.keymanagementserviceshort}} effettuando una chiamata `POST` al seguente endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. Richiama l'[API {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/key-protect){: new_window} con il seguente comando cURL.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.key+json",
       "collectionTotal": 1
     },
    "resources": [
      {
       "type": "application/vnd.ibm.kms.key+json",
       "name": "<key_alias>",
       "description": "<key_description>",
       "expirationDate": "<YYYY-MM-DDTHH:MM:SS.SSZ>",
       "payload": "<key_material>",
       "extractable": <key_type>,
       "encryptionAlgorithm": "<encryption_algorithm>",
       "importToken": "<import_token>"
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
        <td><varname>key_alias</varname></td>
        <td><strong>Obbligatorio</strong> Un nome leggibile dall'utente e univoco per una facile identificazione della tua chiave. Per proteggere la tua privacy, non memorizzare i tuoi dati personali come metadati per la tua chiave.</td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>Una descrizione estesa della tua chiave. Per proteggere la tua privacy, non memorizzare i tuoi dati personali come metadati per la tua chiave.</td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>La data e l'ora di scadenza della chiave nel sistema, nel formato RFC 3339. Se l'attributo <code>expirationDate</code> viene omesso, la chiave non avrà scadenza.</td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>Il materiale della chiave codificato con base64, come ad esempio una chiave di impacchettamento della chiave esistente, che desideri archiviare e gestire nel servizio.</p>
          <p>Assicurati che il materiale della chiave soddisfi i seguenti requisiti:</p>
          <p>
            <ul>
              <li>La chiave deve essere di 128, 192 o 256 bit.</li>
              <li>I byte di dati, ad esempio 32 byte per 256 bit, devo essere codificati utilizzando la codifica base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>Un valore booleano che determina se il materiale della chiave può lasciare il servizio.</p>
          <p>Quando imposti l'attributo <code>extractable</code> su <code>false</code>, il servizio designa una chiave root che puoi utilizzare per le operazioni <code>wrap</code> o <code>unwrap</code>.</p>
        </td>
      </tr>
      <tr>
        <td><varname>encryption_algorithm</varname></td>
        <td>Lo schema di crittografia che hai utilizzato per <a href="#encrypt-root-key">crittografare il materiale della chiave</a>. Al momento, è supportato <code>RSAES_OAEP_SHA_256</code>. Per importare il materiale della chiave root senza utilizzare una chiave di trasporto e un token di importazione, ometti l'attributo <code>encryption_algorithm</code>.</td>
      </tr>
      <tr>
        <td><varname>import_token</varname></td>
        <td>Il token di importazione utilizzato per verificare lo stato di attività e l'integrità di una chiave di trasporto. Se crittografi il tuo materiale della chiave utilizzando una chiave di trasporto, devi fornire lo stesso token di importazione che hai richiamato nel <a href="#retrieve-transport-key">passo 2</a>. Per importare il materiale della chiave root senza utilizzare una chiave di trasporto e un token di importazione, ometti l'attributo <code>importToken</code>.</td>
      </tr>
        <caption style="caption-side:bottom;">Tabella 4. Descrive le variabili necessarie per aggiungere una chiave root con l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Per proteggere la riservatezza dei tuoi dati personali, evita di immettere informazioni d'identificazione personale, come il tuo nome o la tua posizione, quando aggiungi le chiavi al servizio. Per ulteriori esempi di informazioni d'identificazione personale, vedi la sezione 2.2 di [NIST Special Publication 800-122 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: new_window}.
    {: important}

    Una risposta `POST api/v2/keys` corretta restituisce il valore dell'ID per la tua chiave, insieme ad altri metadati. L'ID è un identificativo univoco che viene assegnato alla tua chiave e utilizzato per le seguenti chiamate all'API {{site.data.keyword.keymanagementserviceshort}}.

2. Facoltativo: verifica che la chiave sia stata aggiunta eseguendo la seguente chiamata per sfogliare le chiavi nella tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## Operazioni successive
{: #import-root-key-next-steps}

- Per ulteriori informazioni sulla protezione delle chiavi con la crittografia envelope, controlla [Impacchettamento delle chiavi](/docs/services/key-protect?topic=key-protect-wrap-keys).
- Per ulteriori informazioni sulla gestione a livello programmatico delle tue chiavi, [consulta la documentazione di riferimento API {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/key-protect){: new_window}.
