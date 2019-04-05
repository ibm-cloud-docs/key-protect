---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: data security, Key Protect compliance, encryption key deletion

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

# Sicurezza e conformità
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} dispone di strategie di sicurezza dei dati in atto per soddisfare i tuoi bisogni di conformità e garantire che i tuoi dati rimangano sicuri e protetti nel cloud.
{: shortdesc}

## Crittografia e sicurezza dei dati 
{: #data-security}

{{site.data.keyword.keymanagementserviceshort}} utilizza gli [{{site.data.keyword.cloud_notm}} HSM (hardware security module) ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/hardware-security-module) per generare materiale della chiave gestito dal provider ed eseguire le operazioni di [crittografia envelope](/docs/services/key-protect/envelope-encryption.html). Gli HSM sono dispositivi hardware antimanomissione che archiviano ed utilizzano il materiale della chiave crittografico senza esporre le chiavi all'esterno di un confine crittografico.

L'accesso al servizio avviene tramite HTTPS e la comunicazione {{site.data.keyword.keymanagementserviceshort}} interna utilizza il protocollo TLS (Transport Layer Security) 1.2 per crittografare i dati in transito.

Per ulteriori informazioni su come {{site.data.keyword.keymanagementserviceshort}} soddisfa le tue esigenze di conformità, vedi [Conformità di servizi e piattaforme](/docs/overview/security.html#compliancetable).

## Eliminazione dei dati
{: #data-deletion}

Quando elimini una chiave, il servizio la contrassegna come eliminata e la chiave passa allo stato _Destroyed_. Le chiavi in questo stato non sono più ripristinabili e i servizi cloud che utilizzano la chiave non possono più decrittografare i dati ad essa associati. I tuoi dati rimangono in tali servizi nel loro formato crittografato. I metadati associati a una chiave, come il nome e la cronologia della chiave, vengono conservati nel database {{site.data.keyword.keymanagementserviceshort}}. 

L'eliminazione di una chiave in {{site.data.keyword.keymanagementserviceshort}} è un'operazione distruttiva. Tieni presente che dopo aver eliminato una chiave, l'azione non può essere invertita e tutti i dati associati alla chiave vengono immediatamente persi nel momento in cui viene eliminata la chiave. Prima di eliminare una chiave, controlla i dati associati alla chiave per assicurarti di non aver più bisogno di accedervi. Non eliminare una chiave che sta proteggendo i dati in modo attivo nei tuoi ambienti di produzione. 

Per aiutarti a determinare quali dati vengono protetti da una chiave, puoi vedere come la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}} associa i tuoi servizi cloud esistenti immettendo `ibmcloud iam authorization-policies`. Per ulteriori informazioni sulla visualizzazione delle autorizzazioni del servizio dalla console, vedi [Concessione dell'accesso tra i servizi](/docs/iam/authorizations.html#serviceauth).
{: note}
