---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: envelope encryption, key name, create key in different region, delete service instance

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
{:faq: data-hd-content-type='faq'}

# Domande frequenti (FAQ)
{: #faqs}

Puoi utilizzare le seguenti FAQ con {{site.data.keyword.keymanagementservicelong}}.

## Come funzionano i prezzi per {{site.data.keyword.keymanagementserviceshort}}?
{: #how-does-pricing-work}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} offre un [piano di livello graduale](https://{DomainName}/catalog/services/key-protect) senza alcun costo per gli utenti che hanno bisogno di 20 chiavi o meno. Puoi avere 20 chiavi gratuite per account {{site.data.keyword.cloud_notm}}. Se il tuo team richiede più istanze di {{site.data.keyword.keymanagementserviceshort}}, {{site.data.keyword.cloud_notm}} aggiunge le tue chiavi attive a tutte le istanze all'interno dell'account e poi applica i prezzi. 

## Cos'è una chiave di crittografia interna?
{: #what-is-active-encryption-key}
{: faq}

Quando importi le chiavi di crittografia in {{site.data.keyword.keymanagementserviceshort}} o quando utilizzi {{site.data.keyword.keymanagementserviceshort}} per generare delle chiavi dai suoi HSM, queste chiavi diventano chiavi _attive_. I prezzi si basano su tutte le chiavi attive all'interno di un account {{site.data.keyword.cloud_notm}}. 

## Come posso raggruppare e gestire le mie chiavi?
{: #how-to-group-keys}
{: faq}

Da un punto di vista dei prezzi, il modo migliore di utilizzare {{site.data.keyword.keymanagementserviceshort}} è di creare un numero limitato di chiavi root e poi utilizzarle per crittografare le chiavi di crittografia dei dati create da un'applicazione o un servizio di dati cloud esterno. 

Per ulteriori informazioni sull'utilizzo delle chiavi root per proteggere le chiavi di crittografia dei dati, vedi [Protezione dei dati con la crittografia envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption).

## Cos'è una chiave root?
{: #what-is-root-key}
{: faq}

Le chiavi root sono le risorse principali in {{site.data.keyword.keymanagementserviceshort}}. Sono le chiavi di impacchettamento della chiave simmetriche utilizzate come radice di attendibilità per la protezione di altre chiavi che vengono archiviate nel servizio di dati con la [crittografia envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption). Con {{site.data.keyword.keymanagementserviceshort}}, puoi creare, memorizzare e gestire il ciclo di vita delle chiavi root per ottenere il controllo completo di altre chiavi archiviate nel cloud. 

## Cos'è la crittografia envelope?
{: #what-is-envelope-encryption}
{: faq}

La crittografia envelope è la pratica di codificare i dati con una _chiave di crittografia dei dati_ e poi crittografarla con una _chiave di impacchettamento della chiave_ molto sicura.  I tuoi dati vengono protetti quando non attivi applicando più livelli di crittografia. Per ulteriori informazioni su come abilitare la crittografia envelope per le tue risorse {{site.data.keyword.cloud_notm}}, vedi [Integrazione dei servizi](/docs/services/key-protect?topic=key-protect-integrate-services).

## Che lunghezza può avere un nome chiave?
{: #key-names}
{: faq}

Puoi utilizzare un nome chiave con una lunghezza massima di 90 caratteri.

## Posso archiviare le informazioni personali come dei metadati per le mie chiavi?
{: #personal-data}
{: faq}

Per proteggere la riservatezza dei tuoi dati personali, evita di archiviare informazioni d'identificazione personale (PII) come dei metadati per le tue chiavi. Le informazioni personali includono i tuoi nome, indirizzo, numero di telefono, indirizzo email o altre informazioni che potrebbero identificare, contattare o individuare te, i tuoi clienti o chiunque altro.

Sei responsabile di garantire la sicurezza di tutte le informazioni che archivi come metadati per le chiavi di crittografia e le risorse {{site.data.keyword.keymanagementserviceshort}}. Per ulteriori esempi di dati personali, vedi la sezione 2.2 di [NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
{: important}

## Le chiavi possono essere create in una regione e utilizzate in un'altra regione?
{: #keys-across-regions}
{: faq}

Le tue chiavi di crittografia sono confinate nella regione in cui le hai create. {{site.data.keyword.keymanagementserviceshort}} non copia o esporta le chiavi di crittografia in altre regioni.

## Come controllo chi ha accesso alle chiavi?
{: #access-control}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} supporta un sistema di controllo dell'accesso centralizzato, controllato da
{{site.data.keyword.iamlong}}, per aiutarti a gestire gli utenti e l'accesso alle tue chiavi crittografate. Se sei un amministratore della sicurezza del tuo servizio, puoi assegnare i [ruoli Cloud IAM che corrispondono alle autorizzazioni {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-manage-access#roles) specifiche che vuoi concedere ai membri del tuo team.

## Come monitoro le chiamate API a {{site.data.keyword.keymanagementserviceshort}}?
{: faq}

Puoi utilizzare il servizio {{site.data.keyword.cloudaccesstrailfull_notm}} per tenere traccia di come gli utenti e le applicazioni interagiscono con la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Ad esempio, quando crei, importi, elimini o leggi una chiave in {{site.data.keyword.keymanagementserviceshort}}, viene generato un evento {{site.data.keyword.cloudaccesstrailshort}}. Questi eventi vengono automaticamente inoltrati al servizio {{site.data.keyword.cloudaccesstrailshort}} nella stessa regione in cui è stato eseguito il provisioning del servizio {{site.data.keyword.keymanagementserviceshort}}.

Per ulteriori informazioni, vedi [Eventi di Activity Tracker](/docs/services/key-protect?topic=key-protect-at-events).

## Cosa succede quando elimino una chiave?
{: #key-destruction}
{: faq}

Quando elimini una chiave, il servizio la contrassegna come eliminata e la chiave passa allo stato _Destroyed_. Le chiavi in questo stato non sono più ripristinabili e i servizi cloud che utilizzano la chiave non possono più decrittografare i dati ad essa associati. I tuoi dati rimangono in tali servizi nel loro formato crittografato. I metadati associati a una chiave, come il nome e la cronologia della chiave, vengono conservati nel database {{site.data.keyword.keymanagementserviceshort}}. 

Prima di eliminare una chiave, assicurati di non richiedere più l'accesso ai dati associati alla chiave. Questa azione non può essere annullata.

## Cosa succede quando devo annullare il provisioning della mia istanza del servizio?
{: #deprovision-service}
{: faq}

Se decidi di smettere di utilizzare {{site.data.keyword.keymanagementserviceshort}}, devi eliminare tutte le chiavi rimanenti dalla tua istanza del servizio prima di poter annullare il provisioning del servizio. Dopo aver eliminato la tua istanza del servizio, {{site.data.keyword.keymanagementserviceshort}} utilizza la [crittografia envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption) per eliminare a livello crittografico tutti i dati associati all'istanza del servizio. 

