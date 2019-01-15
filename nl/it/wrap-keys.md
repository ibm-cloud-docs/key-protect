---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Impacchettamento delle chiavi
{: #wrap-keys}

Puoi gestire e proteggere le tue chiavi di crittografia con una chiave root utilizzando l'API {{site.data.keyword.keymanagementservicelong}}, se sei un utente privilegiato.
{: shortdesc}

Quando impacchetti una chiave di crittografia dei dati (o DEK, data encryption key) con una chiave root, {{site.data.keyword.keymanagementserviceshort}} combina la forza di più algoritmi per proteggere la riservatezza e l'integrità dei tuoi dati crittografati.  

Per ulteriori informazioni su come l'impacchettamento ti aiuta a controllare la sicurezza dei dati inattivi nel cloud, consulta [Crittografia envelope](/docs/services/key-protect/concepts/envelope-encryption.html).

## Impacchettamento delle chiavi utilizzando l'API
{: #api}

Puoi proteggere una chiave di crittografia dei dati (o DEK, data encryption key) specificata con una chiave root che gestisci in {{site.data.keyword.keymanagementserviceshort}}.

Quando fornisci una chiave root per l'impacchettamento, assicurati che la chiave root sia di 256, 384 o 512 bit in modo che la chiamata di impacchettamento possa avere esito positivo. Se crei una chiave root nel servizio, {{site.data.keyword.keymanagementserviceshort}} genera una chiave di 256-bit dai suoi HSM, supportata dall'algoritmo AES-GCM.
{: note}

[Dopo aver designato una chiave root nel servizio](/docs/services/key-protect/create-root-keys.html), puoi impacchettare una DEK con la crittografia avanzata effettuando una chiamata `POST` al seguente endpoint.

```
https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=wrap
```
{: codeblock}

1. [Richiama le tue credenziali del servizio e di autenticazione per utilizzare le chiavi nel servizio.](/docs/services/key-protect/access-api.html)

2. Copia il materiale della chiave della DEK che vuoi gestire e proteggere.

    Se disponi dei privilegi da scrittore o gestore per la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}, [puoi richiamare il materiale della chiave per una chiave specifica effettuando una richiesta `GET /v2/keys/<key_ID>`](/docs/services/key-protect/view-keys.html#api).

3. Copia l'ID della chiave root che vuoi utilizzare per l'impacchettamento.

4. Esegui il seguente comando cURL per proteggere la chiave con un'operazione di impacchettamento.

    ```cURL
    curl -X POST \
      'https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=wrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "plaintext": "<data_key>",
      "aad": ["<additional_data>", "<additional_data>"]
    }'
    ```
    {: codeblock}

    Per utilizzare le chiavi in un'organizzazione o uno spazio Cloud Foundry nel tuo account, sostituisci `Bluemix-Instance` con le intestazioni `Bluemix-org` e `Bluemix-space` appropriate. [Per ulteriori informazioni, vedi la documentazione di riferimento API di {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/key-protect){: new_window}.
    {: tip}

    Sostituisci le variabili nella richiesta di esempio in base alla seguente tabella.

    <table>
      <tr>
        <th>Variabile</th>
        <th>Descrizione</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Obbligatorio</strong> L'abbreviazione della regione, come <code>us-south</code> o <code>eu-gb</code>, che rappresenta l'area geografica in cui si trova la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect/regions.html#endpoints">Endpoint di servizio regionali</a>.</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Obbligatorio</strong> L'identificativo univoco della chiave root che vuoi utilizzare per l'impacchettamento.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Obbligatorio</strong> Il tuo token di accesso {{site.data.keyword.cloud_notm}}. Includi il contenuto completo del token <code>IAM</code>, compreso il valore Bearer, nella richiesta cURL. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect/access-api.html#retrieve-token">Richiamo di un token di accesso</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Obbligatorio</strong> L'identificativo univoco che viene assegnato alla tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect/access-api.html#retrieve-instance-ID">Richiamo di un'ID istanza</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>L'identificativo univoco utilizzato per tracciare e correlare le transazioni.</td>
      </tr>
      <tr>
        <td><varname>data_key</varname></td>
        <td>Il materiale della chiave della DEK che vuoi gestire e proteggere. Il valore <code>plaintext</code> deve essere codificato con base64. Per generare una nuova DEK, ometti l'attributo <code>plaintext</code>. Il servizio genera un testo non crittografato casuale (32 byte), impacchetta il valore e restituisce sia il valore generato sia quello impacchettato nella risposta.</td>
      </tr>
      <tr>
        <td><varname>additional_data</varname></td>
        <td>Gli ulteriori dati di autenticazione (o AAD, additional authentication data) che vengono utilizzati per proteggere ulteriormente la chiave. Ogni stringa può contenere fino a 255 caratteri. Se fornisci AAD quando effettui una chiamata di impacchettamento, devi specificare lo stesso AAD durante la seguente chiamata di spacchettamento.<br></br>Importante: il servizio {{site.data.keyword.keymanagementserviceshort}} non salva i dati di autenticazione aggiuntivi. Se fornisci un AAD, salva i dati in un luogo sicuro per assicurarti di poter accedere e fornire lo stesso AAD durante le seguenti richieste di spacchettamento.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabella 1. Descrive le variabili necessarie per impacchettare una chiave specificata in {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    La tua chiave di crittografia dei dati impacchettata (o WDEK, wrapped data encryption key), contenente il materiale codificato con base64, viene restituita nel corpo-entità della risposta. Il seguente oggetto JSON mostra un valore restituito di esempio.

    ```
    {
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}
    
    Se ometti l'attributo `plaintext` quando esegui la richiesta di impacchettamento, il servizio restituisce sia la chiave di crittografia dei dati (o DEK, data encryption key) generata sia la DEK impacchettata in formato con codifica base64.

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg==",
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}

    Il valore <code>plaintext</code> rappresenta la DEK spacchettata e il valore <code>ciphertext</code> rappresenta la DEK impacchettata.
    
    Se desideri che {{site.data.keyword.keymanagementserviceshort}} generi una nuova DEK per tuo conto, puoi anche passare un corpo che non contiene alcun dato in una richiesta di impacchettamento. La tua DEK generata, contenente il materiale con codifica base64, viene restituita nel corpo-entità della risposta, insieme alla DEK impacchettata.
    {: tip}
    
