---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: delete key, delete key API examples

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

# Eliminazione delle chiavi
{: #delete-keys}

Puoi utilizzare {{site.data.keyword.keymanagementservicefull}} per eliminare una chiave crittografata e il suo contenuto, se sei un amministratore del tuo spazio {{site.data.keyword.cloud_notm}} o dell'istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

Quando elimini una chiave, distruggi permanentemente il suo contenuto e i dati associati. L'azione è irreversibile. [Eliminare in modo permanente le risorse](/docs/services/key-protect?topic=key-protect-security-and-compliance#data-deletion) non è consigliato negli ambienti di produzione, ma può essere utile negli ambienti temporanei come test o QA.
{: important}

## Eliminazione delle chiavi con la GUI
{: #delete-key-gui}

Se preferisci eliminare le tue chiavi di crittografia con un'interfaccia grafica, puoi utilizzare la GUI {{site.data.keyword.keymanagementserviceshort}}.

[Dopo aver creato o importato le tue chiavi esistenti nel servizio](/docs/services/key-protect?topic=key-protect-create-root-keys), completa la seguente procedura per eliminare una chiave:

1. [Accedi alla console {{site.data.keyword.cloud_notm}}](https://{DomainName}/){: external}.
2. Vai a **Menu** &gt; **Resource List** per visualizzare un elenco delle tue risorse.
3. Dal tuo elenco risorse {{site.data.keyword.cloud_notm}}, seleziona la tua istanza di cui è stato eseguito il provisioning di {{site.data.keyword.keymanagementserviceshort}}.
4. Nella pagina dei dettagli dell'applicazione, utilizza la tabella **Keys** per sfogliare le chiavi nel tuo servizio.
5. Fai clic sull'icona ⋯ per aprire un elenco di opzioni per la chiave che desideri eliminare.
6. Dal menu di opzioni, fai clic su **Delete key** e conferma l'eliminazione della chiave nella schermata successiva.

Dopo aver eliminato una chiave, la chiave passa allo stato _Destroyed_. Le chiavi in questo stato non sono più ripristinabili. I metadati associati alla chiave, come la data di eliminazione della chiave, vengono conservati nel database {{site.data.keyword.keymanagementserviceshort}}.

## Eliminazione delle chiavi con l'API
{: #delete-key-api}

Per eliminare una chiave e il suo contenuto, effettua una chiamata `DELETE` al seguente endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>
```

1. [Richiama le tue credenziali del servizio e di autenticazione per utilizzare le chiavi nel servizio](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Richiama l'ID della chiave che desideri eliminare.

    Puoi richiamare l'ID di una chiave specificata effettuando una richiesta `GET /v2/keys/` o visualizzando le tue chiavi nel dashboard {{site.data.keyword.keymanagementserviceshort}}.

3. Esegui il seguente comando cURL per eliminare permanentemente la chiave e il suo contenuto.

    ```cURL
    curl -X DELETE \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID> \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'prefer: <return_preference>'
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
        <td><strong>Obbligatorio</strong> L'abbreviazione della regione, come <code>us-south</code> o <code>eu-gb</code>, che rappresenta l'area geografica in cui si trova la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori informazioni, vedi <a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">Endpoint di servizio regionali</a>.</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Obbligatorio</strong> L'identificativo univoco della chiave che desideri eliminare.</td>
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
        <td><varname>return_preference</varname></td>
        <td><p>Un'intestazione che modifica il comportamento del server per le operazioni <code>POST</code> e <code>DELETE</code>.</p><p>Quando imposti la variabile <em>return_preference</em> su <code>return=minimal</code>, il servizio restituisce una risposta di eliminazione positiva. Quando imposti la variabile su <code>return=representation</code>, il servizio restituisce sia i metadati che il materiale della chiave.</p></td>
      </tr>
      <caption style="caption-side:bottom;">Tabella 1. Descrive le variabili necessarie per eliminare le chiavi con l'API {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    Se la variabile _return_preference_ è impostata su `return=representation`, i dettagli della richiesta `DELETE` vengono restituiti nel corpo-entità della risposta. Il seguente oggetto JSON mostra un valore restituito di esempio.
    ```
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
       "collectionTotal": 1
      },
    "resources": [
      {
          "id": "...",
          "type": "application/vnd.ibm.kms.key+json",
          "name": "...",
          "description": "...",
          "state": 5,
          "crn": "...",
          "deleted": true,
          "algorithmType": "AES",
          "createdBy": "...",
          "deletedBy": "...",
          "creationDate": "YYYY-MM-DDTHH:MM:SS.SSZ",
          "deletionDate": "YYYY-MM-DDTHH:MM:SS.SSZ",
          "lastUpdateDate": "YYYY-MM-DDTHH:MM:SS.SSZ",
          "nonactiveStateReason": 2,
          "extractable": true
        }
      ]
    }
    ```
    {: screen}

    Per una descrizione dettagliata dei parametri disponibili, vedi la [documentazione di riferimento API REST](https://{DomainName}/apidocs/key-protect){: external} di {{site.data.keyword.keymanagementserviceshort}}.
