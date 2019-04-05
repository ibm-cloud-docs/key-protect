---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: Key Protect availability, Key Protect disaster recovery

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

# Alta disponibilità e ripristino di emergenza
{: #ha-dr}

{{site.data.keyword.keymanagementservicefull}} è un servizio regionale altamente disponibile con funzionalità automatiche che aiutano a mantenere le tue applicazioni sicure e operative.
{: shortdesc}

Utilizza questa pagina per saperne di più sulle strategie di disponibilità e ripristino di emergenza di {{site.data.keyword.keymanagementserviceshort}}.

## Ubicazioni, tenancy e disponibilità 
{: #availability}

{{site.data.keyword.keymanagementserviceshort}} è un servizio regionale a più tenant. 

Puoi creare risorse {{site.data.keyword.keymanagementserviceshort}} in una delle [regioni {{site.data.keyword.cloud_notm}}](/docs/services/key-protect/regions.html) supportate, che rappresentano l'area geografica in cui vengono gestite ed elaborate le tue richieste {{site.data.keyword.keymanagementserviceshort}}. Ogni regione {{site.data.keyword.cloud_notm}} contiene [più zone di disponibilità ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2018/06/expansion-availability-zones-global-regions/) per soddisfare i requisiti di accesso locale, bassa latenza e sicurezza per la regione. 

Mentre pianifichi la tua strategia di crittografia dei dati inattivi con {{site.data.keyword.cloud_notm}}, tieni presente che il provisioning di {{site.data.keyword.keymanagementserviceshort}} in una regione più vicina a te ha maggiori probabilità di generare connessioni più veloci e affidabili quando interagisci con le API {{site.data.keyword.keymanagementserviceshort}}. Scegli una regione specifica se gli utenti, le applicazioni o i servizi che dipendono da una risorsa {{site.data.keyword.keymanagementserviceshort}} sono geograficamente concentrati. Ricorda che utenti e servizi lontani dalla regione potrebbero riscontrare una maggiore latenza.  

Le tue chiavi di crittografia sono confinate nella regione in cui le hai create. {{site.data.keyword.keymanagementserviceshort}} non copia o esporta le chiavi di crittografia in altre regioni.
{: note}

## Ripristino di emergenza
{: #disaster-recovery}

{{site.data.keyword.keymanagementserviceshort}} dispone di un ripristino di emergenza regionale con un obiettivo del tempo di risposta (RTO) di un'ora. Il servizio segue i requisiti di {{site.data.keyword.cloud_notm}} per la pianificazione e il ripristino in seguito a eventi di emergenza. Per ulteriori informazioni, vedi [Ripristino di emergenza](/docs/overview/zero_downtime.html#disaster-recovery). 


