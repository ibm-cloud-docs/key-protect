---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: release notes, changelog, what's new, service updates, service bulletin

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

# Novità
{: #releases}

Rimani aggiornato con le nuove funzioni disponibili per {{site.data.keyword.keymanagementservicefull}}. 

## Marzo 2019
{: #mar-2019}

### Aggiunto: {{site.data.keyword.keymanagementserviceshort}} aggiunge il supporto per la rotazione della chiave basata sulla politica
{: #added-support-policy-key-rotation}
Novità a partire da: 22-03-2019

Puoi ora utilizzare {{site.data.keyword.keymanagementserviceshort}} per associare una politica di rotazione per le tue chiavi root.

Per ulteriori informazioni, vedi [Configurazione di una politica di rotazione](/docs/services/key-protect?topic=key-protect-set-rotation-policy). Per ulteriori informazioni sulle tue opzioni di rotazione della chiave in {{site.data.keyword.keymanagementserviceshort}}, vedi [Confronto delle tue opzioni di rotazione della chiave](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).

### Aggiunto: {{site.data.keyword.keymanagementserviceshort}} aggiunge il supporto beta per le chiavi di trasporto
Novità a partire da: 20-03-2019

Abilita l'importazione sicura delle chiavi di crittografia nel cloud creando le chiavi di crittografia di trasporto per il tuo servizio {{site.data.keyword.keymanagementserviceshort}}.

Per ulteriori informazioni, vedi [Utilizzo delle tue chiavi di crittografia sul cloud](/docs/services/key-protect?topic=key-protect-importing-keys).

Le chiavi di trasporto sono al momento una funzione beta. Le funzioni beta possono venire modificate in qualsiasi momento e aggiornamenti futuri potrebbero introdurre delle modifiche incompatibili con la versione più recente.
{: important}

## Febbraio 2019
{: #feb-2019}

### Modificato: istanze del servizio {{site.data.keyword.keymanagementserviceshort}} legacy
Novità a partire da: 12-03-2019

Le istanze del servizio {{site.data.keyword.keymanagementserviceshort}} di cui è stato eseguito il provisioning prima del 15 dicembre 2017 sono in esecuzione su un'infrastruttura legacy basata su Cloud Foundry. Questo servizio {{site.data.keyword.keymanagementserviceshort}} legacy sarà disattivato il 15 maggio 2019.

**Significato per l'utente**

Se hai delle chiavi di produzione attive in un'istanza del servizio {{site.data.keyword.keymanagementserviceshort}} precedente, assicurati di migrare le chiavi alla nuova istanza del servizio entro il 15 maggio 2019 per evitare di perdere l'accesso ai tuoi dati crittografati. Puoi controllare se stai utilizzando un'istanza legacy andando al tuo elenco di risorse dalla [console {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/). Se la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}} viene elencata nella sezione **Cloud Foundry Services** dell'elenco di risorse {{site.data.keyword.cloud_notm}} o se stai utilizzando l'endpoint API `https://ibm-key-protect.edge.bluemix.net` per selezionare le operazioni per il servizio, stai utilizzando un'istanza legacy di {{site.data.keyword.keymanagementserviceshort}}. Dopo il 15 maggio 2019, l'endpoint legacy non sarà più accessibile e non potrai selezionare il servizio per le operazioni.

Hai bisogno di aiuto con la migrazione delle tue chiavi di crittografia in una nuova istanza del servizio? Per le istruzioni dettagliate, vedi il [client di migrazione in GitHub ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/IBM-Cloud/kms-migration-client){: new_window}. Se hai ulteriori domande sullo stato delle tue chiavi o sul processo di migrazione, contatta Terry Mosbaugh con [mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com).
{: tip}

## Dicembre 2018
{: #dec-2018}

### Modificato: endpoint API {{site.data.keyword.keymanagementserviceshort}}
Novità a partire da: 19-03-2019

Per allinearsi con la nuova esperienza unificata di {{site.data.keyword.cloud_notm}}, {{site.data.keyword.keymanagementserviceshort}} ha aggiornato gli URL di base per le proprie API del servizio.

Puoi ora aggiornare le tue applicazioni per fare riferimento ai nuovi endpoint `cloud.ibm.com`.

- `keyprotect.us-south.bluemix.net` ora è `us-south.kms.cloud.ibm.com` 
- `keyprotect.us-east.bluemix.net` ora è `us-east.kms.cloud.ibm.com` 
- `keyprotect.eu-gb.bluemix.net` ora è `eu-gb.kms.cloud.ibm.com` 
- `keyprotect.eu-de.bluemix.net` ora è `eu-de.kms.cloud.ibm.com` 
- `keyprotect.au-syd.bluemix.net` ora è `au-syd.kms.cloud.ibm.com` 
- `keyprotect.jp-tok.bluemix.net` ora è `jp-tok.kms.cloud.ibm.com` 

Entrambi gli URL per ognuno degli endpoint del servizio regionale sono al momento supportati. 

## Ottobre 2018
{: #oct-2018}

### Aggiunto: {{site.data.keyword.keymanagementserviceshort}} si espande nella regione Tokyo
Novità a partire da: 31-10-2018

Puoi ora creare le risorse {{site.data.keyword.keymanagementserviceshort}} nella regione Tokyo. 

Per ulteriori informazioni, vedi [Regioni e ubicazioni](/docs/services/key-protect?topic=key-protect-regions).

### Aggiunto: plugin della CLI {{site.data.keyword.keymanagementserviceshort}}
Novità a partire da: 02-10-2018

Puoi ora usare il plugin della CLI {{site.data.keyword.keymanagementserviceshort}} per gestire le chiavi nella tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.

Per ulteriori informazioni su come installare il plugin, vedi [Configurazione della CLI ](/docs/services/key-protect?topic=key-protect-set-up-cli). Per ulteriori informazioni sulla CLI {{site.data.keyword.keymanagementserviceshort}}, [consulta la documentazione di riferimento della CLI](/docs/services/key-protect?topic=key-protect-cli-reference).

## Settembre 2018
{: #sept-2018}

### Aggiunto: {{site.data.keyword.keymanagementserviceshort}} aggiunge il supporto per la rotazione di chiavi su richiesta
Novità a partire da: 28-09-2018

Puoi ora usare {{site.data.keyword.keymanagementserviceshort}} per eseguire la rotazione delle tue chiavi root su richiesta.

Per ulteriori informazioni, vedi [Rotazione delle chiavi](/docs/services/key-protect?topic=key-protect-rotate-keys).

### Aggiunto: esercitazione relativa alla sicurezza end-to-end per la tua applicazione cloud
Novità a partire da: 14-09-2018

Cerchi degli esempi di codice per aiutarti a crittografare il contenuto del bucket di archiviazione con le tue chiavi di crittografia?

Puoi ora esercitarti ad aggiungere la sicurezza end-to-end per la tua applicazione cloud attenendoti alla [nuova esercitazione](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).

Per ulteriori informazioni, [consulta l'applicazione di esempio in GitHub ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/IBM-Cloud/secure-file-storage){: new_window}.

### Aggiunto: {{site.data.keyword.keymanagementserviceshort}} si espande nella regione Washington DC
Novità a partire da: 10-09-2018

Puoi ora creare le risorse {{site.data.keyword.keymanagementserviceshort}} nella regione Washington DC. 

Per ulteriori informazioni, vedi [Regioni e ubicazioni](/docs/services/key-protect?topic=key-protect-regions).

## Agosto 2018
{: #aug-2018}

### Modificato: URL della documentazione dell'API {{site.data.keyword.keymanagementserviceshort}}
Novità a partire da: 28-08-2018

La guida di riferimento API {{site.data.keyword.keymanagementserviceshort}} è stata spostata. 

Puoi ora accedere alla documentazione dell'API all'indirizzo
https://{DomainName}/apidocs/key-protect. 

## Marzo 2018
{: #mar-2018}

### Aggiunto: {{site.data.keyword.keymanagementserviceshort}} si espande nella regione Francoforte
Novità a partire da: 21-03-2018

Puoi ora creare le risorse {{site.data.keyword.keymanagementserviceshort}} nella regione Francoforte. 

Per ulteriori informazioni, vedi [Regioni e ubicazioni](/docs/services/key-protect?topic=key-protect-regions).

## Gennaio 2018
{: #jan-2018}

### Aggiunto: {{site.data.keyword.keymanagementserviceshort}} si espande nella regione Sydney
Novità a partire da: 31-01-2018

Puoi ora creare le risorse {{site.data.keyword.keymanagementserviceshort}} nella regione Sydney. 

Per ulteriori informazioni, vedi [Regioni e ubicazioni](/docs/services/key-protect?topic=key-protect-regions).

## Dicembre 2017
{: #dec-2017}

### Aggiunto: {{site.data.keyword.keymanagementserviceshort}} aggiunge il supporto per BYOK (Bring Your Own Key)
Novità a partire da: 15-12-2017

{{site.data.keyword.keymanagementserviceshort}} ora supporta BYOK (Bring Your Own Key) e la crittografia gestita dal cliente.

- Introdotte le [chiavi root](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types), denominate anche chiavi root del cliente (CRK), come risorse primarie nel servizio. 
- Abilitata la [crittografia envelope](/docs/services/key-protect?topic=key-protect-integrate-cos#kp-cos-how) per i bucket {{site.data.keyword.cos_full_notm}}.

### Aggiunto: {{site.data.keyword.keymanagementserviceshort}} si espande nella regione Londra
Novità a partire da: 15-12-2017

{{site.data.keyword.keymanagementserviceshort}} è ora disponibile nella regione Londra. 

Per ulteriori informazioni, vedi [Regioni e ubicazioni](/docs/services/key-protect?topic=key-protect-regions).

### Modificato: ruoli {{site.data.keyword.iamshort}}
Novità a partire da: 15-12-2017

I ruoli {{site.data.keyword.iamshort}}, che determinano le azioni che puoi eseguire sulle risorse {{site.data.keyword.keymanagementserviceshort}}, sono stati modificati.

- `Administrator` è ora `Manager`
- `Editor` è ora `Writer`
- `Viewer` è ora `Reader`

Per ulteriori informazioni, vedi [Gestione dell'accesso utente](/docs/services/key-protect?topic=key-protect-manage-access).

## Settembre 2017
{: #sept-2017}

### Aggiunto: {{site.data.keyword.keymanagementserviceshort}} aggiunge il supporto per Cloud IAM
Novità a partire da: 19-09-2017

Puoi ora utilizzare {{site.data.keyword.iamshort}} per configurare e gestire le politiche di accesso per le tue risorse {{site.data.keyword.keymanagementserviceshort}}.

Per ulteriori informazioni, vedi [Gestione dell'accesso utente](/docs/services/key-protect?topic=key-protect-manage-access).
