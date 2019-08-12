---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: import encryption key, upload encryption key, Bring Your Own Key, BYOK, secure import, transport encryption key 

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

# Utilizzo delle tue chiavi di crittografia sul cloud
{: #importing-keys}

Le chiavi di crittografia contengono dei sottoinsiemi di informazioni, come ad esempio i metadati, che ti aiutano a identificare la chiave e il _materiale della chiave_ che viene utilizzato per crittografare e decrittografare i dati.
{: shortdesc}

Quando utilizzi {{site.data.keyword.keymanagementserviceshort}} per creare le chiavi, il servizio genera del materiale della chiave crittografico al tuo posto che viene inserito negli HSM (hardware security module) basati sul cloud. Ma a seconda dei tuoi requisiti di business, potresti dover generare del materiale della chiave dalla tua soluzione interna e poi estenderlo dalla tua infrastruttura di gestione della chiave in loco al cloud importando le chiavi in {{site.data.keyword.keymanagementserviceshort}}.

<table>
  <th>Vantaggio</th>
  <th>Descrizione</th>
  <tr>
    <td>BYOK (Bring your own key) </td>
    <td>Vuoi controllare completamente e rafforzare le tue procedure di gestione della chiave generando chiavi solide dai tuoi HSM (hardware security module) in loco. Se scegli di esportare le chiavi simmetriche dalla tua infrastruttura di gestione della chiave interna, puoi utilizzare {{site.data.keyword.keymanagementserviceshort}} per utilizzarle in modo sicuro nel cloud.</td>
  </tr>
  <tr>
    <td>Importazione sicura del materiale della chiave root</td>
    <td>Quando esporti le tue chiavi nel cloud, vuoi assicurarti che il materiale della chiave sia protetto mentre è in transito. Diminuisci gli attacchi man-in-the-middle <a href="#transport-keys">utilizzando una chiave di trasporto</a> per importare in modo sicuro il materiale della chiave root nella tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.</td>
  </tr>
  <caption style="caption-side:bottom;">Tabella 1. Descrive i vantaggi dell'importazione del materiale della chiave</caption>
</table>


## Pianifica in anticipo l'importazione del materiale della chiave
{: #plan-ahead}

Tieni presenti le seguenti considerazioni quando sei pronto ad importare il materiale della chiave root nel servizio.

<dl>
  <dt>Rivedi le tue opzioni per la creazione del materiale della chiave</dt>
    <dd>Esplora le tue opzioni per la creazione di chiavi di crittografia simmetriche a 256-bit in base alle tue esigenze di sicurezza. Ad esempio, puoi utilizzare il sistema di gestione della chiave interno, supportato dall'HSM (hardware security module) in loco e convalidato da FIPS, per generare il materiale della chiave prima di utilizzare le chiavi nel cloud. Se stai creando un POC (Proof of Concept), puoi anche utilizzare una strumentazione di crittografia come <a href="https://www.openssl.org/" target="_blank">OpenSSL</a> per generare il materiale della chiave che puoi importare in {{site.data.keyword.keymanagementserviceshort}} per le tue esigenze di test.</dd>
  <dt>Scegli un'opzione per l'importazione del materiale della chiave in {{site.data.keyword.keymanagementserviceshort}}</dt>
    <dd>Scegli tra le due opzioni per l'importazione delle chiavi root in base al livello di sicurezza richiesto dal tuo ambiente o carico di lavoro. Per impostazione predefinita, {{site.data.keyword.keymanagementserviceshort}} crittografa il tuo materiale della chiave mentre è in transito utilizzando il protocollo TLS (Transport Layer Security) 1.2. Se stai creando un POC (Proof of Concept) o provando il servizio per la prima volta, puoi importare il materiale della chiave root in {{site.data.keyword.keymanagementserviceshort}} utilizzando questa opzione predefinita. Se il tuo carico di lavoro richiede un meccanismo di sicurezza oltre a TLS, puoi anche <a href="#transport-keys">utilizzare una chiave di trasporto</a> per crittografare e importare il materiale della chiave root nel servizio.</dd>
  <dt>Pianifica in anticipo la crittografia del tuo materiale della chiave</dt>
    <dd>Se scegli di crittografare il tuo materiale della chiave utilizzando una chiave di trasporto, determina un metodo per eseguire la crittografia RSA sul materiale della chiave. Devi utilizzare lo schema di crittografia <code>RSAES_OAEP_SHA_256</code> come specificato dallo <a href="https://tools.ietf.org/html/rfc3447" target="_blank">standard PKCS #1 v2.1 per la crittografia RSA</a>. Rivedi le funzionalità del tuo sistema di gestione della chiave interno o l'HSM in loco per determinare le tue opzioni.</dd>
  <dt>Gestisci il ciclo di vita del materiale della chiave importato.</dt>
    <dd>Dopo aver importato il materiale della chiave nel servizio, tieni presente che sei responsabile della gestione del ciclo di vita completo della tua chiave. Utilizzando l'API {{site.data.keyword.keymanagementserviceshort}}, puoi impostare una data di scadenza per la chiave quando decidi di caricarla nel servizio. Tuttavia, se vuoi <a href="/docs/services/key-protect?topic=key-protect-rotate-keys">ruotare una chiave root importata</a>, devi generare e fornire il nuovo materiale della chiave per ritirare e sostituire la chiave esistente. </dd>
</dl>

## Utilizzo delle chiavi di trasporto
{: #transport-keys}

Le chiavi di trasporto sono al momento una funzione beta. Le funzioni beta possono venire modificate in qualsiasi momento e aggiornamenti futuri potrebbero introdurre delle modifiche incompatibili con la versione più recente.
{: important}

Se vuoi crittografare il tuo materiale della chiave prima di importarlo in {{site.data.keyword.keymanagementserviceshort}}, puoi creare una chiave di crittografia di trasporto per la tua istanza del servizio utilizzando l'API {{site.data.keyword.keymanagementserviceshort}}. 

Le chiavi di trasporto sono un tipo di risorsa in {{site.data.keyword.keymanagementserviceshort}} che consente l'importazione sicura del materiale della chiave root nella tua istanza del servizio. Utilizzando una chiave di trasporto per crittografare il tuo materiale della chiave in loco, proteggi le chiavi root mentre sono in transito verso {{site.data.keyword.keymanagementserviceshort}} in base alle politiche che hai specificato. Ad esempio, puoi configurare una politica sulla chiave di trasporto che limita l'utilizzo della chiave in base al tempo e al numero di utilizzi.

### Come funziona
{: #how-transport-keys-work}

Quando [crei una chiave di trasporto](/docs/services/key-protect?topic=key-protect-create-transport-keys) per la tua istanza del servizio, {{site.data.keyword.keymanagementserviceshort}} genera una chiave RSA a 4096-bit. Il servizio crittografa la chiave privata e poi restituisce la chiave pubblica e un token di importazione che puoi utilizzare per la crittografia e l'importazione del tuo materiale della chiave root. 

Quando sei pronto per [importare una chiave root](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-key-api) in {{site.data.keyword.keymanagementserviceshort}}, fornisci il materiale della chiave root crittografato e il token di importazione che è stato utilizzato per verificare l'integrità della chiave pubblica. Per completare la richiesta, {{site.data.keyword.keymanagementserviceshort}} utilizza la chiave privata associata alla tua istanza del servizio per decrittografare il materiale della chiave root crittografato. Questo processo garantisce che solo la chiave di trasporto che hai generato in {{site.data.keyword.keymanagementserviceshort}} possa decrittografare il materiale della chiave che importi e archivi nel servizio.

Puoi creare solo una chiave di trasporto per istanza del servizio. Per ulteriori informazioni sui limiti di richiamo per le chiavi di trasporto, [vedi la documentazione di riferimento API {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/key-protect){: new_window}.
{: note} 

### Metodi API
{: #secure-import-api-methods}

Dietro le quinte, l'API {{site.data.keyword.keymanagementserviceshort}} guida il processo di creazione della chiave di trasporto.  

La seguente tabella elenca i metodi API che configurano un locker e creano le chiavi di trasporto per la tua istanza del servizio.

<table>
  <tr>
    <th>Metodo</th>
    <th>Descrizione</th>
  </tr>
  <tr>
    <td><code>POST api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">Crea una chiave di trasporto</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">Richiama i metadati della chiave di trasporto</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers/{id}</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-import-root-keys">Richiama una chiave di trasporto e un token di importazione</a></td>
  </tr>
  <caption style="caption-side:bottom;">Tabella 2. Descrive i metodi API {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

Per ulteriori informazioni sulla gestione a livello programmatico delle tue chiavi in {{site.data.keyword.keymanagementserviceshort}}, consulta la [Documentazione di riferimento API di {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/key-protect){: new_window}.

## Operazioni successive

- Per ulteriori informazioni su come creare una chiave di trasporto per la tua istanza del servizio, vedi [Creazione di una chiave di trasporto](/docs/services/key-protect?topic=key-protect-create-transport-keys).
- Per ulteriori informazioni sull'importazione delle chiavi nel servizio, vedi [Importazione delle chiavi root](/docs/services/key-protect?topic=key-protect-import-root-keys). 
