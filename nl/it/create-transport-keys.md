---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-20"

keywords: create transport encryption key, secure import, key-wrapping key, transport key API examples

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

# Creazione di una chiave di trasporto
{: #create-transport-keys}

Puoi abilitare l'importazione sicura del materiale della chiave root nel cloud creando prima una chiave di crittografia di trasporto per la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

Le chiavi di trasporto vengono utilizzate per crittografare e importare in modo sicuro il materiale della chiave root in {{site.data.keyword.keymanagementserviceshort}} in base alle politiche che hai specificato. Per ulteriori informazioni sull'importazione delle tue chiavi in modo sicuro nel cloud, vedi [Utilizzo delle tue chiavi di crittografia sul cloud](/docs/services/key-protect/concepts?topic=key-protect-importing-keys).

Le chiavi di trasporto sono al momento una funzione beta. Le funzioni beta possono venire modificate in qualsiasi momento e aggiornamenti futuri potrebbero introdurre delle modifiche incompatibili con la versione più recente.
{: important}

## Creazione di una chiave di trasporto con l'API
{: #create-transport-key-api}

Crea una chiave di trasporto associata con la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}} effettuando una chiamata `POST` al seguente endpoint.

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
          <td>Il numero di volte in cui una chiave di trasporto può essere richiamata entro la sua data di scadenza prima che non sia più accessibile. Il valore predefinito è 1. </td>
        </tr>
          <caption style="caption-side:bottom;">Tabella 1. Descrive le variabili necessarie per aggiungere una chiave root con l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
      </table>

    Una richiesta `POST api/v2/lockers` con esito positivo, crea una chiave di trasporto per la tua istanza del servizio e ne restituisce il valore ID, insieme ad altri metadati. L'ID è un identificativo univoco associato alla tua chiave di trasporto e viene utilizzato per le successive chiamate all'API {{site.data.keyword.keymanagementserviceshort}}.

3. Facoltativo: verifica che la chiave di trasporto sia stata creata eseguendo la seguente chiamata per richiamare i metadati sulla tua istanza del servizio.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## Operazioni successive
{: #create-transport-key-next-steps}

- Per ulteriori informazioni sull'utilizzo di una chiave di trasporto per importare le chiavi root nel servizio, vedi [Importazione delle chiavi root](/docs/services/key-protect?topic=key-protect-import-root-keys).
- Per ulteriori informazioni sulla gestione a livello programmatico delle tue chiavi, [consulta la documentazione di riferimento API {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/key-protect){: new_window}.
