---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: data security, Key Protect compliance, encryption key deletion

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

# Sicurezza e conformità
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} dispone di strategie di sicurezza dei dati in atto per soddisfare i tuoi bisogni di conformità e garantire che i tuoi dati rimangano sicuri e protetti nel cloud.
{: shortdesc}

## Disponibilità della sicurezza
{: #security-ready}

{{site.data.keyword.keymanagementserviceshort}} garantisce la disponibilità della sicurezza aderendo alle procedure ottimali di {{site.data.keyword.IBM_notm}} per i sistemi, la rete e la progettazione sicura. 

Per ulteriori informazioni sui controlli di sicurezza in {{site.data.keyword.cloud_notm}}, vedi [Come faccio a sapere che i miei dati sono sicuri?](/docs/overview?topic=overview-security#security).
{: tip}

### Crittografia dei dati
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}} utilizza gli [HSM (Hardware Security Module) di {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/hardware-security-module){: external} per generare materiale della chiave gestito dal provider ed eseguire operazioni di [crittografia envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption). Gli HSM sono dispositivi hardware antimanomissione che archiviano ed utilizzano il materiale della chiave crittografico senza esporre le chiavi all'esterno di un confine crittografico.

L'accesso al servizio avviene tramite HTTPS e la comunicazione {{site.data.keyword.keymanagementserviceshort}} interna utilizza il protocollo TLS (Transport Layer Security) 1.2 per crittografare i dati in transito.

### Eliminazione dei dati
{: #data-deletion}

Quando elimini una chiave, il servizio la contrassegna come eliminata e la chiave passa allo stato _Destroyed_. Le chiavi in questo stato non sono più ripristinabili e i servizi cloud che utilizzano la chiave non possono più decrittografare i dati ad essa associati. I tuoi dati rimangono in tali servizi nel loro formato crittografato. I metadati associati a una chiave, come il nome e la cronologia della chiave, vengono conservati nel database {{site.data.keyword.keymanagementserviceshort}}. 

L'eliminazione di una chiave in {{site.data.keyword.keymanagementserviceshort}} è un'operazione distruttiva. Tieni presente che dopo aver eliminato una chiave, l'azione non può essere invertita e tutti i dati associati alla chiave vengono immediatamente persi nel momento in cui viene eliminata la chiave. Prima di eliminare una chiave, controlla i dati associati alla chiave per assicurarti di non aver più bisogno di accedervi. Non eliminare una chiave che sta proteggendo i dati in modo attivo nei tuoi ambienti di produzione. 

Per aiutarti a determinare quali dati vengono protetti da una chiave, puoi vedere come la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}} associa i tuoi servizi cloud esistenti immettendo `ibmcloud iam authorization-policies`. Per ulteriori informazioni sulla visualizzazione delle autorizzazioni del servizio dalla console, vedi [Concessione dell'accesso tra i servizi](/docs/iam?topic=iam-serviceauth).
{: note}

## Disponibilità della conformità
{: #compliance-ready}

{{site.data.keyword.keymanagementserviceshort}} soddisfa i controlli per gli standard di conformità globali, di settore e regionali, inclusi GDPR, HIPAA, ISO 27001/27017/27018 e altri. 

Per un elenco completo di certificazioni di conformità {{site.data.keyword.cloud_notm}}, vedi [Compliance on the {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.
{: tip}

### Supporto UE
{: #eu-support}

{{site.data.keyword.keymanagementserviceshort}} dispone di controlli aggiuntivi per proteggere le tue risorse {{site.data.keyword.keymanagementserviceshort}} nell'Unione Europea (UE). 

Se utilizzi le risorse {{site.data.keyword.keymanagementserviceshort}} nella regione di Francoforte, Germania, per elaborare i dati personali per i cittadini europei, puoi abilitare l'impostazione Supportato UE per il tuo account {{site.data.keyword.cloud_notm}}. Per ulteriori informazioni, vedi [Abilitazione dell'impostazione Supportato UE](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported) e [Richiesta di supporto per le risorse nell'Unione Europea](/docs/get-support?topic=get-support-getting-customer-support#eusupported).

### Regolamento generale sulla protezione dei dati (GDPR, General Data Protection Regulation)
{: #gdpr}

Il GDPR intende creare un quadro normativo armonizzato in materia di protezione dei dati in tutta l'UE e mira a restituire ai cittadini il controllo dei propri dati personali, imponendo nel contempo regole rigide a chi ospita ed elabora questi dati, in qualsiasi parte del mondo.

{{site.data.keyword.IBM_notm}} si impegna a fornire ai nostri clienti e ai Business Partner {{site.data.keyword.IBM_notm}} soluzioni innovative per la privacy, la sicurezza e la governance dei dati per aiutarli nel loro viaggio verso la disponibilità del GDPR.

Per garantire la conformità GDPR per le tue risorse {{site.data.keyword.keymanagementserviceshort}}, [abilita l'impostazione Supportato UE](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported) per il tuo account {{site.data.keyword.cloud_notm}}. Puoi scoprire di più su come {{site.data.keyword.keymanagementserviceshort}} elabora e protegge i dati personali controllando i seguenti addendum.

- [{{site.data.keyword.keymanagementservicefull_notm}} Data Sheet Addendum (DSA)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=180A0EC0658B11E5A8DABB56563AC132){: external}
- [{{site.data.keyword.IBM_notm}} Data Processing Addendum (DPA)](https://www.ibm.com/support/customer/csol/terms/?cat=dpa){: external}

### Supporto HIPAA
{: #hipaa-ready}

{{site.data.keyword.keymanagementserviceshort}} soddisfa i controlli per l'HIPAA (Health Insurance Portability and Accountability Act) degli Stati Uniti per garantire la salvaguardia delle informazioni sanitarie protette (PHI). 

Se tu o la tua azienda siete un ente disciplinato come definito dall'HIPAA, puoi abilitare l'impostazione Supportato HIPPA per il tuo account {{site.data.keyword.cloud_notm}}. Per ulteriori informazioni, vedi [Abilitazione dell'impostazione Supportato HIPAA](/docs/account?topic=account-eu-hipaa-supported#enabling-hipaa).

### ISO 27001, 27017, 27018
{: #iso}

{{site.data.keyword.keymanagementserviceshort}} è certificato ISO 27001, 27017, 27018. Puoi visualizzare le certificazioni di conformità consultando [Compliance on the {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}. 

### SOC 2 Type 1
{: #soc2-type1}

{{site.data.keyword.keymanagementserviceshort}} è certificato SOC 2 Type 1. Per informazioni su come richiedere un report {{site.data.keyword.cloud_notm}} SOC 2, vedi [Compliance on the {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.
