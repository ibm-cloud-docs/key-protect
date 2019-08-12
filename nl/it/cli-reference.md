---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: Key Protect CLI plug-in, CLI reference

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

# Guida di riferimento alla CLI {{site.data.keyword.keymanagementserviceshort}}
{: #cli-reference}

Puoi utilizzare il plugin della CLI {{site.data.keyword.keymanagementserviceshort}} per gestire le chiavi nella tua istanza di {{site.data.keyword.keymanagementserviceshort}}.
{:shortdesc}

Per installare il plugin della CLI, vedi [Configurazione della CLI](/docs/services/key-protect?topic=key-protect-set-up-cli). 

Quando accedi alla [CLI {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external}, ricevi una notifica quando sono disponibili degli aggiornamenti. Assicurati di tenere aggiornata la tua CLI in modo da poter usare i comandi e gli indicatori disponibili per il plugin della CLI {{site.data.keyword.keymanagementserviceshort}}.
{: tip}

## Comandi ibmcloud kp
{: #ibmcloud-kp-commands}

Puoi specificare uno dei seguenti comandi:

<table summary="Comandi per la gestione delle chiavi"> 
    <thead>
        <th colspan="5">Comandi per la gestione delle chiavi</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-create">kp create</a></td>
            <td><a href="#kp-delete">kp delete</a></td>
            <td><a href="#kp-list">kp list</a></td>
            <td><a href="#kp-get">kp get</a></td>
            <td><a href="#kp-rotate">kp rotate</a></td>
        </tr>
        <tr>
            <td><a href="#kp-unwrap">kp unwrap</a></td>
            <td><a href="#kp-wrap">kp wrap</a></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">Tabella 1. Comandi per la gestione delle chiavi</caption> 
 </table>

 <table summary="Comandi per la gestione delle politiche delle chiavi"> 
    <thead>
        <th colspan="5">Comandi per la gestione delle politiche delle chiavi</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-policy-list">kp policy list</a></td>
            <td><a href="#kp-policy-get">kp policy get</a></td>
            <td><a href="#kp-policy-set">kp policy set</a></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">Tabella 2. Comandi per la gestione delle politiche delle chiavi</caption> 
 </table>

## kp create
{: #kp-create}

[Crea una chiave root](/docs/services/key-protect?topic=key-protect-create-root-keys) nell'istanza del servizio {{site.data.keyword.keymanagementserviceshort}} che specifichi. 

```
ibmcloud kp create KEY_NAME -i INSTANCE_ID | $INSTANCE_ID
                   [-k, --key-material KEY_MATERIAL] 
                   [-s, --standard-key]
                   [--output FORMAT]
```
{:pre}

```sh
$ ibmcloud kp create sample-root-key -i $KP_INSTANCE_ID
SUCCESS

Key ID                                 Key Name
3df42bc2-a991-41cb-acc2-3f9eab64a63f   sample-root-key
```
{:screen}

### Parametri obbligatori
{: #create-req-params}

<dl>
    <dt><code>KEY_NAME</code></dt>
        <dd>Un alias leggibile dall'utente e univoco da assegnare alla tua chiave.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>L'ID istanza {{site.data.keyword.cloud_notm}} che identifica la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parametri facoltativi
{: #create-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>Il materiale della chiave con codifica base64 che vuoi archiviare e gestire nel servizio. Per importare una chiave esistente, fornisci una chiave a 256 bit. Per generare una nuova chiave, ometti il parametro <code>--key-material</code>.</dd>
    <dt><code>-s, --standard-key</code></dt>
        <dd>Imposta il parametro solo se vuoi creare una <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">chiave standard</a>. Per creare una chiave root, ometti il parametro <code>--standard-key</code>.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Imposta il formato di output della CLI. Per impostazione predefinita, tutti i comandi vengono restituiti nel formato tabella. Per modificare il formato di output con JSON, utilizza <code>--output json</code>.</dd>
</dl>

## kp delete
{: #kp-delete}

[Elimina una chiave](/docs/services/key-protect?topic=key-protect-delete-keys) archiviata nel tuo servizio {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp delete KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

```sh
$ ibmcloud kp delete 584fb0d9-dec2-47b8-bde5-50f05fd66261 -i $KP_INSTANCE_ID
Deleting key: 584fb0d9-dec2-47b8-bde5-50f05fd66261, from instance: 98d39ab8-cf44-4517-9583-2ad05c7e9bd5...

SUCCESS

Deleted Key
584fb0d9-dec2-47b8-bde5-50f05fd66261
```
{: screen}

### Parametri obbligatori
{: #delete-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
   <dd>L'ID della chiave che vuoi eliminare. Per richiamare un elenco delle tue chiavi disponibili, esegui il comando <a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>L'ID istanza {{site.data.keyword.cloud_notm}} che identifica la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

## kp list
{: #kp-list}

Elenca le ultime 200 chiavi disponibili nella tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp list -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

```sh
$ ibmcloud kp list -i $KP_INSTANCE_ID
Retrieving keys...

SUCCESS

Key ID                                 Key Name
3df42bc2-a991-41cb-acc2-3f9eab64a63f   sample-root-key
92e5fab3-00e8-40e9-8a2d-864de334b043   sample-imported-root-key
```
{: screen}

### Parametri obbligatori
{: #list-req-params}

<dl>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>L'ID istanza {{site.data.keyword.cloud_notm}} che identifica la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parametri facoltativi
{: #list-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Imposta il formato di output della CLI. Per impostazione predefinita, tutti i comandi vengono restituiti nel formato tabella. Per modificare il formato di output con JSON, utilizza <code>--output json</code>.</dd>
</dl>

## kp get
{: #kp-get}

Richiama i dettagli su una chiave, come ad esempio i metadati o il materiale della chiave.

Se la chiave è stata progettata come una chiave root, il sistema non può restituire il materiale della chiave per tale chiave.

```
ibmcloud kp get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

```sh
$ ibmcloud kp get 3df42bc2-a991-41cb-acc2-3f9eab64a63f -i $KP_INSTANCE_ID
Grabbing info for key id: 3df42bc2-a991-41cb-acc2-3f9eab64a63f...

SUCCESS

Key ID                                 Key Name          Description     Creation Date                   Expiration Date
3df42bc2-a991-41cb-acc2-3f9eab64a63f   sample-root-key   A sample key.   2019-04-02 16:42:47 +0000 UTC   Key does not expire
```
{:screen}

### Parametri obbligatori
{: #get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>L'ID della chiave che vuoi richiamare. Per richiamare un elenco delle tue chiavi disponibili, esegui il comando <a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>L'ID istanza {{site.data.keyword.cloud_notm}} che identifica la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parametri facoltativi
{: #get-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Imposta il formato di output della CLI. Per impostazione predefinita, tutti i comandi vengono restituiti nel formato tabella. Per modificare il formato di output con JSON, utilizza <code>--output json</code>.</dd>
</dl>

## kp rotate
{: #kp-rotate}

[Esegui la rotazione di una chiave root](/docs/services/key-protect?topic=key-protect-wrap-keys) archiviata nel tuo servizio {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp rotate KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-k, --key-material KEY_MATERIAL] 
```
{: pre}

```sh
$ ibmcloud kp rotate 3df42bc2-a991-41cb-acc2-3f9eab64a63f -i $KP_INSTANCE_ID
Rotating root key...

SUCCESS
```
{:screen}

### Parametri obbligatori
{: #rotate-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>L'ID della chiave root di cui vuoi eseguire la rotazione.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>L'ID istanza {{site.data.keyword.cloud_notm}} che identifica la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parametri facoltativi
{: #rotate-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>Il materiale della chiave con codifica base64 che vuoi usare per eseguire la rotazione di una chiave root esistente. Per eseguire la rotazione di una chiave che era stata inizialmente importata nel servizio, fornisci una nuova chiave da 256 bit. Per eseguire la rotazione di una chiave che era stata inizialmente generata in {{site.data.keyword.keymanagementserviceshort}}, ometti il parametro <code>--key-material</code>.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Imposta il formato di output della CLI. Per impostazione predefinita, tutti i comandi vengono restituiti nel formato tabella. Per modificare il formato di output con JSON, utilizza <code>--output json</code>.</dd>
</dl>

## kp wrap
{: #kp-wrap}

[Impacchetta una chiave di crittografia dei dati (o DEK, data encryption key)](/docs/services/key-protect?topic=key-protect-wrap-keys) utilizzando una chiave root archiviata nell'istanza del servizio {{site.data.keyword.keymanagementserviceshort}} da te specificata.

```
ibmcloud kp wrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --plaintext DATA_KEY] 
                 [-a, --aad ADDITIONAL_DATA]
```
{: pre}


### Parametri obbligatori
{: #wrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>L'ID della chiave root che vuoi utilizzare per l'impacchettamento.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>L'ID istanza {{site.data.keyword.cloud_notm}} che identifica la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parametri facoltativi
{: #wrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd>Gli ulteriori dati di autenticazione (o AAD, additional authentication data) che vengono utilizzati per proteggere ulteriormente una chiave. Se sono stati forniti all'impacchettamento, devono essere forniti allo spacchettamento.</dd>
    <dt><code>-p, --plaintext</code></dt>
        <dd>la DEK (data encryption key) con codifica base64 che vuoi gestire e proteggere. Per importare una chiave esistente, fornisci una chiave a 256 bit. Per generare e impacchettare una nuova DEK, ometti il parametro <code>--plaintext</code>.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Imposta il formato di output della CLI. Per impostazione predefinita, tutti i comandi vengono restituiti nel formato tabella. Per modificare il formato di output con JSON, utilizza <code>--output json</code>.</dd>
</dl>

## kp unwrap
{: #kp-unwrap}

[Spacchetta una DEK (data encryption key)](/docs/services/key-protect?topic=key-protect-unwrap-keys) utilizzando una chiave root archiviata nella tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp unwrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                   CIPHERTEXT_FROM_WRAP
                   [-a, --aad ADDITIONAL_DATA, ..]
```
{: pre}

### Parametri obbligatori
{: #unwrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>L'ID della chiave root che hai utilizzato per la richiesta di impacchettamento iniziale.</dd>
    <dt><code>CIPHERTEXT_FROM_WRAP</code></dt>
        <dd>La chiave di dati crittografata restituita durante l'operazione di impacchettamento iniziale.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>L'ID istanza {{site.data.keyword.cloud_notm}} che identifica la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parametri facoltativi
{: #unwrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd><p>Gli ulteriori dati di autenticazione (o AAD, additional authentication data) che erano stati utilizzati per proteggere ulteriormente una chiave. Puoi fornire fino a 255 stringhe, ciascuna delimitata da una virgola. Se hai fornito gli AAD all'impacchettamento, devi specificare gli stessi AAD allo spacchettamento.</p><p><b>Importante:</b> il servizio {{site.data.keyword.keymanagementserviceshort}} non salva dati di autenticazione aggiuntivi. Se fornisci un AAD, salva i dati in un luogo sicuro per assicurarti di poter accedere e fornire lo stesso AAD durante le seguenti richieste di spacchettamento.</p></dd>
    <dt><code>-o, --output</code></dt>
        <dd>Imposta il formato di output della CLI. Per impostazione predefinita, tutti i comandi vengono restituiti nel formato tabella. Per modificare il formato di output con JSON, utilizza <code>--output json</code>.</dd>
</dl>

## kp policy list
{: #kp-policy-list}

Elenca le politiche associate alla chiave root che hai specificato.

```
ibmcloud kp policy list KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Parametri obbligatori
{: #policy-list-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>L'ID della chiave root di cui vuoi eseguire la query. Per richiamare un elenco delle tue chiavi disponibili, esegui il comando <a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>L'ID istanza {{site.data.keyword.cloud_notm}} che identifica la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parametri facoltativi
{: #policy-list-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Imposta il formato di output della CLI. Per impostazione predefinita, tutti i comandi vengono restituiti nel formato tabella. Per modificare il formato di output con JSON, utilizza <code>--output json</code>.</dd>
</dl>

## kp policy get
{: #kp-policy-get}

Richiama i dettagli su una politica della chiave, ad esempio l'intervallo di rotazione automatica della chiave.

```
ibmcloud kp policy get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Parametri obbligatori
{: #policy-get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>L'ID della chiave di cui vuoi eseguire la query. Per richiamare un elenco delle tue chiavi disponibili, esegui il comando <a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>L'ID istanza {{site.data.keyword.cloud_notm}} che identifica la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parametri facoltativi
{: #policy-get-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Imposta il formato di output della CLI. Per impostazione predefinita, tutti i comandi vengono restituiti nel formato tabella. Per modificare il formato di output con JSON, utilizza <code>--output json</code>.</dd>
</dl>

## kp policy set
{: #kp-policy-set}

Crea o sostituisce la politica associata alla chiave root che hai specificato.

```
ibmcloud kp policy set KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 --set-type POLICY_TYPE 
                 [--policy INTERVAL]
```
{: pre}

### Parametri obbligatori
{: #policy-set-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>L'ID della chiave di cui vuoi eseguire la query. Per richiamare un elenco delle tue chiavi disponibili, esegui il comando <a href="#kp-list">kp list</a>.</dd>
   <dt><code>--set-type</code></dt>
        <dd>Specifica il tipo di politica che vuoi impostare. Per impostare una politica di rotazione, utilizza <code>--set-type rotation</code>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>L'ID istanza {{site.data.keyword.cloud_notm}} che identifica la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parametri facoltativi
{: #policy-set-opt-params}

<dl>
   <dt><code>-p, --policy</code></dt>
        <dd>Specifica l'intervallo di tempo di rotazione (in mesi) per una chiave. Il valore predefinito è 1.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Imposta il formato di output della CLI. Per impostazione predefinita, tutti i comandi vengono restituiti nel formato tabella. Per modificare il formato di output con JSON, utilizza <code>--output json</code>.</dd>
</dl>

