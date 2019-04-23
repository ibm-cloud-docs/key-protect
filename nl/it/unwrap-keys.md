---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-13"

keywords: unwrap key, decrypt key, decrypt data encryption key, access data encryption key, envelope encryption API examples

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

# Spacchettamento delle chiavi
{: #unwrap-keys}

Puoi spacchettare una chiave di crittografia dei dati (o DEK, data encryption key) per accedere ai suoi contenuti utilizzando l'API {{site.data.keyword.keymanagementservicefull}}, se sei un utente privilegiato. Lo spacchettamento di una DEK decodifica e controlla l'integrità del suo contenuto, restituendo il materiale della chiave di origine al tuo servizio di dati {{site.data.keyword.cloud_notm}}.
{: shortdesc}

Per ulteriori informazioni su come l'impacchettamento ti aiuta a controllare la sicurezza dei dati inattivi nel cloud, consulta [Protezione dei dati con la crittografia envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption).

## Spacchettamento delle chiavi utilizzando l'API
{: #unwrap-key-api}

[Dopo aver effettuato una chiamata di impacchettamento al servizio](/docs/services/key-protect?topic=key-protect-wrap-keys), puoi spacchettare una chiave di crittografia dei dati (o DEK, data encryption key) specificata per accedere al suo contenuto eseguendo una chiamata `POST` al seguente endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>?action=unwrap
```
{: codeblock}

1. [Richiama le tue credenziali del servizio e di autenticazione per utilizzare le chiavi nel servizio](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Copia l'ID della chiave root che hai utilizzato per eseguire la richiesta di impacchettamento iniziale.

    Puoi richiamare l'ID di una chiave effettuando una richiesta `GET /v2/keys` o visualizzando le tue chiavi nella GUI {{site.data.keyword.keymanagementserviceshort}}.

3. Copia il valore `ciphertext` restituito durante la richiesta di impacchettamento iniziale.

4. Esegui il seguente comando cURL per decodificare e autenticare il materiale della chiave.

    ```cURL
    curl -X POST \
      'https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=unwrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "ciphertext": "<encrypted_data_key>",
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
        <td><strong>Obbligatorio</strong> L'abbreviazione della regione, come <code>us-south</code> o <code>eu-gb</code>, che rappresenta l'area geografica in cui si trova la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Endpoint di servizio regionali</a>.</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Obbligatorio</strong> L'identificativo univoco della chiave root che hai utilizzato per la richiesta di impacchettamento iniziale.</td>
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
        <td><varname>additional_data</varname></td>
        <td>Gli ulteriori dati di autenticazione (o AAD, additional authentication data) che vengono utilizzati per proteggere ulteriormente la chiave. Ogni stringa può contenere fino a 255 caratteri. Se fornisci AAD quando effettui una chiamata di impacchettamento, devi specificare lo stesso AAD durante la chiamata di spacchettamento.</td>
      </tr>
      <tr>
        <td><varname>encrypted_data_key</varname></td>
        <td><strong>Obbligatorio</strong> Il valore <code>ciphertext</code> restituito durante un'operazione di impacchettamento.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabella 1. Descrive le variabili necessarie per spacchettare le chiavi in {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    Il materiale della chiave originale viene restituito nel corpo-entità della risposta. Il seguente oggetto JSON mostra un valore restituito di esempio.

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg=="
    }
    ```
    {:screen}
